# String formatting
String formatting codes works like with CreativE. To print out the 
% character, use %%
## Immediate codes
```
\n      -  A newline (ascii 10)
\t      -  A tab (ascii 9)
\a      -  A single quote  (')
\q      -  A double quote (")
\e      -  Escape (ascii 27)
\\      -  The backslash itself
\0      -  Nil-byte (ascii 0)
\b      -  A carriage return (ascii 13)
```

## CreativE additions
```
\xHH    -  Insert any character represented by a two digit hex number (HH).
\!      -  A bell (ascii 7)
\v      -  A vertical tabulator (ascii 11)
```

## ECX additions
```
\~         -  Inserts nothing!
\1 .. \9  -  ascii 1 .. ascii 9
```

## Codes used by string formatting functions
```
\d      -  Inserts decimal number, with optional field specifier.
\d[x]

\h      -  Inserts hexadecimal number, with optional field specifier.
\h[x]

\s      -  Inserts a string, with optional field specifier.
\s[x]

\c      -  Inserts a character

\z      -  Zero fill in field (default is space filling). Only affects next 
            field.

\l      -  Place result to the left in field.Affects rest of string (or until 
            \\r).

\r      -  Place result to the right in field (default).  Affects rest of 
            string (or until \\l).
```

## CreativE additions
```
\u      -  Inserts unsigned decimal number, with optional field specifier.
\u[x]
```

## ECX additions
```
\D      -  Inserts 64bit decimal number, with optional field specifier.
\D[x]

\H      -  Inserts 64bit hexadecimal number, with optional field specifier.
\H[x]
```

## Notes:

The (min,max) field specifier of the \\s formatting code is not supported.
