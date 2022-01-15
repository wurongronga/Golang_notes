# Exercises

### Exercise: Loops and Functions

As a way to play with functions and loops, let's implement a square root function: given a number x, we want to find the number z for which z² is most nearly x.

Computers typically compute the square root of x using a loop. Starting with some guess z, we can adjust z based on how close z² is to x, producing a better guess:

```
z -= (z*z - x) / (2*z)
```

Repeating this adjustment makes the guess better and better until we reach an answer that is as close to the actual square root as can be.

Implement this in the `func Sqrt` provided. A decent starting guess for z is 1, no matter what the input. To begin with, repeat the calculation 10 times and print each z along the way. See how close you get to the answer for various values of x (1, 2, 3, ...) and how quickly the guess improves.

Hint: To declare and initialize a floating point value, give it floating point syntax or use a conversion:

```
z := 1.0
z := float64(1)
```

Next, change the loop condition to stop once the value has stopped changing (or only changes by a very small amount). See if that's more or fewer than 10 iterations. Try other initial guesses for z, like x, or x/2. How close are your function's results to the [math.Sqrt](https://go.dev/pkg/math/#Sqrt) in the standard library?

(**Note:** If you are interested in the details of the algorithm, the z² − x above is how far away z² is from where it needs to be (x), and the division by 2z is the derivative of z², to scale how much we adjust z by how quickly z² is changing. This general approach is called [Newton's method](https://en.wikipedia.org/wiki/Newton's\_method). It works well for many functions but especially well for square root.)

```
package main

import (
	"fmt"
	"math"
)

func Sqrt(x float64) float64 {
	z:=1.0
	for i:=0; i<10; i++ {
		z-=(z*z-x)/(2*z)
		fmt.Println("z", z)
		fmt.Println("x sqrt", math.Sqrt(x))
		if math.Abs(z-math.Sqrt(x))<0.00000000000000000000000001{
			fmt.Println(i)
			break
		}
	}
	return z
}

func main() {
	fmt.Println(Sqrt(2))
}

```

### Exercise: Slices

Implement `Pic`. It should return a slice of length `dy`, each element of which is a slice of `dx` 8-bit unsigned integers. When you run the program, it will display your picture, interpreting the integers as grayscale (well, bluescale) values.

The choice of image is up to you. Interesting functions include `(x+y)/2`, `x*y`, and `x^y`.

(You need to use a loop to allocate each `[]uint8` inside the `[][]uint8`.)

(Use `uint8(intValue)` to convert between types.)

```
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	twoD := make([][]uint8,dy)
	
	for i:=0;i<dy;i++ {
		twoD[i] = make([]uint8,dx) //为什么不用:=?
		for j:=0;j<dx;j++{
			twoD[i][j] = uint8(i^j)
		}
	}
	
	return twoD
}

func main() {
	pic.Show(Pic)
}

```

### Exercise: Fibonacci closure

Let's have some fun with functions.

Implement a `fibonacci` function that returns a function (a closure) that returns successive [fibonacci numbers](https://en.wikipedia.org/wiki/Fibonacci\_number) (0, 1, 1, 2, 3, 5, ...).

```
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
	f0:=0
	f1:=1
	
	return func() int{
		f:=f0
		f0,f1=f1,f+f1
		return f1
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}

```

## slice 1

[https://go101.org/quizzes/slice-1.html](https://go101.org/quizzes/slice-1.html)

What does the following program print?

```go
package main

import "fmt"

func main() {
	a := [...]int{0, 1, 2, 3}
	x := a[:1]
	y := a[2:]
	x = append(x, y...)
	x = append(x, y...)
	fmt.Println(a, x)
}
```

Choices:

* \[0 1 2 3] \[0 2 3 3 3]
* \[0 2 3 3] \[0 2 3 3 3]
* \[0 1 2 3] \[0 2 3 2 3]
* \[0 2 3 3] \[0 2 3 2 3]

Answer: \[0 2 3 3] \[0 2 3 3 3]

当x使用append后如果elements超过capacity，append会locate x到新的backing array，因此此时不会影响到x之前refer的a，a中element不变。

Key points:

* Initially, the slice `y` is `[2 3]`, and the slice `x` is `[0]` with three free element slots (in other words, its capacity is 4),
* The 1st `append` call changes the slice `x` to `[0 2 3]` with one free element slot. And as the slice `x` and the array `a` share elements (before line 10), the array `a` is changed to `[0 2 3 3]`. The slice `y` and the array `a` also share elements, so the slice `y` is changed to `[3 3]`.
* For the slice `x` has not enough free capacity, the 2nd `append` call allocates a new backing array for the slice `x`. So the call deesn't modify the array `a`. The slice `x` is changed to `[0 2 3 3 3]`.
