# Predefined preprocessor symbols
These are all new with ECX.

`__AMIGADATE__`

- this is set to current date as '(DD.MM.YYYY)', useful with version strings.

```
#define __AMIGADATE__ '(08.01.2008)'

verstr:
   CHAR '$VER: Blala 1.0 ', __AMIGADATE__, 0
```

`__DATE__`
- this is set to the current date as 'DD-Mmm-YYYY'
`#define __DATE__ '08-Jan-2008'`

`__TIME__`
- this is set to the current time as 'HH:MM:SS'
`#define __TIME__ '19:45:13'`

`__TARGET__`
-  this symbol is set to a name describing the current target, as an immediate 
   string. Example:
   `WriteF('\s\n', __TARGET__) -> MorphOS,PPC`

`__AMIGAOS__`, `__AMIGAOS4__`, `__MORPHOS__`
-  one of these symbols will be set for respective targets operating system.

`__M68K__`, `__PPC__`
-  one of these symbols will be set for respective targets CPU.

`ECX_VERSION`
- this is set to the current version of ECX.
   ```
   46 = v1.2
   47 = v1.3
   48 = v1.4
   49 = v1.4.x
   50 = v1.5
   ```
   This is mainly just to check if compiled with ECX.
   ```
          #ifdef ECX_VERSION
             ..do super cool ecx stuff..
          #endif
   ```