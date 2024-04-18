# Install app/package with #Golang globally
	- Source: https://stackoverflow.com/questions/36650052/golang-equivalent-of-npm-install-g/68559728#68559728
		- Starting with Go >= **1.16** the recommended way to install an executable is to use
		  ```
		  go install package@version
		  ```
		- For example, `go install github.com/fatih/gomodifytags@latest`.
		- Executables (main packages) are installed to the directory named by the `GOBIN` environment variable, which defaults to `$GOPATH/bin` or `$HOME/go/bin` if the `GOPATH` environment variable is not set. You need to add this directory to your `PATH` variable to run executables globally. In my case, I've added this line to my `~/.zshrc` file. (if you are using bash, add it to the `~/.bash_profile` file):
		- ```
		  export PATH="$HOME/go/bin:$PATH"
		  ```
		- Go team published a [blog post](https://blog.golang.org/go116-module-changes#TOC_4.) about this change, here's the explanation quote:
		- > We used to recommend go get -u program to install an executable, but this use caused too much confusion with the meaning of go get for adding or changing module version requirements in go.mod.
		- Refer to [`go install` documentation](https://golang.org/ref/mod#go-install) for more details
		- `go get` will not install packages "globally". It will add them to your project's go.mod file instead.