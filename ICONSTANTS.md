# Internal constants
From EC:

```
TRUE   -> -1
FALSE  -> 0
NIL    -> 0
ALL    -> -1
EMPTY  -> 0
STRLEN -> length of last immediate string
```

New with ECX:

`LINENUM    -> always contains the current line number,`
`           -> useful for debugging.`

Amiga support constants:
```
OLDFILE     -> same as dos/dos.m/MODE_OLDFILE   (EC)
NEWFILE     -> same as dos/dos.m/MODE_NEWFILE   (EC)
READWRITE   -> same as dos/dos.m/MODE_READWRITE (CreativE)
```