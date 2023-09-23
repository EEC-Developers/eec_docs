# Usage
## Quick usage:
`eec <sourcename> <targetname>`

example:

`eec myapp amigaos`

## Commandline parameters:

`SOURCE/A` Required, the source file name, with or without ".e".

## Targets

`POWERPC/S` *DEPRECATED,OBSOLETE* Does the same as MORPHOS/S.
`AMIGAOS/S` Compile code for AmigaOS.
`AMIGAOS4/S` Compile code for AmigaOS4.
`MORPHOS/S` Compile code for MorphOS.

## Optimisation

`REG=NUMREGALLOC/N` Automatically put often used variables in
   registers. 0..4 registers for 680x0, 0..12 for powerpc. It is safe to give 
   too big value here, EEC just tries to use as many registers as possible. 
   Using "NUMREGALLOC -1", will make EEC to decide itself how many registers 
   to use for each procedure (recommended).

`FREG=NUMFREGALLOC/N` Like above, now for DOUBLE variables. 0-12 or -1. 
   (powerpc only).

`OPTI/S` Sets NUMREGALLOC and NUMFREGALLOC to -1.

## Script related

`HOLD/S` Wait for a RETURN before exiting.
`WBTOFRONT/S` Flip the workbench screen to front.
`SHOWFNAME/S` Show the name of SOURCE file in the output.
`QUIET/S` Keep quiet unless there are warnings or errors.
`NOWARN/S` Output no warnings.
`ERRLINE/S` Return linenumber as error code to shell.

## Debugging

`NIL=NILCHECK/S` Inserts code to check for "NIL-pointers".
   If found, the following code will be executed:
   `Throw("NIL", linenum).`

`VARFILL/N` Any LONG/PTR variable without initial value will get this value. 
   Useful to flush out the kind of bugs that hides as long as uninitialised 
   variables happens to reside in cleared memory.

`SYM=SYMBOLHUNK/S` Add symbol information to the executable. Symbols that will 
   be present in the symbolhunk are: Procedures, used internal functions, 
   assembler labels, methods.

`LINE=LINEDEBUG/S` Add line and source information to the module/executable.
   Automatically turns on SYMBOLHUNK.

`VAR=VARDEBUG/S` Add debug information about variables to module/executable.
   Automatically turns on SYMBOLHUNK, LINEDEBUG.

`STEP=STEPDEBUG/S` Makes it possible to debug code with a debugger.
   Automatically turns on SYMBOLHUNK, LINEDEBUG, VARDEBUG.
   Not implemented yet.

## Preprocessor

`DEFINE/K` Define macro(s) on the commandline. Examples:
   > eec myprogg DEFINE MYSWITCH

   > eec myprogg DEFINE "MyMacroValue 100"

   Use ";" to separate multiple macros.
   
   You need "OPT PREPROCESS" in the source.

`SHOWCONDSYMS/S` Show preprocessor symbols being used in conditional
   compilation.


## Input/output paths

`DDIR=DESTDIR/K` Place resulting file(s) in another directory.
   Default is the same directory as the source-file.

`EXENAME/K` Set another name for resulting exe/lib. Default is sourcename 
   without '.e'.

`MODNAME/K` Use another name for resulting module. Also works in librarymode 
   (EXENAME and MODNAME can be combined). '.m' will get appended to name.

`DIR=MODULEDIR/K` Like OPT DIR, allows to add more directories to module 
   search path. The order in which EEC searches modules is
   1. Commandline MODULEDIR/K (right to left),
   2. `OPT DIR` (right to left)
   3. `eec-<target>-dir` environment variable (right to left)

## Other

`NODEFMODS/S` Do not automatically load the default exec.m, dos.m,
   intuition.m, graphics.m

`OUTFORMAT/K` RAW:  Outputs executable in raw format, that is, simply dumping 
   the binary opcodes of the codesection to disk. Any use of relocations will 
   give an error message (but see ABSOLUTE/S).

   `ELF` Outputs executable in ELF format (default for PowerPC)

   `ADOS` Outputs executable in ADOS format (default for 68k)

`TOADDR/N` Generates the raw executable to a specific memory-address instead 
   of writing to disk. Be SURE to have the buffer in memory big enough!

`ABSOLUTE/N` Relocates raw code into absolute code before output. Takes 
   desired address as parameter.
