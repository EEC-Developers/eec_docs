# Class methods
Besides E's normal methods, ECX also supports class methods.

Definition of a class method is as follows:

PROC myClassMethod() OF CLASS myNiceObject

"myNiceObject" is some kind of OBJECT we have previously declared.

Class methods are a powerful kind of syntax sugar and functions alot like a 
macro.

Class methods may be inherited and redefined just as normal methods.

Difference with C++ and Java class/static methods, is that ECX class methods 
has access to "self" just as normal methods do. And there is no such thing as 
a physical "class", only instances of classes (objects).

The object in question does not have to be allocated with NEW, or even created 
from E, it could be any structure from the OS, say "window". It could even be 
NIL, but then the method shouldn't try to peek/poke it `:)`.

ViewModule will mark a class method like this (notice the asterix):
(   *)    mymethod (bla)