## Quickbytes

- In Go, a name is exported if it begins with a capital letter
- The expression `T(v)` converts the value `v` to the type `T`
- Go has only one looping construct, the for loop (init; condition; post)
- The if statement can start with a short statement to execute before the condition
- A defer statement defers the execution of a function until the surrounding function returns
- Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order
- The & operator generates a pointer to its operand. The * operator denotes the pointer's underlying value
- Pointers to structs can access members with `p.X` instead of `(*p).X`
- The length of a slice is the number of elements it contains
- The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice
- The range form of the for loop iterates over a slice or map

## Syntax

```go
// vars
var x = 10
x := 10

// funcs
func swap(x, y int) (int, int) {
    return y, x
}

func swap(x, y) (y, x int) {
    return
}

// conditions
if x := math.Pow(2, 2); x < 5 {
    return true
}

switch os := runtime.GOOS; os {
case "darwin":
    fmt.Println("OS X.")
case "linux":
    fmt.Println("Linux.")
default:
    fmt.Printf("%s.", os)
}

// defer
defer fmt.Println("world")

// pointers
i := 42
p = &i
*p = 21 // This is known as "dereferencing" or "indirecting"

// struct
type Vertex struct {
	X int
	Y int
}

v := Vertex{1, 2}
v.X = 4
p = &v
p.X = 10

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

// arrays, slices
var a [10]int
primes := [6]int{2, 3, 5, 7, 11, 13}
var slice []int = primes[1:4]

[3]bool{true, true, false} // array literal
[]bool{true, true, false} // slice literal

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

var a [10]int
a[0:10] = a[:10] = a[0:] = a[:]

s := []int{2, 3, 5, 7, 11, 13}
len(s) // lenth of slice
cap(s) // capacity of slice

var s []int // nil slice

a := make([]int, 5)

var s []int
s = append(s, 2, 3, 4)

for i, v := range s {
    fmt.Printf("%d = %d\n", i, v)
}

for _, v := range s {
    fmt.Printf("value = %d\n", v)
}

for i := range s {
    fmt.Printf("index = %d\n", i)
}

// maps
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

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}

delete(m, key)
elem, ok := m[key]

```
