# Python
## Anaconda/Miniconda
Anaconda/Miniconda - python env manager. The difference between the two - Miniconda has small footprint because it does not install many python libraries.

### Installation
Install Miniconda from [here](https://docs.conda.io/en/latest/miniconda.html)

### Basic usages
1. `conda create -n <env>` - create new env
2. `conda create -n `<env>` python=3.3.0 ipython - create new env and install specified packages
3. `conda activate <env>` - activate env
4. `conda deactivate <env>` - deactivate env
5. `conda env list` - list existing envs
6. `conda install <package>=<version>` - install package

### Python2 installation
Miniconda repository currently contains only Python3 binaries. To install Python2 add [conda-forge](https://conda-forge.org/) to channels.
```
# install python2
conda install python=2.7.15
```
