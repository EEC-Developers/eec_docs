# Installation
Unpack archive (ecx.lha) to somewhere on your harddrive.

ECX needs the assign "ecxmodules:" to find binary modules.
> ASSIGN ECXMODULES: ECX:modules ECX:moremods

ECX:moremods is for putting third party (or your own) modules in.

The emodules: dir should not be part of ecxmodules: assign anymore, it is 
handled in the environment variables now.

> setenv ecx-amigaos-dir emodules: ecxmodules: ecxmodules:amigaos/
> setenv ecx-morphos-dir emodules: ecxmodules: ecxmodules:morphos/
> setenv ecx-amigaos4-dir emodules: ecxmodules: ecxmodules:amigaos4/
> COPY env:ecx#? envarc:

You probably want to add a PATH also:

> PATH ECX:bin ADD

## The files in the ecx archive:
```
Modules (dir)        -> ecx binary modules here
   amigaos (dir)     -> amigaos 3.x modules
   amigaos4 (dir)    -> amigaos 4.x modules
   ecx (dir)         -> ecx private modules (startups etc)
   graphics (dir)    -> just contains a dummy gfxmacros.m
   morphos (dir)     -> morphos modules
   powerpc (dir)     -> powerpc macros etc

Bin (dir)            -> executable files and tools drawer

Docs (dir)           -> documentation drawer
   ecx.guide         -> this document

Source (dir)         -> various sources
   #? (dir)          -> various directories


Do not put modules for EC/CreativE in ecxmodules:, put them as usual in 
emodules:.