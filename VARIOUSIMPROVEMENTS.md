# Various improvements
* `SELECT` keyword takes any expression, not just variables.
   Expression is only evaluated once.

* CASE in normal SELECT statement now also supports multiple matches
   like this:
   ```
   SELECT x
      CASE 1,2,x
         ...
      CASE a,b,BLA
         ...
         ........
   ```
* EXIT keyword now works in all four loop constructs.
   
* "::" (pointertyping) now also works on variables, indirect longs
   (ptr[]::) and arrays (object::).

* Unification accepts right-hand list typed as :LONG. Only difference is no 
   list-len-comparison is done and so, "exp" could be just any piece of memory.

* "(" ")" grouping is allowed in constant expressions.

* Typed immediate (and static) lists always allocates whole object(s) and 
   clears fields not used.

* Global ARRAYs and OBJECTs are cleared with zeroes.

* Local librarybases is possible. Just define it locally with same name.

* This does not compile with EC/CreativE, but does with ECX. "x OP -y" where 
   OP is math/bitwise/comparison operator.

* Usage of {} around code labels has been relaxed. From now a code label 
   represents its own address without the need for {} around it.

* With EC/CreativE you cannot just use (without DEFining it) globals exported 
   from another module unless you do it from main source. With ECX you can.

* \* and / operators are fully 32bit on all targets.

* Indexing ([]) an object-pointer with a negative value does not work with 
   EC/CreativE because index is treated as unsigned. ECX treats index as 
   signed.

* Objects can now inherit from multiple parents. Just separate parent names 
   with comma ",". Only the first parent may contain methods though.

* RAISE directive now also handles procedures. Procedure must be from a module 
   though. For procedures in same source better to let it raise its own 
   exception.

* Abs and Not are now unary operators instead of built-in functions. It is 
   fully backwards compatible, but means that there is no need for Abs64 and 
   Not64 in 64bit integer mode. Abs() even works in float mode, so Fabs() is 
   not needed anymore but kept for compatibility. Abs and Not also works in 
   constant expressions.

* SIZEOF extensions. 
   Besides `SIZEOF object` there is now also:
   `SIZEOF {variable}` get size of variable (4 or 8, 0 for array)
   `SIZEOF variable[]` get size of one element of pointer/array
   `SIZEOF (array)` get total size of array in bytes (0 for non array).
    Note that STRING type includes nil-term
   `SIZEOF (static)` Like above but for STATIC data.

* Initialisation for private globals in modules.
   Init values and global arrays possible:
   `DEF xxx=12, yyy[100]:ARRAY`, etc
