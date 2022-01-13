# Go tour - packages,variables, and functions

### Exported names

| exported names                                                        | unexported names                         |   |
| --------------------------------------------------------------------- | ---------------------------------------- | - |
| begins with a capital letter.                                         | begins with a lowercase letter           |   |
| `math.Pi`                                                             |                                          |   |
| _When importing a package, you can refer only to its exported names_. | not accessible from outside the package. |   |

_type comes after the variable name._

### Multiple results

A function can return any number of results.

The `swap` function returns two strings.

### Named return values

Go's return values may be named. If so, they are treated as variables defined at the top of the function.

These names should be used to document the meaning of the return values.

A `return` statement without arguments returns the named return values. This is known as a "naked" return.

Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.

```
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

```

{% hint style="info" %}
A `return` statement without arguments returns the named return values. This is known as a "naked" return.

Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.
{% endhint %}

### Packages

a package is a way to group functions, and it's made up of all the files in the same directory

### Variables

The `var` statement declares a list of variables; as in function argument lists, the type is last.

### <mark style="background-color:green;"><mark style="color:green;">Short variable declarations<mark style="color:green;"></mark>

Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with _implicit_ type.

Outside a function, every statement begins with a keyword (`var`, `func`, and so on) and so the `:=` construct is not available.



### Basic types

Go's basic types are

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```



### Zero values

Variables declared without an explicit initial value are given their _zero value_.

The zero value is:

* `0` for numeric types,
* `false` for the boolean type, and
* `""` (the empty string) for strings.

### Type conversions

The expression `T(v)` converts the value `v` to the type `T`.

in Go assignment between items of different type requires an explicit conversion. Try removing the `float64` or `uint` conversions in the example and see what happens.



### Type inference

When declaring a variable without specifying an explicit type (either by using the `:=` syntax or `var =` expression syntax), the variable's type is inferred from the value on the right hand side.

When the right hand side of the declaration is typed, the new variable is of that same type:

```
var i int
j := i // j is an int
```

But when the right hand side contains an untyped numeric constant, the new variable may be an `int`, `float64`, or `complex128` depending on the precision of the constant:

```
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```



### Constants

Constants are declared like variables, but with the `const` keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the `:=` syntax.

### Numeric Constants

Numeric constants are high-precision _values_.

An untyped constant takes the type needed by its context.

