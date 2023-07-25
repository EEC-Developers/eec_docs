# Limits
## Hardcoded limits:

Maximum values:

- globals
   number of global variables in application (private+public)
    4000..8000 total size of global data \*
- immediate
   * number of entries for an immediate list: 32767 (checked)
   * max length of one immediate string: 16000
   * total length of one continued immediate string ("+"): 16000
- static
   * max length of one immediate string: 16000
   * total length of one continued immediate string ("+"): 16000
   * number of list entries / total list size \*
- arrays
   * number of elements in one array: 32767 (checked)
   * total size of one local array: <32k  (checked)
   * total elements of one global array: 32767 (checked)
   * total size of one array inside object: <32k (checked)
- procedures / methods
   * number of arguments: 255
   * number of local variables / procedure: 4000..8000
   * size of procedure local data: 32k (checked)
   *size of procedure code: min 32k (checked)
- internal functions
   * number of arguments to WriteF(), StringF() etc.: 1024
- objects
   * number of members in one object: 2048
   * number of methods in one object: 2048
   * size of one object: 32k (checked)
- assembler
   * length/size of CHAR/INT/LONG/INCBIN data declarations \*
- preprocessor
   * size in bytes of of macro body before/after expansion: 16000 (checked)
   * number of fixed arguments for macro: 16 (checked)
   * number of variable arguments for macro \*
   * macro nesting - (stack checked)
- misc
   * identifier length in characters 255
   * number of modules used in one application \*
   * size of ascii source \*
   * number of lines in ascii source \*
   * width of one line: 1000 tokens
   *total length of a continued ("+", ",", "(", etc)) line *
   * codesize of one module 16M
   * codesize of executable 32M

\* : No real limit (to my knowledge `:-)`).
   Available memory will most likely be the ultimate limit.