# Incompatibilities
* Increment/decrement on object-members affects the *member* and NOT 
   the base variable. I did not change this just to be annoying...
   it also has to do with the uselessness of the construct as it 
   was in AmigaE versus how much more useful it has now become `:)` 
   And I've never seen any source that actually depends on this feature.

   examples:
   ```
   object.member++      -> "member" is incremented.
   object[x].y.z--      -> "z" is decremented.
   bla.x[]--            -> "x" is decremented.
   ```
   * The Int() function of ECX returns a 32bit sign extended 16bit value 
      rather than unsigned as EC does. The new Word() function does what 
      old Int() did.
   * Local STRINGs allocated on the stack are NOT allowed to be linked
      with Link() function.
   * When receiving multiple return values, ECX only allows receiving into 
      simple variables. EC/CreativE allows first of multiple return values to 
      go into a dereferenced variable.


## Unsupported stuff
* "^" operator (use `Long()` or `[]` instead).
* Lisp cells. Never found a use for them and never under six years heard 
   anyone wanted it implemented so it is gone.