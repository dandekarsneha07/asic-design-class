# asic-design-class

<details>
<summary> Assignment-1 </summary>
  
## Write C program and compile it with GCC and RISC-V compiler and compare the outputs
  
### Task A. Writing a C program and compiling with GCC compilet

#### Step 1: open leafpad or any other text editor and write the following code

-> Command to open a file in leafpad
```ruby
  leafpad sum1ton.c &
```

-> Code:
```ruby
#include <stdio.h>

int main() {
	int I, n=25, sum=0;
	for(i=0; i<=n; ++I) {
	  sum =sum + I;
	}
	
	printf(" The sum from1 to %d is &d\n" , n,sum);
	return 0;
}
```

Screenshot 1: 

![image](https://github.com/user-attachments/assets/616043ce-5442-4504-b95f-feb333a0e7bf) 

Screenshot 2:

![image](https://github.com/user-attachments/assets/ecc36ccc-88a0-42d3-b036-02fc239d1d8c)

#### Step 2: compile the code using the gcc compiler and run the executable

-> Command to compile:
```ruby
  gcc sum1ton.c 
```

-> Command to run the executable:
```ruby
  ./a.out
```

Screenshot 3:

![image](https://github.com/user-attachments/assets/16316c28-ddc2-4b96-92f1-3a4b916abb34)

### Task B. Compiling the same program with RISC-V compiler

#### Step 1: Compailing the same C programm with RISC-V compiler ans -O1 optimization

->Command to compile:
```ruby
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
-> Command to dump the output:
```ruby
riscv64-unknown-elf-objdump -d sum1ton.o | less
```
Screenshot 4:
The number of instructions taken to execute the main block can be calculated by counting each individual instruction or subtracting the address of next block and the main block.

![image](https://github.com/user-attachments/assets/08f761f5-92e2-4d4d-a833-131325baf604)

Observation : With the -O1 optimizer, the main block takes total 14 instructions to execute the command

#### Step 2: Compailing the same C programm with RISC-V compiler ans -Ofast optimization

->Command to compile:
```ruby
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
-> Command to dump the output:
```ruby
riscv64-unknown-elf-objdump -d sum1ton.o | less
```
Screenshot 5:

![image](https://github.com/user-attachments/assets/327269c0-5341-464f-8cad-a100d2807ba0)

Observation : With the -Ofast optimizer, the main block takes total 11 instructions to execute the command

</details>

<details>
<summary> Assignment-2 </summary>

## Executing and Debugging the C program with Spike Simulator

#### Step 1: Compile the C program with the RISC-V compiler with -Ofast optimizor
->Command to compile:
```ruby
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

#### Step 2: Executing the compiled file with Spike Simulator
->Command for execution:
```ruby
spike pk sum1ton.o
```

Following is the expected output

![image](https://github.com/user-attachments/assets/5599e7e3-6bf2-4888-a2c6-13dd8acad86a)

#### Step 3: Debugging using Spike Simulator
->Command to open debugger:
```ruby
spike -d pk sumton.o 
```

We will step by step execute the instructions know what changes occur in the register values after each set of instructions run.
For that one can open the dump of the object file simultaneously.

->To run the first instruction
```ruby
until pc 0 100b0 
```

The above command will run the program till the instruction having adderess 100b0. To experiment one can take any differnt adderess and check the corrosponding reg value and observe the changes.

Screenshot of executing the above command

![image](https://github.com/user-attachments/assets/1887e545-b98c-4fe3-9820-220198b35d7a)

->To check the contents of reg
```ruby
reg 0 <reg_name>
```

For eg.
```ruby
reg 0 a0  #view contents of a0 register
reg 0 sp  #view contents of stack pointer
```

To execute next set of instructions press "enter"

![image](https://github.com/user-attachments/assets/4c9fa5a9-c16e-4b3c-9049-04fa5cae9851)

</details>

<details>
<summary> Assignment-3 </summary>
	
## Understanding RISC-V instruction format and implementing exact 32-bit instruction code in the instruction type format 

RISC-V is an instruction set architecture (ISA) designed to support computer architecture research and education.

### Task A. Instruction Set format of RISC-V

Instruction set or ISA are the set of commands which a process can understand and execute. The incstructions can be arithmetic, input, output operations etc. The instructions have some specific format/guidelines, defined by a series of 0s and 1s, that includes details such as the type of operation and the location of data.
For RISC-V also, there is a specific format for representing an instruction so that it can understand and execute specific operation.

1.  R-type 
   	
	R-type i.e Register type instructions handle all arithmetic and logical operations involving three registers. The format type is as follows

 	![image](https://github.com/user-attachments/assets/acc45146-51bc-4d7a-bb4b-3c940cce685c)

	-> funct7 (7 bits):
   		Function code for additional instruction differentiation
   
	-> rs2 (5 bits):
   		Second source register
   
	-> rs1 (5 bits):
   		First source register
   
	-> funct3 (3 bits):
   		Function code for primary instruction differentiation
	
 	-> rd (5 bits):
   		Destination register
	
 	-> opcode (7 bits)
   		Specifies basic operation catagory

2. I-type

	I-type i.e immediate type instruction handle operations with immediate values. The format type is as follows

	![image](https://github.com/user-attachments/assets/6371294d-e3b4-4da7-a7f7-f4ed069fe456)

	-> immediate (12 bits):
   		Immediate value used for operations
	
 	-> rs1 (5 bits):
   		Source register
	
 	-> funct3 (3 bits):
   		Function code for instruction differentiation
	
 	-> rd (5 bits):
   		Destination register
	
 	-> opcode (7 bits):
   		Basic operation code for I-type instructions


 3. S-type
    
	S-type or store type instructions are used to store data from register to memory. The format type is as follows

	![image](https://github.com/user-attachments/assets/d774995f-750c-4383-a0d1-9da2779782f4)

	-> imm[11:5] (7 bits): Upper 7 bits of the immediate value
    
	-> rs2 (5 bits): Second source register (contains the data to be stored)
    	
	-> rs1 (5 bits): First source register (base address register)
    
	-> funct3 (3 bits): Function code for instruction differentiation
    
	-> imm[4:0] (5 bits): Lower 5 bits of the immediate value
	
 	-> opcode (7 bits): Basic operation code for S-type instructions


 4. B-Type

    B-type instructions are used for conditional branch operations. These instructions are designed to alter the flow of execution based on comparisons between two registers. The format type is as follows

    ![image](https://github.com/user-attachments/assets/7426cb97-72d0-4904-b320-edc01ba7bfac)

	-> imm[12] (1 bit): The 12th bit of the immediate value
    
	-> imm[10:5] (6 bits): The 10th to 5th bits of the immediate value
    
	-> rs2 (5 bits): Second source register
    
	-> rs1 (5 bits): First source register
    
	-> funct3 (3 bits): Function code for instruction differentiation
    
	-> imm[4:1] (4 bits): The 4th to 1st bits of the immediate value
    
	-> imm[11] (1 bit): The 11th bit of the immediate value
    
	-> opcode (7 bits): Basic operation code for B-type instructions

5. U-Type

   U-type instructions handle operations involving large immediate values, typically for loading upper immediate values or computing addresses. The format type is as follows

   ![image](https://github.com/user-attachments/assets/b005d643-9fde-4b0f-97bd-d147b1cd9fb4)

	-> immediate[31:12] (20 bits): The upper 20 bits of the immediate value
   
	-> rd (5 bits): Destination register
   
	-> opcode (7 bits): Operation code for U-type instructions


6. J-Type

   J-type i.e jump type instructions are used for altering the program control flow by jumping to a specified address. Tey are typically used for unconditional jumps such as calling functions or implementing loops. The format type is as follows

   ![image](https://github.com/user-attachments/assets/e8343ad7-b3ba-4a5d-8872-230bb94d8147)

	-> imm[20] (1 bit): The 20th bit of the immediate value
   
	-> imm[10:1] (10 bits): The 10th to 1st bits of the immediate value

	-> imm[11] (1 bit): The 11th bit of the immediate value
   
	-> imm[19:12] (8 bits): The 19th to 12th bits of the immediate value
   
	-> rd (5 bits): Destination register where the return address is stored

	-> opcode (7 bits): Operation code for J-type instructions


### Task B. Decoding each Instruction Provided

### Task C. Executing assembly instructions based on a provided Verilog code within a RISC-V processor

| Operation | Standard RISC-V ISA | Hardcoded ISA |
| --------- | ------------------- | ------------- | 
| ADD R6, R2, R1 | 32'h00110333	| 32'h02208300 |
| SUB R7, R1, R2 | 32'h402083b3	| 32'h02209380 |
| AND R8, R1, R3 | 32'h0030f433	| 32'h0230a400 |
| OR R9, R2, R5	 | 32'h005164b3	| 32'h02513480 |
| XOR R10, R1, R4 | 32'h0040c533 | 32'h0240c500 |
| S#LT R1, R2, R4 | 32'h0045a0b3 | 32'h02415580 |
| ADDI R12, R4, 5 | 32'h004120b3 | 32'h00520600 |
| BEQ R0, R0, 15  | 32'h00000f63 | 32'h00f00002 |
| SW R3, R1, 2	  | 32'h0030a123 | 32'h00209181 |
| LW R13, R1, 2	  | 32'h0020a683 | 32'h00208681 |
| SRL R16, R14, R2 | 32'h0030a123 | 32'h00271803 |
| SLL R15, R1, R2  | 32'h002097b3 | 32'h00208783 |


### Task D. Performing functional simulation of RISC-V

-> For referencing the verilog and verilog testbench 
``` ruby
git clone https://github.com/vinayrayapati/iiitb_rv32i
```

#### Step 1. Compile the Verilog Code using the below command
-> Command
``` ruby
iverilog -o iiitb_rv32i iiitb_rv32i.v iiitb_rv32i_tb.v
```

#### Step 2. Execute testbench and create .vdc file
-> Command
``` ruby
 vvp iiitb_rv32i_tb
```

#### Step 3. Opening gtkwave and plotting the waveform 
-> Command
``` ruby
 gtkwave iiitb_rv32i.vdc
```
Screenshot of gtkwave gui

![image](https://github.com/user-attachments/assets/131510a9-f646-4252-8a0d-50838d3a6f0c)


#### Waveforms 

1. ADD R6, R2, R1

![image](https://github.com/user-attachments/assets/07498efb-175d-4dcd-8211-5ce151e52efa)

2. SUB R7, R1, R2

![image](https://github.com/user-attachments/assets/5ad343a5-979f-4c55-a031-805f6c3325de)

3. AND R8, R1, R3

![image](https://github.com/user-attachments/assets/8b15a105-184b-46e4-a7db-f929f0c45c6d)

5. OR R9, R2, R5

![image](https://github.com/user-attachments/assets/2094ea53-c8b1-4aee-98bc-2b7b950a6f1f)

6. XOR R10, R1, R4

![image](https://github.com/user-attachments/assets/3dbcb959-447f-4809-9dd4-96269e754cdf)

7. SLT R1, R2, R4

![image](https://github.com/user-attachments/assets/8d24aa09-ca23-45d3-b7d5-af4effef5e1d)

8. ADDI R12, R4, R5

![image](https://github.com/user-attachments/assets/ce72d2bb-31e3-4a62-baf0-5d9c5ecbaacb)

9. BEQ R0, R0, 15

![image](https://github.com/user-attachments/assets/be03f323-d064-4014-86fd-cb34f14e8cc6)

</details>

<details>
<summary> Assignment-4 </summary>

## Writing C program, compiling it with GCC and RISC-V compiler and compairing the output

### Fibonacci Series Generation

#### What is Fibonacci Series?

Fibonacci series is a sequence where the next number in the equence is the sum of the two preceding ones in the sequence.
The first two numbers of the sequence are 0 and 1.

#### Mathematical Representation 

The first two elements of the sequence 
```
F0 = 0
F1 = 1
```
The  next elements in the sequence are given by 
```
Fn = Fn-1 + Fn-2 (n > 2)
```
For reference let's take an example and check the sequence

```
for n=5
F0 = 0
F1 = 1
F2 = F0 + F1 = 0 + 1 = 1
F3 = F2 + F1 = 1 + 1 = 2
F4 = F3 + F2 = 2 + 1 = 3
F5 = F4 + F3 = 3 + 2 = 5
```

-> Code
```ruby
#include <stdio.h>

// driver code
int main()
{

    int n = 0, num, i;
    int p1 = 1;
    int p2 = 0;

    printf("Enter number of terms want to print the sequence for: \t");
    scanf("%d", &n);

    if (n < 1) {
        printf("Invalid Number of terms\n");
    }


    for (i = 1; i <= n; i++) {
        if (i > 2) {
            num = p1 + p2;
            p2 = p1;
            p1 = num;
            printf("%d \n", num);
        }

        // for first two terms
        if (i == 1) {
            printf("%d \n", p2);
        }
        if (i == 2) {
            printf("%d \n", p1);
        }
    }

    return 0;
}

```
#### Task A. Compiling the code by GCC compiler

-> Compiling the code
```ruby
gcc fibo_series.c
```

-> Executing 
```ruby
./a.out
```

#### Task B. Compiling the code by RISC-V compiler

-> Compiling the code
```ruby
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o fibo_series.o fibo_series.c
```

-> Executing 
```ruby
spike pk fibo_series.o
```

-> Output from GCC and RISC-V compiler

![image](https://github.com/user-attachments/assets/5f37f9ce-b7e7-45ce-a53a-58b5f16ea822)


##### Observation 
The output from both the compilers is same which is expected.

</details>

<details>
<summary> Assignment-5 </summary>



<details> 
<summary> Day-3 </summary>

# Digital logic with TL-Verilog in Makerchip IDE

## Combinational Circuits

### 1. Implementation of MUX

![image](https://github.com/user-attachments/assets/9bb782bb-83de-443d-bdf4-7f926978c74b)

->Code

```ruby
$out[3:0] = $sel ? $in1[3:0] : $in2[3:0];
```
![image](https://github.com/user-attachments/assets/010d0a81-e139-464a-9c1a-9d53e98ae82b)


### 2. Combinational Calculator

![image](https://github.com/user-attachments/assets/b7855a6a-82c3-4e58-8164-8a6b2af38480)

->Code

```ruby
   $val1[31:0] = $rand1[31:0];
   $val2[31:0] = $rand2[31:0];
   $sel[1:0] = $rand3[1:0];
   
   $sum[31:0] = $val1[31:0] + $val2[31:0];
   $diff[31:0] = $val1[31:0] - $val2[31:0];
   $prod[31:0] = $val1[31:0] * $val2[31:0];
   $quot[31:0] = $val1[31:0] / $val2[31:0];
   
   $out1[31:0] = ($sel[1:0]==2'b00) ? $sum : ($sel[1:0]==2'b01) ? $diff : ($sel[1:0]==2'b10) ? $prod : $quot;

```

Output

![image](https://github.com/user-attachments/assets/bf0e3518-5d2b-4440-a920-5c964f6d723c)


## Sequential Circuits

### 1. Free-Running Counter

![image](https://github.com/user-attachments/assets/f8190c77-21f4-4614-a2d3-cc966d9559c7)

->Code

```ruby
   $count[31:0] = $reset ? 0 : (>>1$count + 1);
```

Output

![image](https://github.com/user-attachments/assets/a7ee6faf-46ea-4de1-807a-567c12b733d2)


### 2. Sequential Calculator

![image](https://github.com/user-attachments/assets/551d380f-c44c-4ef6-b3e9-69beb71741ba)

->Code

```ruby
   $val1[31:0] = >>2$out;
   $val2[31:0] = $rand2[3:0];
   $sel[1:0] = $rand3[1:0];
   
   $sum[31:0]  = $val1[31:0] + $val2[31:0];
   $diff[31:0] = $val1[31:0] - $val2[31:0];
   $prod[31:0] = $val1[31:0] * $val2[31:0];
   $quot[31:0] = $val1[31:0] / $val2[31:0];
   
   $int[31:0] = ($sel[1:0] == 2'b00) ? $sum : ($sel[1:0] == 2'b01) ? $diff : ($sel[1:0] == 2'b10) ? $prod[31:0]: $quot[31:0];
   
   $out[31:0] = $reset ? 32'h0 : $int;
```

Output

![image](https://github.com/user-attachments/assets/356bffee-026e-4410-b82d-b009d5053ce8)


## Pipeline

In Transaction-Level Verilog (TL-Verilog), pipelined logic is efficiently expressed using pipeline constructs that represent data flow across design stages, each corresponding to a clock cycle. This approach simplifies the modeling of sequential logic by automatically handling state propagation and enabling clear, concise descriptions of complex, multi-stage operations, improving both design clarity and maintainability.

### Pipelined Calculator 

![image](https://github.com/user-attachments/assets/0a5d2af9-d41d-4974-aecf-4a8473035e7a)


->Code
```ruby
   |cal
      @1
         $reset = *reset;
         $clk_sne = *clk;

         $valid[31:0] = $reset ? 0 : (>>1$valid + 1);
         $valid_or_reset = $reset | ~$valid;
         
         $val1[31:0] = >>2$out;
         $val2[31:0] = $rand2[3:0];
         $sel[1:0] = $rand3[1:0];
      
         $sum[31:0]  = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
      
      @2
         $out[31:0] = $valid_or_reset ? 32'h0 : ($sel[1:0] == 2'b00) ? $sum : ($sel[1:0] == 2'b01) ? $diff : ($sel[1:0] == 2'b10) ? $prod[31:0]: $quot[31:0];


```

Output

![image](https://github.com/user-attachments/assets/606d30be-2f6e-4615-9499-756cf46f8966)


## Validity 
In TL-Verilog, validity is a key concept used to manage data flow through pipelines. It helps ensure that operations are only performed on valid data and prevents the unnecessary processing of invalid or stale data. This concept is essential in pipelined designs, where operations are spread across multiple clock cycles.

### Cycle Calculator with Validity

![image](https://github.com/user-attachments/assets/964ad33d-80ac-4151-8011-e24658f14469)

->Code

```ruby
   |cal
      @1
         $reset = *reset;
         $clk_sne = *clk;
         
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
         $valid = $cnt;
         $valid_or_reset = ($reset | $valid);
         
      ?$valid
         @1
            $val1[31:0] = >>2$out;
            $val2[31:0] = $rand2[3:0];
            $sel[1:0] = $rand3[1:0];
      
            $sum[31:0]  = $val1[31:0] + $val2[31:0];
            $diff[31:0] = $val1[31:0] - $val2[31:0];
            $prod[31:0] = $val1[31:0] * $val2[31:0];
            $quot[31:0] = $val1[31:0] / $val2[31:0];
      
         @2
            $out[31:0] = $valid_or_reset ? 32'h0 : ($sel[1:0] == 2'b00) ? $sum : ($sel[1:0] == 2'b01) ? $diff : ($sel[1:0] == 2'b10) ? $prod[31:0]: $quot[31:0];

```


Output

![image](https://github.com/user-attachments/assets/3db87edb-7e45-48a3-befb-3c301235aa13)

</details>

<details>

<summary> Day-4 </summary>

# Basic RISC-V CPU Micro-Architecture

## RISC-V Block Diagram

![image](https://github.com/user-attachments/assets/b8bac6d7-6c87-44d4-a625-aac8ae25d08a)

## Implementation Plan

![image](https://github.com/user-attachments/assets/f737f0ce-3a8b-445e-8653-55278baeba10)

### 1. Program Counter (PC) and next PC

The CPU uses the program counter to track its location in the code, for example when fetching the next instruction. Usually, the CPU adds four to the PC during instruction execution: addresses are in bytes, and each RISC-V instruction is four bytes long.

![image](https://github.com/user-attachments/assets/bec21aab-224d-4543-b125-18bd1f961125)

->Code

```ruby
         $pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 31'h4 );

```

Output 

![image](https://github.com/user-attachments/assets/f307e945-7cf8-4743-811a-0c6d46a4140a)

### 2. Instruction Fetch

Instruction fetch is the first stage in the instruction execution cycle of a RISC-V CPU. It involves retrieving the instruction pointed to by the Program Counter (PC) from memory so that it can be decoded and executed in subsequent stages.

![image](https://github.com/user-attachments/assets/a284d9ff-d48e-4eed-8aa7-8ef0e1d48f9d)

->Code
```ruby
   |cpu
      @0
         $reset = *reset;
         $clk_sne = *clk;
         
         $pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 31'h4 );
         
         $imem_rd_en = >>1$reset ? 0 : 1;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];

      @1
         $instr[31:0] = $imem_rd_data[31:0];

```

Output

![image](https://github.com/user-attachments/assets/f7fb0c47-56de-49e4-b476-41d62ccd1ee8)


### 3. Instruction Decode

Instruction decode is the second stage in the instruction execution cycle of a RISC-V CPU. After an instruction is fetched from memory, it needs to be interpreted, or "decoded," to understand what operation is to be performed and which operands are involved. The decoding process involves breaking down the binary instruction into its constituent fields, such as opcode, source registers, destination registers, and immediate values.

![image](https://github.com/user-attachments/assets/b16ee5ff-4485-4a54-9670-f9ee48705891)

INSTRUCTION TYPES DECODE

![image](https://github.com/user-attachments/assets/bb6f23ed-d444-4961-83c8-cdd114772390)

->Code
```ruby
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
```

INSTRUCTION IMMEDIATE DECODE

![image](https://github.com/user-attachments/assets/2bdf7f54-2a87-4c84-8dcc-1a4bcc3cf32b)

->Code
```ruby
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
```

INSTRUCTION FIELD DECODE

![image](https://github.com/user-attachments/assets/dae7d804-9061-4373-bd24-1fb52bcc2f2e)

->Code
```ruby
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];

```

INSTRUCTION DECODE

![image](https://github.com/user-attachments/assets/ed14982c-04f4-4173-9610-90e5d0974a6e)

->Code

```ruby
         $opcode[6:0] = $instr[6:0];
         
         $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)

```
Output

![image](https://github.com/user-attachments/assets/1c9fa40b-982f-44a1-9983-bc60d0bd9270)

### Register file Reading

The read register file is a component that contains a collection of registers used to store data during instruction execution. Instructions typically involve accessing data from these registers, with the instruction indicating which registers to read. The retrieved data is then used as operands for operations carried out by the ALU or other CPU components.

![image](https://github.com/user-attachments/assets/986be544-3912-4929-826e-11fb47fa6b60)

->Code

```ruby
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
```

Output

![image](https://github.com/user-attachments/assets/8629f57d-29d7-4bca-bced-9945b69231fb)


### Arithmatic and Logic Unit (ALU)

The Arithmetic Logic Unit (ALU) in a RISC-V CPU is a key component responsible for performing arithmetic and logical operations. The ALU receives inputs from the register file or immediate values from instructions and produces results that are either stored back into registers, used for branching decisions, or forwarded to other components like memory.

![image](https://github.com/user-attachments/assets/80c5ebfc-d93d-4622-bf14-059e9b5f8b4e)

```ruby
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;

```

Output

![image](https://github.com/user-attachments/assets/b82e2cb3-8e20-4754-bede-222680586f15)

### Register file Write

In a RISC-V CPU, the register file write operation is a critical part of the instruction execution process, particularly during the write-back stage of the pipeline. This is where the results of computations or data from memory are written back to a register in the register file

![image](https://github.com/user-attachments/assets/d7059874-6c08-4667-b07b-945c130d83ec)


```ruby
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
```

Output

![image](https://github.com/user-attachments/assets/225bc6d6-49fa-4064-b48c-1dd2701cfc30)


### Branch Instructions

![image](https://github.com/user-attachments/assets/01712267-dd8f-4c0f-afb7-76d2f9762e19)


```ruby
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
```

Output

![image](https://github.com/user-attachments/assets/6b2a1031-96fc-4b8c-9781-8a9c38e74b2f)


</details>

<details>

<summary> Day-5 </summary>

## Complete Pipelined RISC-V CPU Micro-Architecture

->Code
```ruby
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 0 to 10 Program |
   // \====================/
   //
   // Add 0,1,2,3,...,10 (in that order).
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
   m4_asm(ADDI, r12, r10, 1011)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_sne = *clk;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==? 11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU implementation
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch equations
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
   *passed = |cpu/xreg[14]>>10$value == (1+2+3+4+5+6+7+8+9+10);
   //Run for 80 cycles without any checks
   *passed = *cyc_cnt > 80;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule

```
#### Waveforms :

1. clk

![image](https://github.com/user-attachments/assets/5fccd80c-f807-42c9-84be-619c4d9b351f)


2. reset

![image](https://github.com/user-attachments/assets/b48f8c67-1e33-496a-908c-54cb1b0b47e7)


#### Testbench

```ruby
*passed = |cpu/xreg[14]>>10$value == (1+2+3+4+5+6+7+8+9+10);
```

Output
![image](https://github.com/user-attachments/assets/cb1db13e-7102-44f8-9e5d-847930fd4f19)

 
</details>


</details>

