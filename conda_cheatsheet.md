# Inika’s conda cheatsheet

Created: April 2, 2025 9:55 AM
Class: reproducibility

**Aim:** set up environment management with conda, and run R code with packages from a conda environment. 

**Author:** Inika Prasad

**Date:** 27 March, 2025

# Install conda

### Notes

- Tutorial: [https://nbisweden.github.io/workshop-reproducible-research/home_precourse.html#installing-conda](https://nbisweden.github.io/workshop-reproducible-research/home_precourse.html#installing-conda)
- conda should be installed in the base environment and no other packages should be installed into base
- I navigated to /mnt in WSL2 filesystem before running the following.

### Install Miniforge for 64-bit Linux

```bash
curl -L [https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh) -O
bash Miniforge3-Linux-x86_64.sh
rm Miniforge3-Linux-x86_64.sh
```

### Install Miniforge for 64-bit Mac (Apple chip)

```bash
curl -L  [https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh) -O
bash [Miniforge3-MacOSX-arm64.sh](http://miniforge3-macosx-arm64.sh/)
rm [Miniforge3-MacOSX-arm64.sh](http://miniforge3-macosx-arm64.sh/)
```

Remember to restart the terminal for the changes to take effect.

Installed miniforge into root/miniforge

```bash
conda --version
# conda 24.11.3
```

To avoid mixing of the conda-forge and defaults channels:

```bash
conda config --add channels bioconda
conda config --add channels conda-forge
# Warning: 'conda-forge' already in 'channels' list, moving to the top
conda config --set channel_priority strict
```

# Create conda environment

Create an environment with fastqc

```bash
conda create -n myenvname fastqc
```

Create an environment in a particular directory

```bash
conda create -p myprojectdirectory/envs/myenvname fastqc
```

Create an environment from a yaml file. For example of YAML file, see below.

```bash
conda env create -n myenvname -f environment.yml
```

Activate an environment

```bash
conda activate myenvname
```

To deactivate an environment

```bash
conda deactivate
```

To remove an environment entirely

```bash
conda remove --name myenv --all
```

To see all your environments

```bash
conda info --envs
```

**Notes when creating a conda environment at a specific location**

- The environment you are in is in the command line prompt
- If your environment was created with a path, the whole path will be in the prompt (and take up space)
- Current setting:

```bash
conda config --show env_prompt
```

- If you keep Conda environment folder within a parent folder called envs/, and only the final folder name will be used in the command line prompt

# Packages in conda environment

To see installed packages and their versions in the active environment

```bash
conda list
```

To save packages requested to be installed (not including dependencies)

```bash
conda env export --from-history > environment.yml
```

Remove OS-specific build specifications from the resulting yaml file

```bash
conda env export --from-history --no-builds > environment.yml
```

To install a package (e.g. multiqc or python) in your environment

```bash
conda activate myenvname
conda install multiqc
```

To install a package without it automatically updating previously installed packages

```bash
conda install multiqc --freeze-installed
```

To find available versions of a package (e.g. multiqc)

```bash
conda search multiqc
```

To remove a package

```bash
conda remove multiqc
```

Remove unnecessary files to free up disk space (tar-balls from package installations, unused packages, cached data)

```bash
conda clean -a
```

# YAML file example

```
channels:
  - conda-forge
  - bioconda
dependencies:
  - fastqc=0.12.1
```

# Using R with conda environments

- Download VSCode (onto the windows computer)
- Extensions requires: WSL, R, Python
- Create your conda environment from the terminal, activate it like so:

```bash
conda create -n rtester r-base=4.1.3 r-tidyverse
conda activate rtester
```

- Launch VSCode with

```bash
code .
```

# Notes

- Nothing except conda and mamba should live in the base environment
- Creating and deleting environments in the non-default location takes time
- R packages installed in the conda environment still need to be loaded with library()

# M-chip Mac

Some packages are built for the OSX-64 architecture, not the arm-64 architecture present on M-chip macs. Examples include python builds version 3.7 and older, which came out before 2018. To use these packages try Rosetta/Rosetta2 to emulate the OSX-64 architecture. 

```bash
# if you don't have Rosetta2 installed, install it manually:
softwareupdate --install-rosetta
# skipping this step may lead to zsh: bad CPU type in executable

# Create a conda environment with AMD64 architecture
conda create --platform osx-64 --name python-x64 python=3.7.6

# add other packages (for medbioinfo ML course)
conda install jupyter matplotlib pandas keras=2.4.3 tensorflow=2.4.0 numpy=1.19.5 pydot graphviz scikit-learn mpld3 wget nb_conda nb_conda_kernels rise

# test python in command line
python
```

# Other resources

[https://edcarp.github.io/introduction-to-conda-for-data-scientists/02-working-with-environments/index.html#:~:text=Environments created with conda %2C by,environments by running the command](https://edcarp.github.io/introduction-to-conda-for-data-scientists/02-working-with-environments/index.html#:~:text=Environments%20created%20with%20conda%20%2C%20by,environments%20by%20running%20the%20command).