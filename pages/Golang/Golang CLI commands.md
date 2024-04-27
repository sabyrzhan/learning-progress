## [[Recursively scan all subdirectories]]

	- `./...` syntax can be used by most of the Golang tools (`go build ./...`, `go test ./...`, `staticcheck ./...`, `gosec ./...` etc) to tell to scan all subpackages recursively starting from the current directory
- ## [[Install app or package globally]]
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
- ## [[Manage multiple versions]]
	- Source: https://go.dev/doc/manage-install
	- Steps with version 1.10.7 as an example:
		- ```
		  go install golang.org/dl/go1.10.7@latest
		  ```
		- ```
		  go1.10.7 download
		  ```
		- ```
		  go1.10.7 version
		  ```
		- When you have multiple versions installed, you can discover where each is installed, look at the version's `GOROOT` value. For example, run a command such as the following:
		  ```
		  go1.10.7 env GOROOT
		  ```
		- To uninstall a downloaded version, just remove the directory specified by its `GOROOT` environment variable and the goX.Y.Z binary.
	- If you are using Brew, you can also manage with it
		- Source: https://gist.github.com/BigOokie/d5817e88f01e0d452ed585a1590f5aeb
		- Steps:
			- install the desired version:
			  ```
			  brew install go@1.10
			  ```
			- Unlink from already installed version:
			  ```
			  brew unlink go
			  ```
			- ```
			  brew link go@1.10
			  ```
			- In some cases you may need to link them with the --force and --overwrite options:
			  ```
			  brew link --force --overwrite go@1.10
			  ```
-