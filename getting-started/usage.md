---
description: How to run FreePATHS simulations
---

# Usage

FreePATHS is a command line application, so it runs inside Linux, MacOS, or Windows terminal. It takes an input file or config file from the user, which contains all the settings, and outputs the results in a new folder. For an extensive guide on creating config files please see  [config-file-creation-guide.md](../getting-started/config-file-creation-guide.md).

There are two modes of using the program.

* Main mode traces a large number of phonons through a structure and collects statistics about their paths. This mode calculates the thermal flux and Temperature profile of the sample and uses this to calculate effective thermal conductivity.
* The MFP sampling mode measures phonon mean free paths using a small number of phonons and calculates the thermal conductivity by integrating phonon dispersion.

### Demo

First, if you simply run `freepaths` without specifying an input file, the program will run a [demo simulation](../tutorials/nanowire.md) and output some demo results.

### Main mode

In the main mode, the program traces large number of phonons through a structure and calculates various statistical distributions and maps. In this mode, the thermal conductivity will be calculated via Fourier law. See [themal-conductivity-calculation.md](../advanced-tutorials/themal-conductivity-calculation.md) for more information on this.

Run the program as:

```
freepaths your_input_file.py
```

In the `examples` folder, you will find example input files. Try using one of them, for instance, as:

```
freepaths simple_nanowire.py
```

After the simulation, see the results in a newly created `Results` folder.

### MFP sampling mode

Alternatively, you can run FreePATHS in the mean free path sampling mode, which is designed to calculate the thermal conductivity by integrating phonon dispersion, as explained below. To run the program in this mode, reduce the number of phonons to about 30 and add `-s` flag in the command:

```
freepaths -s simple_nanowire.py
```

The thermal conductivity will be output in the terminal. However, other statistical quantities and plots will still be calculated and output in the `Results` folder.

In this mode, the thermal conductivity at a given temperature (_T_) is calculated as:

$$
\kappa = \frac{1}{6\pi^2} \sum_{j} \int \frac{\hslash^2\omega_j^2(q)}{k_bT^2} \frac{\exp\left[\hslash\omega_j(q)/k_bT\right]}{(\exp\left[\hslash\omega_j(q)/k_bT\right]-1)^2} v_j^2(q) \tau_j(q,T) q^2 dq
$$

where _k_ is the Boltzmann constant, ω(q) and _v_(q) are the frequency and group velocity on the branch _j_ of the SiC phonon dispersion at the wavevector _q._ The phonon relaxation time _τ_ (or the phonon mean free path Λ = _v_(_q_)·τ) is measured by running phonons through the structure and recording the average of the distances between diffuse scattering events.

### Troubleshooting

* If simulations are too slow, try using [multiprocessing](../tutorials/basics.md#multiprocessing).
* Rarely, phonons may enter a hole in the structure or break out of structure boundaries. To reduce the impact of this bug, reduce the `TIMESTEP` parameter. However, this usually happens once per thousands of collisions and has negligible impact on the final statistics.
* If you have an error similar to `Cannot mix incompatible Qt library (5.15.7) with this library (5.15.8)` that likely means that you have a program like `qt5-styleplugins` that didn't upgrade to the latest Qt library with rest of the system.
* If at the end of simulation the program report that less than 100% of phonons reached the cold side, you may need to increase `NUMBER_OF_TIMESTEPS` to allow more simulation time.
