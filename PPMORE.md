# Other preprocessor directives
`#fmtstr symbol fstr values...`
* Formats a string with values and assigns it to a symbol.
   * `fstr` is like a normal immediate string with formatting codes.
   * "values..." are the values we want to format. Two kinds of
      identifiers are legal: constants and symbols. Symbols are
      always inserted by using the \\s formatting code, all others
      formatting codes requires a constant.
   * If the body of a symbol-value starts with single quote, the quote
      and its counter part are removed before insertion. Otherwise we
      would get unwanted quotes in formatted string.

      Example:` -> let's create a version string!`
      ```
      #define PROG_NAME 'My Program'
      #define PROG_AUTH 'John Smith'
      CONST PROG_VER = 10,
         PROG_REV = 1

      #fmtstr VERSTR '$VER: \\s \\d.\\d \\s by \\s' \\
         PROG_NAME PROG_VER PROG_REV _DATE_ PROG_AUTH
      ```

      ...would result in VERSTR looking something like:
         `'$VER: My Program 10.1 (06.06.2008) by John Smith'`

      The "\\" char can be used to continue values on next line.

`#error '..error message..'`
* Will make compilation fail and output the error message. Can be useful with 
   conditional compilation.

`#warning '..warning message..'`
* Will output the warning message. Can be useful with conditional compilation.