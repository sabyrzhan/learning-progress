## Anaconda/Miniconda
	- Anaconda/Miniconda - python env manager. The difference between the two - Miniconda has small footprint because it does not install many python libraries.
	- ### Installation
	  Install Miniconda from [here](https://docs.conda.io/en/latest/miniconda.html)
	- ### Basic usages
	  1. `conda create -n <env>` - create new env
	  2. `conda create -n `<env>` python=3.3.0 ipython - create new env and install specified packages
	  3. `conda activate <env>` - activate env
	  4. `conda deactivate <env>` - deactivate env
	  5. `conda env list` - list existing envs
	  6. `conda install <package>=<version>` - install package
	- ### Python2 installation
	  Miniconda repository currently contains only Python3 binaries. To install Python2 add [conda-forge](https://conda-forge.org/) to channels.
	  ```
	  # Add channel
	  conda config --add channels conda-forge
	  conda config --set channel_priority strict 
	  
	  # install python2
	  conda install python=2.7
	  ```
- ## [[Evaluate anchors, aliases and merges]]
	- More: https://python.land/data-processing/python-yaml
	- SO: https://stackoverflow.com/questions/1773805/how-can-i-parse-a-yaml-file-in-python
	- ```
	  # test.yml
	  app:
	    common: &common
	      key1: 1
	      key2: 2
	    prodAndStage:
	      env: &env prod-stage
	      db_url: prod_url
	    valuesProd:
	      <<: *common
	      newEnv: *env
	      key1: 3
	  
	  
	  # python.py
	  import yaml
	  
	  names = ''
	  with open('test.yml') as file:
	      names = yaml.safe_load(file)
	  
	  with open('out.yaml', 'w') as file:
	      yaml.dump(names, file)
	  
	  print(open('out.yaml').read())
	  
	  # CLI: python python.py will output
	  app:
	    common:
	      key1: 1
	      key2: 2
	    prodAndStage:
	      db_url: prod_url
	      env: prod-stage
	    valuesProd:
	      key1: 3
	      key2: 2
	      newEnv: prod-stage
	  
	  ```
- ## [[YAML validator]]
	- Following tools can be used for validating yaml file:
		- https://github.com/mikefarah/yq
			- Docs: https://mikefarah.gitbook.io/yq
		- https://github.com/adrienverge/yamllint
			- https://yamllint.readthedocs.io/en/stable/rules.html - rules and error descriptions
	- More about yaml aliases, anchors and merges:
		- https://ktomk.github.io/writing/yaml-anchor-alias-and-merge-key.html