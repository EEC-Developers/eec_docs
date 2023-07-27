# Tools
## Tools in the E:bin/ drawer
`ViewModule MODULENAME/A` View contents of binary module.

`ViewCache` View contents of modulecache.

`EmptyCache` Empty the modulecache. Optionally may remove modules using pattern.

`TrackHit EXENAME/A, OFFSET` Shows source and linenumber for a given offset into exe.
    ADOS Executable must have been compiled with `LINEDEBUG/S` switch.
    Example: `trackhit bla $eb8`

`DisHunk FILENAME/A, DIS/S` Amiga DOS Hunk viewer, with optional PPC-disassembly. 
    Does not really support much more than is needed to display ECX ADOS binaries `:)`

`DisELF` Show contents of ELF files, with optional PPC-disassembly.

    `FILENAME/A`  --  name of ELF file.

    `DIS/S` --  disassemble sections
    
    `HIT/N` --  offset to locate for hit-information.
    
    `SECT/K` --  view only this section (name).
    
    `LABEL/K` --  disassemble a label
        (For a more advanced tool to look at ELFs, I recommend
        "MorphOS_SDK:DevEnv/Bin/objdump")

`ModuleFromFD` Convert an .fd (function definition) file to an ecx module.
    Currently supports standard Amiga FD and MorphOS FD.
    
    `FD/A` --  the name of the FD file.
    
    `MODULEDIR/A` --  directory to place resulting module in.
    
    `VERBOSE/S` --  show some info.
    
    `BASE/K` --  use another name of librarybase.
    
    `NAMEFIX/S` --  fix case of beginning letters of functions to Xx.

`ModuleFromProto` Convert a textfile with C function prototypes into an E 
    library module. See bin/modulefromproto.readme.

`CeeModule` View module in a C friendlier way. Supports library functions, 
    objects and constants.
    
    `MODULE/A` name of module with or witout .m
    
    `LIBPROTOS/S` Normally library functions are displayed FD-style. With this
        switch they are displayed as C prototypes.

`MakeLibMod` Create a library module. Replaces old ModuleFromFD and 
    ModuleFromProto tools. No more documentation for now. It is in a bit of 
    early state, and things might change, but was used to create all the 
    library modules for OS4 and MOS2.
