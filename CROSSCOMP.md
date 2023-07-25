# Cross compilation tips
Obsolete as of ECX 2.0

When you find yourself having lots of conditional compilation 
in lots of modules to select between target specific modules 
(like hooks, threads, and so on) it might be time to put all 
conditional compilation into its own module instead. Lets call 
it modules.e.  It could look something like this:

```
    OPT PREPROCESS
    OPT MODULE
    OPT EXPORT

    ->  if compiled with ec/creative we automatically set CODE_AMIGAOS
    #ifndef ECX_VERSION
    #define CODE_AMIGAOS
    #endif

    #ifdef CODE_MORPHOS
    #define m_muicustomclass 'muiabox/muicustomclass'
    #define m_ecode 'otherabox/ecode'
    #define m_installhook 'toolsabox/installhook'
    #define m_thread 'toolsabox/thread'
    #define m_boopsi 'aboxlib/boopsi'
    #define m_lists 'aboxlib/lists'
    #define m_time 'aboxlib/time'
    #define m_tasks 'aboxlib/tasks'
    #define m_random 'aboxlib/random'
    #define m_ports 'aboxlib/ports'
    #define m_io 'aboxlib/io'
    #define m_cx 'aboxlib/cx'
    #define m_argarray 'aboxlib/argarray'
    #endif

    #ifdef CODE_AMIGAOS
    #define m_muicustomclass 'mui/muicustomclass'
    #define m_ecode 'other/ecode'
    #define m_installhook 'tools/installhook'
    #define m_thread 'tools/thread'
    #define m_boopsi 'amigalib/boopsi'
    #define m_lists 'amigalib/lists'
    #define m_time 'amigalib/time'
    #define m_tasks 'amigalib/tasks'
    #define m_random 'amigalib/random'
    #define m_ports 'amigalib/ports'
    #define m_io 'amigalib/io'
    #define m_cx 'amigalib/cx'
    #define m_argarray 'amigalib/argarray'
    #endif
```

using the modules would then look something like this:

```
    OPT PREPROCESS
    MODULE '*modules'
    MODULE m_muicustomclass
    MODULE m_ecode
    MODULE m_installhook
    MODULE m_thread
    MODULE m_boopsi
    MODULE m_lists
    MODULE m_time
    MODULE m_tasks
    MODULE m_random
    MODULE m_ports
    MODULE m_io
    MODULE m_cx
    MODULE m_argarray
    ..other modules here..
```

When compiling with ecx, use the DEFINE argument to select target:

> ecx modules.e define CODE_MORPHOS
> ecx ..other modules.. <opts>
> ecx main.e <opts>