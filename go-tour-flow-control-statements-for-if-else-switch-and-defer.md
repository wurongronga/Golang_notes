# Go tour - Flow control statements: for,if,else,switch and defer

### For

_Go has only one looping construct_, the _`for` loop_. There are _no parentheses surrounding the three components_ of the `for` statement and the braces `{ }` are always required.

```
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

The init and post statements are optional.

```
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}

```

`for` without a condition will loop repeatedly until you `break` out of the loop or `return` from the enclosing function.

```
package main

import "fmt"

func main() {

    for {
        fmt.Println("loop")
        break
    }

    for n := 0; n <= 5; n++ {
        if n%2 == 0 {
            continue
        }
        fmt.Println(n)
    }
}
```

### For is Go's "while"

At that point you can drop the semicolons: C's `while` is spelled `for` in Go.

```
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

```

### Forever

If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.

```
package main

func main() {
	for {
	}
}
```

### If

Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses `( )` but the braces `{ }` are required.

### If with a short statement

Like `for`, the `if` statement can start with a short statement to execute before the condition.

Variables declared by the statement are only in scope until the end of the `if`. 有点像for和if和在一起了。

(Try using `v` in the last `return` statement.)

```
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

### Switch

a shorter way to write a sequence of `if - else` statements

```
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}

```

Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

### Switch with no condition

```
func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
```

### Defer

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

_Defer_ is used to ensure that a function call is performed later in a program’s execution, usually for purposes of cleanup. `defer` is often used where e.g. `ensure` and `finally` would be used in other languages.

Deferred function calls are pushed onto a stack.

When a function returns, its deferred calls are executed in last-in-first-out order.

```
import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}

```

