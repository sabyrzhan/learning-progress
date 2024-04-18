## #Docker #cheatsheet
	- [[CMD vs ENTRYPOINT]]
		- [[CMD instruction]]: is used as parameters to ENTRYPOINT. Can be overriden when container is run
		- [[ENTRYPOINT instruction]]: is the starting application and cannot be overriden directly when container is run. (But can be overridden with `--entrypoint` parameter)
		- In both cases including `RUN` command can be invoked in 2 forms:
			- [[SHELL form]]: when you specify command in string format it is passed to `sh -c` and spawns child process when run. Thus it CTRL+C like commands will not be trapped by the child process
			- [[EXEC form]]: you specify the command in the form of array. The command will be directly executed without shell command