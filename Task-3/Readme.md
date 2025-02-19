**RISC-V Instruction Analysis**<br>
This repository focuses on analyzing and decoding 15 distinct RISC-V instructions extracted from application code.
Each instruction is categorized based on its type (R, I, S, B, U, J) and represented by its 32-bit binary encoding.

**Overview**<br>
The primary objectives of this project are to:
Explore the various RISC-V instruction types: R, I, S, B, U, and J.
Extract and classify 15 unique RISC-V instructions from the riscv-objdump output of the application code.
Decode these instructions into their corresponding 32-bit binary representations.<br>
Instructions :                      

**1. addi sp,sp,-16**               

opcode = 0010011 (7 bits) = 0010011

Imm = -16 (12 bits) = 1111 1111 0000
   
   Source register (rs1) = sp (2, 5 bits)                 = 00010
  
   Destination register (dr)= sp (2,5 bits)               = 00010
   
   func3 = 000 (3 bits)                                   = 000

   The 32 bit code is : 0010011 | 1111 1111 0000 | 00010 | 00010 | 000
                                    
**2. sd ra,8(sp)**

opcode = 0100011 (7 bits) = 0100011
   
   Imm = 8 (12 bits)                   = 0000 0000 1000
   
   Source register (rs1) = ra (1, 5 bits)                 = 00001
   
   Base register (dr)= sp (2,5 bits)                      = 00010
   
   func3 = 011 (3 bits)                                   = 011

   The 32 bit code is : 0100011 | 0000 0000 1000 | 00001 | 00010 | 011

**3. li a5,150**

   opcode = 0010011 (7 bits)                              = 0010011
   
   Imm = 150 (12 bits)                                    = 0000 1001 0110
   
   Source register (rs1) = 0 (0, 5 bits)                  = 00000
   
   Destination register (dr)= a5 (15,5 bits)              = 01111
   
   func3 = 000 (3 bits)                                   = 000

   The 32 bit code is : 0010011 | 0000 1001 0110 | 00000 | 01111 | 000

**4. lui a2,0*3**
  
   opcode = 0110111 (7 bits)                              = 0110111
   
   Imm = 0*3 (20 bits)                                    = 0000 0000 
   0000 0000 0000 (0000 0000 0011)
   
   Destination register (dr)= a2 (12,5 bits)               = 01100

   The 32 bit code is : 0110111 | 0000 0000 0000 0000 0011 | 01100

**5. jal ra,10418 <printf>**
   
   opcode = 1101111 (7 bits)                              = 1101111
   
   Imm = 10418 (20 bits)                                  = 0001 0000 0001 0100 1000
   
   Destination register (dr)= ra (1,5 bits)               = 00001

   The 32 bit code is : 1101111 | 0001 0000 0001 0100 1000 | 00001

**6. ld ra,8(sp)**
   
   opcode = 0000011 (7 bits)                              = 0000011
   
   Imm = 8 (12 bits)                                      = 0000 0000 1000
   
   Source register (rs1) = sp (2, 5 bits)                 = 00010

 Destination register (dr)= ra (1,5 bits)               = 00001
   
   func3 = 011 (3 bits)                                   = 011

   The 32 bit code is : 0000011 | 0000 0000 1000 | 00010 | 00001 | 011

**7. mv a1,a0**
   
   opcode = 0010011 (7 bits)                              = 0010011
   
   Imm = 0 (12 bits)                                      = 0000 0000 0000
  
   Source register (rs1) = a0 (10, 5 bits)                 = 01010
   
   Destination register (dr)= a1 (11,5 bits)               = 01011
   
   func3 = 000 (3 bits)                                   = 000

   The 32 bit code is : 0010011 | 0000 0000 0000 | 01010 | 01011 | 000
   
**8. addi a2,a2,-963**
  
   opcode = 0010011 (7 bits)                              = 0010011
   
   Imm = -963 (12 bits)                                   = 1100 0011 1101
   
   Source register (rs1) = a2 (12, 5 bits)                = 01100
   
   Destination register (dr)= a2 (12,5 bits)              = 01100
   
   func3 = 000 (3 bits)                                   = 000

   The 32 bit code is : 0010011 | 1100 0011 1101 | 01100 | 01100 | 000

**9. sd s0,0(sp)**
   
   opcode = 0100011 (7 bits)                              = 0100011
   
   Imm = 0 (12 bits)                                      = 0000 0000 0000
   
   Source register (rs1) = s0 (8, 5 bits)                 = 01000
   
   Destination register (dr)= sp (2,5 bits)               = 00010
   
   func3 = 011 (3 bits)                                   = 011

   The 32 bit code is : 0100011 | 0000 0000 0000 | 01000 | 00010 | 011

**10. li a3,0**
   
   opcode = 0010011 (7 bits)                              = 0010011
   
   Imm = 0 (12 bits)                                      = 0000 0000 0000
  
   Source register (rs1) = 0 (0, 5 bits)                  = 00000
   
   Destination register (dr)= a3 (13,5 bits)              = 01101
   
   func3 = 000 (3 bits)                                   = 000

   The 32 bit code is : 0010011 | 0000 0000 0000 | 00000 | 01101 | 000

**11. jal ra,12eb4 <call_exit_procs>**
   
   opcode = 1101111 (7 bits)                              = 1101111
   
   Imm = 12eb4 (20 bits)                                  = 0001 0010 1110 1011 0100
   
   Destination register (dr)= ra (1,5 bits)               = 00001

   
   The 32 bit code is : 1101111 | 0001 0010 1110 1011 0100 | 00001

**12. addi sp,sp,16**
   
   opcode = 0010011 (7 bits)                              = 0010011
   
   Imm = 16 (12 bits)                                     = 0000 0001 0000
   
   Source register (rs1) = sp (2, 5 bits)                 = 00010
   
   Destination register (dr)= sp (2,5 bits)               = 00010
   
   func3 = 000 (3 bits)                                   = 000

   The 32 bit code is : 0010011 | 0000 0001 0000 | 00010 | 00010 | 000

**13. mv s0,a0**
   
   opcode = 0010011 (7 bits)                              = 0010011
   
   Imm = 0 (12 bits)                                      = 0000 0000 0000
   
   Source register (rs1) = a0 (10, 5 bits)                = 01010
   
   Destination register (dr)= s0 (8,5 bits)               = 01000
  
   func3 = 000 (3 bits)                                   = 000

   The 32 bit code is : 0010011 | 0000 0000 0000 | 01010 | 01000 | 000

**14. ld a5,88(a0)**
   
   opcode = 0000011 (7 bits)                              = 0000011
   
   Imm = 88 (12 bits)                                     = 0000 0101 1000
  
   Source register (rs1) = a0 (10, 5 bits)                = 01010
   
   Destination register (dr)= a5 (15,5 bits)              = 01111
   
   func3 = 011 (3 bits)                                   = 011

   The 32 bit code is : 0000011 | 0000 0101 1000 | 01010 | 01111 | 011

**15. lui a5,0*2**
   
   opcode = 0110111 (7 bits)                              = 0110111
   
   Imm = 0*2 (20 bits)                                    = 0000 0000 0000 0000 0000 (0000 0000 0010)
   
   Destination register (dr)= a5 (15,5 bits)              = 01111

   The 32 bit code is : 0110111 | 0000 0000 0000 0000 0010 | 01111

