# Config file creation guide

Freepaths is used by providing a config file to the program that controls the simulation. This config file is in the form of a python script. In this python script you can either define the freepaths parameters directly like this `NUMBER_OF_PHONONS = 100`, or you can also use python code to set the freepaths parameters like this for example `TIMESTEP = 300e-9 / 10000`.

Each parameter has a default value which is defined in the [default_config.py](https://github.com/anufrievroman/freepaths/blob/master/freepaths/default_config.py) file. If you provide no config file freepaths will simply run the default config. And each parameter that you do not set will use the default value. If you input a non-existent parameter, it will be ignored and no error will be raised, so double-check the parameters.

All freepaths parameters are explained on this site grouped in a logical way. For some parameters not only what they do but also some theoretical explanations and assumptions behind them are detailed. 

In the first section the most important parameters are explained, the second section is detailing some more in depth aspects of the simulation are presented and in the last section the parameters mainly used for adjusting the simulation outputs are shown.

While the default values for the parameters, which are the ones shown on this page, are set to realistic values so that if a parameter is not given the default value should work fine i sill recommend you to read this entire page so that you are informed about all features freepaths has and do not overlook anything when setting up you simulation.

Please be advised that the information on this page might not be quite up to date.

After learning about the parameters please have a look at the example files provided [here](https://github.com/anufrievroman/freepaths/tree/master/examples) some of which have further explanations on the wiki. 

## Basic parameters

### Most basic parameters

These parameters control the most fundamental elements of the simulation.

```python
OUTPUT_FOLDER_NAME             = 'Si nanowire at 300 K'
NUMBER_OF_PHONONS              = 5000
TIMESTEP                       = 2e-12
NUMBER_OF_TIMESTEPS            = 200000
T                              = 300
```

➡️ `OUTPUT_FOLDER_NAME` : string   
The outputs of the simulation will be saved in the Results folder. In this folder another folder with this name will be created which will contain the outputs files. So in this case the result files will be in `Results/Si nanowire at 300 K`. Keep in mind that the Results folder will be created in the folder you executed the freepaths command. Also pay attention that if the simulation is run again the results will be overwritten without warning.  
A useful trick is to use f-strings to automatically name the output folders. For example if the same simulation is to be run at multiple temperatures using `OUTPUT_FOLDER_NAME = f'Simulation at {T}K'` will automatically put the simulation temperature in the output folder name. Just make sure to define the parameters (`T` in this case) before.

➡️ `NUMBER_OF_PHONONS` : int  
This will define how many phonons are simulated. Since the Monte Carlo simulation approach is inherently statistical more phonons should result in more stable results with less standard variation between the results of different simulations at the cost of more calculation time.

➡️ `TIMESTEP` : float  
The phonons are not simulated in a continuous fashion but only every timestep. If the timestep is small the phonon behavior will be more realistic but the simulation time will increase. And vice versa for a large timestep. Because the phonons are only simulated every timestep the time between two scattering events of a particular phonon can not be smaller than the timestep so take this into account especially when simulating at high temperatures where scattering events are more frequent. If you experience wrong or unexpected phonon behavior reducing the timestep can also help with this.

➡️ `NUMBER_OF_TIMESTEPS` : int  
The phonon is simulated until it reaches a cold side or until the number of timesteps is reached. This is to prevent infinite calculation times if a phonon gets stuck somewhere. At the end of the simulation the percentage of phonons that reach the cold side is displayed in the terminal. The number of timesteps should usually be high enough so that most (more than 90% or 95%) of phonons reach the cold side.

➡️ `T` : float  
The temperature of the simulation in Kelvin. Since increasing the temperature increases the number of scattering events and phonons take longer to traverse the structure simulations at hight temperatures take significantly longer than at low temperatures.

### Simulation domain

The simulation domain consists of a box. These parameters control the size of the box. The unit is meters.

```python
THICKNESS                        = 150e-9
WIDTH                            = 500e-9
LENGTH                           = 2000e-9
```

➡️ `THICKNESS` : float  
Defines the thickness of the simulation domain in meters. This corresponds to the z coordinate and the box will span from `-THICKNESS/2` to `THICKNESS/2` on the z axis.

➡️ `WIDTH` : float  
Defines the width of the simulation domain in meters. This corresponds to the x coordinate and the box will span from `-WIDTH/2` to `WIDTH/2` on the x axis. 

➡️ `LENGTH` : float  
Defines the length of the simulation domain in meters. This corresponds to the y coordinate and the box will span from `0` to `LENGTH` on the y axis. 

### Simulation boundaries

Each side of the simulation domain can either be a wall that phonons scatter on, a cold side that phonons disappear on, a hot side where phonons rethermalize or empty so that phonons can fly through it. The floor and ceiling of the box are always physical walls.
By default, the bottom is assumed to be hot, the top is assumed cold, and the left and right walls are just physical walls.

```python
# Walls:
INCLUDE_RIGHT_SIDEWALL           = True
INCLUDE_LEFT_SIDEWALL            = True
INCLUDE_TOP_SIDEWALL             = False
INCLUDE_BOTTOM_SIDEWALL          = False

# Cold sides:
COLD_SIDE_POSITION_TOP           = True
COLD_SIDE_POSITION_BOTTOM        = False
COLD_SIDE_POSITION_RIGHT         = False
COLD_SIDE_POSITION_LEFT          = False

# Hot sides:
HOT_SIDE_POSITION_TOP            = False
HOT_SIDE_POSITION_BOTTOM         = True
HOT_SIDE_POSITION_RIGHT          = False
HOT_SIDE_POSITION_LEFT           = False
```

There will not be a section for every parameter in this section because it would be redundant.  
To set a wall to be solid set the corresponding `INCLUDE_X_SIDEWALL = True` and set `COLD_SIDE_POSITION_X = False` and `HOT_SIDE_POSITION_X = False`. The same goes if you want to set a wall to a hot or cold side. If a wall is assigned multiple functions an error will be thrown. And if you want a wall to be open set all three parameters to `False`.

<figure><img src="../.gitbook/assets/default_setup.png" alt=""><figcaption><p>Top view of a default simulation domain.</p></figcaption></figure>

### Phonon sources

The phonons are not emitted by the hot sides but by phonon sources. A source of phonons is an area where phonons are generated in a given direction. This area can be placed anywhere in the structure. 

```python
PHONON_SOURCES = [Source(
		    	x=0, y=0, z=0, 
			    size_x=0, size_y=0, size_z=0, 
			    angle_distribution="random_up"
                )]
```

➡️ `PHONON_SOURCES` : list  
The list needs to conation one or multiple Sources. Often this will be one source on the hot side. Here is an example (note that all values of a Source that are not set will be zero):

```python
PHONON_SOURCES = [
                Source(x=-300e-9, size_x=100e-9),
                Source(x=0, size_x=100e-9),
                Source(x=300e-9, size_x=100e-9)
                ]
```

It is often very useful to use `WIDTH`, `LENGTH` and `THICKNESS` like in this example:

```python
PHONON_SOURCES = [Source(
			    x=0, y=LENGTH/2, z=0, 
			    size_x=0, size_y=LENGTH, size_z=THICKNESS, 
			    angle_distribution="random_right"
                )]
```

Note that the source angle distribution is also adjusted. The angle distribution can be chosen among one of those shown in the image below. In the case of multiple sources the phonons will be emitted from them with equal probability.

<figure><img src="../.gitbook/assets/distributions.png" alt=""><figcaption><p>Available phonon angle distributions at the phonon source.</p></figcaption></figure>

### Holes and pillars

While holes are not necessarily required for a simulation, which you can see in the empty default value, they are a key aspect of freepaths which is why they are in the necessary parameters section.

```python
HOLES = []
PILLARS = []
```

➡️ `HOLES` : list  
To build any structure in freepaths holes are used. A hole has a certain shape and cuts through the simulation domain in the z direction. A selection of holes and their parameters are shown in the image below. If you want to look at the holes and their parameters in more detail have a look at the [holes.py](https://github.com/anufrievroman/freepaths/blob/master/freepaths/holes.py) file. I also recommend to have a look at the [all_shapes.py](https://github.com/anufrievroman/freepaths/blob/master/examples/all_shapes.py) example file.

<figure><img src="../.gitbook/assets/shapes.png" alt=""><figcaption><p>Possible shapes of holes and walls with their respective parameters.</p></figcaption></figure>

Note that `ParabolaBottom` and `ParabolaTop` are special because you cannot place them anywhere in the simulation domain. They will always appear at the top or bottom side of the structure. See the [parabolic_lens_focusing.py](https://github.com/anufrievroman/freepaths/blob/master/examples/parabolic_lens_focusing.py) example.

To add holes to the simulation simply put them into the list. It is often very useful to generate these holes using some simple python code. For example, the following code will create a 5x6 square lattice of circular holes:

```python
HOLES = []
period = 300e-9
for row in range(5):
    for column in range(6):
        x = - 4 * period / 2 + column * period
        y = (row + 1) * period
        HOLES.append(CircularHole(x=x, y=y, diameter=200e-9))
```

➡️ `PILLARS` : list  
Pillars are a more experimental feature and the only pillar available at the time is `CircularPillar`. Pillars work the same way as holes but instead of preventing phonons to enter a certain area of the simulation domain they extend the simulation domain in z direction locally.

### Multiprocessing parameter

```python
NUMBER_OF_PROCESSES              = 10
```

➡️ `NUMBER_OF_PROCESSES` : int  
Every phonon is simulated independently one after the other. To speed up the calculation the phonons should be distributed across multiple processes which will each simulate phonons independently. This value should be set to a value close to the amount of threads your processor has. Please take note that the progress percentage displayed in the terminal is the progress of a single process and that some processes will take longer than others to finish.

## Advanced simulation parameters

### Material parameters

```python
MEDIA                            = "Si"
SPECIFIC_HEAT_CAPACITY           = 714  # [J/kg/K] for Si at 300 K
IS_TWO_DIMENSIONAL_MATERIAL      = False
```

➡️ `MEDIA` : str  
This parameter describes what material the simulation domain is made of. Phonons speed and internal scattering behavior are examples of what is affected by this. Current choices are:

* Si
* SiC
* Diamond
* AlN
* Graphite

➡️ `SPECIFIC_HEAT_CAPACITY` : float  
This is used for determining the structure's temperature which is necessary for the thermal conductivity calculation. Currently this has to be set manually and should correspond to the simulation temperature `T`. It is planned to have it automatically set through the `MEDIA` and `T` parameters.

➡️ `IS_TWO_DIMENSIONAL_MATERIAL` : bool  
If this is set to `True` the z dimension will be ignored and the simulation will take place only in the x-y plane. This is usually used for Graphene sheet simulation.

### Roughness parameters

When phonons scatter on a surface the surface's roughness influences the probability of specular of diffuse scattering on the surface. 

```python
SIDE_WALL_ROUGHNESS              = 2e-9
TOP_ROUGHNESS                    = 0.2e-9
BOTTOM_ROUGHNESS                 = 0.2e-9
HOLE_ROUGHNESS                   = 2e-9
PILLAR_ROUGHNESS                 = 2e-9
PILLAR_TOP_ROUGHNESS             = 0.2e-9
```

The roughness values are expressed in meters. Most of the variable names are pretty self-explanatory. To clarify, `TOP_ROUGHNESS` references the simulation domain boundary in positive z direction and `BOTTOM_ROUGHNESS` in negative z direction. `SIDE_WALL_ROUGHNESS` are all other simulation domain boundaries.

If the roughness is set to a very low value the scattering will be mostly specular which can be used on the side walls for example as a symmetric/periodic boundary condition. It can also be used for for testing purposes to check if the scattering behavior is programmed correctly.

### Phonon behavior parameters

```python
INCLUDE_INTERNAL_SCATTERING      = True
USE_GRAY_APPROXIMATION_MFP       = False
GRAY_APPROXIMATION_MFP           = None
```

➡️ `INCLUDE_INTERNAL_SCATTERING` : bool  
If this is set to `False` phonons will not experience internal scattering. This means that they will only be scattered on Holes and simulation boundaries. This is mainly used for debugging and testing purposes.

➡️ `USE_GRAY_APPROXIMATION_MFP` : bool  
Use the gray approximation to determine the internal scattering rate.

➡️ `GRAY_APPROXIMATION_MFP` : float  
If `USE_GRAY_APPROXIMATION_MFP` is set this needs to be set to the phonon mean free path to be used for the gray approximation.

### Simulation time parameters

Please do not confuse the "virtual" timesteps discussed in this section with the timesteps discussed in the Most basic parameters section. The `NUMBER_OF_TIMESTEPS` parameter defines the maximum time a phonon has to travel through the structure while the parameters of this section are used to make sure the simulation is in a steady state. 

Considering the `Thermal map.pdf` and the resulting `Temperature profile.pdf` please consider that the physics of the entire simulation behave with the temperature of the parameter `T` even if `Temperature profile.pdf` shows a significantly higher temperature. This is because the temperature in `Temperature profile.pdf` results from the amount of heat that enters the structure which is dependant on `NUMBER_OF_PHONONS`. Thus the temperatures in `Temperature profile.pdf` should not be taken at face value. For the thermal conductivity calculation the gradient of this profile is used.

```python
NUMBER_OF_VIRTUAL_TIMESTEPS      = 300000
INITIALIZATION_TIMESTEPS         = 50000
NUMBER_OF_INITIALIZATION_TIMEFRAMES = 3
```

➡️ `NUMBER_OF_VIRTUAL_TIMESTEPS` : int  
The phonons do not all enter the structure at the same time but a virtual start time is assigned to each phonon randomly and the range of these start times is controlled with this parameter. This means that because no phonons are generated before the simulation starts that the first moments of the simulation are not useful because all phonos are in the beginning of the structure and none are towards the end of the structure. This also means that phonons that enter the structure towards the end of the simulation time and exit the structure after the simulation time are not considered for some calculations during their entire flight time. This is not a huge issue but be aware that the shorter the simulation time is with respect to the time the phonons need to traverse the structure, the more information that is generated is not considered. So this parameter should at least be a couple times larger that the time it takes phonons to traverse the structure. The time it takes phonons to traverse the structure can be determined with `Distribution of travel times.pdf` (Determining the 95% or 99% quantile by eye should be sufficient).

➡️ `INITIALIZATION_TIMESTEPS` : int  
To address the issue of the start of the simulation not being useful the amount of timesteps entered here will not be considered for the final calculation. This value should be about one or two times the time it takes phonons to traverse the structure.

➡️ `NUMBER_OF_INITIALIZATION_TIMEFRAMES` : int  
The temperature profile, heat flux profile ant thermal conductivity are not only calculated for the time after the initialization timesteps but also for some timeframes during the initialization timeframes. This parameter defines how many of these timeframes are created in the initialization time. This can be useful to observe the convergence of the profiles towards the profile of the final timestep.

## Plotting/Output parameters

### Thermal maps

The heat flux maps, the thermal map and the pixel volumes plot are all based on the same pixel grid. The thermal conductivity calculation is heavily dependant on these plots.

Concerning the heat flux maps it is important to consider that the `Heat flux map.pdf` file displays the absolute magnitude of heat flux. So if a phonon travels through a place twice in opposite directions the heat flux will be added. In the `Heat flux map x.pdf` and `Heat flux map y.pdf` the directional heat flux is calculated which means that if a phonon travels through a place twice in opposite directions the heat flux will cancel out.

```python
NUMBER_OF_PIXELS_X               = 25
NUMBER_OF_PIXELS_Y               = 100
IGNORE_FAULTY_PHONONS            = False
```

➡️ `NUMBER_OF_PIXELS_X` `NUMBER_OF_PIXELS_Y` : int
These parameters define the pixel grid which is used for the map creation so a higher number will generally result in higher quality maps. Be informed that this is not purely for looks but that the thermal conductivity calculations rely on the values in these maps. I recommend to set these using this code snippet where the pixel size can be adjusted: 

```python
pixel_size = 30e-9
NUMBER_OF_PIXELS_X             = int(WIDTH / pixel_size)
NUMBER_OF_PIXELS_Y             = int(LENGTH / pixel_size)
```

➡️ `IGNORE_FAULTY_PHONONS` : bool  
It is possible that phonons escape the structure and get trapped outside the structure or travel outside the simulation domain. This can cause the maps to not look nice because the holes are not empty. If this parameter is set to `True` all phonons that are outside the structure are simply ignored in the map generation which improves both the look and the calculations. I recommend to keep this on `False` so that you notice if phonons leave the structure which can be an indicator of some errors or bugs. If phonons leave the structure this can be turned on to still get clean data.

### Structure plots

```python
OUTPUT_SCATTERING_MAP            = False
OUTPUT_TRAJECTORIES_OF_FIRST     = 50
OUTPUT_STRUCTURE_COLOR           = "#F0F0F0"
```

➡️ `OUTPUT_SCATTERING_MAP` : bool  
If this is set to `True` an additional output file `Scattering map.pdf` will be generated in which the position and type of all scattering events is shown. This can be very useful for debugging and testing.

➡️ `OUTPUT_TRAJECTORIES_OF_FIRST` : int  
This parameter defines how many trajectories of phonons are saved into the `Phonon paths.csv` output file and how many phonon trajectories are plotted in the `Phonon paths XY.pdf` and `Phonon paths YZ.pdf` output files.

➡️ `OUTPUT_STRUCTURE_COLOR` : str  
This variable defines the background color in the `Phonon paths XY.pdf` and `Phonon paths YZ.pdf` output plots. You can use a hexadecimal color or a color name like `'blue'` or any color that matplotlib accepts.

### Line plots

```python
NUMBER_OF_NODES                  = 400
NUMBER_OF_LENGTH_SEGMENTS        = 10
```

➡️ `NUMBER_OF_NODES` : int  
This value affects the number of "buckets" for the histogram output plots like for example `Distribution of angles.pdf`.

➡️ `NUMBER_OF_LENGTH_SEGMENTS` : int  
A few plots display information in segments along the y axis like `Scattering rates.pdf` and `Time spent in segments.pdf`. The number of segments for these plots can be adjusted with this parameter.

### Animation

**Animations are currently not working. See [this issue](https://github.com/anufrievroman/freepaths/issues/7).**

Note that NUMBER_OF_TIMESTEPS should not be too large, otherwise the generation of animation may take a very long time, because one frame for each time step will be created. A few hundred time steps is a reasonable value.

```python
OUTPUT_PATH_ANIMATION            = False
OUTPUT_ANIMATION_FPS             = 24
```

➡️ `OUTPUT_PATH_ANIMATION` : bool  
Set this to `True` to generate an animation.

➡️ `OUTPUT_ANIMATION_FPS` : int  
Each timestep corresponds to one frame. This parameter determines the playback speed of the frames in the generated video. Please note that all phonons will start flying at the beginning of the video.
