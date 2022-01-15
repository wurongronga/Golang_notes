# Go tour - structs, slices, and maps

### Pointers

Go has pointers. A pointer holds the memory address of a value.

The type `*T` is a pointer to a `T` value. Its zero value is `nil`.

```
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(p) // memory location of i: 0xc0000b8000 
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}

```

### Structs

A `struct` is a collection of fields. They’re useful for grouping data together to form records.

### Pointers to structs

```
package main

import "fmt"

type person struct {
    name string
    age  int
}

func newPerson(name string) *person {

    p := person{name: name}
    p.age = 42
    return &p
}

func main() {

    fmt.Println(person{"Bob", 20})

    fmt.Println(person{name: "Alice", age: 30})

    fmt.Println(person{name: "Fred"})

    fmt.Println(&person{name: "Ann", age: 40})

    fmt.Println(newPerson("Jon"))

    s := person{name: "Sean", age: 50}
    fmt.Println(s.name)

    sp := &s
    fmt.Println(sp.age)

    sp.age = 51
    fmt.Println(sp.age)
}
```

### Pointers to structs

Struct fields can be accessed through a struct pointer.

To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X`. However, that notation is cumbersome, so the language permits us instead to write just `p.X`, without the explicit dereference.



### Struct Literals

A struct literal denotes a newly allocated struct value by listing the values of its fields.

You can list just a subset of fields by using the `Name:` syntax. (And the order of named fields is irrelevant.)

The special prefix `&` returns a pointer to the struct value.

```
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3) // {1 2} &{1 2} {1 0} {0 0}
}
```



### Arrays

The type `[n]T` is an array of `n` values of type `T`.

The expression

```
var a [10]int
```

declares a variable `a` as an array of ten integers.

Cannot be resize, An array's length is part of its type. But don't worry; Go provides a convenient way of working with arrays.

### Slices

A slice is like a reference ti arrays, it does not store any data, it just describes a section of an underlying array.

Changing the elements of a slice modifies the elements of its underlying array.

```
a[low : high]
```



#### Slice literals

A slice literal is like an array literal without the length.

This is an array literal:

```
[3]bool{true, true, false}
```

And this creates the same array as above, then builds a slice that references it:

```
[]bool{true, true, false}
```

```
package main

import "fmt"

func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}
```

Unlike arrays, slices are typed only by the elements they contain (not the number of elements).

To create an empty slice with non-zero length, use the builtin `make`.

`s := make([]string, 3)`

One of the builtin `append`, which // returns a slice containing one or more new values. Note that wew need to accept a return value from 'append' as we may get a new slice value.

Slice can also be copied.

```
c := make([]string, len(s))
copy(c, s)
```

Slices support a "slice" operator with the syntax `slice[low:high]`. For example, this gets a slice of the elements `s[2]`, `s[3]`, and `s[4]`. Excluding the end.

`l := s[2:5]`&#x20;

`fmt.Println("sl1:", l)`

We can declare and initialize a variable for slice in a single line as well.

```
t := []string{"g", "h", "i"}
fmt.Println("dcl:", t)
```

Slices can be composed into multi-dimensional data structures. The length of the inner slices can vary, unlike with multi-dimensional arrays. Also, slices can contain any type, including other slices.

```
twoD := make([][]int, 3)
for i := 0; i < 3; i++ {
	innerLen := i + 1
	twoD[i] = make([]int, innerLen)
	for j := 0; j < innerLen; j++ {
		twoD[i][j] = i + j
	}
}
fmt.Println("2d: ", twoD) // 2d:  [[0] [1 2] [2 3 4]]
```



#### Slice defaults

When slicing, you may omit the high or low bounds to use their defaults instead.

The default is zero for the low bound and the length of the slice for the high bound.

For the array

```
var a [10]int
```

these slice expressions are equivalent:

```
a[0:10]
a[:10]
a[0:]
a[:]
```

#### Slice length and capacity

A slice has both a _length_ and a _capacity_.

The length of a slice is the number of elements it contains.

The capacity of a slice is the number of elements in the underlying array, counting from **the first element in the slice**.

`len(s)` and `cap(s)`.

#### Nil slices

The zero value of a slice is `nil`.

A nil slice has a length and capacity of 0 and has no underlying array.



### Range

The `range` form of the `for` loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.

```
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
output:
2**0 = 1
2**1 = 2
2**2 = 4
2**3 = 8
2**4 = 16
2**5 = 32
2**6 = 64
2**7 = 128
```



You can skip the index or value by assigning to `_`.

```
for i, _ := range pow
for _, value := range pow
```

If you only want the index, you can omit the second variable.

```
for i := range pow
```

### Maps

A map maps keys to values.

<mark style="background-color:blue;">The zero value of a map is</mark> <mark style="background-color:blue;"></mark><mark style="background-color:blue;">`nil`</mark><mark style="background-color:blue;">. A</mark> <mark style="background-color:blue;"></mark><mark style="background-color:blue;">`nil`</mark> <mark style="background-color:blue;"></mark><mark style="background-color:blue;">map has no keys, nor can keys be added. 此时add content会出现panic。</mark>

<mark style="background-color:blue;">The</mark> <mark style="background-color:blue;"></mark><mark style="background-color:blue;">`make`</mark> <mark style="background-color:blue;"></mark><mark style="background-color:blue;">function returns a map of the given type, initialized and ready for use. 此时value具有初始值而不是nil。</mark>

```
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}

```

```
ackage main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

func main() {
	printlin(m)
}

```



### Mutating Maps

Insert or update an element in map `m`:

```
m[key] = elem
```

Retrieve an element:

```
elem = m[key]
```

Delete an element:

```
delete(m, key)
```

Test that a key is present with a two-value assignment:

```
elem, ok = m[key]
```

If `key` is in `m`, `ok` is `true`. If not, `ok` is `false`.

If `key` is not in the map, then `elem` is the zero value for the map's element type.

**Note:** If `elem` or `ok` have not yet been declared you could use a short declaration form:

```
elem, ok := m[key]
```
