# Go Cheatsheet

## Basics
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

## Variables & Types
```go
// Type inference
var a = 3
b := 5                     // short declaration (func scope only)

// Explicit types
var name string = "Go"
var age int = 30

// Zero values
var i int      // 0
var s string   // ""
var b bool     // false

// Constants
const Pi = 3.14
```

## Collections
```go
// Arrays
arr := [3]int{1, 2, 3}
arr2 := [...]int{1, 2, 3}

// Slices (dynamic)
slice := []int{1, 2, 3}
slice = append(slice, 4)
sub := slice[1:3]

// Maps
m := map[string]int{"a": 1, "b": 2}
delete(m, "a")
val, ok := m["b"]   // check existence
```

## Control Flow
```go
// If
if x > 0 {
    fmt.Println("positive")
} else if x < 0 {
    fmt.Println("negative")
} else {
    fmt.Println("zero")
}

// Switch
switch day {
case "Mon":
    fmt.Println("Monday")
case "Tue", "Wed":
    fmt.Println("Midweek")
default:
    fmt.Println("Other")
}

// For loops
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
for i, v := range slice {
    fmt.Println(i, v)
}
for {                      // infinite
    break
    continue
}
```

## Functions
```go
// Basic
func add(a, b int) int {
    return a + b
}

// Multiple return values
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Variadic
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}

// Closures
adder := func() func(int) int {
    sum := 0
    return func(x int) int {
        sum += x
        return sum
    }
}
```

## Structs & Interfaces
```go
// Struct
type Person struct {
    Name string
    Age  int
}

// Methods
func (p Person) Greet() string {
    return "Hi, I'm " + p.Name
}
func (p *Person) SetAge(age int) {
    p.Age = age
}

// Interface
type Greeter interface {
    Greet() string
}

// Embedding
type Employee struct {
    Person
    Company string
}
```

## Error Handling
```go
result, err := riskyFunc()
if err != nil {
    log.Fatal(err)
}
// use result

// Custom errors
type MyError struct {
    Msg string
}
func (e *MyError) Error() string {
    return e.Msg
}
```

## Goroutines & Channels
```go
// Goroutine
go func() {
    fmt.Println("async")
}()

// Channel
ch := make(chan int)
ch <- 42          // send
val := <-ch      // receive
close(ch)

// Buffered channel
ch := make(chan int, 3)

// Select
select {
case msg := <-ch1:
    fmt.Println(msg)
case ch2 <- data:
    fmt.Println("sent")
default:
    fmt.Println("no activity")
}

// Worker pool
func worker(ch <-chan int, out chan<- int) {
    for v := range ch {
        out <- v * 2
    }
}
```

## Packages & Modules
```go
// go.mod
module github.com/user/myproject

require (
    github.com/pkg/errors v0.9.0
)

// Visibility (capitalized = exported)
func PublicFunc() {}
func privateFunc() {}

// Init
func init() {
    // runs before main
}
```

## Generics (Go 1.18+)
```go
// Type parameters
func Map[T, U any](s []T, f func(T) U) []U {
    result := make([]U, len(s))
    for i, v := range s {
        result[i] = f(v)
    }
    return result
}

// Constraints
type Number interface {
    int | float64
}
func Sum[T Number](nums []T) T {
    var total T
    for _, n := range nums {
        total += n
    }
    return total
}
```

## Common stdlib
```go
fmt, errors, os, io, bufio, flag, log, strings, strconv
json, xml, time, context, sync, atomic
net/http, net/url, html/template
```

## Tips
- `go run .` — run program
- `go build -o binary .` — compile
- `go fmt ./...` — format code
- `go test ./...` — run tests
- `go get package@version` — add dependency
- Use `go vet ./...` for linting
- Pointers: `*T` (dereference), `&val` (address)
- Defer: `defer cleanup()` runs on function exit
- Slices are references; arrays are values
