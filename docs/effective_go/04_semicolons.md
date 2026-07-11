### Semicolons
Semicolons are set by lexer (part of the compiler that tokenizes code), so source code is mostly free of them. The rule:
```
If the last token before a newline is an identifier, a basic literal, or any of break, continue, fallthrough, return, ++, --, ), } the lexer always inserts a semicolon after the token.
``` 
(so anything that can end a statement)
- semicolons can also be omitted before closing brace
- semicolons are only used in structures similar to `for` loop clauses to separate initializer, condition and continuation elements
- still needed to separate multiple statements in one line 
- opening brace for control structure should never be put on the next line