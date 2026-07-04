### Data
There are two allocation primitives: `new` and `make`.

`new(T)`:
Allocates memory: it returns a pointer to a newly allocated zero value of type `T`
- this is similar to the address-of operator `&`
``` 
type Person struct {
    Name  string
}

// Using new()
p1 := new(Player)
p1.Name = "Alice"

// Using a composite literal with & (More Idiomatic)
p2 := &Player{
    Name:  "Alice",
}
```
- composite literals are preferred, as they allow direct initialization 

`make(T, args)`:
Creates **slices**, maps and channels only and returns an initialized (not zeroed) value of type `T`. (note: not arrays!)
```
make([]int, 10, 100) // slice with length 10 and capacity of 100 (of the underlying array) 
``` 

`Arrays`:
Used for the detailed layout of memory but primarly, they are building blocks of slices. 
- arrays are values
- if you pass an array to a function, it receives a copy and not a pointer to the array
- the size is part of its type
- not idiomatic

`Slices`:
Wrap arrays to give a more general interface to sequences of data.
- slices are reference types, referring to the underlying array
- slices can grow and shrink
```
func main() {
    // --- ARRAY BEHAVIOR ---
    arrA := [3]int{1, 2, 3}
    arrB := arrA          // Copies the entire array
    arrB[0] = 99          // Modifies ONLY arrB
    fmt.Println(arrA)     // Prints: [1 2 3]

    // --- SLICE BEHAVIOR ---
    sliceA := []int{1, 2, 3}
    sliceB := sliceA      // Copies the header
    sliceB[0] = 99        // Modifies the underlying data
    fmt.Println(sliceA)   // Prints: [99 2 3]
}
```
- the `append()` method either appends to an array and increases its length or allocated a new, larger underlying array, copies the existing elements over, appends the new element, and updates the slice header

`Maps`:
Associate values of different types, where the key can be anny type for which the equality operator is defined. 
- reference type, similar to slices
- fetching an entry that is not present will return the zero value for the entry type
- `set` can be implemented with a map of `type` to `bool`
- "comma ok" idiom allows to check if entry for given key exists
``` 
seconds, ok := timeZone[tz]
```
- deletion enabled through built-in `delete` function that needs the map and key to be deleted as arguments (note: this is safe if key does not exists)
```
delete(timeZone, "PDT")
```

Printing:
Formatted printing given by the `fmt` package, includes string formatting without printing as well.
```
fmt.Printf("Hello %d\n", 23)
fmt.Fprint(os.Stdout, "Hello ", 23, "\n")
fmt.Println("Hello", 23)
fmt.Println(fmt.Sprint("Hello ", 23))
```
- `fmt.Fprint` takes as a first argument any object that implements the `ìo.Writer` interface
- catchall format `%v` for "value", can be used for maps, slices and so on as well
- `%T` prints the type of a value

`append`:
- schematicly looks like this
```
func append(slice []T, elements...T) []T
```
- `T` is a placeholder for any type, though generally, Go does not support functions where the type `T` is determined by the caller. That is why the function is built in as it is supported by the compiler (note: this is actually outdated, generics exist now in Go)
```
// T is a type constraint meaning "any type". 
// This ensures that the slice and the elements are strictly the same type T.
func CustomAppend[T any](slice []T, elements ...T)[ ]T {
    // Custom implementation logic here
}
```