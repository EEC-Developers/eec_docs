# Value system (32bit)
The original AmigaE can be seen as having a single-value system. All 
mathematical, bitwise, comparisons, etc, operations are done on the 32bit 
LONG value. ECX allows other types of values of other sizes to coexist with 
the default 32bit LONG value.

1. The (LONG) General purpose value [default]

   LONG can hold
   Addresses: {},  `, ARRAY OF *, PTR TO *
   Single floats: FLOAT
   32bit Immediate values: 100, 99.78, -6, MY_VALUE*10, ..

   Values smaller than LONG are automatically converted to/from LONG:

   CHAR/INT  <=> LONG

2. The (DOUBLE) Floating point value

   Values smaller than DOUBLE are automatically converted to/from DOUBLE:

   LONG/99.78/-6.0/!MY_VALUE*10.0, ..  <=> DOUBLE

3. The (WIDE) 64bit integer

   Values scale WIDE <=> LONG/INT/CHAR

4. Possibly in future: VECTOR type.

   Would use intrinsics to do math/bitwise/comparison.