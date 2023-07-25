# Library mode
What is different ?

* FAST environment lookup (a single instruction)
* Immediate lists [with runtime values] are safe to use, even if library is 
   used by several tasks at the same time.
* An application may open a library several times, just remember to close it 
   as many times.

How does it work ?
   When OpenLibrary() is called on library, a new librarybase that is shared 
   between the library and its opener, is created. This librarybase contains 
   the jump/func-table and the global e-environment needed for each opener. 
   There is no 32k limit on this environment as is usual with other similar 
   solutions. If the same task does OpenLibrary() on our library several 
   times, the same librarybase will be returned as from the first call.

Usage is pretty much like with AmigaE, but keep this in mind:

The main() routine (if defined) should return <>NIL if all went okay, else 
FALSE. Returning FALSE will make OpenLibrary() to fail.

The `LIBRARY ...` declaration must be placed before any `RAISE`, `DEF`, 
   `PROC`, `label:`.

## The private librarybase
A pointer to the librarybase can be obtained through the global "librarybase" 
variable. This is not the librarybase in execbase.liblist, but the one the 
application using the library has obtained.

The 18 bytes after the standard library header of this base is free to use in 
any way by the library/application. You should have GOOD reasons to use this 
area, else don't do it. An example of use would be plugin-libraries for GoldED 
(Dietmar Eilert), that needs to have a special binary ID poked into offset 36 
of the librarybase. This would be done in the main() routine.

librarybase.version is a bit special, it contains the version requested by 
OpenLibrary(), not necessarily the actual version of your library. This might 
be handy if you need to do different stuff depending on the version.. but you 
probably should not do it :)

## Extensions
Using "EMPTY" instead of a procedure name in the list of entries creates a 
dummy function that just returns NIL.

In powerpc mode for morphos, the library will by default have a 68k ABI, which 
will be callable from 68k programs. You should define the registers used for 
arguments and not let the compiler decide, because ECX and EC/CreativE uses 
different defaults. And it makes things more clear.

Using the "SYSV" switch will create a library with MorphOS SYSV ABI. This is 
faster, but cannot be called from 68K programs anymore.

Like this:

```
OPT POWERPC
LIBRARY SYSV name,ver,rev,idstr IS ...
```
A MorphOS 2.0+ library may define a "query" function at the offset of the 
normally unused "ExtFunc" (offset -24), it should look something like this:

(Note: we have no global environment inside query() function! For this reason 
"utilitybase" (needed by FindTagItem()) is passed to it so that we do not have 
to open and close it everytime ourselves.)

```
PROC query(data:PTR TO LONG, attr, utilitybase)
   DEF ti:PTR TO tagitem
   IF ti := FindTagItem(attr, mytags)
      data[] := ti.data
      RETURN TRUE
   ELSE
      RETURN FALSE
   ENDIF
ENDPROC
```
A MorphOS 2.0+ library may use an extended resident structure containing tags. 

Example:

```
LIBRARY 'bla.library', 50, 1, 'bla.library by nisse 2007' TAGS mytags IS
   ...
   ...

mytags:
   LONG TAG_XXX, VAL_XXX,
   TAG_YYY, VAL_YYY,
   TAG_END
```

Other

Finally, it should also be noted that with ECX it is possible to create your 
library or even device "by hand". Just use OPT NOSTARTUP at the top of your 
source and... well that is beyond the scope of this guide.