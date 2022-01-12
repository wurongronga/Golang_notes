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
