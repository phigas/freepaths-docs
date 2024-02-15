---
description: General flow of the simulation
---

# Algorithm

The algorithm runs the simulations step-by-step and phonon-by-phonon. For each phonon, the algorithm calculates a trajectory, with a time step _dt_ and for as many time steps as required for the phonon to reach the cold side or until a maximum number of time steps is reached.

<figure><img src="../.gitbook/assets/MCscheme.png" alt=""><figcaption><p>Scheme of the simulated system, indicating some of the parameters set in the input files.</p></figcaption></figure>

### Initialization

At the beginning of time, each phonon is generated at the hot side. At this moment, each phonon is assigned a frequency (wavelength) and the random direction. The frequency is assigned according to the Plank distribution of phonon frequencies at this temperature (_T_). See the picture on the [main page](./) for the example of the Plank distribution function. From the assigned frequency, the algorithm determines the phonon group velocity (_v_) from the phonon dispersion in the given material. If more than one polarization branch is available at this frequency, it is chosen randomly.

The phonon starts moving step-by-step in the assigned direction according to the following equations:

$$
\Delta x = sin(\theta)·abs(cos(\phi))·v·dt
\\
\Delta y = cos(\theta)·abs(cos(\phi))·v·dt
\\
\Delta z = sin(\phi)·v·dt
$$

where θ is the angle between the projection to _x-y_ plane and _y_-axis, and ψ is the angle to the horizontal plane, _v_ is the speed, and _dt_ is the time step.

### Scattering on boundaries

At each time step _dt_, the algorithm checks if phonon crossed any of the boundaries. The boundaries include top and bottom of the simulation, left and right walls, or walls of the holes or pillars. If the boundary is crossed, the corresponding function calculates at which angle _a_ phonon hits the boundary. Then, the algorithm calculates the specular scattering probability, determined by Soffer's equation:

$$
p = exp(-16 \pi ^2 \sigma^2 cos^2(\alpha) / \lambda ^2)
$$

where _p_ is the specularity probability (number between zero and one), σ is the surface roughness, α is the angle to the surface, and λ is the wavelength of the phonon. Then the algorithm draws a random number between zero and one. If the number is greater than p than the scattering is diffuse, otherwise specular. If the scattering is specular, the phonon is reflected elastically, from the surface. If the scattering is diffuse, the phonon is reflected in a random direction, but using the [Lambert cosine distribution](https://en.wikipedia.org/wiki/Lambert's\_cosine\_law) of probability. After the scattering, phonon continues the movement and keeps moving and being scattered until it either reaches the cold side, or returns to the hot side, or time of the simulation for each phonon is over.

### Internal scattering

Beside the surface scattering, phonon can also experience [internal scattering](https://en.wikipedia.org/wiki/Phonon\_scattering). The internal scattering means all scattering processes like impurity scattering, phonon-phonon scattering (including Normal and Umklapp scatterings). In the simulation, this kind of scattering programmed to occur when a certain time (relaxation time) passed since last scattering of any kind. In other words, if phonon runs for too long without any scatterings, it's getting more and more likely to be scattered due to phonon-phonon process. The relaxation time is calculated depending on the phonon frequency, temperature, and polarization.

### Distributions and statistics

Once the simulation is over for a given phonon, the algorithm collects all the phonon properties (frequency, speed, path etc.), it's entrance and exit angles, and other results and stores it in files. Then, the process repeats for the required number of phonons. After simulations for all phonons is done, the algorithm calculates various distributions and statistical facts about the simulation. For example, it calculates phonon spectrum at the beginning, phonon trajectories, phonon exit angle distribution, group velocities etc. The examples of the distribution are shown above in the picture. Then, it outputs all this information into the graphs and into .csv files. Examples below shows scattering maps and statistics obtained using the Monte Carlo code for a [serpentine nanowire](https://pubs.rsc.org/en/content/articlelanding/2019/NR/C9NR03863A).

<figure><img src="../.gitbook/assets/MCscheme.png" alt=""><figcaption><p>Scheme of simulated structure indicating several of the basic parameters.</p></figcaption></figure>
