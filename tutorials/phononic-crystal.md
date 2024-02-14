---
description: Square array of hole in a thin plate at 4 K
---

# Phononic crystal

This example shows how to add hole to your structure and can be reproduced with `examples/phononic_crystal.py` input file. The structure contains the square array of holes defined in the input file as:

```
# Phonon source:
PHONON_SOURCES = [Source(size_x=WIDTH,  size_y=0, size_z=THICKNESS, angle_distribution="random_up")]

# Lattice of holes:
HOLES = []
period = 300e-9
for row in range(6):
    for column in range(5):
        x = - 4 * period / 2 + column * period
        y = (row + 1) * period
        HOLES.append(CircularHole(x=x, y=y, diameter=200e-9))
```

Here, the `HOLES` list is filled with the `CircularHole` objects. This will create a structure that looks like so:

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="207"><figcaption><p>Phonon trajectories in a phononic crystal</p></figcaption></figure>

If we increase the number of phonon to several thousand, we can see some interesting distributions, for example the distributions of phonon angle at the beginning (red) and end (blue) of the structure. Interestingly, the array of holes "aligned" phonon trajectories along the passages between the holes. See the references below for more details on this effect.

<figure><img src="../.gitbook/assets/image (4).png" alt="" width="375"><figcaption><p>Angular distribution show that while initially angles of phonons are random (red), many phonons have about zero angle degrees at the top of the structure (blue).</p></figcaption></figure>

One can also output, for example, phonon frequency spectrum or time phonons took to reach the cold side.

<div>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>Phonon frequency spectrum.</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>Time that phonons took to reach the cold side.</p></figcaption></figure>

</div>

The file `information.txt` also contains various statistical insides, for example, the scattering likelihood:

```
99% of phonons reached the cold side

11.09% - scattering on side walls (14.98% - diffuse, 85.02% - specular)
48.50% - scattering on top and bottom walls (0.94% - diffuse, 99.06% - specular)
3.68% - rethermalization at the hot side
0.00% - internal scattering processes
45.52% - scattering on hole walls (19.61% - diffuse, 80.39% - specular)
```

### References

* Singh et al. [Applied Physics Letters, (2023)](https://aip.scitation.org/doi/10.1063/5.0137221)
* Anufriev et al. [Materials Today Physics 15, 100272 (2021)](https://www.sciencedirect.com/science/article/pii/S2542529320300961)

