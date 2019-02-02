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
- Go does not have classes. However, you can define methods on types. A method is a function with a special receiver argument
- You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as int)
- Methods with pointer receivers take either a value or a pointer as the receiver when they are called
- An interface type is defined as a set of method signatures
- Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement
- Interface under the hood can be thought of as a tuple with value & concrete type `(value, type)`
- A nil interface value holds neither value nor concrete types. Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which concrete method to call
- The interface type that specifies zero methods is known as the empty interface
- An empty interface may hold values of any type. (Every type implements at least zero methods.)
- Empty interfaces are used by code that handles values of unknown type. For example, fmt.Print takes any number of arguments of type interface{}
- A goroutine is a lightweight thread managed by the Go runtime
- Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`
- The select statement lets a goroutine wait on multiple communication operations
- If we don't need communication & if we just want to make sure only one goroutine can access a variable at a time to avoid conflicts then we use `sync.Mutex`

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

// closures
func fibonacci() func() int {
	x, y, z := 0, 1, 0
	return func () int {
		z, x, y = x, y, x + y
		return z
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}

// methods
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK

var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK

// interfaces
type Abser interface {
	Abs() float64
}

var i interface{}
t, ok := i.(T) // type assertion

switch v := i.(type) {
case T:
    // here v has type T
case S:
    // here v has type S
default:
    // no match; here v has the same type as i
}

// errors
type error interface {
    Error() string
}

i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
    return
}
fmt.Println("Converted integer:", i)

// readers
func (T) Read(b []byte) (n int, err error) // io.Reader interface

type MyReader struct{}

func (r MyReader) Read(b []byte) (n int, err error) {
	n = len(b)
	for i:=0; i < n; i++ {
		b[i] = 'A'
	}
	return n, nil
}

func main() {
	reader.Validate(MyReader{})
}

// images
type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}

// goroutines
go f(x, y, z)

// channels
ch := make(chan int)

ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.

ch := make(chan int, 100) // buffer of 100

v, ok := <-ch

close(ch)

// select
select {
case c <- x:
    x, y = y, x+y
case <-quit:
    fmt.Println("quit")
    return
}

type Tree struct {
    Left  *Tree
    Value int
    Right *Tree
}

sync.Mutex.Lock()
sync.Mutex.Unlock()
```