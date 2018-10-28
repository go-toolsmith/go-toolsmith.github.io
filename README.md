# go-toolsmith.github.io

Site for Go tools crafters: combined documentation for [go-toolsmith](https://github.com/go-toolsmith)
packages, external references and other useful resources.

There are two major project goals:
1. Provide reusable libraries that make Go tools implementation easier.
2. Knowledge sharing. Make tools development less frustrating by providing docs for parts that are not easy to grasp and/or hard to find on the internet for whatever reason.

## Packages

List of packages maintained under `go-toolsmith` project.

### [astinfo](https://github.com/go-toolsmith/astinfo) [![GoDoc](https://godoc.org/github.com/go-toolsmith.astinfo?status.svg)](https://godoc.org/github.com/go-toolsmith.astinfo)

Package astinfo records useful AST information like node parents and such.

### [astcopy](https://github.com/go-toolsmith/astcopy) [![GoDoc](https://godoc.org/github.com/go-toolsmith/astcopy?status.svg)](https://godoc.org/github.com/go-toolsmith/astcopy)

Package astcopy implements Go AST deep copy operations. 

### [astequal](https://github.com/go-toolsmith/astequal) [![GoDoc](https://godoc.org/github.com/go-toolsmith/astequal?status.svg)](https://godoc.org/github.com/go-toolsmith/astequal)

Package astequal provides AST (deep) equallity check operations. 

### [astfmt](https://github.com/go-toolsmith/astfmt) [![GoDoc](https://godoc.org/github.com/go-toolsmith/astfmt?status.svg)](https://godoc.org/github.com/go-toolsmith/astfmt)

Package astfmt implements `ast.Node` formatting with fmt-like API. 

### [astp](https://github.com/go-toolsmith/astp) [![GoDoc](https://godoc.org/github.com/go-toolsmith/astp?status.svg)](https://godoc.org/github.com/go-toolsmith/astp)

Package astp provides AST predicates. 

### [astcast](https://github.com/go-toolsmith/astcast) [![GoDoc](https://godoc.org/github.com/go-toolsmith/astcast?status.svg)](https://godoc.org/github.com/go-toolsmith/astcast)

 Package astcast wraps type assertion operations in such way that you don't have to worry about nil pointer results anymore. 

### [strparse](https://github.com/go-toolsmith/strparse) [![GoDoc](https://godoc.org/github.com/go-toolsmith/strparse?status.svg)](https://godoc.org/github.com/go-toolsmith/strparse)

Package strparse provides convenience wrappers around `go/parser` for simple expr/stmt/decl parsing from string. 

## Users

Projects that use `go-toolsmith` and are willing to share that fact.

* [go-critic](https://github.com/go-critic/go-critic) linter
* [go-consistent](https://github.com/Quasilyte/go-consistent) linter
* [go-lintpack](https://github.com/go-lintpack/lintpack) linter builder
