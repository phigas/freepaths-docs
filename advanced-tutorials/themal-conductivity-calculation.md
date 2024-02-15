# Thermal conductivity calculation

Freepaths will calculate the thermal conductivity of the structure after the simulation. This page will give some additional information on this feature.

## How the calculation is done

The basis of the calculation is the Fourier law:

$$
J =  \kappa \nabla T,
$$

The thermal flux and temperature gradient are obtained through the temperature and heat flux profiles:

<figure><img src="../.gitbook/assets/thermal profile.jpg" alt=""><figcaption><p>Temperature profiles at different time instervals converge to the linear profile.</p></figcaption></figure>
<figure><img src="../.gitbook/assets/heat flux (2).jpg" alt=""><figcaption><p>Heat flux profiles converge to the flat line.</p></figcaption></figure>

The temperature gradient is obtained by a linear regression and the heat flux value through the mean over the y profile. As the simulation progresses you can see the value for the thermal conductivity converge:

<figure><img src="../.gitbook/assets/thermal conductivity profile.jpg" alt="" width="375"><figcaption><p>Thermal conductivity converges as the system reaches steady state.</p></figcaption></figure>

There are two heat flux profiles calculated in the simulation. The one called `Heat flux profile effective.pdf` is calculated by averaging the heat flux over the entire simulation domain and the `Heat flux profile material.pdf` only averages the heat flux where there is material so not where there are holes. This means that both the effective and material thermal conductivity are provided in `Thermal conductivity.pdf` (although if you are interested in the material thermal conductivity the MFP sampling mode could be more interesting).

## How to use it

The software can currently only calculate the thermal conductivity in y direction. Also make sure that the phonons are generated on the very side of the simulation.

The software does not give correct results if pillars are present in the simulation.

Because the calculation relies on the pixel grid to calculate the profiles make sure that it is small enough and looks good (some discontinuities in the pixels adjacent to holes are expected and correct).

Make sure that you simulate enough phonons (at least a couple thousand) to get good results.

Make sure that NUMBER_OF_VIRTUAL_TIMESTEPS and INITIALIZATION_TIMESTEPS are set correctly.

## References

* Anufriev et al. [Nanoscale, 11, 13407-13414 (2019)](https://pubs.rsc.org/en/content/articlehtml/2019/nr/c9nr03863a)
* Anufriev et al. [ACS Nano 12, 11928 (2018)](https://pubs.acs.org/doi/abs/10.1021/acsnano.8b07597)