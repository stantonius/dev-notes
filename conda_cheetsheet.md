# Conda (Miniconda) Cheetsheet

As a new user to conda/miniconda, I am tracking all of the commands needed to leverage this toolset

## Why do we care 

Conda is, in my opinion, a simpler way to do virtual environment management, which is key for Python development (specifically for avoiding package dependency issues). It also allows you to leverage the conda install feature (instead of pip), and utilise other channels which are quite popular like conda-forge.

## How to open

* Run `wt` to open the new [Windows Terminal](https://github.com/microsoft/terminal) and select the minconda profile from the dropdown list
    * Awesome post about using this setup [here](https://dev.to/voodu/windows-terminal-conda-d3e)
    * Also note that if you want to change the startiong directory, you need to change the `startingDirectory` value in the settings json file
* Search "miniconda" in windows search

## Conda Base

Starting a conda terminal, if you see `(base)` you know you are in the **main/default conda environment**

## Common Commands

* `conda list` - shows all packages installed in that environment
* `conda env list` - get all environments

## Create new environment

* `conda create --name newEnv` - create ONLY, not yet active
    * Create with specific python version: `conda create --name newEnv python=3.8`
* `activate newEnv` - actually activates it for use

You can specify the Python version with the following command:
 * `conda create -n newEnv python=3.6`

## Installing Packages
### Globally

* Ensure you are in the default/base environment and there is **no active environment**
* Simply run `conda install packageName`

### Individual Environment

* Ensure you are in the active environment and run `conda install packageName`
* Alternatively specify the environment name by running `conda install --name newEnv packageName`

**NOTE**: if installing packages directly in Jupyter notebooks using `!conda install packageName` it *will not work unless `always_yes` is set to true*. There are two ways to do this:
1. Add `--yes` flag in the command line entry (ie. `!conda install packageName --yes`)
2. Set it globally via setting `always_yes` feature to true (ie. `conda config --set always_yes True`)
    * More info about Conda settings can be found [here](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#:~:text=%20Using%20the%20.condarc%20conda%20configuration%20file%20%C2%B6,set%20this%20with%20the%20CONDA_BLD_PATH%20environment...%20More%20)
    * A quick way to see the conda config options is to run the command `conda config --describe`

### Channels

You can install packages from different conda channels. For example, fastai is not available in the regular conda channel, but rather the fastai one. You specify this with the tag followed by the specified channel: `-c CHANNEL`

## Link to Jupyter Kernel

Use the package [nb_conda_kernels](https://github.com/Anaconda-Platform/nb_conda_kernels)

* Run `conda install -c conda-forge nb_conda_kernels`
* Then start Jupter Notebook `jupyter notebook` and you will see your active environment in the dropdown list of kernels
    * Use `jupyter lab` instead

## JupyterLab

The next phase of Jupyter notebooks. Need to install with the following code `conda install -c conda-forge jupyterlab`

### Using Kite Autocompletion (DON'T USE - TOO SLOW)

* Need to have the desktop application installed
* Need to have the JupyterLab extension installed (may need to enable extensions)
* Use `pip` to isntall `pip install jupyter-kite`

### Using [jupyterlab-lsp](https://github.com/krassowski/jupyterlab-lsp) for Autocompletion

* As long as we have Python and JupyterLab installed, run the following: `conda install -c conda-forge jupyter-lsp-python`
    * This installs both the lsp and the language server - both are needed and can be installed independently
* Then you will need to find the  extension in the jupyter lab extension list and install
    * May need to rebuild the notebook (can also be done manually in conda cli through `jupyter lab build`)

## Removing environment

* `conda env remove -n newEnv`

## Saving Conda Requirements

`conda list -e > requirements.txt`

To create an environment using a requirements text file:

`conda create --name <env> --file requirements.txt`

## The Actual Cheetsheet

[Here](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)
