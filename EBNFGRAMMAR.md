# ECX Grammar Description (EBNF)
E Grammar Description for ECX 1.9
Corrections are welcome
Leif Salomonsson 2004-2008
```
    Extended BNF        Operator         Meaning
    ----------------------------------------------------
    unquoted words                     non-terminal symbol
    "..."                              terminal symbol
    '...'                              terminal symbol
    (...)                              grouping
    [...]                              optional symbols
    {...}                              symbols repeated zero or more times
    {...}-                           symbols repeated one or more times
    =                  in               defining symbol
    ;                  post            rule terminator
    |                  in               alternative
    ,                  in               concatenation
    -                  in               except
    *                  in               occurrences of
    (*...*)            in               comment
    ?...?                              special sequence

    *)

    (* Digits *)

    Digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;

    (* Letters *)

    LetLC = "a" | "b" | "c" | "d" | "e" | "f" | "g" |
            "h" | "i" | "j" | "k" | "l" | "m" | "n" |
            "o" | "p" | "q" | "r" | "s" | "t" | "u" |
            "v" | "w" | "x" | "y" | "z" ;

    LetUC = "A" | "B" | "C" | "D" | "E" | "F" | "G" |
            "H" | "I" | "J" | "K" | "L" | "M" | "N" |
            "O" | "P" | "Q" | "R" | "S" | "T" | "U" |
            "V" | "W" | "X" | "Y" | "Z" ;

    LetAC = LetLC | LetUC ;

    (* Values *)

    LetHex = Dig | "a" | "b" | "c" | "d" | "e" | "f" | "A" | "B" | "C" | "D" | "E" | "F" ;

    ValDec = Dig, {Dig} ;

    ValHex = "$", {LetHex}- ;

    ValBin = "%" {"0" | "1"}- ;

    ValChar = any-character-except-doublequote;

    ValStr = '"' [ValChar] [ValChar] [ValChar] [ValChar] '"' ;

    Float = [ValDec] "." {Digit} ;

    Int = ValDec | ValHex | ValBin | ValStr ;

    ImmVal = Float | Int ;

    (* Termination *)

    Sep = {";"}- ;

    Term = {NEWLINE | Sep}- ;

    MORE = "," [NEWLINE] ;

    (* Comment *)

    Comment = ("/*" {AnyThing} "*/") | ("->" {AnyThingButNewLine} NewLineOrEOF) ;

    (* Labels *)

    LabLC = ("_" | LetLC) {"_" | LetAC | Digit} ;

    LabUC = (LetUC) {"_" | LetAC | Digit} ;

    LabAC = ("_" | LetAC) {"_" | LetAC | Digit} ;

    (* Identifiers *)

    Var   = LabLC ;

    Const = LabUC ;

    Obj   = LabLC ;

    Proc  = LabLC ;

    Meth  = LabLC ;

    Memb = LabLC ;

    Ifunc = LetUC, LetLC, {LetAC} ;

    Lfunc = LabUC ;

    Define = LabAC ;

    Macro = LabUC ;

    Asm = {LetUC}- ;

    Reg = ("R" | ("F" ["P"]) | "A" | "D") {Digit}- ;

    Label = LabAC ;

    Arg = LabLC ;

    Static = LabLC ;

    (* OPT *)

    OptItem = OptName ["=" (Int | NameString)] ;

    OPT = "OPT" OptItem {MORE OptItem} Term ;

    (* MODULE *)

    MODULE = "MODULE" NameString {MORE NameString} Term ;

    (* Declarations *)

    MembMerge = "@" Memb | (["-"] Int) ;

    MembDec = Memb [MembType [MembMerge]] ;

    VarDef = Var ["=" ConstExp] VarType ;

    RaiseDef = Const | ImmVal "IF" (Lfunc "(",")") | (Ifunc "()") Compare ConstExp ;

    ArgItem = Arg ["=" ConstExp] [ArgType] ;

    ArgList =  [ArgItem {MORE ArgItem}] ;

    RValue = "LONG" | "PTR" | "DOUBLE" | "REAL"
    RValueDef = {RValue {MORE RValue}}-

    StaticExp = (String {"+" String}) | StatList ;

    StaticDef = Static "=" StaticExp ;

    StatListExp = ConstExp | String | Label | StatList ;

    StatList = "[" StatListExp {MORE StatListExp} "]" [":" (BasicType | ObjType)] ;

    DECL = ( ["EXPORT"] "OBJECT" Obj ["OF", Obj] Term
             {
                ("PRIVATE" | "PUBLIC") [Term] {MembDec {"," MembDec} Term}
             }
             "ENDOBJECT" Term

           ) |

           ( ["EXPORT"] "PROC" Proc "(" ArgList ")" ["(" RValueDef ")"] ["OF", Obj] (["HANDLE"] Term
                "DEF" VarDef {MORE VarDef} Term
                STATES
             ["EXCEPT" ["DO"] Term
                STATES]
             "ENDPROC" Returns) | ("IS" Returns) Term

           ) |

           ( ["EXPORT"] "CONST" Const = ConstExp {MORE Const = ConstExp} Term

           ) |

           ( ["EXPORT"] "SET" Const ["=" ConstExp] {MORE Const ["=" ConstExp]} Term

           ) |

           ( ["EXPORT"] "ENUM" Const ["=" ConstExp] {MORE Const ["=" ConstExp]} Term

           ) |

           ( ["EXPORT"] "DEF" VarDef {MORE VarDef} Term

           ) |

           ( ["EXPORT"] "STATIC" StaticDef {MORE StaticDef} Term

           ) |

           ( "RAISE" RaiseDef {MORE RaiseDef} Term

           ) ;

    (* Inline *)

    LongItem = ConstExp | Label | ImmString ;

    INLINE = (["EXPORT"] Label ":") |

             (Asm [operands] Term) |

             (("DOUBLE" | "REAL") ConstExp {MORE ConstExp} Term) |

             (("LONG" | "PTR") LongItem {MORE LongItem} Term) |

             (("INT" | "WORD") ConstExp {MORE ConstExp} Term) |

             (("CHAR" | "BYTE") (ConstExp | String) {MORE (ConstExp | String)} Term) |

             ("INCBIN" NameString Term) ;

    (* Statements *)

    STAT = ( "IF" Exp Term
               STATES
             {"ELSEIF" Exp Term
               STATES}
             ["ELSE" Term
               STATES]
             "ENDIF" Term

           ) |

           ( "WHILE" Exp Term
                STATES | {"EXIT" Exp Term}
             "ENDWHILE" Term

           ) |

           ( "REPEAT" Term
                STATES | {"EXIT" Exp Term}
             "UNTIL" Exp Term

           ) |

           ( "FOR" Var ":=" Exp "TO" Exp ["STEP" ["-"] Int] Term
                STATES | {"EXIT" Exp Term}
             "ENDFOR" Term

           ) |

           ( "LOOP" Term,
                STATES | {"EXIT" Exp Term}
             "ENDLOOP" Term

           ) |

           ( "SELECT" Exp Term
             ({"CASE" Exp {MORE Exp} Term}
                STATES)
             ["DEFAULT" Term
                STATES]
             "ENDSELECT" Term

           ) |

           ( "SELECT" ConstExp "OF" Exp Term
             ({"CASE" ConstExp ["TO" ConstExp] | {MORE ConstExp ["TO" ConstExp]} Term}
                STATES)
             ["DEFAULT" Term
                STATES]
             "ENDSELECT" Term

           ) |

           ( SINGSTAT

           ) |

           ( INLINE

           ) ;

    SINGSTAT = (("IF" Exp "THEN" SINGSTAT ["ELSE" SINGSTAT]

                ) |

                ("WHILE" Exp "DO" SINGSTAT

                ) |

                ("FOR" Var ":=" Exp "TO" Exp ["STEP" ["-"] Int] "DO" SINGSTAT

                ) |

                ("INC" Var

                ) |

                ("DEC" Var

                ) |

                ("NEW" (Var [Deref] [Method] {MORE Var [Deref] [Method]}) |
                        (ImmList) |
                        (ImmString)

                ) |

                ("END" Var [Deref] {MORE Var [Deref]}

                ) |

                ("SUPER" Var [Deref] Method

                ) |

                ("JUMP" Label

                ) |

                ("RETURN" ExpList

                ) |

                (VarDo

                ) |

                (UniExp

                ) |

                (Var "," Var ["," Var] ["," Var] ":=" Exp

                ) |

                (Function

                )

                ("VOID" Exp

                )

               ) {"BUT" SINGSTAT} Term ;

    STATES = {STAT} ;

    (* Variables, members, index, methods *)

    Index = "[" [Exp] "]" [Select | PtrType] ;

    Select = "." Memb [Deref] ;

    Method = "." Meth Params ;

    PtrType = "::" Obj Select ;

    Deref = PtrType | Index | Select ;

    IncDec = "++" | "--" ;

    Assign = ":=" Exp ;

    VarDo = Var [Deref] ([IncDec] [Assign]) | Method ;

    VarExp = VarDo | (["NEW"] Var [Deref]) | ("SUPER" Var [Deref] Method) ;

    LabExp = Label [Deref] ;

    (* Strings *)

    StringChar = any-character-except-quote ;

    NameChar = LetAC | "_" | Digit | "/" | ":" ;

    NameString = "'" {NameChar} "'" ;

    String = "'" {StringChar} "'" ;

    ImmString = ["NEW"] String {"+" String} ;

    (* Lists *)

    ExpList = [Exp {MORE Exp}] ;

    ImmList = ["NEW"] "[" ExpList "]" ListType ;

    Params = "(" ExpList ")" ;

    Returns = ExpList ;

    (* Types *)

    BasicTypeName = "CHAR" | "BYTE" | "INT" | "WORD" | "LONG" | "FLOAT" | "DOUBLE" | "REAL" | "PTR" ;

    BasicType = ":" BasicTypeName ;

    ObjType = ":" Obj ;

    PtrType = ":" "PTR" "TO" (BasicTypeName | Obj) ;

    ArrayType = "[" ConstExp "]" ":" "ARRAY" "OF" (BasictypeName | Obj) ;

    ListType = "[" ConstExp "]" ":" "LIST" ;

    StringType = "[" ConstExp "]" ":" "STRING" ;

    ListType = "" | BasicType | ObjType ;

    FuncType = "(" {"LONG" | "PTR" | "DOUBLE" | "REAL"} ")" ["(" {"LONG" | "PTR" | "DOUBLE" | "REAL"} ")"] ;

    VarType = "" | BasicType | ObjType | PtrType | ArrayType | ListType | StringType | FuncType ;

    MembType = "" | BasicType | ObjType | PtrType | ArrayType ;

    ArgType = "" | PtrType | "LONG" | "FLOAT" | "REAL" | "DOUBLE" | "PTR" | FuncType ;

    (* Operators *)

    Math = "+" | "-" | "*" | "/" ;

    Bitwise = "AND" | "OR" | "SHL" | "SHR" ;

    Compare = "<" | ">" | "<=" | ">=" | "<>" | "=" ;

    (* ConstExp *)

    ConstVal = ["-"] Const | ImmVal | ("SIZEOF" Obj) | "STRLEN" ["!"] ;

    ConstOp = Math | Bitwise ;

    ConstExp = ["!"] ConstVal {ConstOp ConstVal} ;

    (* Expressions *)

    Function = (Var Params) | (Ifunc Params) | (Lfunc Params) | (Proc Params) ;

    LabAddr = ("{" Label "}") | Label ;

    VarAddr = "{" Var "}" ;

    IfExp = "IF" Exp "THEN" Exp "ELSE" Exp ;

    SimpleExp = VarExp |
                ConstExp |
                ImmString |
                ImmList |
                Function |
                LabAddr |
                LabExp |
                VarAddr |
                Reg ;

    ExpVal = ["-"] SimpleExp | IfExp | UniExp ["!"] ;

    ExpOp = Math | Bitwise | Compare ;

    Exp = ["`"] ["!"] ExpVal {ExpOp ExpVal} ["BUT" Exp] ;

    (* Unification *)

    UniItem = Var | ImmVal | Const | "*" | UniList ;

    UniList = ("[" [UniItem {MORE UniItem}] "]" [":" "LONG"]) ;

    UniExp = SimpleExp "<=>" UniList ;


    (* Grouping *)

    (* ..Grouping "(" ... ")" can be applied to almost any valid expression.. *)

    (* Program *)

    Program = {OPT} {MODULE} {DECL | INLINE} ;
```