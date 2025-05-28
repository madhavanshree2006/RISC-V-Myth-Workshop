# VSD

## Day 1: Introduction to RISC-V ISA and GNU Compiler Toolchain

### RV-D1SK1 - Introduction to RISC-V Basic Keywords

### RV_D1SK1_L1⇒ Introduction

- Introduction to RISC-V Instruction Set Architecture (ISA).
- RISC-V Architecture (C code ) → Implementation-RTL ( PicoRV32 core) → Layout (qflow).
- Flow : Application layer → System software → Hardware layer.

### RV_D1SK1_L2⇒ From Apps To Hardware

- Software Flow :
    
    Applications → System Software → OS → High-level languages (C, C++, Java, VB) → Compiler → ISA → Assembler
    
- Hardware flow:
    
    RTL snippet (understands instructions like add x6, x10, ...) → Synthesized netlist → Physical design implementation.
    

- Course outline:
    1. RISC-V ISA(Part 1) 
    2. RTL & Synthesis of RISC-V CPU core (PicoRV32) (Part 2)
    3. Physical design implementation of PicoRV32 (Part 3)

### RV_D1SK1_L3 ⇒ Detailed Description of Course Content

C program examples and compilation. 

1. Integer Addition C-code

```c
#include <stdio.h>

int main()
{
    int num1, num2;
    register int sum;
    
    printf("\nEnter the Number 1 : ");
    scanf("%d", &num1);
    
    printf("\nEnter the Number 2 : ");
    scanf("%d", &num2);
    
    sum = num1 + num2;
    
    printf("\nSum of Numbers : %d", sum);
    printf("\n");
    
    return 0;
}

```

 Integer addition RISC-V instructions       

      

```nasm
0000000000000000 <main>:
   3: 00000537           lui     a0,0x0
   7: fe010113           addi    sp,sp,-32
   b: 00050613           mv      a2,a0
   f: 01813023           sd      a0,24(sp)
  13: 01013423           sd      a0,16(sp)
  17: 00000097           auipc   ra,0x0
  1b: 000080e7           jalr    ra
  1f: 00000437           lui     s0,0x0
  23: 00810513           addi    a0,sp,8
  27: 00050413           mv      s0,a0
  2b: 00000097           auipc   ra,0x0
  2f: 000080e7           jalr    ra
  33: 00000537           lui     a0,0x0
  37: 00050513           mv      a0,a0
  3b: 00000097           auipc   ra,0x0
  3f: 000080e7           jalr    ra
  43: 00c10513           addi    a0,sp,12
  47: 00050413           mv      s0,a0
  4b: 00000097           auipc   ra,0x0
  4f: 000080e7           jalr    ra
  53: 00c12783           lw      a5,12(sp)
  57: 00812583           lw      a1,8(sp)
  5b: 00058793           mv      a5,a1
  5f: 00b78533           addw    a0,a1,a5
  63: 00000097           auipc   ra,0x0
  67: 000080e7           jalr    ra
  6b: 00a00513           li      a0,10
  6f: 00000097           auipc   ra,0x0
  73: 000080e7           jalr    ra
  77: 01813083           ld      ra,24(sp)
  7b: 01013403           ld      s0,16(sp)
  7f: 02010113           addi    sp,sp,32
  83: 00008067           ret

```

1. Integer multiplication / division

```c
#include <stdio.h>

int main()
{
    int num1, num2;
    register int mul, div;

    printf("\nEnter the Number 1 : ");
    scanf("%d", &num1);

    printf("\nEnter the Number 2 : ");
    scanf("%d", &num2);

    mul = num1 * num2;
    div = num1 / num2;

    printf("\nInt multiplication of Numbers : %d", mul);
    printf("\nInt division of Numbers : %d", div);
    printf("\n");

    return 0;
}

```

 Integer multiplication / division RISC-V instructions 

   

```nasm
0000000000000003 <main>:
   3:   00000537            lui     a0,0x0
   7:   fd010113            addi    sp,sp,-48
   b:   00050613            mv      a2,a0
   f:   02113423            sd      ra,40(sp)
  13:   02813023            sd      s0,32(sp)
  17:   02912c23            sd      s1,24(sp)
  1b:   00000537            lui     a0,0x0
  1f:   00000097            auipc   ra,0x0
  23:   000080e7            jalr    ra
  27:   00810593            addi    a1,sp,8
  2b:   00000513            mv      a0,a0
  2f:   00000517            auipc   a0,0x0
  33:   000080e7            jalr    ra
  37:   00000513            mv      a0,a0
  3b:   00000517            auipc   a0,0x0
  3f:   000080e7            jalr    ra
  43:   00c10593            addi    a1,sp,12
  47:   00000513            mv      a0,a0
  4b:   00000517            auipc   a0,0x0
  4f:   000080e7            jalr    ra
  53:   00812783            lw      a5,8(sp)
  57:   00c12803            lw      a6,12(sp)
  5b:   00000537            lui     a0,0x0
  5f:   00050613            mv      a2,a0
  63:   02e488b3            mulw    a7,s1,s0
  67:   00000517            auipc   a0,0x0
  6b:   000080e7            jalr    ra
  6f:   02c4c5bb            divw    a1,s1,s0
  73:   00000537            lui     a0,0x0
  77:   00050613            mv      a2,a0
  7b:   00000517            auipc   a0,0x0
  7f:   000080e7            jalr    ra
  83:   00000517            auipc   a0,0x0
  87:   000080e7            jalr    ra
  8b:   02813083            ld      ra,40(sp)
  8f:   02013403            ld      s0,32(sp)
  93:   01813483            ld      s1,24(sp)

```

Instruction categories: 

1. Base Integer Instructions (RV64I)
2. Pseudo instructions Multiply extension (RV64M)
3. Floating-point extensions (RV64F & RV64D)
4. Application Binary Interface (ABI) registers (a0, ra, etc.).
5. Memory allocation & stack pointer usage (a1, sp, offsets).

## **RV-D1SK2 - Labwork for RISC-V Software Toolchain**

### **RV_D1SK2_L1: C Program to Compute Sum from 1 to N**

```c
#include <stdio.h>

int main() {
    int i, sum = 0, n = 5;
    for (i = 0; i <= n; i++) {
        sum += i;
    }
    printf("Sum of 1 to %d is %d", n, sum);
    return 0;
}
```

```bash
gcc sum1ton.c
./a.out
```

![Screenshot_from_2025-05-28_22-02-43](https://github.com/user-attachments/assets/8f0af3b2-b587-49c8-92ed-9076645f5868)


### 

## **RV_D1SK2_L2: RISC-V GCC Compile and Disassemble**

```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

Risc-v compilation 

```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1_to_n.o sum1_to_n.c
riscv64-unknown-elf-objdump -d sum1ton.o | less

```

![WhatsApp_Image_2025-05-28_at_23 10 57_5b10a76e](https://github.com/user-attachments/assets/bf58c5f8-165d-4e33-8a12-80b7ef07e9a9)


![WhatsApp_Image_2025-05-28_at_23 10 58_fab1cdfd](https://github.com/user-attachments/assets/aff4db76-ca52-435a-a358-a4c900a7691a)


With `-Ofast` optimization:

![WhatsApp_Image_2025-05-28_at_23 10 58_1361696d](https://github.com/user-attachments/assets/2e7779eb-57a6-4f67-b7a0-1957d855918c)


```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

Now only ~12 instructions in `main()`.

Now only ~12 instructions in `main()`.

![1fffbba5-bad4-460b-b14c-beb758f98320](https://github.com/user-attachments/assets/c347beaa-1cf8-45bf-a3a6-f305ba03da2b)


### **RV_D1SK2_L3: Spike Simulation and Debug**

Run this bash code 

```bash
spike pk sum1ton.o
```

- Start Spike debugger:
    
    ```
    spike -d pk sum1ton.o
    ```
    
- Example debugger commands:
    
    ```
    :until pc 0x0100b0   # Stop before main()
    :reg a2             # View register a2
    
    ```
    
- Common instructions:
    - `lui a0, %hi(.LC1)`
    - `addi a0, a0, %lo(.LC1)`

![5556e981-18c1-4493-a37e-271b98208e19](https://github.com/user-attachments/assets/3e84a35b-f218-4f49-9d94-fba0d8c08030)


![7bd2ff16-d260-47fe-9fd8-7f1c6220ca43](https://github.com/user-attachments/assets/a8f42bfd-0b1b-4d03-ad2d-4dac92a90adc)


---

## **RV-D1SK3 - Integer Number Representation**

### **RV_D1SK3_L1: 64-bit Unsigned Numbers**

- **Double Word (64-bit):** Range 0 to 2^64 - 1.

### **RV_D1SK3_L2: 64-bit Signed Numbers**

- MSB = 0 → positive; MSB = 1 → negative.
- Two's complement procedure:
    1. Binary representation.
    2. Invert bits (1's complement).
    3. Add 1.

### **RV_D1SK3_L3: Lab for Signed and Unsigned Numbers**

| **Data Type** | **Memory (Bytes)** | **Format Specifier** |
| --- | --- | --- |
| `unsigned int` | 4 | `%u` |
| `int` | 4 | `%d` |
| `unsigned long long int` | 8 | `%llu` |
| `long long int` | 8 | `%lld` |
|  |  |  |
