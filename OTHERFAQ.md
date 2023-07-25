# Other FAQ
Q: How do i use MUI on MorphOS ?

A: As of ECX 2.0, no changes are normally needed, but see next question.

---

Q: My hookfunction is defined like this and it doesn't work:
```
PROC myhook(o, m)
   ...
   ...
ENDPROC
```

A: This is a sloppy hookfunction that just happens to work on 68K
   because the stack is used to store arguments. This does not work
   on PPC. Always use the correct # of arguments (3), even if you dont
   use all of them. (Your function will continue to work on 68K).

   `PROC myhook(h, o, m)`

---

Q: EasyGUI wont compile/work on PowerPC

A: Forget EasyGUI. It is not PowerPC compatible at all but does tricks on the 
   68k stack with arguments which cannot work on PowerPC. It is not enough to 
   fix EasyGUI itself, all applications using it must be fixed too.
   So... forget it.
