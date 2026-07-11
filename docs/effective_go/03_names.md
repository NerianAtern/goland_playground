### Names
Semantic effect: visibility of a name outside the package driven by upper/lower case of first letter.

Package names:
- on import, become accessor of contents
```
import "bytes"

bytes.Buffer // <- accessor!
```
- short, concise (imported entities are always addressed with their package name), evocative but does not need to be unique
- lower case and single-world, no need for underscores or lowerCamelCase (side note for Camel Case: acronyms are in capital letters!)
- conventation: use base name of source directoy 

Getters:
- it is not necessary or idiomatic to put `Get` in a getter's name. Use e.g. upper-case name for getters of unexported fields

Interfaces:
- one-method interfaces are named by the method name + *-er* suffix
- avoid the use of canonical names

**Most importantly: Use Camel Case!** 