# Namespaces
X   = uppercase
x   = lowercase or "_"
... = anything
#   = anycase
XXX = all uppercase

## Namespace #1:
```
Constants               -> X...
Variables               -> x...
Procedures              -> x...
Asm labels              -> #...
Internal functions      -> Xx...
System functions        -> X...
Inline asm registers    -> XXX
Define symbols          -> #...
```

## Namespace #2:
```
Objects                 -> x...
Inline asm instructions -> XXX
Assembler MACROs        -> X...
```

## Reserved keywords:
`NEW`, `END`, `AND`, `OR`, `BUT`, `OPT`, `MODULE`, `OBJECT`, `ENDOBJECT`, `CONST`,
`SET`, `ENUM`, `PROC`, `ENDPROC`, `IS`, `DEF`, `SUPER`, `FOR`, `STEP`, `ENDFOR`,
`LOOP`, `ENDLOOP`, `WHILE`, `ENDWHILE`, `REPEAT`, `UNTIL`, `JUMP`, `REG`, `IF`,
`THEN`, `ELSE`, `ELSEIF`, `ENDIF`, `SELECT`, `INC`, `CASE`, `DEFAULT`, `ENDSELECT`,
`CHAR`, `INT`, `LONG`, `STRING`, `LIST`, `DEC`, `ARRAY`, `PTR`, `TO`, `DO`, `OF`, `STRLEN`,
`EXPORT`, `SIZEOF`, `RETURN`, `EXCEPT`, `HANDLE`, `EXIT`, `RAISE`, `MACRO`, `ENDM`,
`SHL`, `SHR`, `NOP`, `LIBRARY`, `INCBIN`, `DOUBLE`, `VECTOR`, `CLASS`, `PRIVATE`,
`PUBLIC`, `FLOAT`, `WIDE`, `UWIDE`, `REAL`, `BYTE`, `WORD`, `ULONG`, `AS`, `LINKOBJECT`,
`IFN`, `ELSEIFN`, `WHILEN`, `UNTILN`, `EXITN`