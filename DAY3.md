# Day3: Digital Logic with TL-Verilog and Makerchip

## **D3SK1 - Combinational logic in TL-Verilog using Makerchip**

### **RV_D3SK1_L0_Welcome**

- Introduction to TL-Verilog on Makerchip for coding TL-Verilog and do built-in visualization.

---

### **RV_D3SK1_L1_Introduction to Logic Gates**

1. Learning about Logic gates ⇒ The fundamental logical blocks of the circuit primarily 
{ NOT, AND, OR, NAND, NOR, XOR, XNOR }

![Screenshot_2025-05-31_062422](https://github.com/user-attachments/assets/c05cd419-47e8-4cf8-9ddc-331f0b534d27)


1. Then moving on to the basic combinational circuit ⇒ “1’b FULL ADDER ” 

- INPUTS: A, B, carry  & OUTPUTS : Sum and Cout

![Screenshot_2025-05-31_062502](https://github.com/user-attachments/assets/6e1b1b37-2352-40dd-912d-34cb65bf7646)


![Screenshot_2025-05-31_062534](https://github.com/user-attachments/assets/269da909-2dfe-4dc6-9540-3379f9cae84b)


1. Basic Boolean operators and their symbolic representaion in various types 

![Screenshot_2025-05-31_062610](https://github.com/user-attachments/assets/44cd5b21-a21d-4e99-ae84-b72644b3f16a)


---

### **RV_D3SK1_L2_Basic Mux Implementation And Introduction To Makerchip**

The lecture2 briefs about the Multiplexer (MUX) 

- below code for mux is based on sign ternary operator

![Screenshot_2025-05-31_063643](https://github.com/user-attachments/assets/5835269c-90cd-4137-b1aa-487dd26033b8)


```verilog
assign f=s? x1: x2;
```

- the interesting thing is there are 6 reasonable ways to code a MUX

![Screenshot_2025-05-31_063818](https://github.com/user-attachments/assets/d23b8bda-f016-41d1-841e-9f7021babdef)


```verilog
assign f= sel[0] ? a: (sel[1] ?b: (sel[2] ? c: d));
```

- equivalently

```verilog
assign f= 
	sel[0]
		?a:
	sel[1]
		?b:
	sel[2]
		? c:
		d;
```

- Makerchip offers an integrated TL-Verilog environment with instant compile, simulation, waveform view, and visual block diagrams.
- Example : Pipelined FPGA multiplier
    
    ![Screenshot_2025-05-31_065205](https://github.com/user-attachments/assets/b3d7f579-b72c-4da5-8ca7-53441fc027bd)

    

---

### **RV_D3SK1_L3_Labs for Combinational Logic**

1. INVERTER

![Screenshot_2025-05-31_065829](https://github.com/user-attachments/assets/6164f3a1-c81c-449a-bbfd-bfa9ddf9448e)


1. VECTORS
- 4bit VECTOR

![Screenshot_2025-05-31_070040](https://github.com/user-attachments/assets/9f2b68c2-5781-469a-9f7a-40d4268d6762)


- 8bit VECTOR


![442005752-1d524faf-668a-415c-b396-4ea50fac9d3c](https://github.com/user-attachments/assets/5a271564-e90a-49c6-8eb8-cdafa61b3c89)

1. MULTIPLEXER (MUX)

![Screenshot_2025-05-31_070141](https://github.com/user-attachments/assets/4648e5b2-9768-40c3-a0d3-70e014f43faf)


1. Arithmetic Logic Unit(ALU)

![442006670-e88e0e93-fda3-4d5c-ba5e-2b9ddc76a83c](https://github.com/user-attachments/assets/078a53c5-706b-4681-bee5-df8023a61ca1)


---

## **D3SK2 - Sequential logic**

### **RV_D3SK2_L1_Introduction To Sequential Logic And Counter Lab**

1. D-FLIP_FLOPS

![image](https://github.com/user-attachments/assets/2507ed71-db17-416e-96bb-80b7021cd89e)

1. Sequential Logic- Fibonacci Series

![image (1)](https://github.com/user-attachments/assets/185d3fba-46ef-464f-ae24-7c4531b920c3)


1. **Free-Running Counter**: Increments a register each clock cycle.

    
    ![Screenshot 2025-05-31 123304](https://github.com/user-attachments/assets/ad7cc450-b8e4-465b-8e27-326bb15ff88e)

    

---

### **RV_D3SK2_L2_Sequential Calculator Lab**

![Screenshot 2025-05-31 123536](https://github.com/user-attachments/assets/2ba4b1d0-f5a3-41dd-8523-56ffe7e60bc4)


![image](https://github.com/user-attachments/assets/a03cb629-7ce9-42a9-b3e6-0e65140ae947)


---

## **RV-D3SK3 – Pipelined Logic**

### **RV_D3SK3_L1_Timing**

- **Pipelined logic** is a design approach where a process is broken down into sequential stages that operate concurrently, like an assembly line
- 
    
    ![Screenshot 2025-05-31 131452](https://github.com/user-attachments/assets/e2b045a2-0b63-403f-9259-a3a9e285d5c0)


- 
    
    ![Screenshot 2025-05-31 131501](https://github.com/user-attachments/assets/d4a777a7-b3f7-493b-aaca-d82db21feda4)

    

```verilog
|calc 
@1 
$aa_sq[31:0] = $aa $aa; 
$bb_sq[31:0] = $bb * $bb; 
@2 
$cc_sq[31:0] = $aa_sq + $bb_sq; 
@3 
$cc [31] = sqrt($cc_sq);
```

- VERILOG VS TL-VERILOG
- comparing the Verilog the TL-Verilog is more faster and efficient

![Screenshot 2025-05-31 131515](https://github.com/user-attachments/assets/e4aa90e5-3294-41ca-b1cc-81ae90d0f2cc)


Retiming :

![image](https://github.com/user-attachments/assets/80affa31-f7b0-42bf-ac82-86aad0b1170f)


Way1:

```verilog
**|calc 
@1 
$aa_sq[31:0] = $aa $aa; 
$bb_sq[31:0] = $bb $bb; 
@2 
$cc_sq[31:0] = $aa_sq + $bb_sq; 
@3 
$cc [31:0] = sqrt($cc_sq);** 

```

Way2:

```verilog
**|calc 
@1 
$aa_sq[31:0] = $aa $aa; 
@2
$bb_sq[31:0] = $bb $bb; 
@3 
$cc_sq[31:0] = $aa_sq + $bb_sq; 
@4 
$cc [31:0] = sqrt($cc_sq**
```

---

### **RV_D3SK3_L2_Pipeline Logic Advantages and Demo in Platform**

- In High frequency  we could send more data

![image](https://github.com/user-attachments/assets/b02a5a70-4b14-46aa-aea6-af84249ca783)


Advantages of TL-Verilog


![442014310-1a2ecd34-c1c4-4345-abba-aa27083da1ba](https://github.com/user-attachments/assets/7786a4a2-b64d-44b7-8be3-267e5722468e)

---

### **RV_D3SK3_L3_Lab On Error Conditions Within Computation Pipeline**

`$pipe_signal` tokens, validity of identifiers, and timing-abstract error detection.

![442014776-ac7a1bb6-131e-4f3e-bfc5-a3f96452e064](https://github.com/user-attachments/assets/06f820aa-99c3-4082-9aec-f6cec96a1145)


Fibonacci Series in a Pipeline

![image](https://github.com/user-attachments/assets/9bb37b73-7773-4d2f-8814-fb231385b6dc)


---

### **RV_D3SK3_L3_Lab Cycle Calculator**

Cycle Calculator 

Circuit Diagram :

![Screenshot 2025-05-31 140001](https://github.com/user-attachments/assets/e2ebbe32-7834-495d-ba64-67a261c74b7c)


![image](https://github.com/user-attachments/assets/d45ac2b9-2ca4-4fae-a52f-7021bbfc55a6)


---

## **RV-D3SK4 – Validity**

### **RV_D3SK4_L1_Introduction to Validity and Its Advantages**

1. Validating Functionality in TL-Verilog 

EXAMPLE:

```verilog
**|calc 
@1
$valid= ....;
?$valid 
	@1
	$aa_sq[31:0] = $aa $aa; 
	$bb_sq[31:0] = $bb $bb; 
	@2 
	$cc_sq[31:0] = $aa_sq + $bb_sq; 
	@3 
	$cc [31:0] = sqrt($cc_sq);** 
```

1. Clock Gating

![Screenshot 2025-05-31 140757](https://github.com/user-attachments/assets/92836984-c579-47f6-97d8-949f225aefee)

---

### **RV_D3SK4_L2_Lab on Validity and Valid-When Condition**

- Add validity flags to the code

```verilog
|calc 
 
?valid 

// Pythagoras's Theorem 

$aa sq[31:0] $aa[3:0] Saa 
$bb_sq[31:0] $bb [3:0] Sbb; 

@2 

$cc_sq[31:0] $aa_sq+ $bb_sq; 

@3
$cc[31:0] sqrt(Scc_sq); 
```

![442017880-1e28e158-07c8-4d6b-be19-5150e04981b5](https://github.com/user-attachments/assets/f3636036-04c1-41e2-bdd4-bf50e4ba10cd)


---

### **RV_D3SK4_L3_Lab to Compute Total Distance**

- Chain valid transactions by multiple stage computation

```verilog
|calc 
@1 
$reset *reset; 
$valid $reset? 8: (>>1$valid + 1); 
$valid_or_reset = $valid || $reset; 
$val1 [31:0] = >>2$out [31:0]; 
$val2 [31:0] = $rand2 [3:0]; 
$op [2:0] = $rand3 [2:0]; 
?$valid_or_reset 
01 
$sum [31:0] = $val1 + $val2; 
$diff [31:0] = $val1 -$val2; 
$prod [31:0] = $val1 $val2; 
$quot[31:0] = $val1 / $val2; 
2 
//mem operation 
$mem [31:0] = $reset?' ($op 3'b101) ? (>>2$out): (>>2$mem); 
$recall[31:0] = >>2$mem; 
$ temp[31:0] = $op [2] ? (($op [1:0] == 2'b00) ? $recall: '0): 
($op[1]? ($op [8] ? Squot: $prod): ($op [0] ? $diff: $sum)); 
@3 
Sout [31:0] = $reset? 0: Stemp;
```

![442018004-07ff7eca-3751-4a66-8881-b7f232afbdcf](https://github.com/user-attachments/assets/7fb7812b-4b01-4b12-bd1f-c13323139197)


---

### **RV_D3SK4_L4_Lab on 2-Cycle Calculator with Validity**

- pipeline combining for great calculator

```verilog
|calc 
@1 
$reset *reset; 
$val2 [31:0] = $rand2 [3:0]; 
Svalid $reset? 0 (>>1$valid + 1); 
$valid_or_reset = $valid || $reset; 
?Svalid_or_reset 
@1 
//init input values with 4bit random numbers 
$val1 [31:0] >>2$out [31:0]; 
$val2[31:0] = $rand2 [3:08]; 
// arithmetic operations 
//sum 
$sum[31:0] = $val1 + $val2; 
//diff 
$diff [31:0] = $val1 $val2; 
//prod 
$prod [31:0] = $val1 $val2; 
//division 
$quot[31:0] = $val1 / $val2; 
// single bit counter for validation 
$valid $reset? 8 (>>1$valid + 1); 
@2 
$mem[31:0] = ($op [1]) ? ((Sop[0]) ? $quot[31:0]: $prod [31:0]) : (($op[0]) ? $diff [31:0]: $sum [31:0] 
// validity check 
$sel_sig $reset | (!$valid); 
$out [31:0] = $sel_sig? 32'de: $mem;
```

![442018060-5609ea7c-3029-46b6-a3b3-a28db196d6f5](https://github.com/user-attachments/assets/8f637b06-88dc-42c5-a5d6-fdde9a3691c1)


---

### **RV_D3SK4_L5_Calculator Single Value Memory Lab**

- Add a memory element for storing purposes

```verilog
|calc 
@1 
$reset *reset; 
$valid $reset? 8: (>>1$valid + 1); 
$valid_or_reset = $valid || $reset; 
$val1 [31:0] = >>2$out [31:0]; 
$val2 [31:0] = $rand2 [3:0]; 
$op [2:0] = $rand3 [2:0]; 
?$valid_or_reset 
01 
$sum [31:0] = $val1 + $val2; 
$diff [31:0] = $val1 -$val2; 
$prod [31:0] = $val1 $val2; 
$quot[31:0] = $val1 / $val2; 
2 
//mem operation 
$mem [31:0] = $reset?' ($op 3'b101) ? (>>2$out): (>>2$mem); 
$recall[31:0] = >>2$mem; 
$ temp[31:0] = $op [2] ? (($op [1:0] == 2'b00) ? $recall: '0): 
($op[1]? ($op [8] ? Squot: $prod): ($op [0] ? $diff: $sum)); 
@3 
Sout [31:0] = $reset? 0: Stemp;
```

![442018171-38168f07-58b6-40aa-86f3-dd9f43bdba5b](https://github.com/user-attachments/assets/0b0ca283-3182-4307-b115-71265887197b)


---
