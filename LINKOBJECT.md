# Linkobject mode
`LINKOBJECT` in source switches on LINKOBJECT mode.

In this mode, ECX will output an ELF linkable object.

This is mainly useful if you want to link some E code together with some C 
code or so, by using an external linker.

Notes:
- Code from modules can not be used from linkobject. Anything else from a 
  module can be used.
- You can not use the internal functions in linkobject mode. If you attempt to 
  use them, the linkobject will be having external references to the functions 
  that you used. This way if you have another linkobject with replacement 
  functions in it, just link it with your linkobject..  and voila.
- You can make use of external functions/labels from your linkobject by using 
  the USES keyword after LINKOBJECT:
  Example:
  ```
    LINKOBJECT USES
      Bla(),
      alabel,
      AnotherFunc(x,y,z),
      YetAnotherOne(i:LONG, f:DOUBLE) (DOUBLE,LONG)
  ```
- You can export globals, procedures and constants from your linkobject, just 
  put EXPORT in front of the stuff you want exported.
- Be aware that some language constructs might need access to internal 
  functions, which means these functions have to be available from some other
  linkobject when linking. NEW/END needs access to FastNew()/FastDispose() and 
  int->float conversion needs a special function. Nilcheck needs access to 
  Throw() function.
- External librarybases needs special handling. C uses DOSBase, E uses 
  dosbase... etc.
  `EXPORT DEF` has an extension in LINKOBJECT mode for this situation:
  Example:
  ```
  OPT MORPHOS
  LINKOBJECT
  MODULE 'exec'
  EXPORT DEF execbase AS SysBase
  ```
- No default modules are automatically loaded.
- This is experimental stuff for now...