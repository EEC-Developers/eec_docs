# Debugging tips
When you find yourself having a lot of

```
#ifdef DEBUG
DebugF(...)
#endif
```

all over your source, it could be time to make things more maintainable.

```
#define DEBUG /* comment me out for no debugging */

#ifdef DEBUG
   #define DEBUGF(str,...) DebugF(str,...)
#else
   #define DEBUGF(str,...)
#endif
```

...now you can just put "DEBUGF(...)" in your source instead and save 2 lines 
each time.

Maybe you want to have debugging to standard console instead, just define 
"DEBUGF()" as "WriteF()" and so on...