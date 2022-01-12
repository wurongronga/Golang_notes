# call code in external package

use the pkg.go.dev site to find published modules



import the `rsc.io/quote` package and add a call to its `Go` function.

After adding the highlighted lines, your code should include the following:

```
package main

import "fmt"

import "rsc.io/quote"

func main() {
    fmt.Println(quote.Go())
}
```



Go will add the `quote` module as a requirement, as well as a go.sum file for use in authenticating the module. For more, see [Authenticating modules](https://go.dev/ref/mod#authenticating) in the Go Modules Reference.

use go mod tidy to get new modules:

```
$ go mod tidy
go: finding module for package rsc.io/quote
go: found rsc.io/quote in rsc.io/quote v1.5.2
```
