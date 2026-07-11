### Initialization
Complex structures can be built during initialization.

Constants:
- created at compile time
- can only be numbers, strings or booleans
- must be constant expressions, evaluatable by the compiler
- `iota` can be used for sequential integer constants
```
const (
    Low = iota // 0
    Medium     // 1 
    High       // 2 
)
```
- `iota` can be used in arithmetric or bit operations as well
```
const (
    First = iota + 1 // 0 + 1 = 1
    Second           // 1 + 1 = 2
    Third            // 2 + 1 = 3
)

// iota resets with each const 
const (
    _  = iota             // ignore first value
    KB = 1 << (10 * iota) // 1 << 10 = 1024
    MB                    // 1 << 20 = 1,048,576
    GB                    // 1 << 30 = 1,073,741,824
)
```

Variables:
- can be initialized like constants but allow general expressions computed at run time
```
var (
    HOME = os.Getenv("HOME")
    USER = os.Getenv("USER")
    GOROOT = os.Getenv("GOROOT")
)
```

`init` function:
Each source file can define its own niladic `init` function.
```
func init() {
    if USER == "" {
        log.Fatal("$USER not set")
    }
    if HOME == "" {
        HOME = "/usr/" + USER
    }
    if GOROOT == "" {
        GOROOT = HOME + "/go"
    }
    // GOROOT may be overridden by --goroot flag on command line.
    flag.StringVar(&GOROOT, "goroot", GOROOT, "Go root  directory")
}
```