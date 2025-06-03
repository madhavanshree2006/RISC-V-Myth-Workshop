Day 5: Complete Pipelined RISC-V CPU Micro-architecture
---

## **RV-D5SK1 – Pipelining the CPU**

### **RV_D5SK1_L1_Introduction to Control Flow Hazards and Read-After-Write Hazards**

- Pipeline hazards occur when instructions in different stages of the pipeline conflict. Control-flow hazards happen when a branch or jump changes the PC and speculative instructions must be invalidated. Read-after-write hazards arise when an instruction needs a result that has not yet been written back to the register file.

- Waterfall Logic Diagram

![image](https://github.com/user-attachments/assets/4d178e9f-a1b7-453b-9c83-1bdb5e0e3c41)

---

### **RV_D5SK1_L2_Lab to Create a 3-Cycle Valid Signal**

1. Define a single-cycle start pulse on reset de-assertion.
2. Shift that pulse through three registers to mark instructions valid only in those stages.

![image (1)](https://github.com/user-attachments/assets/1ea355b5-0969-4b07-95a2-1bee86fa2efe)

![442207691-57e037dc-072a-4423-b17b-7955e48ebbfb](https://github.com/user-attachments/assets/019a6697-dafa-468a-a4f8-3cc0f0547568)

---

### **RV_D5SK1_L3_Lab to Take Care Of Invalid Cycles**

- single-cycle CPU to three stages (fetch, decode, execute) using the valid signal. Ensure that when valid is low, no instruction data is processed or written back, effectively pausing the pipeline.

![image](https://github.com/user-attachments/assets/aa227bc9-e73a-4d88-817f-99a01e114992)

![442207913-d029fb86-c6fb-4b34-8522-d3a65b996549](https://github.com/user-attachments/assets/00d95133-0fa9-4a3a-9c4f-c57dde9f707a)

---

### **RV_D5SK1_L4_Lab to Distribute Logic**

Re-organize combinational logic so each stage contains only the operations needed for that cycle. For example, move immediate generation into decode and keep ALU operations purely in execute. Verify correct timing and valid propagation.

![image](https://github.com/user-attachments/assets/d872ab06-ee7e-47db-8f44-cc63d388ddfb)

![442208544-a89b2e64-8b10-4578-a317-4ec3801ad6a0](https://github.com/user-attachments/assets/d9900090-3db1-4917-bf97-b409b95c7c7d)

---

## **RV-D5SK2 – Solutions to Pipeline Hazards**

### **RV_D5SK2_L1_Lab for After-Wr Hazards**

Add bypass multiplexers from the write-back stage back to execute inputs. This lets a result be forwarded directly to the next instruction’s ALU input without waiting for the register file write.

![image](https://github.com/user-attachments/assets/04491b57-5a33-4e57-82e9-395152187323)

![image (1)](https://github.com/user-attachments/assets/eac9a628-1017-49e0-9bd6-f19cbe68a6bd)

![442209858-bd125dc5-d4b1-4497-8c95-34ad32f1bed2](https://github.com/user-attachments/assets/bb26de97-398a-4c42-9450-2d3c156fdbc2)

---

### **RV_D5SK2_L2_Lab For Branches To Correct The Branch Target Path**

- Implement branch target computation in the execute stage, then redirect PC and invalidate any speculatively fetched instructions.
- You’ll introduce a branch-taken signal and use valid gating to flush bad instructions. On taking a branch, the CPU must redirect the PC correctly and invalidate speculative instructions.
- Due to three-cycle latency in decoding, reading, and computing the target, a two-cycle penalty is unavoidable.
- The PC redirection path remains a three-cycle loop.
- Valid signal logic is updated to allow valid instructions only when prior branches are not taken.
- PC loop is updated to a 1-cycle loop for normal execution, retaining a 3-cycle loop for branch redirection.
- Simulate with the near one-instruction-per-cycle model.
- Validate the bypass and branch handling logic.

- Complete Instruction Decode

![image](https://github.com/user-attachments/assets/2a8b67a0-cbb1-47fd-88ca-a15a47c2309f)

![442213934-ab7382dd-a627-4269-979d-fb8a56af9e44](https://github.com/user-attachments/assets/85f89d62-23db-41b6-aa97-90ddff143cb7)

---

### **RV_D5SK2_L3_Lab To Complete Instruction Decode Except Fence, Ecall, Ebreak**

- decoding all RV32I base instructions except fence, ecall, and ebreak. Create an `is_load` signal to group all load variants and simplify control logic.
- Completing the remaining instruction except load(L*).

```verilog
$dec bits[10:0] = -
{$funct7[5],$funct3,$opcode}; 
$is_beq = $dec_bits ==? 
-
-
11'bx_000_1100011; 
// Until instrs are implemented, 
// quiet down the warnings. 
`BOGUS USE($is beq $is bne ...) 
```

![442214903-77f25ead-ee10-4515-a80d-e40f9ce6af48](https://github.com/user-attachments/assets/23fc6fee-e207-41a8-8396-4602d20f8036)

---

### **RV_D5SK2_L4_Lab To Code Complete ALU**

- Complete ALU instruction in TL-Verilog

```verilog
//Assign $result for other instrs 
ANDI 
$src1_value & $imm; 
ORI 
$src1_value | $imm; 
XORI 
$src1_value ^ $imm; 
ADDI 
$src1_value + $imm; 
SLLI 
$src1_value << $imm[5:0]; 
SRLI 
$src1_value >> $imm[5:0]; 
AND 
$src1_value & $src2_value; 
OR 
$src1_value | $src2_value; 
XOR 
$src1_value ^ $src2_value;

ADD 
$src1_value + $src2_value; 
SUB 
$src1_value - $src2_value; 
SLL 
$src1_value << $src2_value[4:0]; 
SRL 
$src1_value >> $src2_value[4:0]; 
SLTU 
$src1_value < $src2_value; 
SLTIU 
$src1_value < $imm; 
LUI 
($imm[31:12], 12'b0};

AUIPC 
$pc+ 
$imm: 
JAL 
$pc + 4 
JALR 
$pc + 4;

SRAI 
{ {32{$src1_value[31]}}, $src1_value} >> $imm[4:01; 
SLT 
($src1_value[31] == $src2_value[31] ) ( $sltu_rslt:{31'b0, $src1_value[31]};
SLTI  
($src1_value[31] == $imm [31]) ? $sltiu_rslt: {31'b0, $src1_value[31]}; 
SRA { {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0];
```

![442215802-5ad74783-9fb6-4227-9b89-8f5a881e9f34](https://github.com/user-attachments/assets/76c818de-c56d-4bcb-9376-fcd3c9dce22d)

---

## **RV-D5SK3 – Load/Store Instructions and Completing the RISC-V CPU**

### **RV_D5SK3_L1_Introduction to Load/Store Instructions and Lab to Redirect Loads**

- load and store instructions traverse the pipeline and introduce data-memory forwarding

![image](https://github.com/user-attachments/assets/ac89e412-5f6b-4f05-b8d4-1262e6561b01)

- LOAD flow of the instruction that we want to load

![image (1)](https://github.com/user-attachments/assets/b2aeddf2-3ae5-4c32-801c-094f033f5afd)

---

### **RV_D5SK3_L2_Lab to Load Data from Memory into the Register File**

- Wire the data-memory read port to your pipeline’s write-back stage. Ensure that load results are captured correctly and bypassed when necessary.
- use `$ld_data` to write after the valid of `$load`

![image](https://github.com/user-attachments/assets/ca3ecb22-5eee-4adb-bb1b-a431854e30b3)

---

### **RV_D5SK3_L3_Lab to Instantiate Data Memory in the CPU**

- Integrate a synchronous data RAM module, connect address, write-enable, and data signals, then validate with store and load sequences.
- let’s first uncomment `//m4+dmeme(@4)`
- after that connect interface signals to the address bits[5:2] to load and store

![image (1)](https://github.com/user-attachments/assets/2a0f63b9-29cf-4577-bed7-875bd84c6750)

---

### **RV_D5SK3_L4_Lab To Add Stores And Loads To The Test Program**

- Extend  instruction memory testbench to include store and load operations. Confirm that memory contents change and that loads read updated values.
- Modify the test program to store the final result value to address 4, then load it into x15.
`m4_asm(SW, ro, r10, 100)
m4_asm(LW, r15, r0, 100)`
- Update passing condition to look in `xreg [15]`.
- Debug. Does the loop properly fall-through and execute store/load.

---

### **RV_D5SK3_L5_Lab to Add Control Logic for Jump Instructions**

- Implement JAL and JALR by computing and writing return addresses, updating PC in the execute stage, and invalidating speculative instructions.
- JUMPS instructions
1. `JAL`  → Jump to PC + IMM
2. `jalr`->Jumps to SRC1 + IMM

---

### **RV_D5SK3_L6_Wrap Up**

- BY running  full functional test: fetch, decode, execute, memory, and write-back across multiple instructions. Verify correct behavior of arithmetic, branches, loads/stores, and jumps through your pipeline testbench.

FINAL CODE :

```verilog
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
   |cpu
      @0
         $reset = *reset;
         $start = $reset ? 0 : (>>1$reset);
         //$valid = $reset ? 0 : $start ? $start : >>3$valid ;
         //$pc[31:0] = >>1$reset ? 0 : ($pc[31:0] + 32'd4);
         $pc[31:0] = >>1$reset ? 0 : 
                     >>3$valid_taken_br ? >>3$br_tgt_pc :
                     >>3$valid_load ? >>3$incr_pc :
                     (>>3$valid_jump && >>3$is_jal) ? >>3$br_tgt_pc :
                     (>>3$valid_jump && >>3$is_jalr) ? >>3$jalr_tgt_pc :
                     >>1$incr_pc;
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
      @1
         $instr[31:0] = $imem_rd_data;
         
         $incr_pc[31:0] = $pc + 32'd4;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||  $instr[6:2] ==? 5'b001x0 || $instr[6:2] ==? 5'b11001;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b011x0 || $instr[6:2] ==? 5'b01011 || $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;

         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8],1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'h000} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} : 32'h0;
         
         // using one valid signal for rs1 , func3
         $funct3_or_rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr ;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr ;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr ;
         
         $opcode[6:0] = $instr[6:0];

         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         ?$funct3_or_rs1_valid
            $rs1[4:0] = $instr[19:15];
            
         ?$funct3_or_rs1_valid
            $funct3[2:0] = $instr[14:12];
            
         ?$is_r_instr
            $funct7[6:0] = $instr[31:25];
            
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
            
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         $is_lb    = $dec_bits ==? 11'bx_000_0000011;
         $is_lh    = $dec_bits ==? 11'bx_001_0000011;
         $is_lw    = $dec_bits ==? 11'bx_010_0000011;
         $is_lbu   = $dec_bits ==? 11'bx_100_0000011;
         $is_lhu   = $dec_bits ==? 11'bx_101_0000011;
         
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Branching
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         
         $is_add   = $dec_bits ==? 11'b0_000_0110011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         `BOGUS_USE($is_lui $is_auipc $is_jal $is_jalr $is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_lb $is_lh $is_lw $is_lbu $is_lhu $is_sb $is_sh $is_sw $is_addi $is_slti $is_sltiu $is_xori $is_ori $is_andi $is_slli $is_srli $is_srai $is_add $is_sub $is_sll $is_slt $is_sltu $is_xor $is_srl $is_sra $is_or $is_and)
         
          @2
         
         $rf_rd_en1 = $rs1_valid;
         ?$funct3_or_rs1_valid
            $rf_rd_index1[4:0] = $rs1;
            
         $src1_value[31:0] = (>>1$rf_wr_en && (>>1$rd == $rs1)) ? >>1$result : $rf_rd_data1;
         
         $rf_rd_en2 = $rs2_valid;
         ?$rs2_valid
            $rf_rd_index2[4:0] = $rs2;
            
         $src2_value[31:0] = (>>1$rf_wr_en && (>>1$rd == $rs2)) ? >>1$result : $rf_rd_data2;
         
         $br_tgt_pc[31:0] = $pc + $imm;
         
      @3 
         $sltu_rslt = $src1_value < $src2_value ;
         $sltiu_rslt = $src1_value < $imm ;
         
         $result[31:0] = 
                 $is_andi ? $src1_value & $imm :
                 $is_ori ? $src1_value | $imm :
                 $is_xori ? $src1_value ^ $imm :
                 $is_addi ? $src1_value + $imm :
                 $is_slli ? $src1_value << $imm :
                 $is_srli ? $src1_value >> $imm :
                 $is_and ? $src1_value & $src2_value :
                 $is_or ? $src1_value | $src2_value :
                 $is_xor ? $src1_value ^ $src2_value :
                 $is_add ? $src1_value + $src2_value :
                 $is_sub ? $src1_value - $src2_value :
                 $is_sll ? $src1_value << $src2_value :
                 $is_srl ? $src1_value >> $src2_value :
                 $is_sltu ? $sltu_rslt :
                 $is_sltiu ? $sltiu_rslt :
                 $is_lui ? { $imm[31:12], 12'b0 } :
                 $is_auipc ? $pc + $imm :
                 $is_jal ? $pc + 32'd4 :
                 $is_jalr ? $pc + 32'd4 :
                 $is_srai ? { {32{$src1_value[31]}}, $src1_value } >> $imm[4:0] :
                 $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                 $is_slti ? ($src1_value[31] == $imm) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                 $is_sra ? { {32{$src1_value[31]}}, $src1_value } >> $src2_value[4:0] :
                 32'bx;
         
         $rf_wr_en = ($rd_valid && ($rd[4:0] != 5'b00000) && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = !(>>2$valid_load) ? $rd : >>2$rd;
         $rf_wr_data[31:0] = !(>>2$valid_load) ? $result : >>2$ld_data;
         
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? ($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31]) :
                     $is_bge ? ($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31]) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) : 1'b0;
         
   *failed = 1'b0;
   *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)

   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
\SV
   endmodule
         
```

---

DIAGRAM:

![442227749-01304771-e93d-4935-96ea-8f776d8718a7](https://github.com/user-attachments/assets/b232d0ee-824b-4fbf-b44b-0b095cfde521)

---

WAVEFORM:

![442230015-c3cac8e6-2b9b-4d42-aee2-3b1d7c9c9557](https://github.com/user-attachments/assets/cad3e8a9-6fb7-4b36-abfd-7fdde2279c6d)


---

THANKYOU ALL ✨
