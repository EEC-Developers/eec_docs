# Preprocessor / assembler macros
Macros with multiple statements like the assembler "MACRO" macros 
are also supported. Quite useful when using PowerPC asm, but is not 
restricted to asm instructions.

```
<label> MACRO
  ...
  ...
  ...
ENDM
```

Arguments are received in "\\1" to "\\9".
"\\0" is reserved for an optional "." / ".L" / ".W" / ".B" / ".D".

example:

A PowerPC asm macro:

```
ROTLW MACRO -> Rotate Left Word
   RLWNM\\0 \\1,\\2,\\3,0,31
ENDM
```

Using the macro:

```
ROTLW R3, R4, R5  -> produces : RLWNM R3, R4, R5, 0, 31
ROTLW. R3, R4, R5 -> produces : RLWNM. R3, R4, R5, 0, 31
ROTLW x,y         -> produces : RLWNM x y, 0, 31 -> error !
```

## A different FOR construct:
```
FOR2 MACRO
   \\1
   WHILE \\2
ENDM

ENDFOR2 MACRO
   \\1
   ENDWHILE
ENDM
```

Use it:
```
FOR2 x := 3, x < 1000
   WriteF('x=\\d\n', x)
ENDFOR2 x := x * 3
```

"EXPORT" may be used:

```
EXPORT COPY MACRO
   ...
```

To use a MACRO, it must be the first thing on a line (white space not counted) 
or it will not be recognised as a MACRO.