# 680x0 inline assembler
Most of the 68020 instructions (except Bitfield, BCD, Exceptions, 
etc) are supported and a subset of addressing modes are available.

## 68k addressing modes supported
* Immediate
   #<32bit constant expression>
* Data register
   Dx
   regvar
* Address register
   Ax
* Address register indirect
   (Ax)
* Address register indirect predecrement, postincrement
   -(Ax)
   (Ax)+
* Address register indirect with offset
   100(Ax)
   BLA(Ax)
   -16(Ax)
   AddTail(Ax)
   variable
   .member(Ax:object)
* Address register indirect with index and scale
   * notes:
      ofs8 = 8bit signed, can be omitted
      using scale higher than 1 requires 68020+

      ofs8(Ax,Dx)             -> ofs8(Ax,Dx.L*1)
      ofs8(Ax,Dx.L)           -> ofs8(Ax,Dx.L*1)
      ofs8(Ax,Dx.L*scale)     -> scale = 1/2/4/8
* PC relative label
   mylabel
   mylabel(PC)
* Additionally for MOVEM instruction registerlists are supported
   MOVEM.L D2-D7/A3/A5, -(A7)
* And 68020+ double register operands for MULx.L and DIVx.L instructions.
   MULS.L D4, D5:D6

## Not supported compared to EC
* No absolute addressing
* No support for Dx.W in ofs8(Ax,Dx.W)

## Instructions supported
```
   MOVE
   MOVEA
   LEA
   NOP
   RTS
   ADD
   ADDA
   ADDI
   SUB
   SUBA
   SUBI
   ADDQ
   SUBQ
   CLR
   CMP
   CMPA
   CMPI
   EXT
   EXTB
   NEG
   AND
   ANDI
   OR
   ORI
   EOR
   EORI
   NOT
   MOVEQ
   PEA
   UNLK
   RTD
   TST
   ASL
   ASR
   LSL
   LSR
   ROL
   ROR
   ROXL
   ROXR
   SWAP
   EXG
   LINK
   BSR
   Bcc
   DBcc
   MOVEM
   MULS
   MULU
   DIVS
   DIVU
   Scc
   JSR
   JMP
   ADDX
   SUBX
   RTR
   RTE
   NEGX
   CMPM
```