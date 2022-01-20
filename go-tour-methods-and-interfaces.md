# Go tour -methods and interfaces

### Methods

Go does not have classes. However, you can define methods on types.

**A method is a function with a special **_**receiver**_** argument.**

The receiver appears in its own argument list between the `func` keyword and the method name.

You can only declare a method with a receiver whose type is defined in the same package as the method. _You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as `int`)._

#### struct type

```
package main

import (
	"fmt"
	"math"
)

// init a type, named "Vertex" using struct
type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}

```

#### Non-struct type

```
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}

```
