---
description: Simple nanowire at 300 K
---

# Nanowire

This simulation runs by default if you run the program without any input files, as:

`freepahts`

The simulation models thermal transport in a Si nanowire at 300 K. The algorithm calculates the thermal profiles, heat flux, and the thermal conductivity at different time intervals. Here is an example of phonon paths in such structure showing somewhat diffusive behavior:

<figure><img src="../.gitbook/assets/paths.jpg" alt="" width="563"><figcaption><p>Example phonon paths in the structure.</p></figcaption></figure>

From the profile plots, we can see how the temperature and heat flux profiles converges with time as the system is reaching the state (the different colored lines represent the system state at different times):

<div>

<figure><img src="../.gitbook/assets/thermal profile.jpg" alt=""><figcaption><p>Temperature profiles at different time instervals converge to the linear profile.</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/heat flux (2).jpg" alt=""><figcaption><p>Heat flux profiles converge to the flat line.</p></figcaption></figure>

This can be used to estimate the thermal conductivity of the structure. See the [thermal conductivity calculation](../advanced-tutorials/themal-conductivity-calculation.md) page for more information.
