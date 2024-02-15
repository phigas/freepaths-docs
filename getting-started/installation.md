# Installation

FreePATHS requires python 3. On Linux and MacOS, it is probably already installed. On Windows, you may choose to install [Anaconda](https://www.anaconda.com) package, which will install everything for you.

Install the package from PyPi repository by entering this command into a terminal or a python console:

```
pipx install --upgrade freepaths
```

### Development

If you'd like to work on the code, and run it without installing into your python folders, download the repository:

```
git clone https://github.com/anufrievroman/freepaths
```

Change your terminal directory to the directory of the repository (it' where the README file is):

```
cd freepaths
```

Run _freepaths_ as:

```
python -m freepaths
```

This will run the code located inside the `freepaths` folder (which is inside the downloaded `freepaths` folder). In this case, the folder `freepahts` is considered a python module.\
\
In case of troubles, make sure to uninstall all other copies of `freepaths` from your system, for example those installed with `pipx`:

```
pipx uninstall freepaths
```

### Troubleshooting

* If during installation terminal says something like `pipx is not found` that means you need to [install pip in your system](https://pipx.pypa.io/latest/installation/).
* If after installation the program does not run and the error says something like `freepaths is not a recognised command` or `freepaths is not found`, but the `freepaths` package was actually installed, try running everything with `python -m` prefix, for example: `python -m freepaths`. This error occurs when system can't find a path to installed packages, so you may need to restart the terminal or change `PATH` variable.
* If installation fails, you can simply download this repository, start the terminal in its main directory (where license file is) and run the program as `python -m freepaths`. In this case, you'll always need to run the program from this folder and place your input files there.
