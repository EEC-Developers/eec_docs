# PowerPC inline assembler
The PowerPC 32bit instructionset is fully supported and complete.

For the simplified PowerPC mnemonics, resort to this module with 
preprocessing enabled: powerpc/simple.m

Up to 12 general purpose and 12 floating point registers may be used 
for register-variables.

## Special addressing modes
There are no special instructions for accessing E variables.

To load a variable into register:

   `LWZ Rx, variable`

To store a register into a variable:

   `STW Rx, variable`

To get the address of a variable, use the "ADDI" instruction:

   Translates to:
   `ADDI Rx, localvar          -> ADDI Rx, R1, localvar_offset`
   `ADDI Rx, globalvar         -> ADDI Rx, R13, globalvar_offset`

Local ARRAY/STRING/LIST "variables" are a bit different. They are not real 
variables but only represents the address of some storage on the stack.

To get the address of that *data*:
   `ADDI Rx, local_array  -> ADDI Rx, local_array_offset(R1)`

Using a load or store instruction is still possible:

   `LWZ Rx, local_array   -> LWZ Rx, local_array_offset(R1)`
   `-> this loads the first longword of the array into Rx.`

For getting address of code labels, use the special "LA" instruction:

   `LA Rx, codelabel`

We may dereference object members:
```
   LWZ Rx, .member(Ry:object)  -> LWZ Rx, member_offset(Ry)
   STB Rx .member(Ry:object)   -> STB Rx, member_offset(Ry)
   ADDI Rx, .member(Ry:object) -> ADDI Rx, Ry, member_offset
   STW Rx, .member(Ry:self)    -> STW Rx, member_offset(Ry) [in methods]
```
Registervariables (DEF <name>:REG ...) may be used directly as if they 
were registers:

```
   DEF y:REG, x:REG PTR TO object
      LBZ y, .member(x)          -> LBZ Ry, member_offset(Rx)
      ADDI y, .member(x)         -> ADDI Ry, Rx, member_offset
```

Libraryfunction identifier may be used as constant (offset):
```
      ADDI R3, R0, AddTail        ..68K
      LWZ Rx, FindTaskByPID(Ry)   ..SYSV
```

Other special instructions:

   Load Immediate Word

   `LIW Rx, <constant expression>    -> May use two instructions if constant`
   `-> expression does not fit in 16bit.`

   Add Immediate Word

   `ADDIW Rx, <constant expression>  -> May use two instructions if constant`
   `-> expression does not fit in 16bit.`