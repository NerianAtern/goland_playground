### Formatting
Formatting is taken care of by `the `gofmt` package to standardized code.
- tabs for indentation (limit use of spaces)
- no line lengths limits
- less need for parantheses
- operator precdence hierarchy encoded in spacing
```
x<<8 + y<<16
```

- **GROUPING OF DECLERATIONS:** introduces relationship within a set of variables, e.g. protected by a mutex:
``` 
var (
    countLock sync.Mutex
    inputCount uint32
    outputCount uint32
    errorCount uint32
)
```