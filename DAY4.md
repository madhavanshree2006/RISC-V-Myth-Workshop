# #Day 4: Basic RISC-V CPU Micro-architecture

## **RV-D4SK1 – Introduction to Simple RISC-V Micro-architecture**

### **RV_D4SK1_L1_Micro-architecture of Single-Cycle RISC-V CPU**

- single-cycle RISC-V CPU → instruction completes in one clock cycle—fetch, decode, execute, memory access, and write-back stages all occur in parallel hardware structures.
    
    ![Screenshot 2025-06-02 175712](https://github.com/user-attachments/assets/d0de2064-c3bb-42cf-b131-f1b6e92f3646)

    

---

### **RV_D4SK1_L2_Starting Point Code for RISC-V Labs Part-1**

![442161811-89e1374e-b63a-4095-8aa5-939adeb64a81](https://github.com/user-attachments/assets/313495c0-e17a-4df3-adad-af1f6256f1da)


### **RV_D4SK1_L3_Starting Point Code for RISC-V Labs Part-2**

![442162293-9bd74bce-7256-4c22-95f5-86754597b017](https://github.com/user-attachments/assets/130cabeb-17b0-44f8-bd03-ecba1b7be259)

![442162713-44487020-979f-4523-80ac-30222aac77e2](https://github.com/user-attachments/assets/688c5365-10d1-47a6-80a4-82e9ff51fabf)

![442162786-1bedb11b-e70c-4bc7-aa9f-40d84ba63ba6](https://github.com/user-attachments/assets/80bd7f1c-afb6-422e-9282-810c657c1e4a)


---

## **RV-D4SK2 – Fetch and Decode**

### **RV_D4SK2_L1_Implementation Plan and Lab for PC**

![image](https://github.com/user-attachments/assets/69c3e79d-0da2-4374-bc8d-77a86e14137f)


- logic to update PC ← PC + 4 or PC ← target on branch.
    
    
    ![442163732-b959ad28-1479-4b71-ac8f-b38aca0b59f5](https://github.com/user-attachments/assets/261154e2-cb3b-4c8b-88af-a3f801dc0233)


---

### **RV_D4SK2_L2_Lab for Instruction Fetch Logic**

- Verifying  for each clock, the CPU reads the correct 32-bit instruction from address PC.
- FETCH-PART1
    
    ![Screenshot 2025-06-02 184242](https://github.com/user-attachments/assets/6d3962ce-825f-4f0d-b410-0f9c9e7e33b4)
    
- FETCH-PART3

![image](https://github.com/user-attachments/assets/fa49c6f0-0722-478d-853f-b975db79993c)

![442164095-13548797-d011-40b5-b70d-1bef45898eb7](https://github.com/user-attachments/assets/0e1126bc-a5e0-4364-bb60-b02e01830a1f)


---

### **RV_D4SK2_L3_Lab For RV Instruction Types IRSBJU Decode Logic**

Implement the top-level decode:

- Extract opcode[6:0] and funct3[14:12].
- Classify instructions into R, I, S, B, U, or J types.
- Route fields to the appropriate immediate-generator or register-address modules.

- Instruction Types Decode

![image](https://github.com/user-attachments/assets/c3a7344f-fcea-449b-8b9a-c442dc044db8)


![442163732-b959ad28-1479-4b71-ac8f-b38aca0b59f5](https://github.com/user-attachments/assets/323d4cb8-6e4a-4ecf-af98-03e1d6075b70)


---

### **RV_D4SK2_L4_Lab for Instruction Immediate Decode Logic for RV-ISBUJ**

- Sign-extend I-type immediates.
- Assemble S/B/U/J immediates by concatenating instruction bits.
- Test each format against known encodings.

![image](https://github.com/user-attachments/assets/7cead930-8b41-4146-ba2b-2bee0f61611b)


![442167057-2103904b-8855-4615-a727-d5f5319ac213](https://github.com/user-attachments/assets/4ef0cb1f-daf3-4c7d-8001-eb9380b856da)


---

### **RV_D4SK2_L5_Lab to Decode Other Fields of Instructions for RV-ISBUJ**

- Source registers rs1/rs2 from bits [19:15]/[24:20].
- Destination register rd from bits [11:7].
- funct7[31:25] for R-type ALU sub-operations.

![442165138-840cc2a3-0339-49ee-a703-296aa94f054e](https://github.com/user-attachments/assets/ae0dcf74-db5c-4bdb-8961-2ebee1927a0c)


---

### **RV_D4SK2_L6_Lab to Decode Instruction Field Based on Instr Type RV-ISBUJ**

- immediate-generator and register-field decoders into a unified decode module that outputs a full control word and operand indices for the datapath.
    
    ![442165514-52f76d72-1ecd-4504-8a08-c089cde31f4a](https://github.com/user-attachments/assets/8eccb452-e2ee-4900-b8c9-ed1b2f134b96)
    

---

### **RV_D4SK2_L7_Lab to Decode Individual Instruction**

- ALU operation select.
- MemRead/MemWrite enable.
- Branch condition signals.

![442167273-6c3b03f0-3de2-4bb2-9d7a-15ac1d48861f](https://github.com/user-attachments/assets/d2759382-522e-486d-8480-aa0fe32d4026)

---

## **RV-D4SK3 – RISC-V Control Logic**

### **RV_D4SK3_L1_Lab for Register File Read Part-1 (USE UPDATED SHELL CODE)**

- Two read addresses (rs1, rs2) → two read data outputs.
- Connect these outputs to the ALU inputs.

- Register  file Read

![image](https://github.com/user-attachments/assets/fcb8b93c-ab70-4ccd-9022-758af7e2653b)

---

### **RV_D4SK3_L2_Lab for Register File Read Part-2**

![442168051-0c30eb5e-f4ba-4408-b39a-178c8be527d3](https://github.com/user-attachments/assets/1218155d-78c9-4538-99c5-1c7f4e5e793c)

---

### **RV_D4SK3_L3_Lab for ALU Operations for ADD/ADDI**

Implement ALU operations:

- R-type ADD: operand1 + operand2.
- I-type ADDI: operand1 + immediate.

![image](https://github.com/user-attachments/assets/3f29ffe7-26e7-490e-8876-146ba4a4f409)

![442168328-b8665257-22fe-44cb-84f5-283166f74aec](https://github.com/user-attachments/assets/fbfa0b33-6f00-4c27-b252-bb5205f8a29a)

---

### **RV_D4SK3_L4_Lab for Register File Write**

- ALU result and memory data back into the register file’s write port (rd) when RegWrite is asserted.

![image](https://github.com/user-attachments/assets/810a1f0e-f88f-4d14-8102-12712181f7d2)

---

### **RV_D4SK3_L5_Concept of Array and Register File Details**

- Register file as an array of 32 64-bit registers in RV64, each indexed by a 5-bit address; examine how x0 is hardwired to zero.

![Screenshot 2025-06-02 200039](https://github.com/user-attachments/assets/2c539c6d-fa97-4a76-8f72-dcbb3f11e2dd)

![Screenshot 2025-06-02 200059](https://github.com/user-attachments/assets/17ee4bea-f7c0-4eec-93e6-2000e20fd6a9)

---

### **RV_D4SK3_L6_Lab for Implementing Branch Instructions**

- branch comparators (BEQ, BNE, BLT, BGE) by comparing read data and generating a branch-taken signal to redirect PC.

![image](https://github.com/user-attachments/assets/cc01afab-15e6-4239-a7a5-e9135620fb5f)

---

### **RV_D4SK3_L7_Lab for Completing Branch Instruction Implementation**

- **Connect the branch-taken signal to PC update logic; test with simple loop programs to verify correct branching.**
    
    ![442168622-2ec0cb3c-78b5-4a3d-88e4-05e04e0a34f4 (1)](https://github.com/user-attachments/assets/ae522f7e-df25-4a3f-8091-3bb4099e0208)
    

---

### **RV_D4SK3_L8_Lab to Create Simple Testbench**

1. Instantiates your CPU top module.
2. Drives a clock and reset.
3. Loads a small instruction memory.
4. Asserts pass/fail based on register file contents after program execution.

![442169102-a98236c4-ab7e-45d9-842e-b132270dd6ab](https://github.com/user-attachments/assets/2836ea5b-6b8c-4d4f-acfa-d16621d6b948)


---
