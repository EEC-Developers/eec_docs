# Internal functions
All EC 68k internal functions except lisp cell functions are implemented for 
AmigaOS target.

All of above except `Mouse()` and `Gadget()` are implemented for AmigaOS4 and 
MorphOS targets.

PowerPC `List()`, `String()`, `DisposeLink()` now accepts an optional 
   argument: `mempool`. `mempool` should be a pool created with 
   exec.library/CreatePool().

PowerPC `ForAll()`, `SelectList()`, `MapList()`, `Exists()` Currently does not 
   check list length so make sure destination list is big enough! Also, these 
   functions really cannot be used recursively for now (implementation quirk).

   `StrCopy()` now handles zero length copy correctly (EC/CreativE version 
   just returns without setting new string length / nil term).

New functions:

   `ObjName(eobj)`, `ObjSize(eobj)`.
      Use these functions to get the name and size of an E object with methods.
      (Class methods does not qualify! (no method table))

   `DebugF(fmtstr,...)`.
      Will normally send its output to the serial port, unless something like 
      sashimi is installed.  Functions like debug_lib/KPrintF().

   `NewList(list)`
      Initialises exec list header.