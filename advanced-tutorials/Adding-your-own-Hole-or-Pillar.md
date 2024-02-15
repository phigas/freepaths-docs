# Adding your own Hole or Pillar

This page will guide you through the process of adding and testing your own hole into freepaths (pillars work similarly to holes but this page will focus on holes). Please note that you need some Python knowledge to do this. It is recommended to [fork the main repository ](https://github.com/anufrievroman/freepaths/fork) so you have your own version to work on.

## Understanding the code structure

All holes and pillars are defined in the [holes.py](https://github.com/anufrievroman/freepaths/blob/master/freepaths/holes.py) file. And each hole and pillar is defined as a class in this file. For the hole to work with freepaths some requirements need to be fulfilled. (Pillars have an added requirement of needing to have both `x0` and `y0` as class attributes)

I will walk you through the requirements step by step in the following sections. Please feel free to look at the other holes to see how they work if you need some inspiration.

## Creating a minimal example

The class needs to have some predefined functions to work and i will go through them one by one. Let's start by creating the class and it's `__init__` method:

```python
class NewHole(Hole)
	def __init__(self):
		pass
```

The class inherits from the general Hole class (which does not do anything currently). As for any python class use the `__init__` method to receive all parameters for the class and do any heavy precalculations you will need for the other methods here as this is only executed once.

Let's continue by adding the `is_inside` method:

```python
    def is_inside(self, x, y, z, cf):
        pass
```

Except for the `__init__` method the arguments of all methods of the hole classes need to be exactly as specified to work and the methods need to return the correct object types.

The `is_inside` method is used to check if a point is inside the hole or not. If the point is not inside the hole the method has to return nothing (`None`) but if the point is inside the method should return what surface of the hole the phonon will scatter off. This is later used in the `check_if_scattering` method. Let's add that one now:

```python
	def check_if_scattering(self, ph, scattering_types, x, y, z, cf):
		if self.is_inside(x, y, z, cf):
			pass
``` 

This method will first use the `is_inside` method to check if a point is inside the hole and will then perform the scattering if it is. This function updates the `ph` and `scattering_types` objects it is given and as such returns nothing. 

Let's add the last method:

```python
	def get_patch(self, color_holes, cf):
		return Circle(
            (0, 0),
            1e-8,
            facecolor=color_holes,
        )
```

This method is used to plot the hole onto the output plots like the `Structure XY.pdf` output file. It needs to return a [matplotlib Patch object](https://matplotlib.org/stable/api/patches_api.html) or an error will be thrown so let's just put in a small circle at the origin as a placeholder.

You now have a working hole that you can use in freepaths without it throwing any errors. But obviously it doesn't do anything yet so let's start adding some functionality. 

## Creating the hole shape

The first step is to finish the `__init__` method to get and store things like the hole position or size and to do any preliminary calculations. I recommend avoiding to use `x` and `y` as attribute names and to use `x0` and `y0` instead since `x` and `y` and `z` are used in the other methods.

After that let's add the `is_inside` method. Remember that this method should evaluate if the given point is inside the hole and return what surface the phonon will scatter off (or you can see it as what region of the hole the phonon is in) if it is inside. But really the is_inside method just needs to return anything if the point is inside and nothing if not and you can do any further analysis in the `check_if_scattering` method.

After you finished the `is_inside` method you can test it by running any simulation and checking the `Pixel volumes.pdf` output file. It should show the shape of your hole in black. (Make sure to increase `NUMBER_OF_PIXELS_X` and `NUMBER_OF_PIXELS_Y` to get a higher resolution image. Also make sure to check if all parameters of the hole class (like the position and size) have the desired effect.

Example config file you can use for testing:

```python
# General parameters
OUTPUT_FOLDER_NAME               = "New hole test"
NUMBER_OF_PHONONS                = 10
NUMBER_OF_TIMESTEPS              = 100
NUMBER_OF_PROCESSES              = 1

# System dimensions [m]:
WIDTH                          = 1000e-9
LENGTH                         = 1000e-9

# Map & profiles parameters:
pixel_size = 10e-9
NUMBER_OF_PIXELS_X             = int(WIDTH / pixel_size)
NUMBER_OF_PIXELS_Y             = int(LENGTH / pixel_size)

# Holes
# you can also add multiple holes to test
HOLES                          = [NewHole(x=0, y=LENGTH/2, size_x=300e-9, size_y=400e-9)]
```

## Making the hole visible

Next let's finalize the `get_patch` method so that the hole will show up in the structure plots. While this step is not necessarily required for the hole to behave correctly i still highly recommend doing it now so that you can use it later to debug the scattering.

As explained before the `get_path` method needs to return a [matplotlib Patch object](https://matplotlib.org/stable/api/patches_api.html) that describes the shape of the hole. Please check the [matplotlib documentation](https://matplotlib.org/stable/api/patches_api.html) but you will probably end up using a [Polygon patch](https://matplotlib.org/stable/api/_as_gen/matplotlib.patches.Polygon.html#matplotlib.patches.Polygon).

Now you can just run the simulation again and make sure your hole shows up correctly in `Structure XY.pdf` or `Phonon paths XY.pdf`. You can compare `Structure XY.pdf` and `Pixel volumes.pdf` to see if they look the same.

## Making the hole functional

The last step is to add the scattering behavior in the `check_if_scattering` method. This is a bit more difficult and i will explain the basics here but please have a look at how it is done in the [holes.py](https://github.com/anufrievroman/freepaths/blob/master/freepaths/holes.py) file. 

In the `check_if_scattering` method the first step is to check if the point is inside the hole with the `is_inside` method. Sometimes you only need to check for one area like in the `CircularHole` and sometimes it makes sense to check for multiple areas like the `TriangularUpHalfHole` for example. After you detected where the phonon will scatter it's time to implement the scattering behavior. This usually means calculating the angle with which the phonon is striking the hole. After that usually a scattering function is called which will return a scattering event and put it into `scattering_types.holes` and it will change `ph.phi` and `ph.theta` to correspond to the direction after the scattering. Oftentimes you don't need to create a new scattering function but you can use one of the ones in the [scattering_primitives.py](https://github.com/anufrievroman/freepaths/blob/master/freepaths/scattering_primitives.py) file. One example of a hole that uses a scattering function is `CircularHole`. Some holes have their scattering behavior programmed in like `ParabolaBottom` for example.

A small trick: If you have problems with phonons getting stuck inside the hole (especially at low temperatures) it can help to check if the phonon is traveling towards the hole. This can be done by comparing the current position of the hole (`ph.x`, `ph.y` and `ph.z`) and the next position (`x`, `y`, `z`). See `CircularHole` for an example.

Now you can test the hole with the config i provide below. Watch for stuff like phonons traveling through the hole or phonons scattering off the hole at weird angles. Also try to increase and decrease the `TIMESTEP` to see how it affects the scattering behavior. It is normal that the phonons do not all scatter exactly at the hole surface but close to the phonon surface. This is where the `get_patch` method comes in handy because you can see the actual border of the hole. Make sure that one is also correct while you're at it.

Here is the example config you can use to test the scattering behavior:

```python
# General parameters
OUTPUT_FOLDER_NAME               = "New hole scattering test"
NUMBER_OF_PHONONS                = 500
OUTPUT_TRAJECTORIES_OF_FIRST     = NUMBER_OF_PHONONS
TIMESTEP                         = 2e-12
NUMBER_OF_TIMESTEPS              = 200000
NUMBER_OF_PROCESSES              = 10

# System dimensions [m]:
WIDTH                            = 1000e-9
LENGTH                           = 1000e-9

# Map & profiles parameters:
pixel_size = 10e-9
NUMBER_OF_PIXELS_X               = int(WIDTH / pixel_size)
NUMBER_OF_PIXELS_Y               = int(LENGTH / pixel_size)

# Make sure only specular scattering
T                                = 1
INCLUDE_INTERNAL_SCATTERING      = False
SIDE_WALL_ROUGHNESS              = 1e-12
HOLE_ROUGHNESS                   = 1e-12
TOP_ROUGHNESS                    = 1e-12
BOTTOM_ROUGHNESS                 = 1e-12

# Holes
# you can also add multiple holes to test
HOLES                          = [NewHole(x=0, y=LENGTH/2, size_x=300e-9, size_y=400e-9)]

# Sources
PHONON_SOURCES                 = [Source(x=0, y=0, z=0, size_x=WIDTH,  size_y=0, size_z=THICKNESS, angle_distribution="random_up")]
```

## Afterwards

I hope you enjoyed this small tutorial and that you managed to make your hole work 😉. 
If the hole works well feel free to make a pull request into the main repository so that other people can use it. But please make sure that the pull request is clean and does not contain temporary files or unrelated changes.
