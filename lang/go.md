# GoLang

## General

* Compiler: go >= `1.11` (`xtruder/pkgs/go/1.11` in nixpkgs).
* Coding style: `gofmt`
* Filestyle: `gofmt`
* Project layout: `https://github.com/golang-standards/project-layout/`

At least version `1.11` in `xtruder/pkgs/go/1.11` nixpkgs repo.

## Tooling

### Environment variables

The following environment variables are advised to be set on project. Something like
https://direnv.net/ can be used to automatically load env variables:

```
# enable go modules
export GO111MODULE=on

# set bin path to current folder
export GOBIN=$PWD/bin

# include GOBIN to path
export PATH=$GOBIN:$PATH
```

### Dev tools and generators

To manage tools in go `tools.go` should be created in root of the project with required tools
specified as imports. GO will automatically download these binaries and put them in `GOBIN` folder.

```
// +build tools

// Add imports for tools here: https://github.com/golang/go/wiki/Modules#how-can-i-track-tool-dependencies-for-a-module

package main

import (
	_ "github.com/b3log/header"
	_ "github.com/mailru/easyjson"
)
```

If you have set `GOBIN` to current folder and set `PATH` accordingly, you can use tools directly.

### Version managment

Go mod is a new dependency managment tool for go, it is included in go version
`1.11`. For more information on how to use it take a look [here](https://github.com/golang/go/wiki/Modules).

### Licensing

Go has a license header in every file. It's advised to use `github.com/b3log/header` tool for 
auto header injection

### Editors

#### VSCode

**go modules support**

VSCode has a sufficient support for gomodules, but requires the latest version of golang
extension, also some configration options have to be set:

```
go.docsTool: "godoc"
go.formatTool: "goimports"
```

**Specify build tags**

```
go.buildTags: "sqlite json1"
```

## Programmer notes

### Documentation

Use `doc.go` in every folder.
