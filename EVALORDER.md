# Order of evaluation
Apart from assignment, all expressions are always evaluated 
from left to right. Be it math/bitwise/comparison or parameters 
to functions or elements of an immediate list.

"a+b*c" is evaluated as:

1. load value of a
2. add value of b
3. multiply with value of c

"bla(x[]++, x[]++)" is evaluated as:

1. load value of x[] as first parameter
2. increment x
3. load value of x[] as second paramater
4. increment x
5. call function

Assignment always evaluates right hand side expression first, then if needed, 
evaluates the left hand side.

"x[exp] := y[]++" is evaluated as:

1. load value from y[]
2. increment y
3. evaluate exp
4. store value in x[exp]