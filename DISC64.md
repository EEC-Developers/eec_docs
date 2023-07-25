# 64bit discussion
Who knows, some day there might be a 64bit E compiler/language. 
Let me just say, I have currently no plans to do something like that. 
Anyway, Here are some hints, tips, thoughts and a few extra features 
to help a future migration to a 64bit E language.

Migrating to a 64bit PowerPC CPU will be more problematic than 
moving from 32bit 68K => 32bit PPC, that was relatively easy.

Default (untyped) values, PTRs, and all addresses, will suddenly 
be 64bit. CHAR,INT,LONG,DOUBLE will remain unchanged, the 64bit 
WIDE type will be added.

Lets go through areas that needs attention:
1. Untyped variables, arguments, members.
   * On a 32bit system these will all be treated as "LONG", 
   * on a 64bit system they will be treated as "WIDE".
2. Untyped listelements.
   * In a 32bit language, elements will default to "LONG", 
   * on a 64bit system they will default to "WIDE".

3. Storing pointers in arrays.
   Not an uncommon thing to do.
   * In a 32bit system we use "ARRAY OF LONG" and "PTR TO LONG" for this,
   * on a 64bit system we would use "ARRAY OF WIDE", "PTR TO WIDE".
   Now, should we have to change all occurrences of these constructs 
   in our sources, or is there a solution ? There is a solution: 
   * "ARRAY OF PTR" and "PTR TO PTR". This small addition to the ECX 
      typesystem solves this. On a 32bit system "ARRAY OF PTR" will equal 
      "ARRAY OF LONG", on a 64bit system "PTR TO PTR" will equal "PTR TO WIDE".
4. Inline pointers.
   ECX supports inline pointers like this:
      `LONG mylabel, 'blabla', NIL`
   But to be 64bit compatible, this is required:
      `PTR mylabel, 'blabla', NIL`
5. Size of pointers.
   "SIZEOF PTR" gives the size of pointers, 
   The size is 4 on 32bit systems, 8 on 64bit systems.
6. Using 32bit compiled modules with a 64bit compiler.
   A. Constants will be signextended to 64bit integers. 
      This is definitely a problem for any floating point constants.
   B. PTRs in objects will be treated as plain LONGs.
   C. Code in modules will not understand any possible 64bit 
      pointers passed as arguments to procedures. 
      Code in modules will not understand any integer values
      larger than >32bit signed. Code in modules assumes the stack 
      is a 32bit one.. Code in modules will not work.

      *Conclusion, recompile your modules!*

7. Peeking/poking pointers with Long()/PutLong().
   On a 64bit system, pointers will not fit in LONG. 
   ECX adds two replacement functions for this, for future 
   64bit compatibility: Ptr(), PutPtr(). On a 32bit system, 
   these works just as Long(), PutLong(). On a 64bit system, 
   they will do the same as Wide(), PutWide().
8. LONG variables.
   If possible, avoid giving variables the "LONG" type, 
   unless really needed, just leave them untyped. 
   This way on a 64bit system they will be treated as WIDE, 
   and possibly optimised into a register. Especially avoid 
   "x:REG LONG", it will likely not work on 64bit target.
9. X86_64
   64bit PPC would be easiest to implement, if working from 
   the ECX sourcecode. But X86_64 seems like a relatively nice CPU too `:)`
   Certainly it would be possible, but then there will be endian-issues 
   to care about also, if old code is to be ported. And there is 
   no magical solution to this but to rewrite code where needed, 
   use macros if portability X86<=>PPC is desired.

Have you noticed, on the 64bit system there are two types 
with the same size: DOUBLE and WIDE. Why is this ?

1. It gives compiler the possibility to place variables meant 
   for float-values in float registers, variables meant for 
   integers/pointers in general purpose registers.
2. It also has to do with automatic scaling. 
   Assignments LONG<=>DOUBLE automatically scales single<=>double floats. 
   Assignments CHAR/INT <=> LONG <=> WIDE automatically scales in a similar 
   way but now for integers.

Finally, the 64bit version of the type-table:

| Type                           | Size       | Reference
|--------------------------------|------------|----------
| CHAR, BYTE                     | 1          | Value   (WIDE)
| INT, WORD                      | 2          | Value   (WIDE)
| LONG, ULONG                    | 4          | Value   (WIDE)
| WIDE                           | 8          | Value   (WIDE)
| PTR TO *                       | 8          | Value   (WIDE)
| DOUBLE                         | 8          | Value   (REAL)
| ARRAY OF *, LIST, STRING       | x          | Address (WIDE)
