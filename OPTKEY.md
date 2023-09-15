# The OPT keyword
`OPT` may be followed by:
- `MODULE` Declare source as module.
- `EXPORT` Export everything from module, except PRIVATE stuff.
- `NOWARN` Disable the output of all warnings.
- `OSVERSION` ECX 1.6.0+
    Explicitly define the minimum os version we require.
    ECX will warn if we make use of a module with a version
    number higher than the version we have requested with
    OPT OSVERSION, but only if the module in question contains
    code. Startup will fail as usual if started on a version
    of the os < OSVERSION.

    If OSVERION is not defined, ECX will automatically set it
    to the highest OSVERSION of any module used, _that contains
    code_, otherwise leave it to zero. ECX shows the osversion
    in the normal shell output, if it is > 0. Note that the
    startup may require a higher osversion than the one you
    have defined..
## Input / output / paths
- `DIR` Add another directory to the list of directories to search
    for modules in. Directory will be placed at top of list.
    Multiple directories may be specified, the right-most will
    get searched first. ex: OPT DIR = 'ecymodules: bla:urgh/'.
- `EXENAME` Tell ECX to name resulting executable/library differently.
    Filename only, does not alter directory.
    Example: `OPT EXENAME = 'myfantasticexe'`
- `MODNAME` Tell ECX to name resulting module differently.
    Filename only, does not alter directory. '.m' will get appended to name.
    Example: `OPT MODNAME = 'myfantasticmod'`
## Compatibility
- `ASM` Does nothing. For compatibility.
- `LARGE` For compatibility. does nothing.
- `FPEXP` For compatibility with CreativE. Turns on OPT ROUNDNEAR.
## Optimisation
- `REG` Does the same as NUMREGALLOC/N command line option. ex: "REG=5".
- `FREG` Like REG, but for DOUBLE vars.
## Preprocessing
- `PREPROCESS` Enable preprocessing of source.
## Targets
- `POWERPC` *DEPRECATED,OBSOLETE* Does the same as MORPHOS.
- `AMIGAOS` Compile code for AmigaOS.
- `AMIGAOS4` Compile code for AmigaOS4.
- `MORPHOS` Compile code for MorphOS.
- `NATURALALIGN` Forces natural alignment of OBJECT members, that is
    LONG at 4-byte boundaries, DOUBLE at 8-byte boundaries, etc.
- `ROUNDNEAR` Make integer to float conversion round to nearest instead of the default 
    towards zero.

## Startupcode
- `STACK` Manually set the stacksize that startupcode will allocate.
                    ex : `OPT STACK=50000`

- `MINSTARTUP` A very minimal startup code will be used.
    No libraries are opened (except execbase).
    No stdin, stdout or wbmessage.
    Built-in memory functions, exceptions, globals,
    inline lists, will work.
    Avoid using mixed code (ppc+68k).
    (Similar to CreativE's `OPT NOSTARTUP`.
    Sorry for name confusion.
    `NOSTARTUP` works different with ECX, see below).

- `NOSTARTUP` No startupcode will be used. Execution will start at the first piece of code 
    in the main source (through a jump though). Nothing is initialised at 
    this point. Immediate lists will not work, neither will any global 
    variables or exceptions.
## Other
- `NODEFMODS` Disable loading of the default modules: exec.m,dos.m,intuition.m,graphics.m