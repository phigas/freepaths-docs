---
description: Collimation and focusing using parabolic lenses
---

# Parabolic lens

In the `examples/parabolic_lens_collimation.py` example, we create a collimated phonon flux by reflecting phonons from a parabolic surface. The figure below illustrates the concept:

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="563"><figcaption><p>Concept of collimation and focusing using parabolic surface.</p></figcaption></figure>

We place the parabolic boundary at the bottom and move the hot spot that emits the phonon to _y_ = 300 nm, as shown in the input file:

```
# Hot and cold sides:
COLD_SIDE_POSITION_TOP = True

# Phonon source:
PHONON_SOURCES = [Source(y=300e-9, size_x=100e-9,  size_y=100e-9, size_z=THICKNESS, angle_distribution="uniform")]

# Parabolic mirror:
HOLES = [ParabolaBottom(tip=0, focus=300e-9)]
```

Note that `angle_distribution="uniform"` because we need to emit phonons in all directions evenly. This model produces the following structure, which collimates phonons after reflection from the parabolic boundary:

<div>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Phonon trajectories show collimation of phonons reflected from the parabolic boundary.</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>Angular distribution show that many phonons have an angle of zero degrees at the top of the structure (blue), while initial distribution was uniform (red).</p></figcaption></figure>

</div>

The parabolic lens can also be inverted and placed at the top as follows

```
# Phonon source:
PHONON_SOURCES = [Source(x=0, y=0, z=0, size_x=WIDTH,  size_y=0, size_z=THICKNESS, angle_distribution="directional")]

# Parabolic boundary:
HOLES = [ParabolaTop(tip=1000e-9, focus=100e-9)]
```

Here, the source is place in the bottom, while parabola at the top.

### Reference

* Singh et al. [Applied Physics Letters, (2023)](https://aip.scitation.org/doi/10.1063/5.0137221)
