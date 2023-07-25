# Preprocessor / conditional compilation
Directives from EC:
* `#ifdef <symbol>`
* `#ifndef <symbol>`
* `#endif`

New directives with ECX:
* `#else`
   "else"
* `#elifdef <symbol>`
   "else if defined"
* `#elifndef <symbol>`
   "else if not defined"

## Notes:
Conditional compilation directives are "free-form", and may be put anywhere 
on a line and does not need to be terminated with newline.

`#ifdef DEBUG WriteF('x=\\d\\n', x := y) #else x := y #endif`