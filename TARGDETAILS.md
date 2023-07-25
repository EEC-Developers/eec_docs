# Target Details
## 68k binaries
* Register usage:
   ```
   D0-D2   : return values [scratch]
   D3      : local scratch (callee save for compatibility)
   D4-D7   : register-variables [save]
   A0-A3   : scratch
   A4      : global pointer
   A5      : frame pointer
   A6      : librarybases [scratch]
   A7      : stack pointer

   F0 - F7 : floating point scratch
   F1 - F7 : double parameters
   ```
* Internal functions:
   Thanks to Wouter for letting me use the internal functions of the EC compiler.

## PPC binaries
* A lot of the smaller internal functions are permanently put inline.
   Inlined functions:
      `Char()`, `Int()`, `Long()`, `PutChar()`, `PutInt()`, `PutLong()`, `ListItem()`,
            `ListLen()`, `EstrLen()`, `StrMax()`, `ListMax()`, `SetStr()`, `SetList()`,
            `ObjSize()`, `ObjName()`, `Shr()`, `Shl()`, `Not()`, `Or()`, `Eor()`, `And()`,
            `Odd()`, `Even()`, `Link()`, `Next()`, `Eval()`, `Mul()`, `Div()`, `Car()`, `Cdr()`,
            `Double()`, `PutDouble()`, `Ptr()`, `PutPtr()`, `Real()`, `PutReal()`, `Float()`,
            `PutFloat()`, `Word()`, `PutWord()`, `Byte()`, `PutByte()`, `Fabs()`

* Functions/procedures takes their arguments in registers (r3..r10).

* Additional parameters to functions and procedures (>8 params)
   are passed on the stack. "self" is passed in R12.

* Register usage is following: (SysV / MorphOS)
```
R0        : scratch
R1        : stack pointer
R2        : reserved for system
R3  - R5  : return-values [scratch]
R3  - R10 : parameters to functions and procedures [scratch]
R11,  R12 : scratch
R13       : global environment pointer
R14 - R19 : save
R20 - R31 : register variables [save]

F0  - F13 : scratch
F1  - F8  : floating point parameters to functions and procedures [scratch]
F1  - F4  : floating point return values [scratch]
F14 - F19 : save
F20 - F31 : floating point register variables [save]

V0  - V19 : vector scratch
V2  - V13 : vector parameters to functions and procedures [scratch]
V2  - V5  : vector return values [scratch]
V20 - V31 : vector register variables [save]
```

## 68k and ppc binaries

`String()`, `List()`, `DisposeLink()` functions now uses a private 
   exec/memorypool. This makes allocation faster, deallocation and 
   end_of_program_auto_allocation much faster. You also save 4 bytes for each 
   string/list. It also means runtime memlist gets less cluttered so Dispose() 
   function may get much faster too.