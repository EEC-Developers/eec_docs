# STATIC data
The `STATIC` keyword is used to define tables or strings of data.
```
STATIC mystring = 'hello universe!!',
   mylongstring = 'this string is so very..' +
      'very very very very very ' +
      '..long!!'

OBJECT myobj
   w,x,y,z
ENDOBJECT

STATIC mylist = [1,2,3,4],
   myobj = [4,5,6,7]:myobj,
   myarray = [3,4,5]:INT
```

List/array elements can be constant expressions, strings, code labels / 
statics and other lists.
```
STATIC mycomplexlist =
   [1,2,3,[myfunction, mylist,'hello!',
   [-1, 10*MYCONST, 9.999]:someobj]:PTR,NIL]
```
Note that static lists has no problems being typed with object containing 
arrays (or other objects):
```
OBJECT blaha
   a,b,c
   array[2]:ARRAY OF REAL
ENDOBJECT

STATIC myobj2 = [1,2,3,[10.0,-150.5]:REAL]:blaha
```

Dereferencing of static data is much like normal dereferencing of
variables/members. Difference is you cannot make assignments,
increment/decrement, NEW/END etc. A (derefenced) label is always
an expression.
```
WriteF('\\s', mystring)     -> prints 'hello universe!!'
WriteF('\\c', mystring[4])  -> prints 'o'

WriteF('\\d', myobj.z)      -> prints '7'
WriteF('\\d', myarray[1])   -> prints '4'
```

Selection (.), indexing ([]) and pointer typing (::) can be used to
dereference a static label.