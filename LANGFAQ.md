# Language FAQ
Q: I define an array and later I try to modify it like a variable, but it 
   doesn't work!?

A: Arrays are not variables to ECX, they are addresses made up of an offset 
   relative to a baseregister. So it cannot be changed. This is not true for 
   global arrays though, because they need to be able to merge with exported 
   globals from modules. ECX will complain anyway if you modify a global 
   array. Solution: use pointers, or modify _contents_ of array instead.

---

Q: I have a float expression like
   `y := Fabs(!x+3.3)*0.5`
   and ECX compiles this wrong !?!

A: EC/Creative in the above example lets the "!x+3.3" float expression "leak" 
   the float mode outside of the parentheses. This is undocumented behaviour, 
   not even sure if it is deliberate. A problem with this is that it makes an 
   expression like following impossible:
   `y := func_returning_integer_but_taking_float(!a*b) + 12`
   And what should happen in this case ?
   `y := myfunc(a, !x+3.3, b) * 0.5`

   Anyway, the correct syntax for the code in question (works on all E compilers):

   `y := !Fabs(!x+3.3)*0.5`

---

Q: I have allocated large arrays of data on the stack and ECX complains, what 
   gives?

A: ECX limits data on the stack for each procedure to 32k. Making ECX support 
   above that is not worth the effort.

   Workarounds:

   a. Make the array global instead. Great solution unless you need to be 
      recursive or multi threading safe.

   b. Allocate/deallocate the array dynamically. Might sometimes have a 
      negative inpact on speed, but allows size to change runtime.

   c. Simply let the procedure take the array as argument from its caller. A 
      very powerful solution when it fits the situation.

---

Q: SdivMod32()/UdivMod32() from utility.library does not work when compiling 
   for PowerPC MorphOS, why?

A: These functions do not work for PowerPC native programs from C either, 
   because of a bug in their implementation. Maybe this has been fixed in 
   MorphOS 2.x, maybe not. Anyway, ECX's built-in Mod() is fully 32bit so 
   atleast SdivMod32() is not needed anymore.