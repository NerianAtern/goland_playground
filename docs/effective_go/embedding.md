### Embedding
Go does not support subclassing, but can allow "borrowing" pieces off an implementation by embedding types within a struct or interface.
- e.g. the `ìo.ReadWriter` is an interface of `ìo.Reader` and `io.Writer`, i.e. a `ReadWriter` can do what the other two types can -> a union of embedded interfaces
- calling methods of embedded interfaces invokes the inner type, not the outer one
- type name of the field serves as field name (ignoring package name)
- this introduces potential naming conflicts, resolved by the fact that on the same level, fields may not share names