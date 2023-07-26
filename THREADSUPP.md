# Thread support functions
Note: These are not built in functions, see the module tools/thread.m

These functions cater for the 3 most important things when it comes to
thread creation: synchronisation, error handling, argument/environment
passing. They also provide a neat interface that should be quite portable
atleast across Amiga-like OSes. They do not involve themselves in any way
with what is done after the synchronised creation is done. This is all
up to the programmer.

---
`process, r := newProcess(proc,pri,name,arg,stack=NIL,tags=NIL)`

   This function starts a new asynchronous process.

   "proc" is address of the procedure that will be run as a process.

   "pri" is the priority (-128..127).

   "name" is the name of the process.

   "arg" is always passed as argument to "proc", put whatever you please here.

   "stack" is the desired stacksize. it defaults to 16000 bytes for powerpc,
   10000 bytes for 68k.

   "tags" is additional tags for dos.library/CreateNewProc(), if needed.

   On failure, "process" will be NIL, and "r" will contain the error ("SIG" for no
   signals, "PROC" for CreateNewProc() failure), other errors may come from the
   new process returning them.
---
`releaseSuccess(private,return=NIL)`

   If nothing went wrong, the newProcess() function will put the
   calling process to sleep until the new process calls the
   releaseSuccess() function or returns an error code.
   This allows the new process to make initialisations and
   inform mother about its success.

   "private" is a private argument to "proc".

   "return" can be anything and will be returned as "r" from the
   newProcess() call on success.

   The thread-procedure should look something like this:
   ```
      PROC myThread(private, arg)

         ... make initialisations ...

         IF all_okay
            releaseSuccess(private)
         ELSE
            RETURN error -> "error" ends up in "r"
         ENDIF

         ... do stuff here ...

      ENDPROC NIL -> Thread should ALWAYS return NIL when exiting normally !
   ```
   Note: "private" really is private, do not assume anything about it.

   For an example of use, see E:Source/MUI/Subtask.e
---
`success := newEnvironment()`

   This function is for use in new processes (created with
   newProcess()) only. It will allocate a new global environment,
   initialise it, and replace the current one. A good place to call
   it is before releaseSuccess(). Creating a new environment is
   normally not needed. It might be useful to allow exception
   handling and use of built-in memory allocation functions like New(),
   etc.

   "success" will be TRUE if memory could be allocated, FALSE if not.

   wbmessage, arg, stdin, stdout, etc, will all be NIL !

   execbase,dosbase,gfxbase,intuitionbase will be copied
   from previous environment.

   Global arrays/lists/strings will be NIL, allocate them
   yourself if needed.

   Memory can be allocated with memoryfunctions as usual,
   and will be deallocated when the process ends.

   Exception-handling will work, but as with libraries,
   never pass an exception out the window :)

   FreeStack() will work.

   Following float support functions will work:
   Fabs(),Ffloor(),Fceil(),Fsin(),Fsqrt(), Fcos(),
   Fexp(),RealF(),RealVal().
   More may be made to work in future.

   As stdout is NIL, WriteF()/PrintF() will open a console,
   unless the stdout variable has been set to a valid filehandle.

   Does not work in librarymode.