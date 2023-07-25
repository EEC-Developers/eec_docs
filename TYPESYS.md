# Typesystem (32bit) 

Type                           | Size      | Reference
-------------------------------|-----------|-----------------
CHAR, BYTE                     | 1         | Value   (LONG)
INT, WORD                      | 2         | Value   (LONG)
LONG, ULONG                    | 4         | Value   (LONG)
PTR TO *                       | 4         | Value   (LONG)
DOUBLE                         | 8         | Value   (REAL)
WIDE                           | 8         | Value   (WIDE)
ARRAY OF *, LIST, STRING       | x         | Address (LONG)
