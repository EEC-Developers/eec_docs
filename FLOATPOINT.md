# Floating point support
Floating point computations with +-*/ are 64bit and uses inline floating point 
instructions as opposed to calling libraryroutines for this.

In powerpc mode, also all internal Fxxx floating point support functions 
does computations in full 64bit. Exception is RealF() which only supports 
single precision for now.

The basic, pointer and array types for dealing with 64bit floats:

   DOUBLE, PTR TO DOUBLE, ARRAY OF DOUBLE

And 32bit floats:

   FLOAT, PTR TO FLOAT, ARRAY OF FLOAT

And a generic float type;

   REAL, PTR TO REAL, ARRAY OF REAL

The REAL type means "the default floating point precision" and is currently 
the same as DOUBLE, that is 64 bits. but it could be different on some 
targets. Always use REAL unless you absolutely need it to be exactly 64 bits 
(DOUBLE) or 32 bits (FLOAT).

Note that the LONG type can still be used to store floats just as in original 
AmigaE. The FLOAT/ARRAY OF FLOAT type only differs in that it is automatically 
4-byte aligned in objects and as inline data, instead of 2-byte. This is 
needed for some cpu's like the PowerPC. DOUBLE/ARRAY OF DOUBLE is aligned the 
same way.

REAL/DOUBLE may be an argument.

```
PROC doublestuff(d:REAL,x)
ENDPROC ! d * 2.0 -> returns 32bit float in normal return register.
```

It is possible to return a float in a floating point register.

```
PROC doublestuff(d:REAL,x) (REAL) -> tell compiler to return in float register.
ENDPROC ! d * 2.0 -> returns value in 64bit float register.
```
       
Six new functions:
```
d := Double(adr)
r := Real(adr)
```
   reads a double (64bit) float from memory.

```
PutDouble(adr,f)
PutReal(adr,f)
```
writes a double float to memory at address "adr". "f" should be any float 
expression.

`r := Float(adr)`
   reads a single (32bit) float from memory. It differs from Long() in that 
   result is returned in floating point register.

`PutFloat(adr,f)`
   writes a single (32bit) float to memory at address "adr". "f" should be 
   any float expression. It differs from PutLong() in that "f" is passed in 
   floating point register.

## Other notes:

1. Up to all 4 returnvalues may use floating point registers.
2. LONG/PTR can be used to specify non float values when several mixed type 
   return values are used, example:

   `PROC bla(x) (LONG,REAL,PTR,REAL)`

3. Avoid "REG DOUBLE" for future compatibility. Use "REG REAL" instead.
4. Using REAL variables is quicker than using LONG/FLOAT because ECX keeps 
   LONG/FLOAT variables in 32bit integer registers. This especially makes 
   difference on the PowerPC. Another reason to type your float variables to 
   REAL.

Note that ECX does not support double precision immediate/constant values 
currently, but values will be converted from single to double precision.