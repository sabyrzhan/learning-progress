## Disable `pager`
	- Execute it terminal:
	  ```
	  git config --global core.pager ""
	  ```
- ## Change username and email in all commits
	- WARNING: This rewrites whole history and changes in all commits
		- Source: https://stackoverflow.com/a/2920001
	- ```
	  git filter-branch -f --env-filter \
	  "GIT_AUTHOR_NAME='Newname'; GIT_AUTHOR_EMAIL='Newemail'; \
	  GIT_COMMITTER_NAME='committed-name'; GIT_COMMITTER_EMAIL='committed-email';" HEAD
	  ```
- ## Local and Global username and email
	- For just one repo, go into to the relevant repo DIR and:
	  ```
	  git config user.name "Your Name Here"
	  git config user.email your@email.example
	  ```
	- For (global) default email (which is configured in your ~/.gitconfig):
	  ```
	  git config --global user.name "Your Name Here"
	  git config --global user.email your@email.example
	  ```
	- You can check your Git settings with: `git config user.name && git config user.email`
	- If you are in a specific repo which you setup a new user/config for (different to global) then it should show that local config, otherwise it will show your global config.
- ## Compare 2 repositories
	- ```
	  git remote add -f b path/to/repo_b.git
	  git remote update
	  git diff master remotes/b/master
	  git remote rm b
	  ```