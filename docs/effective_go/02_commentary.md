### Commentary
C-style `/* */` and C++-style `//` comments, while line comments are the norm.

Documentation extracted by `godoc` (note: minimal approach):
- document by a comment preceding declaration with no intervening blank like
- comment must begin with the name of the element
- general package documentation above package declaration (first sentence will appear in `godoc`'s package list). Multi-file packages require this comment only in one file.
```
// Package example is an example.
package example
```
- bugs are documented with ``BUG(user_name)`
- deprecated functions/struct are documented with `Deprecated: `
- URLs are converted to HTML links