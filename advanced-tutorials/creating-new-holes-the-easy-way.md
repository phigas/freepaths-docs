# Creating new shapes with PointLineHole

This tutorial explains how you can crate new hole shapes using the very flexible `PointLineHole` and the even easier to use `FunctionLineHole`. There is an [alternative way](adding-your-own-hole-or-pillar-the-advanced-way.md) of creating new hole shapes through programming but this is a lot more difficult.

In simple works you can use the `PointLineHole` to draw any line you want which will then be a hole. It is quite easy to define these lines using mathematical functions or python functions. How to do this is explained below with more detail.

## How does the PointLineHole work behind the scenes

The `PointLineHole` creates many circular holes that are at closely spaced points along a line. If the circular holes are spaces closely enough this will give the impression of a continuous line with a certain thickness. 

## How to use a mathematical function to define a shape

## Ideas to add features (remove before push)

- different ending types
- different scattering calculation (slopes)
  - handeling corners to be pointy?
- variable thickness