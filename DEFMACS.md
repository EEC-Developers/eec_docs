# Preprocessor / #define macros
Differences compared with EC/CreativE:

* Null-arg macros are supported
   Example: `#define BLA() ..stuff..`
* Variable arguments are supported
   Example: #define Bla(x,y,...) [1,2,x,y,...,NIL]
   Note: If "..." in body is immediately preceded by "," (as in above example), 
   The "," will get removed automatically if the "..." argument is empty.
* Defines may be exported explicitly:
   `EXPORT #define ...`