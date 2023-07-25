# 64bit integers
ECX supports 64bit integers on 32bit targets.

The WIDE type has the usual versions:

   `WIDE`, `PTR TO WIDE`, `ARRAY OF WIDE`

It can be used anywhere (objects, locals, arguments, globals, lists).

Usage is similar to how floating point is handled.

To make computations (math,bitwise,comparison) in full 64bit, the @ (at) sign 
is used:

```
x := @ y * z -> switch into 64bit mode and do multiplication.

IF @ x < y   -> switch into 64bit mode and make comparison.
```

Note that there is not need to explicetly convert between 64bit integers and 
32bit integers, scaling is automatic.

Assignment, ++, --, +=, etc does not need the @ sign. Same with simple testing 
of values with IF, WHILE, UNTIL, EXIT, etc:

```
   wide := wide2
   long := wide
   wide := wideptr[]++
   widey := func()
   wide += 100
   WHILE mywide--
   IF widereturningfunc()
   ...
```

SELECT accepts wides too:

   `SELECT mywide`
   `CASE something -> compares in full 64bit`
   `...`

Note that the compiler looks at "mywide" here to decide whether to go into 
64bit mode or not. "something" can have any number of bits but does not alter 
mode.

New internal functions:

   `Mod64()`        -  64bit version of Mod()
   `UlongToWide()`  -  Convert LONG to WIDE without extending sign.

Notes:

   All the math/bitwise/comparison operators works in 64bit mode too.

   64bit "/" operator and Mod64() function does not support negative
   values for now. They might in future. So only pass them positive
   values.

   "WIDE" and "ARRAY OF WIDE" are 4byte aligned in objects by default.

   This feature is only for powerpc targets for now.

   ECX does not support 64bit immediate values yet. Seldom needed though.