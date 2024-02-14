---
description: Simple nanowire at 300 K
---

# Nanowire

This simulation runs by default if you run the program without any input files, as:

`freepahts`

The simulation models thermal transport in a Si nanowire at 300 K. The algorithm calculates the thermal profiles, heat flux, and the thermal conductivity at different time intervals. Here is an example of phonon paths in such structure showing somewhat diffusive behavior:

<figure><img src="../.gitbook/assets/paths.jpg" alt="" width="563"><figcaption><p>Example phonon paths in the structure.</p></figcaption></figure>

From the profile plots, we can see how the temperature and heat flux profiles converges with time as the system is reaching the state:

<div>

<figure><img src="../.gitbook/assets/thermal profile.jpg" alt=""><figcaption><p>Temperature profiles at different time instervals converge to the linear profile.</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/heat flux (2).jpg" alt=""><figcaption><p>Heat flux profiles converge to the flat line.</p></figcaption></figure>

</div>

Using the Fourier law:

$$
J =  \kappa \nabla T,
$$

we can calculate the thermal conductivity from the obtained heat flux and temperature profiles:

<figure><img src="../.gitbook/assets/thermal conductivity profile.jpg" alt="" width="375"><figcaption><p>Thermal conductivity converges as the system reaches steady state.</p></figcaption></figure>

We can observe how the thermal conductivity converges to about 55 W/mK as the system is reaching the steady state.

{% hint style="danger" %}
Note that the thermal conductivity calculated with this method, at the moment, is only reliable for simple structures like nanowires or membranes without holes. Any holes or pillars will introduce irregularities in the thermal profiles and render the thermal conductivity invalid.
{% endhint %}

### References

* Anufriev et al. [Nanoscale, 11, 13407-13414 (2019)](https://pubs.rsc.org/en/content/articlehtml/2019/nr/c9nr03863a)
* Anufriev et al. [ACS Nano 12, 11928 (2018)](https://pubs.acs.org/doi/abs/10.1021/acsnano.8b07597)
