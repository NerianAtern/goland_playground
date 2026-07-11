### Errors
Go uses a multivalue return for easy error indication alongside the normal return values.
```
type error interface {
    Error() string
}
```
This interface shall be used for proper error description that makes it easy for the caller to understand what happened.
- when feasible, error strings should identify their origin
- type switch/assertion can be used to check for specific errors
```
if err == nil {
    return
}
// second *if* is idiomatic
if e, ok := err.(*os.PathError); ok && e.Err == syscall.ENOSPC {
    deleteTempFiles() // Recover some space.
}
```

`panic`:
- errors are returned as an extra return value
- `panic` creates a run-time error for unrecoverable errors
- compiler ignores the check for return values if `panic` keyword is used
- `panic` should be used sparsely, e.g. during startup (`init`)

`recover`:
- `panic` kill a program, so the whole goroutine stack, running all deferred function calls
- a call to `recover` stops the unwinding of goroutines, useful in deferred functions
- `recover` must be called by a deferred function to work!
```
func server(workChan <-chan *Work) {
    for work := range workChan {
        go safelyDo(work)
    }
}

// result is logged and goroutine exists safely
func safelyDo(work *Work) {
    defer func() {
        if err := recover(); err != nil {
            log.Println("work failed:", err)
        }
    }()
    do(work)
}
```
- (instead of looking ahead with a try-catch-block, go looks backward with the `recover()`)
- go calls `recover()` and whatever it finds is assigned to `err