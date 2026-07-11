### Functions
Functions can return multiple values, allowing e.g. for in-band error returns. 
- return values can be given names -> they become *parameters* and are initialized with their respective zero values
- names are not mandatory but can simplify code

`defer`:
Schedules a function call to be run before the parent function returns.
- useful for e.g. the release of resources
``` 
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()
    // ...
```
- this guarantees that closing resources will not be forgotten
- cleaner and clearer if *open* and *close* are next to each other
- parameters passed to the deferred function are evaluated on `defer`, not on the function's execution
- execution is performed in last-in-first-out order 