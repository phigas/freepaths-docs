---
description: Math behind the simulations
---

# Equations

### Motion equations

The vector of phonon motion is given by:

$$
x = sin(\theta)·abs(cos(\phi)) \\y = cos(\theta)·abs(cos(\phi))\\\\z = sin(\phi)
$$

where θ is the angle between the projection to _x-y_ plane and _y_-axis, and φ is the angle to the horizontal plane. Thus, phonon moves step-by-step with the speed _v_ in the assigned direction according to the following equations:

$$
\Delta x = sin(\theta)·abs(cos(\phi))·v·dt
\\
\Delta y = cos(\theta)·abs(cos(\phi))·v·dt
\\
\Delta z = sin(\phi)·v·dt
$$

where _dt_ is the duration of one time step.

### Angles to the walls

At each collision with walls, we calculate the angle between the phonon motion vector and the normal vectors to the walls of different kinds:

**Horizontal wall**

$$
\alpha = arccos(cos(\phi)·cos(\theta))
$$

**Vertical wall**

$$
\alpha = arccos(cos(\phi)·sin(abs(\theta)))
$$

**Inclined wall of the triangle**

For triangle facing up

$$
\alpha = arccos(cos(\phi)·cos(\theta+sign(x-x_0)·(\pi/2-\beta)))
$$

For triangle facing down

$$
\alpha = arccos(cos(\phi)·cos(\theta-sign(x-x_0)·(\pi/2-\beta)))
$$

where β is the half-angle of the tip of the triangle, and _sign_(_x-x_0) shows from which side phonon strike the triangle.

**Wall of the circular hole**

$$
\alpha = arccos(cos(\phi)·cos(\theta+sign(y-y_0)·\theta_{tangent}))
$$

where

$$
\theta_{tangent} = arctan((x-x_0)/(y-y_0))
$$

is the normal to the surface of the circle, with _x0_ and _y0_ as circle center coordinates.

### Specular scattering probability

Then, specular scattering probability, determined by Soffer's equation:

$$
p = exp(-16 \pi ^2 \sigma^2 cos^2(\alpha) / \lambda ^2)
$$

where _p_ is the specularity probability (number between zero and one), σ is the surface roughness, α is the angle to the surface, and λ is the phonon wavelength.

