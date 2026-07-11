### Control Structures
No `do` or `while` loop, `for` is generalized and `switch` is more flexible. Also, go introduces a communication multiplexer `select`.

`if`:
- no mandatory braces to not encourage simple `if` statements on multiple lines
- accepts initialization statement, e.g. 
``` 
if err := someFunc(); err != nil {
    return err
}
```
- `èlse` can be omitted, in particular if cases end in `return` statements 

Redeclaration:
- duplication, e.g. for the redeclaration of `err` is legal. If `:=` declares mutliple variables, it only re-assigns to already declared variables 

`for`:
The `for`-loop unifies `for` and `while.
- `for`-loop defined by
```
for init; condition; post { }
```
- `while`-loop defined by
```
for condition {}
```
- infinite loop defined by
```
for {}
```
- looping over array/slice elements defined by `range` clause (key corresponds to index)
```
for key, value := range oldMap {
    newMap[key] = value
}
```
- if only the key/index is needed
```
for key := range m {
    // do something
}

```
- ``range` clause breaks strings in Unicode chars
```
for pos, char := range "дъб" {
    fmt.Printf("character %c starts at byte position %d\n", char, pos)
}

/*
character д starts at byte position 0
character ъ starts at byte position 2
character б starts at byte position 4
*/
```

`switch`:
- xxpressions do not need to be constants or integers 
- cases are evaluated top to bottom until match is found (mo automatic fall through)
- multiple cases can be put into a comma-separated list
```
func shouldEscape(c byte) bool {
    switch c {
    case ’ ’, ’?’, ’&’, ’=’, ’#’, ’+’, ’%’:
        return true
    }
    return false
}
```
- `switch` can be used on type
```
switch t := interfaceValue.(type) {
default:
    fmt.Printf("unexpected type %T", t) // %T prints type
case bool:
    fmt.Printf("boolean %t\n", t)
case *bool:
    fmt.Printf("pointer to boolean %t\n", *t)
}
```