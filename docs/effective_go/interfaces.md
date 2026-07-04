### Interfaces
Interfaces define needed behaviour of a type to be used in method defined on an implementation of the interface.
- interfaces with only one or two methods are common
- a type can implement multiple interfaces
- conversion between types works as long as they only differ by name. The underlying memory type must match

Generality:
- exporting types that only exist an interface should not be exported
- exporting the interface, that defines the anticipated behaviour, is clearer
- this also avoids duplicate documentation
