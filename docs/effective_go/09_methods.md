### Methods
Can be defined for any named type that is not a pointer or an interface, receiver does not have to be a struct.
```
type ByteSlice []byte

func (slice ByteSlice) Append(data []byte) []byte {
    // Body, needs to return updated slice
}

func (p *ByteSlice) Append(data []byte) {
    slice := *p
    // Body as above, without the return.
    *p = slice
}
```
- value methods can be invoked in pointers and values
- pointer methods can only be invoked on pointers