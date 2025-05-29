# Day 2: Introduction to ABI and Basic Verification Flow

## **RV-D2SK1 - Application Binary Interface (ABI)**

### **RV-D2SK1_L1: Introduction to ABI**

An **Application Binary Interface (ABI)** **defines the low-level conventions for how compiled programs and their components (like libraries) interact at the binary level**. It's essentially a contract that ensures different parts of a system can be built independently and then seamlessly work together.

- Calling conventions
- Register usage and conventions.
- Memory layout and data type sizes.

Software to hardware flow :

Applications Layer → Standard Libraries → Operating System (OS) → Instruction Set Architecture (RISC-V/ARM/x86) → Hardware (RTL)

![Screenshot_2025-05-29_000957](https://github.com/user-attachments/assets/1c968e25-4097-403c-9e68-fca35ed4fb9e)


RISC-V provides **32 integer registers** accessed via ABI names.

- **XLEN** determines register width: 32-bit for RV32, 64-bit for RV64.

### **RV-D2SK1_L2: Memory Allocation for Double Words**

- There are only 32 registers available in RISC-V architecture
- On RV64 (XLEN=64), double words (64-bit values) can be:
    
            1.   Stored directly in a 64-bit register.
    
    1. Stored in memory across eight consecutive bytes (`m[0]` holds LSB, `m[7]` holds MSB in little-endian order).

![Screenshot_2025-05-29_011236](https://github.com/user-attachments/assets/309b7c3e-5945-4dbc-a415-bc7bfdfc25bd)


- Were the RISC-V is based on “**LITTELE-ENDIAN** ” memory addressing
- For an array of three double words:
    
    `Bytes 0–7   → First double word
    Bytes 8–15  → Second double word
    Bytes 16–23 → Third double word`
    

### **RV-D2SK1_L3: Load, Add, and Store Instructions**

- We need the address of the memory for loading Data into the Register
- Instruction for loading :

```bash
id x8,16(x23) --> //load doubleword//
```

**Register-Register Add (R-type)**

```
add x8, x24, x8
```

| **funct7 [31:25]** | **rs2 (x8) [24:20]** | **rs1 (x24) [19:15]** | **funct3 [14:12]** | **rd (x8) [11:7]** | **opcode (add) [6:0]** |
| --- | --- | --- | --- | --- | --- |

**Store Double Word (S-type)**

```
sd x8, 8(x23)
```

| **imm[11:5] [31:25]** | **rs2 (x8) [24:20]** | **rs1 (x23) [19:15]** | **funct3 [14:12]** | **imm[4:0] [11:7]** | **opcode (sd) [6:0]** |
| --- | --- | --- | --- | --- | --- |

**Instruction types :**

- **R-type**: Register-register (e.g., `add`).
- **I-type**: Register and immediate (e.g., `ld`).
- **S-type**: Store with register and immediate (e.g., `sd`).
- The RISC-V supports ,  5 bits for each register field (`rs1`, `rs2`, `rd`)

## **RV-D2SK2 - Lab Work: Custom Sum Algorithm in Assembly**

### **RV-D2SK2_L1: C Program with Assembly Function**

- Flow of a sum of numbers in c program
    
    ![Screenshot_2025-05-29_195349](https://github.com/user-attachments/assets/a9841119-e090-487e-8955-4d3d81289f51)

    

### **RV_D2SK2_L2_Review ASM Function Call**

- open the gedit by creating the file name 1_to_9.c

![WhatsApp_Image_2025-05-29_at_20 32 09_eef94292](https://github.com/user-attachments/assets/aa01d079-fd6f-4ce4-a4a2-af600d7b2d65)


- The C program code :

```c
#include <stdio.h>

extern int load(int a, int b);

int main() {
    int result = 0;
    int count = 9;
    result = load(0x0, count + 1);
    printf("Sum of number from 1 to %d is %d\n", count, result);
}

```

```bash
gedit load.s &
```

- By accomplishing the above bash code we would get the assembly instructions of  the C code

```nasm
.section .text
.global load
.type load, @function

load:
    add     a4, a0, zero     // Initialize sum register a4 with 0x0
    add     a2, a0, a1       // Store count of 10 in register a2. Register a1 is loaded with 0xa (decimal 10) from main
    add     a3, a0, zero     // Initialize intermediate sum register a3 by 0
loop:
    add     a4, a3, a4       // Incremental addition
    addi    a3, a3, 1        // Increment intermediate register by 1
    blt     a3, a2, loop     // If a3 is less than a2, branch to label named <loop>
    add     a0, a4, zero     // Store final result to register a0 so that it can be read by main program
    ret

```

### **RV_D2SK2_L3_Simulate New C Program With Function Call**

- Using cat command we could see the code inside the code file we wrote earlier

```nasm
cat 1_to_9.c
cat load.s 
```

![WhatsApp_Image_2025-05-29_at_20 39 01_fd6da6cf](https://github.com/user-attachments/assets/4f3e563d-2014-4533-8f4d-59e8889a4024)


```bash

# Compile C and assembly together
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o 1to9_custom.o 1to9_custom.c load.S
```

![80605c6b-a914-4601-a531-c0d8bd7e9a5a](https://github.com/user-attachments/assets/21a2177d-a0c5-45d8-9ccf-8b5ff3f46970)


```bash
# Run on Spike simulator
spike pk 1to9_custom.o
```


![440851162-9924f7f1-5bdf-445d-8944-392dfd64a932](https://github.com/user-attachments/assets/44a5a004-72ce-4321-8bb6-89de6fcf5d69)


```bash
# Disassemble and inspect
riscv64-unknown-elf-objdump -d 1to9_custom.o | less
```

![a209b553-e8fe-4116-a38e-fac3cd0f0d5e](https://github.com/user-attachments/assets/bfcaee24-6481-4c89-9cc4-5f35f2a62664)


## **RV-D2SK3 - Basic Verification Flow using Icarus Verilog (iverilog)**

### **Program On RISC-V CPU**

 we run our C-generated program on a RISC-V CPU core written in Verilog (e.g., PicoRV32) and verify its execution.

**Setup and Execution**

1. **Clone Verification Collaterals**
    
    ```bash
    git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
    cd riscv_workshop_collaterals/labs
    ```
    

        *Snippet of testbench.v*

![440849340-06d2af57-c07a-4f82-8c38-5bc9f78b5275](https://github.com/user-attachments/assets/7c8dbc90-6ae8-4b1c-adf0-21bb1ff0c4a8)


1. **Inspect and Prepare Files**
    
    ```bash
    ls -ltr
    vim picorv32.v       # RISC-V CPU core implementation
    vim testbench.v      # Testbench for simulation
    vim rv32im.sh        # Shell script to assemble and run tests
    ```
    
    *Snippet of testbench.v*
    
    ![440850513-f5525971-4207-4d8c-8984-ac41355e5f5a](https://github.com/user-attachments/assets/0ab9d996-c465-4d0f-9877-ececad1afdf5)

    
    *Snippet of rv32im.sh*
    
   ![440850907-659359d7-df33-4df7-b71c-0337c6005bdd](https://github.com/user-attachments/assets/de295fe6-fe68-495e-9a67-4f9f32cb9de6)

    
2. **Run Verification Script**
    
    ```bash
    chmod +x rv32im.sh
    ./rv32im.sh
    ```
    
    This script compiles the CPU core and testbench with `iverilog`, runs the simulation, and displays outputs for analysis.
    
![440851162-9924f7f1-5bdf-445d-8944-392dfd64a932](https://github.com/user-attachments/assets/1296edc3-f0ec-4335-ab04-30b405566572)

