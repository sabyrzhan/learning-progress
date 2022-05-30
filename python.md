# Python
## Anaconda/Miniconda
Anaconda/Miniconda - python env manager. The difference between the two - Miniconda has small footprint because it does not install many python libraries.

### Installation
Install Miniconda from [here](https://docs.conda.io/en/latest/miniconda.html)

### Basic usages
1. `conda create -n <env>` - create new env
2. `conda activate <env>` - activate env
3. `conda deactivate <env>` - deactivate env
4. `conda env list` - list existing envs
5. `conda install <package>=<version>` - install package

### Python2 installation
Miniconda repository currently contains only Python3 binaries. To install Python2 add [conda-forge](https://conda-forge.org/) to channels.
```
# install python2
conda install python=2.7.15
```
