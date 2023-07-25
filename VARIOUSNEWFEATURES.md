# Various new features
## From CreativE:
* New operators "<<", ">>"
   `<<` Bitwise shift left. Same binary function as Shl().
   `>>` Arithmetic shift right. Same binary function as Shr().
* LONG label
   Example: `LONG myLabel`
* Modification.. ex:
   ```
   x+=y                 -> x := x + y
   x.y[]++ *= z.bla     -> x.y[] := x.y[] * z.bla ; x.y++
   ```
   Works for math/bitwise operators.

   ECX addition: If destination is of type FLOAT/DOUBLE/REAL
   operation will automatically be done in floating point mode:
   ```
          DEF f:REAL
          f+=3.0               -> f := ! f + 3.0
   ```

## New with ECX:
* New operators "SHR", "XOR"
   `SHR` - Bitwise shift right. (Non arithmetic shift)
   `XOR` - Exclusive OR. Same binary function as Eor().

   Usage is allowed in code and in declarations such as CONST etc..
   "Precedence" is like other math/bitwise/comparison operators.
* LONG 'string'
   Example: `LONG theProcedure, 'of drinking', 10, 'beers'`
* Float constants is possible.
   Example: `CONST MYFLOAT=10.99`
      `CONST MYOTHERFLOAT=!MYFLOAT + 0.5`
      `CONST MYINTEGER=!MYFLOAT!`
* Local class objects (DEF myclass:<classobjname>) is possible. ".end()" is 
   called automatically at procedure end.

   Note: It is not possible to use the object in RETURN/IS/ENDPROC part of 
   procedure. Reason is .end() method has already been called just before 
   this. Implementation quirk that will not be fixed.

* Object members new features:
```
   OBJECT test
      test_as_array[0]:ARRAY OF LONG
      a:PTR TO LONG,b:PTR TO INT,c:PTR TO CHAR
      x:PTR TO object @ a
      private1 @ -4
      private2 @ -8
   ENDOBJECT
```
   1. arrays in objects may have zero-size.
   2. union-members are members that are placed at the offset of another 
      already defined member, but takes no place, and does not change the size 
      of an object. The "@" sign (at) is used for this purpose.
   3. members may be placed at a specific offset, similar to 2.

   Viewmodule would print out "test" this way:
```
   (----) OBJECT test
   (   0)   test_as_array[0]:ARRAY OF LONG
   (   0)   a:PTR TO LONG
   (   4)   b:PTR TO INT
   (   8)   c:PTR TO CHAR
   (   0)   x:PTR TO object
   (  -4)   private1:LONG
   (  -8)   private2:LONG
   (----) ENDOBJECT /* SIZEOF = 8 */
```

* Methods have also access to the virtual "self" object.
   ```
   PROC method(s:PTR TO self)  OF bla
      DEF s:self, s:PTR TO self
      ...
   ```

* `OFFSETOF object.member`
   Gives you the offset of member inside object. Handles objects in objects 
   recursively.

* It is now possible to define function variables with DEF.
```
   DEF myfunc(LONG,REAL), x, a:REAL, b, c:REAL
   DEF myfunc2(REAL,REAL,PTR)(REAL,LONG,REAL)

   x := myfunc(1,10.0)
   a,b,c := myfunc2(1.0,2.0,[1,2,3])
```
* Inverted versions of IF, ELSEIF, WHILE, UNTIL, EXIT, a feature I copied from 
   the PowerD language:
   ```
   IFN x       -> IF x = NIL
   ELSEIFN x   -> ELSEIF x = NIL
   WHILEN x    -> WHILE x = NIL
   UNTILN x    -> UNTIL x = NIL
   EXITN x     -> EXIT x = NIL
   ```
   Or even:
   `IFN x > y   -> IF x <= y`

* New types:
   signed CHAR, called `BYTE` (and `PTR TO BYTE`, `ARRAY OF BYTE`)
   unsigned INT, called `WORD` (and `PTR TO WORD`, `ARRAY OF WORD`)

   32bit FLOAT, called `FLOAT` (and `PTR TO FLOAT`, `ARRAY OF FLOAT`)

* Local labels
   Simply put a "." in front of label.
   Label must be inside a procedure and will not be visbile outside ot it.

   .blah:
   JUMP .blah