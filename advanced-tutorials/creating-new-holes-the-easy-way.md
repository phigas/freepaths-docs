# Creating new holes (the easy way)

This tutorial explains how you can crate new hole shapes using the very flexible `PointLineHole` and the even easier to use `FunctionLineHole`. There is an [alternative way](adding-your-own-hole-or-pillar-the-advanced-way.md) of creating new hole shapes through programming but this is a lot more difficult.

In simple words you can use the `PointLineHole` to draw any line you want which will then become a hole. It is quite easy to define these lines using mathematical functions or python functions. How to do this is explained below with more detail. As an example for a hole defined with a mathematical function here is a hole build using the function $sin(x)/x$.

<figure><img src="../.gitbook/assets/sinxdivx.png" alt=""><figcaption><p>Example hole in the shape of $sin(x)/x$.</p></figcaption></figure>

## How does the PointLineHole work behind the scenes

The `PointLineHole` creates many circular holes that are at closely spaced points along a line. If the circular holes are spaces closely enough this will give the impression of a continuous line with a certain thickness. If there are not enough circles or points the shape will become irregular and the scattering will be inconsistent as you can see in the below image. So always make sure that the holes are spaced closely enough.

<figure><img src="../.gitbook/assets/lowresolution.png" alt=""><figcaption><p>Example of low number of circles.</p></figcaption></figure>

The `FunctionLineHole` is just a wrapper for `PointLineHole` where you provide a mathematical function that is used to generate the holes.

## How to use a mathematical function to define a hole

The `FunctionLineHole` uses a mathematical function to define the hole shape. Let's use $sin(x)/x$ as an example. Here is how this is coded.

```python
HOLES = [FunctionLineHole(x=0, y=200e-9, function_range=(-2*np.pi, np.pi), function=lambda x: np.sin(x)/x, size_x=500e-9, size_y=300e-9, resolution=1e-9)]
```

Let's go through the arguments one by one:
- With the `x` and `y` arguments can be used to position the hole in the structure. It is an offset between the origin of the simulation's coordinate system and the functions coordinate system. So if both are set to 0 the function will simply be drawn in the structure's coordinate system.
- The `function_range` argument defines from what x value to what x value the function is plotted. For $sin(x)/x$ $-2pi$ and $2pi$ are good spots. You can see this example above. For demonstration purposes here i will use an range of $-2pi$ and $pi$. Notice how with an asymmetric range the origin of the function and therefore the reference point for it's position is not at it's center.
- The simplest way of defining the `function` is using a lambda function. Lambda functions are very easy to use: Just write `lambda x: ` and put the expression to be evaluated behind this. It is also possible do simply define a regular function which we will come back to.
- You can use `size_x` and `size_y` to define the size of the resulting structure. This will take into account the thickness of the hole so that the total hole size corresponds to what was put in. We will also come back to this.
- The `resolution` is the distance in x between two adjacent points or circles. Pay attention that this is only the distance in x direction so if the function is steep the actual distance between the points will be significantly larger than this. 

<figure><img src="../.gitbook/assets/asymmetric.png" alt=""><figcaption><p>Example with asymmetric range.</p></figcaption></figure>

## Ideas to add features (remove before push)

- different ending types
- different scattering calculation (slopes)
  - handeling corners to be pointy?
- variable thickness