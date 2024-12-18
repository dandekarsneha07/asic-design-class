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

<details>

 <summary> Assignment-6 </summary>

## Converting the RISC-V tlv code to verilog code

RISC-V processor was initially designed in TL-Verilog using the Makerchip IDE. Now, for this lab we need to convert the TLV to verilog code and compare the outputs of both.

For converting the TLV code to verilog code we are using the sandpiper-saas compiler. Installation steps for the same are as below.

#### Step-1 Installing python and related packages
```ruby
sudo apt install make python python3 python3-pip
sudo apt-get install python3-venv
```

#### Step-2
```ruby
source ~/.venv/bin/activate
python3 -m venv .venv
```

#### Step-3
```ruby
pip3 install pyyaml click sandpiper-saas
```

#### Step-4 Installing git iverilog gtkwave
```ruby
sudo apt install git iverilog gtkwave
```

### Converting the tlv to verilog

#### Step-1 Reference flow 
```ruby
git clone https://github.com/manili/VSDBabySoC.git
```
#### Step-2
Replacing the rvmyth.tlv with desired RISC-V tlv.

#### Step-3 Converting to verilog

```ruby
cd VSDBabySoC
sandpiper-saas -i ./src/module/rvmyth.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./
```

#### Step-4 Running verilog code

```ruby
make pre_synth_sim
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

![image](https://github.com/user-attachments/assets/2ad69a3c-5eac-4f00-bcab-fcda7e6aaa27)


Output:
![image](https://github.com/user-attachments/assets/165bc05d-2df6-4414-b198-906684351dd4)


### Compare the output waveforms with the MakerChip simulation results 

1. clk

![image](https://github.com/user-attachments/assets/5fccd80c-f807-42c9-84be-619c4d9b351f)

2. reset

![image](https://github.com/user-attachments/assets/b48f8c67-1e33-496a-908c-54cb1b0b47e7)


3. Output
   
![image](https://github.com/user-attachments/assets/cb1db13e-7102-44f8-9e5d-847930fd4f19)

</details>

<details>

 <summary> Assignment-7 </summary>

## Simulation of BabySoC and generating waveforms for PLL and DAC 

For this lab we need to use the previously generated verilog code, and simulate the BabySoC.  This process involves generating DAC (Digital-to-Analog Converter) and PLL (Phase-Locked Loop) waveforms for the RISC-V processor.

### PLL
A phase-locked loop (PLL) is an electronic circuit with a voltage or voltage-driven oscillator that constantly adjusts to match the frequency of an input signal. PLLs are used to generate, stabilize, modulate, demodulate, filter or recover a signal from a "noisy" communications channel where data has been interrupted.

### DAC 
A Digital-to-Analog Converter (DAC) is an electronic component that transforms digital signals, often in binary form, into corresponding analog signals, such as voltage or current. 

### Cloning the BabySOC for simulation

### Step-1 Cloning BabySoC
```ruby
git clone https://github.com/Subhasis-Sahu/BabySoC_Simulation.git
```

### BabySoc 
Files structure BabySoC

src/module - contains all RTL files and testbench.v used for simulating our BabySoC design.

src/include - contains RTL files used in `include define in main RTL files in src/module.

### Step-2
Update the rvmyth.v according to the previous assignment steps.

### Step-3
Compiling the verilog
```ruby
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
```

### Step-4
Plotting the waveforms
```ruby
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

Output:

 ![image](https://github.com/user-attachments/assets/ce9dc58c-5e6c-4c51-a975-14bd8da0602e)

</details>

<details>

<summary> Assignment-8 </summary>

<details>
	
<summary> Day 0 </summary>

<details>

<summary> Yosys </summary>

``` ruby
 	git clone https://github.com/YosysHQ/yosys.git 
	cd yosys-master  
     	sudo apt install make  
     	sudo apt-get install build-essential clang bison flex \ 
     	libreadline-dev gawk tcl-dev libffi-dev git \ 
     	graphviz xdot pkg-config python3 libboost-system-dev \ 
     	libboost-python-dev libboost-filesystem-dev zlib1g-dev 
     	make  
     	sudo make install

```
 
</details>

<details>

<summary> iverilog </summary>

```ruby
 sudo apt-get install iverilog
```
 
</details>

<details>

<summary> gtkwave </summary>

```ruby
sudo apt-get install gtkwave
```
 
</details>

<details>

<summary> NgSpice </summary>

```ruby
tar -zxvf ngspice-40.tar.gz
cd ngspice-40
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
make
sudo make install
```
 
</details>

<details>

<summary> Magic </summary>

```ruby
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev	
```
 
</details>

<details>

<summary> OpenSTA </summary>

```ruby
sudo apt-get install cmake clang gcctcl swig bison flex
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake ..
make	
```
 
</details>

<details>

<summary> OpenLANE </summary>

```ruby
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 

# After reboot
docker run hello-world

```
 
</details>
 
</details>

<details>
	
<summary> Day 1 </summary>

# Introduction to Verilog RTL design and Synthesis

<details> 
<summary> Introduction to Verilog RTL design and Synthesis </summary>

## 1.1 Introduction to open source iverilog 

#### Register Transfer Level (RTL) 

RTL is a methodology that enables data transfer between registers. In RTL design, we use Hardware Description Languages (HDLs) like Verilog to code both sequential and combinational circuits. These codes can simulate the functionality of both hardware and logic operations. An RTL design may be composed of a single Verilog file or multiple ones. It is essential to ensure that RTL designs are written using efficient and synthesizable code, making them realizable in physical gate-level implementations.

##### -> Example templet for RTL

```verilog
module <module_name> (port list);
	//declarations;
	//initializations;
	//continuos concurrent assigments;
	//procedural blocks;
endmodule

```

![image](https://github.com/user-attachments/assets/19bad4e5-10d6-41bf-8521-ac93da401ab2)

#### Testbench 

Verilog enables the creation of testbenches that simulate the behavior of hardware designs by generating input signals and validating the resulting output signals. Testbenches are vital for larger and more complex designs as they help confirm that the simulation outcomes align with the expected behavior of the actual hardware after synthesis. Typically, a testbench includes two components: one for generating input signals for the design under test and another for verifying the accuracy of the output signals.

##### -> Example templet for Testbench

```verilog

`timescale 1ns/1ps  // Specify the time unit and precision

// Testbench Module
module <testbench_name>;

// Signal Declarations

// Instantiate the Design Under Test (DUT)
<module_name> DUT (
    .clk(clk),
    .reset(reset),
    .input_signal(input_signal),
    .output_signal(output_signal)
);

// Clock Generation
always #5 clk = ~clk;

// Initial Block for Signal Initialization
// Stimulus Generation
// Dump waveform to view in simulation tools (optional)
initial begin
    $dumpfile("testbench.vcd");
    $dumpvars(0, <testbench_name>);
end

endmodule

```

#### Simulation 

RTL designs are validated against their specifications by simulating them with a variety of input samples. This process is crucial for identifying and correcting design errors early in the development cycle.

#### Simulator 

A simulator is a software tool used to execute RTL designs and verify their functionality. It tracks changes in input signals and computes the corresponding output signals. If there are no changes in the input signals, the simulator does not detect any variations in the output signals.

#### Iverilog 

Icarus Verilog (Iverilog) is a free and open-source Verilog simulator used for verifying the functionality of digital circuits. It supports numerous Verilog language constructs, such as modules, ports, wires, registers, gates, and behavioral descriptions.

![image](https://github.com/user-attachments/assets/d9298b9e-4948-427d-ae22-45d02454e92d)


#### gtkwave 

gtkwave is a free and open-source waveform viewer widely used in digital design and verification. It offers a graphical interface for visualizing and analyzing digital waveforms, making it an essential tool for debugging and understanding the behavior of hardware designs.


</details>

<details>

<summary>Lab examples using iverilog and gtkwave </summary>
 
## 1.2 Lab examples using iverilog and gtkwave

### Step-1 Clone the following repository

```

git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop

```
### Step-2 Running the verilog code and analyzing the output

-> Command to run verilog code with testbench

```
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```

-> Code

```verilog
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

-> Testbench

```verilog
`timescale 1ns / 1ps
module tb_good_mux;
// Inputs
reg i0,i1,sel;
// Outputs
wire y;
// Instantiate the Unit Under Test (UUT), name based instantiation
	good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));
	//good_mux uut (sel,i0,i1,y);  //order based instantiation
initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
end
always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```

### -> Snapshot of the commands 

![image](https://github.com/user-attachments/assets/f45f7124-e4ed-4f4b-b5b6-dbf7681d28b5)

### -> gtkwave output

![image](https://github.com/user-attachments/assets/fcaa9c43-ab92-4825-9777-84d947bcd6cb)
</details>

<details>

<summary> Introduction to yosys and logic synthesizer </summary>

## 1.3 Introduction to yosys and logic synthesizer

### 1.3.1 Introduction to Yosys synthesizer

#### Synthesis

Synthesis is the process of converting a high-level design into a low-level netlist of logic gates, optimized for a specific technology library. This involves dividing the design into fundamental logic components, mapping these components to actual gates, and optimizing the netlist to meet design constraints. This step ensures that the abstract design can be implemented as physical hardware.

#### Synthesizer

A synthesizer is a tool used to translate RTL design code into a netlist. In this workshop, Yosys is used as the synthesizer tool.

#### Yosys

Yosys aims to converting high-level hardware descriptions into optimized gate-level representations that can be targeted for various FPGA and ASIC technologies. The flow for yosys is we feed the yosys with the design which is in RTL level and the .lib file which contain standard library cells then the yosys synthesizes and gives us the netlist file.

![image](https://github.com/user-attachments/assets/aab45dd9-917b-4259-8e6f-8e371acf2a44)

Below are the commands to perform above synthesis.

1. RTL Design - read_verilog
2. .lib - read_liberty
3. netlist file - write_verilog

#### Verifyling Synthesis 

In order to make sure that there are no errors in the netlist, we'll have to verify the synthesized circuit. 
The gtkwave output for the netlist should match the output waveform for the RTL design file. As netlist and design code have same set of inputs and outputs, we can use the same testbench and compare the waveforms.

![image](https://github.com/user-attachments/assets/a1d15eb0-7957-47f5-866e-a71ee1e64091)

### 1.3.2 Introduction to Logic Synthesis

#### Understanding of .lib

It contains detailed information about the standard cells used in digital design. These cells are the fundamental building blocks, like logic gates (AND, OR, NOT) and flip-flops, which are used to create more complex circuits. It has different variations (essentially different driving strengths, number of inputs) of these funadamental gates with different performaces.

Different performances refer to faster or slower gates. 
Faster the charging or dicharging of load (i.e capacitance), lesser is the cell delay. However, for a quick charging or discharging of load, we need transistors capable of sourcing more current i.e, we need wider transistors. Wider transistors have lesser delay but consume more area and power. Narrow transistors are other way around. Faster cells come with a cost of area and power.

Wider transistors -> low delay -> More Power and Area

Narrow transitor -> More delay -> Less Area and Power

#### Use-case of cells

There is always a trade off between power area and timing. More use of fast cells can lead to more power and area but lesser delay and use of slow cells can lead to a sluggish circuit consuming less power.
We need to give 'constaraints' which can help optimize the selection of cells with different driving strengths.

#### Infering the RTL design into synthesized design

![image](https://github.com/user-attachments/assets/bf5cd0f4-1dac-4971-9b91-b94bfd6f249c)


</details>

<details>

<summary> Labs using Yosys and SKY130 PDKs </summary>

## 1.4 Labs using Yosys and SKY130 PDKs

Invoking Yosys in the verilog_file directory and running following commands.

![image](https://github.com/user-attachments/assets/aacae2cf-34fd-4c4e-a9ab-e75dc60fc69f)

#### -> Commands

```
yosys> read_liberty -lib <path_for_sky130>/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> read_verilog <path_for_sky130_verilog_file>
yosys> synth -top good_mux
yosys> abc -liberty <path_for_sky130>/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```


![image](https://github.com/user-attachments/assets/f702f07a-976d-4922-841c-8c171f416616)


```
yosys> write_verilog good_mux_netlist.v
yosys> write_verilog -noattr good_mux_netlist.v
```


![image](https://github.com/user-attachments/assets/6d09915c-2d60-403b-bfd2-8a42830b41ed)


</details>
 
</details>

<details>
	
<summary> Day 2 </summary>

# Timing libs, hierarchical vs flat synthesis and efficient flop coding styles

<details> 
<summary> Introduction to timing .libs </summary>

## 2.1 Introduction to timing .libs

The .lib file, also known as a "library file" or "standard cell library file," is a crucial component in the VLSI (Very-Large-Scale Integration) design process. It contains detailed information about the standard cells used in digital design. These cells are the fundamental building blocks, like logic gates (AND, OR, NOT) and flip-flops, which are used to create more complex circuits.

The .lib file provides all the data necessary to model the behavior and characteristics of these cells during synthesis, simulation, and timing analysis.

liberty file contains following information:

1. Cell Descriptions
2. Timing 
3. Power 
4. Area 
5. Input/Output Pin Details
6. Operating Conditions (PVT)

#### Opening the .lib file

```
 gvim sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Understading the structure of .lib

Significance of sky130_fd_sc_hd__tt_025C_1v80 

1. sky130 - 130nm tech node PDK
2. tt - typical process (typical nmos, typical pmos)
3. 25C - 25 degree C temperature
4. 1v80 - operating voltage of 1.80

The following image shows the structure of a cell in .lib

1. Cell name is defined, it is an AND gate with 2 inputs with min drivng strength (here notation for same is and2_0)
2. Cell area is defined with cell footprint
3. The leakage power in mentioned for different when confitions (input combinations)
4. Power pins are defined with respective operating rail voltages

![image](https://github.com/user-attachments/assets/cac63137-3093-4470-ba23-e54b29ec0fe7)

#### Understanding different AND gates in the .lib

Here in the image 2 input AND gate is highlighted with different driving strengths hence the naming convention

and2_0, and2_1, and2_2

As we can observe that the leakage power is increasing as the driving strength is increasing.

leakage power : and2_0 < and2_1 < and2_2

![image](https://github.com/user-attachments/assets/e39dd2a5-d38d-423d-96f5-ac3f13fb797b)

</details>

<details> 
<summary> Hierarchical vs Flat synthesis </summary>

## 2.2 Hierarchical vs Flat synthesis

For understanding Hierarchical  and flat synthesis open multiple_module.v

-> Code

```verilog
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
wire net1;
sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```

![image](https://github.com/user-attachments/assets/4ec97bce-b19a-4508-9451-ce181fb20763)

-> launching yosys and synthesizing 

```
yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show multiple_modules 
```

![image](https://github.com/user-attachments/assets/1a4838d5-809a-4ad5-910b-368807683625)

![image](https://github.com/user-attachments/assets/616a9fdc-9209-4463-b5d7-1c5deebe8731)


Hierarchical design code for the multiple_modules:

-> Code

``` verilog
module multiple_modules(a, b, c, y);
	  input a;
	 input b;
	 input c;
	  wire net1;
	 output y;
  sub_module1 u1 (.a(a),.b(b),.y(net1) );
  sub_module2 u2 (.a(net1),.b(c),.y(y));
endmodule

module sub_module1(a, b, y);
 wire _0_;
 wire _1_;
 wire _2_;
 input a;
 input b;
 output y;
 sky130_fd_sc_hd__and2_0 _3_ (.A(_1_),.B(_0_),.X(_2_));
 assign _1_ = b;
 assign _0_ = a;
 assign y = _2_;
endmodule

module sub_module2(a, b, y);
wire _0_;
 wire _1_;
 wire _2_;
input a;
input b;
 output y;
 sky130_fd_sc_hd__lpflow_inputiso1p_1 _3_ (.A(_1_),.SLEEP(_0_),.X(_2_) );
 assign _1_ = b;
 assign _0_ = a;
 assign y = _2_;
endmodule

```

Generating netlist for hierarchical design

```
yosys> write_verilog -noattr multiple_modules_hier.v
yosys> !vim multiple_modules_hier.v
```

![image](https://github.com/user-attachments/assets/fe22c28e-d27b-4262-b70c-fc060519d291)


Flattened design code for the multiple_modules:

-> Code

```verilog
module multiple_modules(a, b, c, y);
	 wire _0_;
	 wire _1_;
	 wire _2_;
	 wire _3_;
	 wire _4_;
	 wire _5_;
	 input a;
	 input b;
	 input c;
	 wire net1;
	 wire \u1.a ;
	 wire \u1.b ;
	 wire \u1.y ;
	 wire \u2.a ;
	 wire \u2.b ;
	 wire \u2.y ;
	output y;
	 sky130_fd_sc_hd__and2_0 _6_ (
	  .A(_1_),
	 .B(_0_),
	 .X(_2_)
	);
	 sky130_fd_sc_hd__lpflow_inputiso1p_1 _7_ (
	  .A(_4_),
	  .SLEEP(_3_),
	  .X(_5_)
	 );
	 assign _4_ = \u2.b ;
	 assign _3_ = \u2.a ;
	 assign \u2.y  = _5_;
	 assign \u2.a  = net1;
	 assign \u2.b  = c;
	 assign y = \u2.y ;
	 assign _1_ = \u1.b ;
	 assign _0_ = \u1.a ;
	 assign \u1.y  = _2_;
	 assign \u1.a  = a;
	 assign \u1.b  = b;
	 assign net1 = \u1.y ;
	endmodule

```

Generating netlist for flat design

```
yosys> flatten
yosys> write_verilog -noattr multiple_modules_flat.v
yosys> !vim multiple_modules_flat.v
```

![image](https://github.com/user-attachments/assets/83d1e162-8a14-4d3b-b7d6-09922869d8a2)


</details>

<details>

<summary> Various flop coding styles and optimization </summary>

## 2.3 Various flop coding styles and optimization

### 2.3.1 Flops and Flops coding style

#### Understanding the Flip-Flop

In a combinational circuit, there is always a propagation delay before the output reflects the given inputs. During this delay, intermediate outputs can appear due to different paths within the circuit, resulting in what are known as glitches. The more combinational circuits present, the more likely it is for these glitches to occur, causing the output to become unstable.

To prevent these glitches, we use an element called a flip-flop (flop) to store the value. By using a flop, we ensure that the output of the combinational circuit is stored and only passed on to the next circuit at the rising or falling edge of the clock. This process stabilizes the input to the subsequent combinational circuit, thereby reducing glitches.

It's important to initialize the flop; otherwise, the following combinational circuit might receive a garbage value as input. For this purpose, we use control pins like reset and set.

Coadind styles for Flip Flops
1. Asynchronous reset
2. Synchronous reset
3. Asynchronous set
4. Synchronous set
5. Both Asynchronous reset and Synchronous reset
6. Both Asynchronous set and Synchronous set

### 2.3.2 Lab for flop synthesis simulations

#### D Flip-Flop with Asynchronous Reset

Here, the D flip flop has asynchronous reset i.e whenever the reset is applied, say here active low ( reset = 0 ) the the output of flip flop will be reseated irrespective of the clock's active edge (poseedge or negedge)

-> Code

```verilog
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
	always @ (posedge clk , posedge async_reset)
	begin
		if(async_reset)
			q <= 1'b0;
		else	
			q <= d;
	end
endmodule
```

##### Generating the outputs

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave dff_asyncres.vcd
```

![image](https://github.com/user-attachments/assets/d3946f7a-6caf-46f7-8d32-1d95cdce39c4)

##### Synthesis on yosys

```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_asyncres.v 
yosys> synth -top dff_asyncres
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

![image](https://github.com/user-attachments/assets/7bf80416-d353-448e-ba73-b718ec5c5012)


#### D Flip-Flop with Synchronous Reset

Here, the D flip flop has synchronous, whenever the reset is applied and there is an active edge of clock the filp flop will be resseted.

->Code

```verilog
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

##### Output

![image](https://github.com/user-attachments/assets/007088d5-61d9-4b90-b01b-2cc11dc8e2dd)

#### Synthesized Design

![image](https://github.com/user-attachments/assets/d6be9ae9-b3b2-4d8c-96f9-3801737b14dd)


#### D Flip-Flop with Asynchronous Set

Here whenever the set is activated the D filp flop is set irrespective of the active clock edge.

-> Code
```verilog
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
	always @ (posedge clk , posedge async_set)
	begin
		if(async_set)
			q <= 1'b1;
		else
			q <= d;
	end
endmodule
```

##### Output

![image](https://github.com/user-attachments/assets/5ff34d90-9957-4585-a32b-53eb976024d2)

##### Synthesized Circuit

![image](https://github.com/user-attachments/assets/b8ed9ad6-415f-44db-be71-0d0ca7ddd8d6)


#### D Flip-Flop with Synchronous/Asynchronous Reset

-> Code

```verilog
module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
##### Output

![image](https://github.com/user-attachments/assets/b435442c-4f06-4e19-8b8e-b303274e5b06)

##### Synthesized Design

![image](https://github.com/user-attachments/assets/3af1736f-80e9-4cb3-aee2-011d541fe4a4)


### 2.3.3 Optimizations

This lab session explores automatic and effective optimizations of circuits based on logic principles. In the example below, multiplying a number by 2 requires no extra hardware; it only involves connecting the bits from a to y while grounding the least significant bit (LSB) of y, as demonstrated by yosys.

```verilog
module mul2 (input [2:0] a, output [3:0] y);
	assign y = a * 2;
endmodule
```


```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog mult_2.v 
yosys> synth -top mul2
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show
```

##### Synthesized Output

![image](https://github.com/user-attachments/assets/32d5f5e4-6e75-4c41-82e4-78d9cf7404c9)

##### Synthesized Netlist

![image](https://github.com/user-attachments/assets/0c612572-354d-4879-99c6-5b68ff848bfc)


```verilog
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```

##### Synthesized Output

![image](https://github.com/user-attachments/assets/3707df3e-ce66-4df2-8549-5e0dcaee5c29)

##### Synthesized Netlist

![image](https://github.com/user-attachments/assets/597a8d4d-763f-45f7-98b8-7026799b3bde)


</details>

</details>

<details>

<summary>Day 3</summary>

# Combinational and sequential optimizations

<details> 
<summary> Introduction to Optimization </summary>

## 3.1 Introduction to Optimization

The synthesis tool Yosys employs various optimization techniques to produce an efficient circuit design, refining the logic to achieve the most optimized result.

The techniques used for Combinational Logic Optimization are:
1. Constant propagation : It is a direct optimization technique.
2. Boolean Logic Optimization

The techniques used for Sequential Logical Optimization are:
1. Sequential constant propagation
2. State Optimization : Optimization of unused states.
3. Retiming
4. Sequential logic cloning (Floorplan aware synthesis)

Command to optimize the circuit by yosys is 

```
yosys> opt_clean -purge
```

</details>

<details> 
<summary> Combinational Logic Optimization </summary>

## 3.2 Combinational Logic Optimization

### Example 1

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

#### Synthesized Output


![image](https://github.com/user-attachments/assets/f065dc2d-061d-4f45-a549-c66796557623)


### Example 2

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

#### Synthesized Output

![image](https://github.com/user-attachments/assets/eef9c758-dcab-4090-a497-7e4ba6f18f30)


### Example 3

```verilog
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

#### Synthesized Output

![image](https://github.com/user-attachments/assets/b30e12de-64c3-4a0a-9e06-881bc180596b)




### Example 4

```verilog
module opt_check4 (input a , input b , input c , output y);
	assign y = a?(b?(a & c ):c):(!c);
endmodule
```

#### Synthesized Output

![image](https://github.com/user-attachments/assets/04d8a7d4-9d06-4645-9c8f-b81919ebb6bb)




### Example 5

```verilog
module sub_module(input a , input b , output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
	wire n1,n2,n3;
	sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
	sub_module U2 (.a(b), .b(c) , .y(n2));
	sub_module U3 (.a(n2), .b(d) , .y(n3));
	sub_module U4 (.a(n3), .b(n1) , .y(y));
endmodule
```

```
yosys>read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys>read_verilog multiple_module_opt2.v
yosys>synth -top multiple_module_opt2
yosys>abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys>flatten
yosys>opt_clean -purge
yosys>show
```

#### Synthesized Output

![image](https://github.com/user-attachments/assets/553a9042-e935-4023-bd06-95fdcbceb246)

### Example 6

```verilog
module sub_module1(input a , input b , output y);
assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;
sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 
endmodule
```

#### Synthesized Output

![image](https://github.com/user-attachments/assets/cc0f718b-908d-44a8-901f-c6a4dbe8b35d)


</details>

<details> 
<summary> Sequential logic optimization </summary>

## 3.3 Sequential logic optimization

Basic Sequential Constant Propagation: In this technique, only the first logic can be optimized because the output of the first flip-flop is always zero. However, for the second flip-flop, where the output changes continuously, constant propagation cannot be applied.

Advanced State Optimization: This technique focuses on optimizing unused states in a design. By applying this method, we can achieve a more efficient and streamlined state machine.

Sequential Logic Cloning (Floorplan-Aware Synthesis): This is applied during physical-aware synthesis. For example, if flip-flop A is connected to flip-flops B and C through combinational logic, and B and C are placed far from A in the floorplan, a routing delay can occur. To minimize this delay, two intermediate flip-flops are introduced between A and B/C, reducing the delay. This process is called cloning because two new flip-flops with the same functionality as A are created.

Retiming: Retiming is an advanced sequential optimization technique that involves moving registers across combinational logic or optimizing the number of registers. This improves performance by balancing power and delay trade-offs, without altering the circuit's input-output behavior.

#### Example-1

```verilog
module dff_const1(input clk, input reset, output reg q);
	always @(posedge clk, posedge reset)
	begin
		if(reset)
			q <= 1'b0;
		else
			q <= 1'b1;
	end
endmodule
```

##### Output

![image](https://github.com/user-attachments/assets/64bbadf8-4676-46e6-bf14-60f86aa17c74)

##### Synthesized Output

![image](https://github.com/user-attachments/assets/cf7ce3e2-a68a-4b04-a9c4-5ee97be065b5)


#### Example-2

```verilog
module dff_const2(input clk, input reset, output reg q);
	always @(posedge clk, posedge reset)
	begin
		if(reset)
			q <= 1'b1;
		else
			q <= 1'b1;
	end
endmodule
```

##### Output

![image](https://github.com/user-attachments/assets/b2ceac80-68df-40c4-95cb-8effa56e87e3)


##### Synthesized Output

![image](https://github.com/user-attachments/assets/eace1928-db97-419c-add7-c1f2cd043690)


#### Example-3

```verilog
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

##### Output

![image](https://github.com/user-attachments/assets/a3ce43f3-1913-419e-b0c8-bf854917a1db)

##### Synthesized Output

![image](https://github.com/user-attachments/assets/f7a34f88-7080-47b8-8e3f-c22f4364154b)


#### Example-4

```verilog
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```


##### Output

![image](https://github.com/user-attachments/assets/cae00013-d2d1-4bbb-a5c7-d45569cec9ff)


##### Synthesized Output

![image](https://github.com/user-attachments/assets/f1036e58-be45-4609-af12-e6fee65e33ea)


#### Example-5

```verilog
module dff_const5(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
	begin
	if(reset)
	begin
		q <= 1'b0;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

##### Output

![image](https://github.com/user-attachments/assets/f6d1a9f7-b312-4aed-978b-d1ea9d32eb9e)


##### Synthesized Output

![image](https://github.com/user-attachments/assets/1e5856ba-023d-4973-8935-8b2d88cefa01)


</details>

<details> 
<summary> Sequential optimization for unused outputs </summary>

## 3.4 Sequential optimization for unused outputs

-> Code

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule
```

#### Output

![image](https://github.com/user-attachments/assets/53462ece-4f9d-452b-9298-d7044bafc81e)


#### Udpdated counter code

-> Code

```verilog
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = {count[2:0]==3'b100};
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule
```

#### Output

![image](https://github.com/user-attachments/assets/8ca6c2d4-6fab-4bd7-8563-c0e012acb181)


</details>

</details>

<details>

<summary>Day 4</summary>

# GLS, blocking vs non-blocking and synthesis-simulation

<details>
<summary> GLS, Synthesis-Simulation mismatch and Blocking/Non-Blocking statements </summary>

## 4.1 GLS, Synthesis-Simulation mismatch and Blocking/Non-Blocking statements

### Gate-Level Synthesis 

GLS is the process of converting a high-level hardware description language (HDL) design (such as Verilog or VHDL) into a gate-level netlist that represents the circuit in terms of basic logic gates like AND, OR, NAND, NOR, flip-flops, etc. 

In GLS, we run test bench with netlist as the Design under test instead of the RTL code. Basically, Netlist is logically equal to RTL code as the netlist is obtained by converting RTL code into standard cell gates.
We will use GLS to verify the logical correctness of design after synthesis and also to ensure the timing of the design is met. (run with delay annotations.)

### Why GLS?

To verify the logical correctness of the design after synthesis.
Ensuring the timing of the design is met -- For this GLS needs to run with delay annotation.

### GLS using IVERILOG

Below picture gives an insight of the procedure. Here while using iverilog, we also include gate level verilog models to generate GLS simulation.

![image](https://github.com/user-attachments/assets/29ca7fbe-6382-4d9a-a600-f7a0113333f4)

### Synthesis and Simulation Mismatch

These can happen because of some reasons and few of the are:

1. Missing Sensitivity List
2. Blocking vs Non-Blocking assignments
3. Non-standard verilog coding

To avoid the synthesis and simulation mismatch. It is very important to check the behaviour of the circuit first and then match it with the expected output seen in simulation and make sure there are no synthesis and simulation mismatches. This is why we use GLS.

### Blocking vs Non-Blocking Assignments

Blocking statements execute the statemetns in the order they are written inside the always block. Non-Blocking statements execute all the RHS and once always block is entered, the values are assigned to LHS. This will give mismatch as sometimes, improper use of blocking statements can create latches.

</details>

<details>
<summary> Labs on GLS and Synthesis-Simulation mismatch </summary>

## 4.2 Labs on GLS and Synthesis-Simulation mismatch

### Example-1

```verilog
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
endmodule
```

#### Simulation Output 

![image](https://github.com/user-attachments/assets/18ae2e25-c450-444c-a3f6-7364cf0083ea)


#### Synthesized Output

![image](https://github.com/user-attachments/assets/0139a637-85b5-458c-8e65-29474429f165)

-> Commands for netlist simulation

```
iverilog ../my_lib/verilog_module/primitives.v ../my_lib/verilog_module/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

#### Netlist Simulation

![image](https://github.com/user-attachments/assets/258a8b0c-ae88-4792-bf7b-63eee18e2bed)

Here the synthesis and simulation results are same, There is no synthesis simulation mismatch.

### Example-2

```verilog
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
if(sel)
	y <= i1;
else 
	y <= i0;
end
endmodule
```

#### Simulation Output 

![image](https://github.com/user-attachments/assets/73dfdca6-801a-42e3-9c33-3dc2377dcba1)

#### Synthesys Output

![image](https://github.com/user-attachments/assets/307e094f-44c6-435d-ae0e-eba31f53f42e)

#### Commands for netlist simulation

```
iverilog ../my_lib/verilog_module/primitives.v ../my_lib/verilog_module/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

#### Netlist Simulation

![image](https://github.com/user-attachments/assets/0d73b47a-cfc8-4788-8462-ea48c603e4d7)

Here the synthesis and simulation results are not same, there is synthesis simulation mismatch. The netlist simulation output depicts the true behaviour of a MUX.

</details>

<details>

<summary> Labs on synth-sim mismatch for blocking statement </summary>

## 4.3 Labs on synth-sim mismatch for blocking statement

```verilog
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
	begin
	d = x & c;
	x = a | b;
end
endmodule
```

#### Output Simulation

![image](https://github.com/user-attachments/assets/d125531b-6297-4c19-8de3-9ef437ed4cef)

-> Commands for synthesis

```
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog blocking_caveat.v 
yosys> synth -top blocking_caveat
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> write_verilog -noattr blocking_caveat_net.v
yosys> show
```


#### Synthesized Output

![image](https://github.com/user-attachments/assets/d8955554-4f30-4b3f-a9f1-e78859030e95)


-> Commands for netlist simulation

```
iverilog ../my_lib/verilog_module/primitives.v ../my_lib/verilog_module/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```


#### Netlist Simulation

![image](https://github.com/user-attachments/assets/cf3a236c-e350-422f-b293-3ad56e6ad296)

#### Synthesis Simultaion mismatch

![image](https://github.com/user-attachments/assets/bd20e52f-5ca6-4dc7-9711-0af533126123)

Here second wave window shows the netlist simulation which shows the proper working of the DUT while the first pic shows the improper working of DUT as we have used blocking statement here which causes synthesis simulation mismatch which is sorted out by GLS while providing netlist simulation

 
</details>

</details>
</details>

<details>

<summary> Assignment-9 </summary>

# Synthesis of RISC-V using yosys and Post synthesis simulation of BabySoC using iverilog and gtkwave

## Generating GLS using yosys

#### To do synthesis of rvmyth follow the below steps

```
read liberty - lib src/lib/sky130_fd schd tt 025C 1v80. lib
read_verilog -I src/include/-I src/module/ src/module/rvmyth.v src/module/clk_gate.v src/module/vsdbabysoc.v
synth - top rvmyth
abc - liberty src/lib/sky130_fd_sc_hd_tt_025C_Iv80. lib
dfflibmap - liberty src/lib/sky130_fd_schd tt_ 025C_1v80. lib
write_verilog -noattr rvmyth_net.v
```

These commands will generate the vsdbaby soc top level netlist file called as rvmyth_net.v

## Post Synthesis simulation of GLS

Following are the commands to compile using iverilog and plot the post synthesized simulation with gtkwave

```
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -DPOST_SYNTH_SIM -o ./post_synth_sim.out ./src/module/testbench.v -I ./src/include -I ./src/module -I ./src/gls _model -I ./src/
./post_synth_sim.out
gtkwave post_synth_sim.vcd
```

##### For getting the pre synthesized simulation output again you can follow below given steps

```
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

#### -> Output waveforms for post_synth_sim

![image](https://github.com/user-attachments/assets/713d8767-1a49-4fe5-a7a5-7192d1189178)

-> Zoomed wave window

![image](https://github.com/user-attachments/assets/44d049e5-3fe2-401b-b0d2-fb7e58c81efc)


#### -> Output waveforms for pre_synth_sim

![image](https://github.com/user-attachments/assets/7c5df48f-0e2c-439e-891d-e545d29f4c0e)

-> Zoomed wave window

![image](https://github.com/user-attachments/assets/4f120e2c-3715-4d04-b6d8-944e148e5d1c)


clk_sne: This is the input clk signal of the rvmyth core. This signal comes from the PLL.

reset: This is the input reset signal of the rvmyth core. This signal comes from an external source, originally. Here it is generated by the testbench

OUT: This is a real datatype wire which can simulate analog values. It is the output wire real OUT signal of the DAC module. This signal comes from the DAC.


#### Pre-synthesis and Posr-synthesis simuation waveforms are found to be identical.

#### -> Following are the waveform comparison images

![image](https://github.com/user-attachments/assets/f2325c38-7eb7-4c8c-b64b-402578de7fe7)

-> Zoomed wave window

![image](https://github.com/user-attachments/assets/1e2ad27f-4688-43c9-ab0d-07e78773761e)


#### -> Following are images showing the Standard Cells which are mapped in rvmyth core

![image](https://github.com/user-attachments/assets/8d2a172d-024a-4ba2-9c09-2762c3a5b18e)


</details>

<details>

<summary> Assignmnet-10 </summary>

# Static Timing Analysis of Synthesized RISC-V core using OpenSTA

## Understanding Static Timing Analysis (STA)

Static Timing Analysis (STA) is a technique used to verify the timing of a digital design without using specific input data values, setting it apart from timing simulation or dynamic timing analysis, which checks both timing and functionality by applying inputs and observing the design's response. 

STA is called "static" because it doesn't depend on changing input conditions instead it examines timing constraints across the whole design. In general, timing analysis refers to checking for timing-related issues, whether through STA or timing simulation, depending on the verification requirements.

Static timing analysis is a complete and exhaustive verification of all timing checks of a design. Other timing analysis methods such as simulation can only verify the portions of the design that get exercised by stimulus. Verification through timing simulation is only as exhaustive as the test vectors used. To simulate and verify all timing conditions of a design with 10-100 million gates is very slow and the timing cannot be verified completely.
Thus, it is very difficult to do exhaustive verification through simulation.

Static timing analysis on the other hand provides a faster and simpler way of checking and analyzing all the timing paths in a design for any timing violations.

- Timing Paths: STA evaluates all possible paths through a circuit from input to output, taking into account the propagation delays of gates and interconnects. There are mainly four types of timing paths defined in STA.

  	1.  reg-to-reg : register to register this path is purely sequential. The start point of this is from the clk to q of reg1 and the end point is the input data pin of reg2.
  
  	2.  reg-to-out : register to output here the start point is the reg and the end point of timing path is an output port.
  	4.  in-to-reg : input to register here the start point is an input port of block and end point is the input data pin of reg/flip-flop.
  	5.  in-to-out : input to output this is combinational path the start point is input port of desig and end point is output port of design


- Setup and Hold Times: It checks for setup and hold time violations. The setup time is the minimum time before the clock edge that the input data must be stable, while the hold time is the minimum time after the clock edge that the data must remain stable.

- Clock Constraints: STA incorporates clock definitions, including the clock frequency, period, and any variations (like skew or jitter).

- Worst-case Scenario: STA assumes worst-case conditions for delay values (like maximum load, temperature, and voltage) to ensure that the circuit will perform correctly under all expected operating conditions.

- Tools: There are various tools for performing STA, such as Synopsys PrimeTime, Cadence Tempus, and others, which automate the process and provide detailed reports on timing violations.


## Understanding the analysis of register to register path 

A reg-to-reg path, or register-to-register path, is a timing path in a digital circuit linking two sequential components, such as flip-flops or registers. This path is significant in Static Timing Analysis (STA) as it reflects the transfer of data between registers through combinational logic.

reg-to-reg paths are fundamental for maintaining accurate data flow and synchronization within digital circuits, particularly in designs involving pipelining or sequential processes. Analyzing these paths is essential to confirm that data is processed correctly across clock cycles, thereby supporting the circuit's functionality and dependability.

- Sequential Logic: reg-to-reg paths are part of sequential circuits where data is stored in registers and passed from one register to another after being processed by combinational logic.

- Setup and Hold Timing:

	1. Setup Time: reg-to-reg paths are analyzed for setup time constraints to ensure that the data output from the first register (FF1) arrives at the second register (FF2) before the clock edge that triggers FF2.
	2. Hold Time: These paths are also evaluated for hold time constraints to ensure that the data remains stable at the input of FF2 for a specified period after the clock edge that triggers FF1.
	3. Combinational Logic Delay: The timing analysis of a reg-to-reg path includes the propagation delay through the combinational logic that connects the two registers. This delay can vary based on the logic elements and their 		configuration.

- Critical Paths: reg-to-reg paths can often be critical paths if they take longer than other paths in the design, which can limit the maximum operating frequency of the circuit.

- Path Analysis: STA tools evaluate reg-to-reg paths to check for timing violations, allowing designers to optimize the circuit by adjusting the logic, resizing gates, or modifying the layout.

- Clock Domain Crossing: If the two registers belong to different clock domains, additional considerations for metastability and synchronization are needed, which complicates the reg-to-reg timing analysis.

## OpenSTA

### Initilizing libs and linking design

-> reading liberty files - command to read Liberty library files

We can specify process corner for corner delay calculation, min for min delay calculation (eg hold) and max for max delay calculation.

```
read_liberty -min/max/corner <lib_name> 
```

For example,
We will give the lib with worst process voltage temp for max i.e 

Maximum - Process SS, Voltage low, Temperature max

-> reading netlist files - command reads a gate level verilog netlist

```
read_verilog <filename>
```

-> linking design 

```
link_design <top_module_name>
```


### Writing .sdc

This format is primarily used to specify the timing constraints of a design. It is a text file. Some inportant SDC commands are listed here.

-> .sdc templet

```
# clock generation and defination
# clock related constraints
# input output delay and transition constraints
```

-> Creating clock
```
create_clock -name <clk_name> -period <period_in_ns> -wavrform {rising_edge, falling_edge} <assigning_clk_port>
```

-> Clock latency - The command describes expected delays of the clock tree when analyzing a design using ideal clocks

```
set_clock_latency -source <value> <assigning_clk_port>		#if its source latency
```

-> clock uncertainty - The command specifies the uncertainty or jitter in a clock.

```
set_clock_uncertainty -setup/hold <value> <assigning_clk_port> 
```

-> Input Transition - The command is used to specify the transition time (slew) of an input signal

```
set_input_transition -max/min  <value> <assigning_ports>
```

-> Input Delay - The command is used to specify the arrival times on the input ports

```
set_input_delay -clock <clk_name> -max/min <value> <assigning_input_ports> 
```

-> Output Delay - The command is used to specify the arrival times on the output port

```
set_output_delay -clock <clk_name> -max/min <value> <assigning_ports> 
```

-> To read the sdc file following command is used

```
read_sdc <file_name/file_path>
```

## STA of core RISC-V

-> Commands

```
# reading libs and linking design
read_liberty <lib_path>/sky130_fd_sc_hd__tt_025C_1v80.lib	# reading timing
read_verilog <netlist_path>/rvmyth_net.v			# reading netlist
link_design rvmyth						# linking top design

# synopsys design constraints 
create_clock -name clk -period 11.9 [get_ports clk] 			# clock defination and generation
set_clock_uncertainty [expr 0.05 * 11.9] -setup [get_clocks clk] 	# 5% setup uncertainty
set_clock_uncertainty [expr 0.08 * 11.9] -hold [get_clocks clk] 	# 8% hold uncertainty
set_clock_transition [expr 0.05 * 11.9] [get_clocks clk]  		# clock slew
set_input_transition [expr 0.08 * 11.9] [all_inputs -no_clocks] 	# input slews

# creating timing reports
report_checks -path_delay max 	# setup slack report
report_checks -path_delay min	# hold slack report
```

#### -> clock properties

![image](https://github.com/user-attachments/assets/616718ec-4346-4629-b3e4-802b99938ceb)


#### -> Setup timing report


![image](https://github.com/user-attachments/assets/a15cd47f-ac6e-4c0c-a2a8-6770c168862d)


#### -> Hold timing report

![image](https://github.com/user-attachments/assets/26e73227-7355-4655-ae8d-f1a80c96379b)


</details>

<details>

<summary> Assignment-11 </summary>

# Static Timing Analysis of Synthesized RISC-V core using different PVT Corner Library Files on OpenSTA

#### PVT (Process, Voltage, Temperature)

The PVT analysis is done to ensure that a chip functions reliably across all possible operating conditions, it is simulated under various combinations of these factors, known as "corners." Each of these parameters directly impacts the delay of the cell.

#### Process (P): 

Process variation refers to deviations in the semiconductor fabrication process, such as variations in impurity concentration, oxide thickness, and transistor dimensions. These process variations can cause changes in transistor parameters like threshold voltage, mobility, and current drive, which in turn impact the circuit delay and performance. Circuits designed with a "fast" process will have lower delays, while "slow" process corners will have higher delays.

![image](https://github.com/user-attachments/assets/924a6135-97f0-40c1-878e-35fdab861653)


#### Voltage (V): 

With shrinking nodes, the supply voltage for a chip also reduces. For instance, if a chip operates at 1.2V, this voltage might fluctuate at different times, ranging between 1.5V and 0.8V. To account for this, voltage variation is considered in simulations.

Voltage variation can occur due to multiple factors:

- IR Drop: Caused by current flow over the power grid network.
- Supply Noise: Occurs due to parasitic inductance interacting with resistance and capacitance, resulting in voltage fluctuations when current flows through parasitic inductance.

![image](https://github.com/user-attachments/assets/d8252016-cd72-4616-8fba-5cea785af6c0)


#### Temperature (T):

The operating temperature of the chip can vary widely depending on the environment and power dissipation within the chip. Higher temperatures generally decrease carrier mobility, leading to increased delays.

![image](https://github.com/user-attachments/assets/0d2026cd-5d13-4be9-bd2c-20490d1790be)


The constraints file from the earlier lab is also read(clock-11.90 ns with 5% of clock period for clock uncertainity and data transition delay for setup and 8% of clock period for clock uncertainity and data transition delay for hold) with additional constraints is as follows

#### -> .sdc

```tcl

#*** Setting clock constraints  ****

# create clock of 11.9ns
create_clock -name clk -period 11.9 [get_ports clk]

# setting clock uncertainty
set_clock_uncertainty [expr 0.05 * 11.9] -setup [get_clocks clk] 
set_clock_uncertainty [expr 0.08 * 11.9] -hold [get_clocks clk]

# setting clock transition
set_clock_transition [expr 0.05 * 11.9] [get_clocks clk]  

# setting clock latency
set_clock_latency 1 [get_clocks clk]
set_clock_latency -source 1.5 [get_clocks clk]

#*** Setting input output constraints  ****


#setting the load
set_load -min -pin_load 0.5 [all_outputs]

#setting input transition
set_input_transition [expr 0.08 * 11.9] [all_inputs -no_clocks]

# setting input delay
set_input_delay -clock clk -max 4 [all_inputs -no_clocks]
set_input_delay -clock clk -min 1 [all_inputs -no_clocks]

#setting output delay
set_output_delay -clock clk -max 4 [all_outputs]
set_output_delay -clock clk -min 1 [all_outputs]

```

The tcl script to run the reprots for all corners is as below

```tcl
# lib list

set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__tt_100C_1v80.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v76.lib"



for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty /home/vsduser/SFAL-VSD/skywater-pdk-libs-sky130_fd_sc_hd/timing/$list_of_lib_files($i)
read_verilog /home/vsduser/assignments/VSDBabySoC/rvmyth_net.v
link_design rvmyth
current_design
read_sdc /home/vsduser/assignments/VSDBabySoC/rvmyth.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /home/vsduser/assignments/VSDBabySoC/sta_output/min_max_$list_of_lib_files($i).txt

exec echo "$list_of_lib_files($i)" >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_worst_max_slack.txt
report_worst_slack -max -digits {4} >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_worst_max_slack.txt

exec echo "$list_of_lib_files($i)" >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_worst_min_slack.txt
report_worst_slack -min -digits {4} >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_worst_min_slack.txt

exec echo "$list_of_lib_files($i)" >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_tns.txt
report_tns -digits {4} >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_tns.txt

exec echo "$list_of_lib_files($i)" >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_wns.txt
report_wns -digits {4} >> /home/vsduser/assignments/VSDBabySoC/sta_output/sta_wns.txt
}

```

#### Worst Setup Slack (WNS):

![image](https://github.com/user-attachments/assets/562441da-8efa-4ee2-adeb-5097f4ec7fb8)

#### Worst Hold Slack (WHS):

![image](https://github.com/user-attachments/assets/5e6a5b2d-ea3c-4783-b537-28ff35c66d85)


</details>

<details>

<summary>Assignment-12</summary>

<details>

<summary> Day-1</summary>

# Inseption of open-source EDA, OpenLANE and Sky130 PDK

<details>

<summary> Theory </summary>

#### -> Package

In any embedded board, the component we commonly refer to as the chip is actually the package of the chip. This package is essentially a protective casing around the actual manufactured chip, which is typically located at the center of the package. Connections from the package to the chip are established using the wire bonding method, where thin wires connect the two, forming a basic wired link to facilitate signal transfer.


#### -> Chip

All signals entering and leaving in the chip from the external world pass through pads. The region enclosed by these pads is called the core, where all the digital logic of the chip resides. Together, the core and pads form the die, which serves as the fundamental manufacturing unit for semiconductor chips.

#### -> Foundary

A foundry is the facility where semiconductor chips are manufactured, and foundry IPs refer to Intellectual Properties tailored to a particular foundry. Developing these IPs requires specialized expertise. In contrast, reusable digital logic blocks within these designs are referred to as macros.

#### -> ISA (Instruction Set Architecture)

To run a C program on a specific hardware layout, such as the chip within a laptop, a particular flow must be followed. First, the C program is compiled into assembly language using the RISC-V ISA (Reduced Instruction Set Computing - V Instruction Set Architecture). This assembly code is then translated into machine language, represented in binary (0s and 1s), which the computer’s hardware can process. Following this, the RISC-V specifications are implemented in RTL (Register Transfer Level) using a hardware description language. Finally, the design moves from RTL to layout in a standard PnR (Place and Route) or RTL to GDSII flow.

#### -> RTL IPs (Register Transfer Level Intellectual Property)

RTL IPs refer to pre-designed and pre-verified digital hardware components or blocks represented at the Register Transfer Level (RTL). RTL is a level of abstraction in hardware description languages (HDLs) that describes data flow between registers and the operations performed on this data. In IC design, an IP block is a reusable component that can be integrated into larger designs, and RTL IPs, in particular, are developed at this level to simplify design integration. These IPs can either be licensed from vendors or developed in-house, enabling designers to focus on higher-level aspects rather than dealing with low-level implementation details. Using RTL IPs enhances productivity, shortens time-to-market, and improves design reliability, as they are well-optimized and pre-tested, minimizing potential errors and promoting design reuse in complex systems.

#### -> EDA (Electronic Design Automation)

EDA tools are software applications used to design, develop, and analyze electronic systems, such as integrated circuits (ICs) and printed circuit boards (PCBs). These tools automate many design tasks, increasing efficiency and speeding up time-to-market.

#### -> Process Design Kit (PDK)

A Process Design Kit (PDK) is a collection of files used to model a semiconductor fabrication process within design tools for IC development. PDKs are typically proprietary and specific to a particular foundry, often protected by non-disclosure agreements. Recently, however, open-source PDKs have been introduced to encourage innovation and collaboration in the design community. Open-source PDKs, such as SKY130, GFU180, and ASAP7, allow designers to access, customize, and modify the kit to suit specific design requirements. This flexibility promotes knowledge sharing, reduces barriers for new designers, and encourages collaborative development in IC and electronic system design.

### RTL to GDSII flow


#### -> RTL Design (Register Transfer Level)

Using Hardware Description Languages (HDLs) like Verilog or VHDL, the designer describes the logical operations and functionality of each module. RTL code represents the behavior of the circuit at the register level, laying the groundwork for synthesis.

#### -> Synthesis

Convert the RTL code into a gate-level netlist using a synthesis tool. This process maps the RTL description onto standard cells provided by the foundry, creating a network of logic gates that can be implemented on silicon. Constraints such as timing, power, and area are applied here.

#### -> Design for Testability (DFT)

Incorporate test structures, such as scan chains and built-in self-test (BIST) circuits, to make post-manufacturing testing possible. DFT enhances the ability to detect defects in the silicon after production.

### -> ASIC Flow

![image](https://github.com/user-attachments/assets/83564b15-da2f-4c06-b9c2-58b0ca674108)

#### -> Floorplanning and Powerplanning 

It is a crucial step in the digital design flow that involves partitioning the chip's area and determining the placement of major components and functional blocks. It establishes an initial high-level layout and defines the overall chip dimensions, locations of critical modules, power grid distribution, and I/O placement.The primary goals of floor planning are: Area Partitioning, Power Distribution, Signal Flow and Interconnect Planning, Placement of Key Components, Design Constraints and Optimization.

#### -> Placement 

Placement involves assigning the physical coordinates to each gate-level cell on the chip's layout. The placement process aims to minimize wirelength, optimize signal delay, and satisfy design rules and constraints. Modern placement algorithms use techniques like global placement and detailed placement to achieve an optimal placement solution.

#### ->  Clock Tree Synthesis (CTS)

Design the clock distribution network to deliver the clock signal with minimal skew and delay across the chip. CTS ensures synchronous operation of the logic by maintaining uniform timing throughout the design.

#### -> Routing

Connect all the placed cells and blocks with metal wires according to the netlist. Routing is done in multiple layers and needs to meet timing and power constraints. Ensuring that the connections do not cause crosstalk or violate design rules is crucial at this stage.

#### -> Signoff and Physical Verification

Verify the physical layout against the design rules provided by the foundry (DRC - Design Rule Check) and ensure the layout matches the intended logic design (LVS - Layout vs. Schematic). Additional checks include timing analysis (STA - Static Timing Analysis), power analysis, and IR drop analysis.



</details>

## 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs. Commands to invoke the OpenLANE flow and perform synthesis

```ruby
cd Desktop/work/tools/openlane_working_dir/openlane

#invoke docker
docker

docker sub-system
./flow.tcl -interactive

#required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

#prep the design
prep -design picorv32a
```


![Screenshot 2024-11-12 134603](https://github.com/user-attachments/assets/2756432c-eb83-4cfe-8b0a-338b4fe16d9b)


#### Running Synthesis

-> Command

```ruby
#run synthesis using following command
run_synthesis

```

![running_synth](https://github.com/user-attachments/assets/638d0cde-1387-494b-8d03-3d79ab7324a6)

#### -> Synthesis statistics report file

![stat_syth_1](https://github.com/user-attachments/assets/7cd6bc79-2c7b-44e5-845e-6f90aeab09e2)


![stat_synth_2](https://github.com/user-attachments/assets/9a050314-d2e7-41a9-a83c-954093a1884a)


**Calculation of Flop Ratio and DFF % from synthesis statistics report file

Flop Ratio = 1613/14876 = 0.108429685

Percentage of Flip Flops = 0.108429685 ∗ 100 = 10.84296854%**

#### -> TNS and WNS

![synth_tns](https://github.com/user-attachments/assets/91170212-808b-4502-9faf-cd5070380bac)

![synth_wns](https://github.com/user-attachments/assets/c9e5cc99-5d73-4470-a652-c38e06464bc4)

</details>


<details>

<summary> Day-2 </summary>

## Good Floorplan vs Bad Floorplan and Introduction to Library Cell

Floorplanning and power planning are essential steps in ASIC physical design. Together, they define the spatial arrangement of components and the distribution of power within the chip, ensuring that design goals like performance, area, power efficiency, and signal integrity are met.

#### -> Pre-Placed Cells

Pre-placed cells are specific components in an ASIC design that are positioned on the layout early in the design flow, prior to general cell placement. They include essential elements such as input/output (I/O) pads, power management cells, memory blocks, analog cells, and IP cores. These pre-placed cells should be surrounded by decoupling capacitors, which are large capacitors designed to store electrical charge and maintain a voltage close to that of the power supply. When a circuit switches, the decoupling capacitor temporarily acts as a power source, effectively isolating the circuit from the main power supply. During switching events, it provides the necessary current to the circuit, helping to prevent voltage drops. By placing these capacitors close to the circuit, they ensure stable current delivery during switching operations. The decoupling capacitor charges the circuit by transferring some of its stored charge during switching, then recharges itself from the power supply when the circuit is idle.

#### ->  Floorplanning
Floorplanning is the process of organizing and arranging large blocks or modules of an ASIC design on the chip’s layout. This phase has a significant impact on the final performance, timing, and area of the chip.

#### -> Power Planning
Power planning is the process of designing an efficient power distribution network to deliver stable power across the chip. It aims to prevent power drops, minimize IR drop, and manage the overall power integrity of the design.

-> Floorplanning

```ruby
#run floorplan using following command
run_floorplan
```

![floorplanning_running_area](https://github.com/user-attachments/assets/65c15569-ba31-474e-a100-d886ed061bba)

-> Command

```ruby
#path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<your_path>/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```


![image](https://github.com/user-attachments/assets/7576eafd-b08a-4a35-9440-5c75743021ac)


-> Tap Cells

![tap_cells](https://github.com/user-attachments/assets/2a8db58b-d549-4f0a-a533-ad4d950968f9)

The tap cells are placed in staggered manner i.e they are placed in checker board pattern. Tap cells, which are typically used for providing electrical connections to the power (VDD) and ground (VSS) rails in a chip, are placed in a staggered pattern to improve the overall power integrity and signal performance.

![tap_cells](https://github.com/user-attachments/assets/f429c874-fde3-4a0e-9e37-c6836f90b740)


-> IO ports, boundary cells, IO ports

![floorplan_ios_tapcell](https://github.com/user-attachments/assets/f9e36144-2f42-4dbf-af7f-163386637d3d)


-> IO port layers


![floorplan_io](https://github.com/user-attachments/assets/f43cb0bd-31c0-496a-a433-b0fffa48d974)

-> Unplaced standard cells


![standard_cells](https://github.com/user-attachments/assets/3dcdc781-de72-43dc-bd85-ac0f174fb5c6)


-> Floorplaning DEF


![image](https://github.com/user-attachments/assets/c1fee97b-ba7a-487b-acc2-01ca8f1ae649)


According to floorplan def 1000 Unit Distance = 1 micron Die width in unit distance 

= 660685 − 0 

= 660685 Die height in unit distance = 671405 − 0 

= 671405 Distance in microns 

= Value in unit distance / 1000 Die width in microns 

= 660685/1000 = 660.685 microns Die height in microns 

= 671405/1000 = 671.405 microns Are os die in microns 

= 660.685 ∗ 671.405 = 443587.212425 square microns

-> Powerplanning

![powerplanning_running](https://github.com/user-attachments/assets/fe515f3f-9f5f-429c-bd3c-e6e34fab170f)


### Placement

Placement in ASIC design is the step where standard cells (like logic gates and flip-flops) are positioned on the chip layout based on the floorplan. This stage directly affects the chip’s performance, timing, area, and power efficiency. Placement can be divided into two main stages: global placement and detailed placement.

-> Congestion aware placement

Congestion-aware placement refers to the process of positioning cells on the chip layout while considering potential routing congestion. The goal is to place cells in such a way that the interconnects (wires) connecting them can be routed efficiently, without excessive overlap or interference that could lead to design rule violations, signal delays, or even physical errors.

-> Timing aware placement

Timing-aware placement focuses on ensuring that the cells are placed in a way that optimizes the chip’s timing performance. The objective is to minimize the delay along critical signal paths (often referred to as critical paths) to meet the required timing constraints (setup and hold times).


-> Command
```ruby
# Congestion aware placement by default
run_placement
```

-> Opening the DEF file

```ruby
# path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/<your_path>/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```


![placement](https://github.com/user-attachments/assets/0152dc05-807e-4113-9c96-f61b3c665af3)


-> Legalized placement of standard cells

we can see the power rails for standard cells as well as the legalized placed standard cells

![standard_cell_grid_power](https://github.com/user-attachments/assets/78a1ffbc-beaa-4246-97aa-9d0fa74fe80a)


</details>

<details>

<summary> Day-3 </summary>

## Design Library Cell Using Magic Layout and Cell characterization

<details>

<summary> 16-Mask CMOS Fabrication Theory </summary>

#### Substrate Selection

This is the most initial phase of the process where the subrstrate is chosen.Here we are chosing a p-substrate.

![image](https://github.com/user-attachments/assets/6509394f-08d5-4cff-9d9c-7cb11ee740ec)

#### Active Region Creation

To isolate the active regions for transistors, the process starts with the deposition of SiO₂ (silicon dioxide) and Si₃N₄ (silicon nitride) layers. This is followed by photolithography and etching of the silicon nitride layer. This method is known as LOCOS (Local Oxidation of Silicon), where an oxide layer is grown in specific areas to define the active regions. Finally, the Si₃N₄ layer is removed using hot H₂SO₄ (sulfuric acid).

![image](https://github.com/user-attachments/assets/e28d5eb5-d0cf-47d2-8b70-5baed533202c)

#### N-Well and P-Well Formation

The N-well and P-well regions are formed independently. For P-well formation, boron ions are implanted, while for N-well formation, phosphorus ions are used. A high-temperature furnace process is then applied to drive-in the diffusion of these ions, establishing the well depths in a step commonly referred to as the tub process.

![image](https://github.com/user-attachments/assets/b6841e78-efac-4c5f-a401-2f861ea74ec6)

#### Gate Formation

The gate is a crucial terminal in CMOS transistors, as it regulates the threshold voltage for transistor switching. The gates for both NMOS and PMOS transistors are created using photolithography techniques. Key factors in gate formation include the oxide capacitance and the doping concentration, which influence the transistor's performance.

![image](https://github.com/user-attachments/assets/61c0c95f-03ea-497d-85b1-015a6744cc30)

#### Lightly dopped Drain(LDD)

LDD formed to avoid the hot electron effect.

![image](https://github.com/user-attachments/assets/6ddcb8ec-67de-4ede-8a94-8ef9f948be78)

#### Source and Drain Formation

Screen oxide added to avoid channelling during implants followed by Aresenic implantation and high temperature annealing.

![image](https://github.com/user-attachments/assets/1730d209-9f8c-4580-a1b3-968dc40bcf58)

#### Local Interconnect Formation

The screen oxide layer is removed using HF etching, followed by the deposition of titanium (Ti) to create low-resistance contacts. Heat treatment is then applied, leading to chemical reactions that form titanium silicide at the contact points for low-resistance interconnects, and titanium nitride at the top-level connections, facilitating local signal routing.

![image](https://github.com/user-attachments/assets/ed09ac14-0ec4-454d-b947-fa347730dcdd)

#### Higher Level Metal Formation

Chemical Mechanical Polishing (CMP) is performed by doping silicon oxide with boron or phosphorus to achieve surface planarization. This process is followed by the deposition of titanium nitride (TiN) and tungsten. An aluminum (Al) layer is then deposited, patterned using photolithography, and further polished with CMP. This forms the first interconnect layer. Additional interconnect layers can be stacked on top to achieve higher levels of metal connections. Finally, a dielectric layer, typically Si₃N₄ (silicon nitride), is added on top to protect the chip.

![image](https://github.com/user-attachments/assets/5824d368-de9e-4db3-a4fc-d4faa066b007)


</details>

### Clone custom inverter standard cell design from github repository

```ruby
# directory - openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech ./

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_sne_inv.mag &
```

![image](https://github.com/user-attachments/assets/5da47815-5c85-4171-be1f-115e2db78db7)


-> n-well

![image](https://github.com/user-attachments/assets/1c47be57-c663-43b6-bce5-0afdd44c08c5)

-> VDD i.e VPWR

![image](https://github.com/user-attachments/assets/0fb177d0-dd6a-4970-a038-f816c6faf937)


-> Poly

![image](https://github.com/user-attachments/assets/0bf251c0-988d-419e-a016-7d65431582f9)


```ruby
#extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```


-> .spice

![image](https://github.com/user-attachments/assets/1459c5da-844f-4f90-96ca-b46a55d9e7eb)

-> Grid values

![image](https://github.com/user-attachments/assets/8b8357c0-fa65-428f-86fc-d920993d632f)


![image](https://github.com/user-attachments/assets/91fc74fd-939b-4c05-a119-bdf0b20b8dbe)

-> Command

```ruby
ngspice <name>.spice
plot y vs time a
```

![image](https://github.com/user-attachments/assets/be4be022-3c27-4ecb-a59a-a12bc7f8b58e)



-> plot

![image](https://github.com/user-attachments/assets/9d630059-458c-4d29-8f84-36eba7b8df7b)

There are four timing parameters used to characterize the inverter standard cell:

1. Rise transition - Time taken for the output to rise from 20% to 80% of max value
2. Fall Transition: Time taken for the output to fall from 80% to 20% of max value
3. Cell Rise delay: difference in time(50% output rise) to time(50% input fall)
4. Cell Fall delay: difference in time(50% output fall) to time(50% input rise)

-> Rising Transition

-> 20% rising output

![image](https://github.com/user-attachments/assets/6f4a079e-cbeb-42e1-bf53-c286f908aec5)

-> 20% rising output

![image](https://github.com/user-attachments/assets/a3204c5b-9c77-44a0-af9f-3d48c9b10d22)

```
Rise Transistion = 2.24 - 2.18 = 0.065ns
```

-> Falling Transition

-> 20% falling output

![image](https://github.com/user-attachments/assets/632a26e9-9aef-4edd-b724-059a9a65140a)


-> 80% falling output


![image](https://github.com/user-attachments/assets/925c3207-0314-40a0-a125-67c7d5b04af5)

```
Fall Transistion = 4.09 - 4.05 = 0.04 ns 
```

->  Cell Rise Delay

-> 50% from input falling

![image](https://github.com/user-attachments/assets/7895cdc4-96a7-4937-93e1-d346bd215364)


-> 50% from output rising

![image](https://github.com/user-attachments/assets/0ae3f3a6-e744-4cad-a3cd-67b8985707ca)


```
Rising delay =  2.21 - 2.15 = 0.06 ns
```

-> Cell Fall Delay

-> 50% from input rising

![image](https://github.com/user-attachments/assets/9abb34f9-8a26-4026-9ef0-73bd62bd1dce)


-> 50% from output falling

![image](https://github.com/user-attachments/assets/acc3a3a6-b405-4265-83c3-58d808368328)

```
Falling delay =  4.08 - 4.05 = 0.03 ns
```

![image](https://github.com/user-attachments/assets/a967f187-4783-42f4-be0b-3f7a5408d7b2)

### Design Rule Check (DRC)

DRC verifies whether a design meets the predefined process technology rules given by the foundry for its manufacturing. DRC checking is an essential part of the physical design flow and ensures the design meets manufacturing requirements and will not result in a chip failure. It defines the Quality of chip. They are so many DRCs, let us see few of them

### Design rules for physical wires

Minimum width of the wire, Minimum spacing between the wires, Minimum pitch of the wire To solve signal short violation, we take the metal layer and put it on to upper metal layer. we check via rules, Via width, via spacing.

```
# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &

```

Load Sky130 tech rules for drc challenges
First load the poly file by load poly.mag on tkcon window.

```
load poly.mag
```


![Screenshot 2024-11-13 223603](https://github.com/user-attachments/assets/47e4b886-aa6c-43bf-89d3-a10cf6e3989b)



![image](https://github.com/user-attachments/assets/37bb2680-a6eb-427e-bfe2-82efeb603de8)


In line
```
spacing npres *nsd 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```

change to

```
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```

Also,

```
spacing xhrpoly,uhrpoly,xpc alldiff 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
```

change to

```
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \
	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
```

![image](https://github.com/user-attachments/assets/e083ad97-e653-4a0d-97c3-4eab1799b215)


</details>

<details>

<summary> Day-4 </summary>

## Pre-Layout timing analysis and Importance of good clock tree

Tracks.info contains the information needed to obtain the grids; cd to the specific directory and open the file

```
li1 X 0.23 0.46  
li1 Y 0.17 0.34   
met1 X 0.17 0.34
met1 Y 0.17 0.34
met2 X 0.23 0.46
met2 Y 0.23 0.46
met3 X 0.34 0.68
met3 Y 0.34 0.68
met4 X 0.46 0.92
met4 Y 0.46 0.92
met5 X 1.70 3.40
met5 Y 1.70 3.40
```

Use the below command in the tkcon window to get grid on magic.

```
grid 0.46um 0.34um 0.23um 0.17um
```

![image](https://github.com/user-attachments/assets/8ab2ce73-6c9b-4e79-8e7c-9156807cff97)


![image](https://github.com/user-attachments/assets/2923ce9e-14ae-4226-a436-f24f9d6d583f)

-> Command

```
lef write
```

![image](https://github.com/user-attachments/assets/8bf9b7cf-ebd5-45e2-bef9-c00386dfc194)



### Including Custom Cells in ASIC Design

In the first stages of an inverter, we produced a proprietary standard cell. Move the lef files, sky130_fd_sc_hd_typical.lib, sky130_fd_sc_hd_slow.lib, and sky130_fd_sc_hd_fast.lib from the libs folder vsdstdcelldesign to the picorv32a's src folder. Next, make the following changes to config.tcl.

```
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "$::env(DESIGN_DIR)/src/picorv32a.v"

set ::env(CLOCK_PORT) "clk"
set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(GLB_RESIZER_TIMING_OPTIMIZATIONS) {1}

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set filename $::env(DESIGN_DIR)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
	source $filename
}
```

### Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

-> Commands
```
# Copy lef file
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

### Run openlane flow synthesis with newly inserted custom inverter cell.

-> Commands

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```

![image](https://github.com/user-attachments/assets/38589461-eea9-47a8-9d93-f6717309db43)

![image](https://github.com/user-attachments/assets/67fae605-8092-4864-bf84-bc8865398bed)

![image](https://github.com/user-attachments/assets/d6b44c4b-ac70-45ec-8869-144a0bdab9a4)



-> To remove violations of tns and wns

```
prep -design picorv32a -tag <your_folder> -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 3"
echo $::env(SYNTH_BUFFERING)
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis
run_floorplan
```

![image](https://github.com/user-attachments/assets/228a6e86-6d0d-4334-9ad2-4104801f0ee1)

![image](https://github.com/user-attachments/assets/cb0b6550-5d12-4609-ba8b-0b35796b9266)

![image](https://github.com/user-attachments/assets/8d27029d-282f-4d41-8d06-328bd364f1db)



During run_floorplan the error is found

-> Screenshot of error

![image](https://github.com/user-attachments/assets/50892088-353a-43ad-886d-ee29afd97607)

-> Commands

```
# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or
```

-> Now that floorplan is done we can do placement using following command

![image](https://github.com/user-attachments/assets/239a11f7-66d8-4dd2-a287-9eac1ad21865)

![image](https://github.com/user-attachments/assets/41a4fc50-c933-424f-974a-01800869fbbf)


```
# Now we are ready to run placement
run_placement
```

-> Commands

```
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-58/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

-> Screenshot of placement

![image](https://github.com/user-attachments/assets/72d6b869-6c9f-407c-8f99-e68e19904f97)

![image](https://github.com/user-attachments/assets/2747bdf8-6c1e-485a-ab52-bb7883fbb8bb)


-> sky130_sne_inv

![image](https://github.com/user-attachments/assets/82c744e7-cdcd-4ebd-8340-52d8b11a1ecf)

![image](https://github.com/user-attachments/assets/033dbeeb-3ee3-4f28-ab1d-bf812dd9a009)

-> layout

![image](https://github.com/user-attachments/assets/6f34de2a-31f2-44f1-b7fb-41e56813bfef)


### Post-Synthesis timing analysis with OpenSTA tool

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing
Commands to invoke the OpenLANE flow include new lef and perform synthesis

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis
```

Newly created pre_sta.conf for STA analysis in openlane directory

-> Screenshot
![image](https://github.com/user-attachments/assets/1eaaa906-ddc8-4ce5-9579-6e4241a4eae6)


Newly created my_base.sdc for STA analysis in openlane/designs/picorv32a/src directory based on the file openlane/scripts/base.sdc

![image](https://github.com/user-attachments/assets/14ec3c99-6025-4fec-8678-072d2519e580)


-> Screenshot

![image](https://github.com/user-attachments/assets/e24e1d16-d9bb-4c7a-96e9-e6abe79550e2)


Commands to run STA in another terminal

-> Commands

```
# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

We are getting 0 violation design, clean design for further stages

-> CTS

```
run_cts
```

### Post-CTS OpenROAD timing analysis.
Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```
# Command to run OpenROAD tool
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_21-58/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_21-58/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs//13-11_21-58/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
help report_checks
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit
```


### Explore post-CTS OpenROAD timing analysis 

```
report_clock_skew -hold
report_clock_skew -setup
exit
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
echo $::env(CTS_CLK_BUFFER_LIST)
```


### Post-Route OpenSTA timing analysis
Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

-> Commands

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_21-58/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_21-58/results/routing/picorv32a.def
write_db pico_route.db
read_db pico_route.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_21-58/results/synthesis/picorv32a.synthesis_preroute.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
read_spef /openLANE_flow/designs/picorv32a/runs/13-11_21-58/results/routing/picorv32a.spef
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit
```

-> Screenshots

![image](https://github.com/user-attachments/assets/8a57302a-6897-4abe-9364-71b72c40ae21)


![image](https://github.com/user-attachments/assets/64fe68bb-16fc-4904-a727-5ebea9f5f19a)

</details>

<details>

<summary>Day-5</summary>

## Final steps for RTL2GDS using tritonRoute and openSTA

### Power Distribution Network generation

Power Distribution Network generation is not a part of floorplan run in OpenLANE. PDN must be generated after CTS and post-CTS STA analyses

Power rings,strapes and rails are created by PDN. From VDD and VSS pads, power is drawn to power rings. Next, the horizontal and vertical strapes connected to rings draw the power from strapes. Stapes are connected to rings and these rings are connected to std cells. So, standard cells get power from rails.

### Routing

In the realm of routing within Electronic Design Automation (EDA) tools, such as both OpenLANE and commercial EDA tools, the routing process is exceptionally intricate due to the vast design space. To simplify this complexity, the routing procedure is typically divided into two distinct stages: Global Routing and Detailed Routing.

The two routing engines responsible for handling these two stages are as follows:

Global Routing: In this stage, the routing region is subdivided into rectangular grid cells and represented as a coarse 3D routing graph. This task is accomplished by the "FASTE ROUTE" engine.

Detailed Routing: Here, finer grid granularity and routing guides are employed to implement the physical wiring. The "tritonRoute" engine comes into play at this stage. "Fast Route" generates initial routing guides, while "Triton Route" utilizes the Global Route information and further refines the routing, employing various strategies and optimizations to determine the most optimal path for connecting the pins.

### Generation of Power Distribution Network (PDN)


-> Commands 

```
run_pdn
```

![image](https://github.com/user-attachments/assets/bf58cbc7-cb52-4fdd-a584-40f02719f6e5)

![image](https://github.com/user-attachments/assets/014baa3f-52a2-44ac-b8d5-33fc800f417b)

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-58/tmp/floorplan/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```

![image](https://github.com/user-attachments/assets/b71fc332-3552-45ad-989c-f3fe6877df47)

![image](https://github.com/user-attachments/assets/f469b7c3-f4db-457e-bee0-f191f4086532)



### Perfrom detailed routing 

-> Commands

```
echo $::env(CURRENT_DEF)
echo $::env(ROUTING_STRATEGY)
run_routing
```

![image](https://github.com/user-attachments/assets/b7f5bea7-3d53-4f38-a797-ba45227db740)


-> Commands
```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-58/results/routing/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```

![image](https://github.com/user-attachments/assets/9cc865b0-9e59-4ddf-ac1b-cb90c85835d1)

![image](https://github.com/user-attachments/assets/7c85a48f-3abb-4771-8171-cc7eeb4a47e5)


### Post-Route parasitic extraction using SPEF extractor

-> Commands for SPEF extraction 

```
cd Desktop/work/tools/openlane_working_dir/openlane/scripts/spef_extraction

python3 main.py --lef_file /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-58/tmp/merged.lef --def_file /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_21-58/results/routing/picorv32a.def
```

![image](https://github.com/user-attachments/assets/dd363921-49c9-4010-a8f6-cb8e5a5e6b2d)


### Post-Route OpenSTA timing analysis with the extracted parasitics of the route

-> Commands 

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_21-58/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_21-58/results/routing/picorv32a.def
write_db pico_route.db
read_db pico_route.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_21-58/results/synthesis/picorv32a.synthesis_preroute.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
read_spef /openLANE_flow/designs/picorv32a/runs/13-11_21-58/results/routing/picorv32a.spef
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit
```

![image](https://github.com/user-attachments/assets/3a83ce66-56c2-4051-bcb3-c0b2e61e26f8)

![image](https://github.com/user-attachments/assets/be3a4b45-abb9-415c-9985-2c887bdf5e3a)



</details>

</details>

<details>

<summary>Assignment-13</summary>

<details>

<summary> OpenROAD Installation </summary>

## Installing and Setting up OpenROAD Flow scripts

OpenROAD-flow-scripts (ORFS) is a fully autonomous, RTL-GDSII flow for rapid architecture and design space exploration, early prediction of QoR and detailed physical design implementation. However, ORFS also enables manual intervention for finer user control of individual flow stages through Tcl commands and Python APIs.

The OpenROAD project supports two main flow controllers,

- OpenROAD-flow-scripts(ORFS) is a flow controller that provides a collection of open-source tools for automated digital ASIC design from synthesis to layout. It provides a fully automated RTL-to-GDSII design flow, which includes Synthesis, Placement and Routing (PnR), STA (Static Timing Analysis), DRC (Design Rule Check) and LVS (Layout Versus Schematic) checks. ORFS aims to provide a flexible and customizable environment for digital ASIC design, allowing users to choose and combine different tools as needed.

- In ORFS, OpenROAD is used as a plugin for the physical design stage, and it can be configured and customized to meet the specific needs of the design project. The OpenROAD plugin in ORFS provides access to several advanced features, such as hierarchical placement, global routing, and detailed routing optimization.

- ORFS supports several public and private PDKs (under NDA). Available public PDK's are GF180, Skywater130, ASAP7 etc.

- OpenLane is a complete automated RTL-to-GDSII flow similiar to ORFS and is developed by Efabless for the skywater130 MPW Program

#### -> Clone the following given link

```ruby
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts.git
```

![image](https://github.com/user-attachments/assets/2efa9913-2a3d-46ce-99e9-68594251cf1b)


#### -> Setup and Build

```ruby
cd OpenROAD-flow-scripts
sudo ./setup.sh
./build_openroad.sh --local
```

#### ->verify

```ruby
source ./env.sh
yosys -help
openroad -help
cd flow
make
```

![image](https://github.com/user-attachments/assets/02b66133-edce-40c8-a478-ebe5829eb17c)

![image](https://github.com/user-attachments/assets/ae77ac8b-b752-45e3-b6c2-78a0db670d52)

### Understanding the directory structure

```
├── OpenROAD-flow-scripts             
│   ├── docker           // It has Docker based installation, run scripts and all saved here
│   ├── docs             // Documentation for OpenROAD or its flow scripts.  
│   ├── flow             // Files related to run RTL to GDS flow  
|   ├── jenkins          // It contains the regression test designed for each build update
│   ├── tools            // It contains all the required tools to run RTL to GDS flow
│   ├── etc              // Has the dependency installer script and other things
│   ├── setup_env.sh     // Its the source file to source all our OpenROAD rules to run the RTL to GDS flow
```

```
├── flow           
│   ├── design           // It has built-in examples from RTL to GDS flow across different technology nodes
│   ├── Makefile         // The automated flow runs through makefile setup
│   ├── platform         // It has different technology note libraries, lef files, GDS etc 
|   ├── tutorials        
│   ├── util            
│   ├── scripts 	//Contains all the tcl scripts
│   ├── logs		// logs dir which contains screen logs of each design and each flow
│   ├── objects 	// Includes ABC constraints and temp lib files
│   ├── reports		// reports for each stage of design flow contains timing and DRC rpts
│   ├── results		// results o=of each stages has .v .sdc .odb .db files
```


### Running Example Design

#### -> Process 

Configuration: Once ORFS is installed, you can configure the framework to meet the specific needs of your design project. This involves specifying the design parameters, such as the target technology node, the design constraints, and the tool settings.

Design entry: You can enter the design into ORFS in different ways, depending on your design entry method. ORFS supports different input formats such as Verilog

Synthesis: The synthesis stage involves transforming the RTL design into a gate-level netlist. ORFS includes several open-source synthesis tools, such as Yosys and ABC, which can be used for this stage.

Floorplanning: In the floorplanning stage, the placement of the different design modules within the chip area is determined. ORFS includes several floorplanning tools, such as RePlAce and Capo, which can be used for this stage.

Placement: The placement stage involves determining the exact location of each gate or cell within the chip area. ORFS includes several placement tools, such as OpenROAD, which can be used for this stage.

Routing: The routing stage involves connecting the gates and cells using metal wires to form a complete circuit. ORFS includes several routing tools, such as FastRoute and TritonRoute, which can be used for this stage.

Layout verification: After the routing stage, the design is verified for layout correctness using tools such as Magic, which is included in ORFS.

GDSII generation: Once the design is verified, the final GDSII layout file is generated using ORFS tools such as Magic and KLayout.

#### -> Commands to run the flow

```
make DESIGN_CONFIG=./designs/nangate45/gcd/config.mk synth 		//synthesis
make DESIGN_CONFIG=./designs/nangate45/gcd/config.mk floorplan 		//floorplan
make DESIGN_CONFIG=./designs/nangate45/gcd/config.mk place 		//placement
make DESIGN_CONFIG=./designs/nangate45/gcd/config.mk cts 		//clock tree synthesis
make DESIGN_CONFIG=./designs/nangate45/gcd/config.mk route 		//routing the nets
make DESIGN_CONFIG=./designs/nangate45/gcd/config.mk final 		//final step
```

One more option to run the design flow is by adding  ```DESIGN_CONFIG=./designs/nangate45/gcd/config.mk``` in the Makefile and run following commands

```
make synth 		//synthesis
make floorplan 		//floorplan
make all		// for running the entire flow
```

#### -> Command to view the design in GUI

```
make DESIGN_CONFIG=./designs/nangate45/gcd/config.mk gui_final 		//to view final design in gui
						OR
openroad -gui
						OR
make gui_final			//Add DESIGN_CONFIG=./designs/nangate45/gcd/config.mk in the Makefile and run following commands
```

#### -> GUI 

![image](https://github.com/user-attachments/assets/f81703d0-6b47-40e3-a600-caea2b1e3917)


</details>

<details>
<summary> rvmyth_core </summary>

## Understanding physical design steps and running flow for rvmyth core

#### -> Initial Steps:

- As discussed previously we will configure the design by the following directory structure
- First we need to create a directory vsdbabysoc inside
  ```OpenROAD-flow-scripts/flow/designs/sky130hd/<design_name>``` and ```OpenROAD-flow-scripts/flow/designs/src/<design_name>```

```
├── flow           
│   ├── design          
|	│   ├── sky130hd         
|	|	|   ├── config.mk        
|	|	│   ├── constraints.sdc            
|	│   ├── src 	
|	|	│   ├── <design_name>		
|	|	|	│   ├── <design_files_.v> 	
|	|	|	│   ├── lef		
|	|	|	│   ├── def		
|	|	|	│   ├── gds
|	|	|	│   ├── include
|	|	|	│   ├── cfg
```

  
- Now copy the folders gds, include, lef and lib from the design folder in your system into this directory. - The gds folder would contain the files avsddac.gds and avsdpll.gds - The include folder would contain the files sandpiper.vh, sandpiper_gen.vh, sp_default.vh and sp_verilog.vh - The gds folder would contain the files avsddac.lef and avsdpll.lef - The lib folder would contain the files avsddac.lib and avsdpll.lib as shown above
- Copy the constraints file(constraints.sdc) from the design folder in your system into this directory.
- Copy the files(macro.cfg and pin_order.cfg) from the design folder in your system into this directory.

Following ```config.mk``` file is used

```
export DESIGN_NICKNAME = rvmyth_sne
export DESIGN_NAME = rvmyth_sne
export PLATFORM    = sky130hd

export ADDITIONAL_LIBS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lib/avsddac.lib \
						 $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lib/avsdpll.lib

export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/rvmyth_sne.v \
					   $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/include/clk_gate.v

export VERILOG_INCLUDE_DIRS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/include

export SDC_FILE      = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/constraint.sdc

export ADDITIONAL_LEFS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lef/avsddac.lef \
 						 $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lef/avsdpll.lef

export ADDITIONAL_GDS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/gds/avsddac.gds \
						$(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/gds/avsdpll.gds

#export MACRO_PLACEMENT = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/cfg/macro.cfg

export DIE_AREA   = 0 0 650 620
export CORE_AREA  = 20 20 640 610

export MACRO_PLACE_HALO = 50 50
export MACRO_PLACE_CHANNEL = 70 70
export TNS_END_PERCENT = 100

export REMOVE_ABC_BUFFERS = 1
```

.sdc file

```
set_units -time ns
set PERIOD 11.9

// clock generation
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD

//clock constraints
set_clock_uncertainty [expr 0.05 * $PERIOD] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * $PERIOD] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * $PERIOD] [get_clocks clk]

//input constraints
set_input_transition [expr $PERIOD * 0.08] [get_ports reset]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

## Synthesis

![image](https://github.com/user-attachments/assets/31462b12-a8a9-446f-b410-30180137be1f)

```

24. Printing statistics.

=== rvmyth_sne ===

   Number of wires:               7191
   Number of wire bits:           7200
   Number of public wires:        1401
   Number of public wire bits:    1410
   Number of ports:                  3
   Number of port bits:             12
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:               7060
     sky130_fd_sc_hd__a2111o_1       1
     sky130_fd_sc_hd__a2111oi_0      3
     sky130_fd_sc_hd__a2111oi_1      1
     sky130_fd_sc_hd__a2111oi_2      2
     sky130_fd_sc_hd__a211o_1        8
     sky130_fd_sc_hd__a211oi_1      19
     sky130_fd_sc_hd__a211oi_2       2
     sky130_fd_sc_hd__a21boi_0       7
     sky130_fd_sc_hd__a21boi_1       2
     sky130_fd_sc_hd__a21o_1        15
     sky130_fd_sc_hd__a21oi_1      828
     sky130_fd_sc_hd__a221o_1        5
     sky130_fd_sc_hd__a221oi_1      95
     sky130_fd_sc_hd__a221oi_2       1
     sky130_fd_sc_hd__a22o_1        85
     sky130_fd_sc_hd__a22oi_1      499
     sky130_fd_sc_hd__a22oi_2        3
     sky130_fd_sc_hd__a2bb2oi_1      1
     sky130_fd_sc_hd__a311o_1        1
     sky130_fd_sc_hd__a311oi_1      56
     sky130_fd_sc_hd__a31o_2         2
     sky130_fd_sc_hd__a31oi_1       16
     sky130_fd_sc_hd__a32oi_2        1
     sky130_fd_sc_hd__a41oi_1        2
     sky130_fd_sc_hd__and2_0         1
     sky130_fd_sc_hd__and2_1         5
     sky130_fd_sc_hd__and3_1        53
     sky130_fd_sc_hd__and4_1         4
     sky130_fd_sc_hd__and4bb_4       1
     sky130_fd_sc_hd__buf_1         50
     sky130_fd_sc_hd__buf_2         32
     sky130_fd_sc_hd__buf_4          4
     sky130_fd_sc_hd__buf_6          1
     sky130_fd_sc_hd__clkbuf_1     570
     sky130_fd_sc_hd__clkbuf_2       1
     sky130_fd_sc_hd__conb_1         1
     sky130_fd_sc_hd__dfxtp_1     1274
     sky130_fd_sc_hd__fa_1           3
     sky130_fd_sc_hd__ha_1         135
     sky130_fd_sc_hd__inv_1        143
     sky130_fd_sc_hd__inv_2          2
     sky130_fd_sc_hd__mux2_2         7
     sky130_fd_sc_hd__mux2_4         1
     sky130_fd_sc_hd__mux2i_1       49
     sky130_fd_sc_hd__mux2i_2        1
     sky130_fd_sc_hd__nand2_1     1395
     sky130_fd_sc_hd__nand2b_1      32
     sky130_fd_sc_hd__nand3_1      315
     sky130_fd_sc_hd__nand3b_1      35
     sky130_fd_sc_hd__nand4_1      121
     sky130_fd_sc_hd__nand4b_1       1
     sky130_fd_sc_hd__nand4bb_1      1
     sky130_fd_sc_hd__nor2_1       282
     sky130_fd_sc_hd__nor2_2         8
     sky130_fd_sc_hd__nor2b_1       53
     sky130_fd_sc_hd__nor3_1        56
     sky130_fd_sc_hd__nor3_2         2
     sky130_fd_sc_hd__nor3b_1        3
     sky130_fd_sc_hd__nor4_1        29
     sky130_fd_sc_hd__nor4b_1        1
     sky130_fd_sc_hd__o2111a_1       1
     sky130_fd_sc_hd__o2111ai_1      1
     sky130_fd_sc_hd__o211a_1        4
     sky130_fd_sc_hd__o211ai_1      57
     sky130_fd_sc_hd__o211ai_2       1
     sky130_fd_sc_hd__o21a_1        23
     sky130_fd_sc_hd__o21ai_0      266
     sky130_fd_sc_hd__o21ai_1        4
     sky130_fd_sc_hd__o21ai_4        1
     sky130_fd_sc_hd__o21ba_2        3
     sky130_fd_sc_hd__o21bai_1      22
     sky130_fd_sc_hd__o221ai_1      81
     sky130_fd_sc_hd__o22a_1        32
     sky130_fd_sc_hd__o22ai_1       73
     sky130_fd_sc_hd__o22ai_2        1
     sky130_fd_sc_hd__o2bb2ai_1      1
     sky130_fd_sc_hd__o311ai_0       2
     sky130_fd_sc_hd__o311ai_1       2
     sky130_fd_sc_hd__o311ai_2       1
     sky130_fd_sc_hd__o31a_1         2
     sky130_fd_sc_hd__o31ai_1       45
     sky130_fd_sc_hd__o31ai_2        3
     sky130_fd_sc_hd__o32a_1         2
     sky130_fd_sc_hd__o32ai_4        1
     sky130_fd_sc_hd__o41ai_1        5
     sky130_fd_sc_hd__or2_1          1
     sky130_fd_sc_hd__or2_2          3
     sky130_fd_sc_hd__or3_1         26
     sky130_fd_sc_hd__or4_1          4
     sky130_fd_sc_hd__xnor2_1       57
     sky130_fd_sc_hd__xor2_1         8

   Chip area for module '\rvmyth_sne': 57111.024000
     of which used for sequential elements: 25504.460800 (44.66%)

```

## Floorplan

The primary objectives of floorplanning are to minimize area, timing, wire length, and power consumption while ensuring easy routing and reliability.

-> Running Floorplan

![image](https://github.com/user-attachments/assets/4a46c0ae-63c7-4662-b4a7-d30928b60bc8)

-> VDD nets after power grid generation

![image](https://github.com/user-attachments/assets/af4a284b-fe92-4645-9985-ddb7f2b835e6)

-> VSS nets after power grid generation

![image](https://github.com/user-attachments/assets/2876126f-dc2c-467b-9a71-aedc599d91e1)

## Placement

#### Steps in Placement:

Placement can be divided into several key stages:
  
- Pre-Placement: This initial stage involves checks and the placement of physical-only cells (e.g., end-cap cells, well-tap cells) to ensure a clean design environment before standard cell placement begins.

- Global Placement: During this phase, the tool determines approximate locations for each standard cell based on various constraints such as timing, congestion, and power. This step does not enforce design rule checks (DRCs), allowing for overlaps.

- Legalization: After global placement, the tool adjusts the positions of cells to eliminate overlaps and ensure that all cells are in legal locations according to the design rules. This process may involve flipping cells to match power pin requirements.

- High Fanout Net Synthesis (HFNS): This step addresses nets with high fanout by distributing the load across multiple drivers and adding buffers as necessary to meet timing constraints[1][2].

- Post-Placement Optimization: After initial placement, further iterations are conducted to refine the design, focusing on timing, congestion, and power optimizations. This may include techniques like scan-chain reordering and tie cell insertion.



-> Running Placement

![image](https://github.com/user-attachments/assets/d1fa293e-a5e6-47c0-bdcf-4bb676b58177)

![image](https://github.com/user-attachments/assets/01bcda2f-e7de-477c-9bc5-a21c872d7a62)

-> Placement Density

![image](https://github.com/user-attachments/assets/d65b6a75-cb2b-42c9-a669-343fe6c49936)

-> Resizing

![image](https://github.com/user-attachments/assets/341c9858-6a95-46eb-a13f-04384cd6eaf9)

```

==========================================================================
resizer report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
resizer report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
resizer report_worst_slack
--------------------------------------------------------------------------
worst slack 7.29

==========================================================================
resizer report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: CPU_is_add_a2$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_is_add_a3$_DFF_P_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ CPU_is_add_a2$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.02    0.42    0.42 v CPU_is_add_a2$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_is_add_a2 (net)
                  0.02    0.00    0.42 v CPU_is_add_a3$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  0.42   data arrival time

                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.95    0.95   clock uncertainty
                          0.00    0.95   clock reconvergence pessimism
                                  0.95 ^ CPU_is_add_a3$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                          0.07    1.02   library hold time
                                  1.02   data required time
-----------------------------------------------------------------------------
                                  1.02   data required time
                                 -0.42   data arrival time
-----------------------------------------------------------------------------
                                 -0.60   slack (VIOLATED)



==========================================================================
resizer report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.04    0.44    0.44 v CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_valid_load_a5 (net)
                  0.04    0.00    0.44 v _05790_/A (sky130_fd_sc_hd__or4_4)
    38    0.36    0.50    0.96    1.40 v _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  0.50    0.01    1.41 v _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.48    0.36    0.43    1.84 ^ _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.36    0.01    1.84 ^ _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.15    0.20    2.04 v _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.15    0.00    2.04 v _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.01    0.09    0.25    2.29 v _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.09    0.00    2.29 v _07959_/A (sky130_fd_sc_hd__or2_4)
     5    0.06    0.12    0.35    2.64 v _07959_/X (sky130_fd_sc_hd__or2_4)
                                         _02574_ (net)
                  0.12    0.00    2.65 v _09231_/B (sky130_fd_sc_hd__nor3_4)
    19    0.12    1.14    0.93    3.57 ^ _09231_/Y (sky130_fd_sc_hd__nor3_4)
                                         _03619_ (net)
                  1.14    0.01    3.58 ^ _09260_/B (sky130_fd_sc_hd__nand2_4)
     8    0.04    0.21    0.21    3.79 v _09260_/Y (sky130_fd_sc_hd__nand2_4)
                                         _03640_ (net)
                  0.21    0.00    3.79 v _09305_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.19    0.21    4.00 ^ _09305_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _00792_ (net)
                  0.19    0.00    4.00 ^ CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.00   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02   11.29   library setup time
                                 11.29   data required time
-----------------------------------------------------------------------------
                                 11.29   data required time
                                 -4.00   data arrival time
-----------------------------------------------------------------------------
                                  7.29   slack (MET)



==========================================================================
resizer report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.04    0.44    0.44 v CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_valid_load_a5 (net)
                  0.04    0.00    0.44 v _05790_/A (sky130_fd_sc_hd__or4_4)
    38    0.36    0.50    0.96    1.40 v _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  0.50    0.01    1.41 v _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.48    0.36    0.43    1.84 ^ _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.36    0.01    1.84 ^ _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.15    0.20    2.04 v _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.15    0.00    2.04 v _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.01    0.09    0.25    2.29 v _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.09    0.00    2.29 v _07959_/A (sky130_fd_sc_hd__or2_4)
     5    0.06    0.12    0.35    2.64 v _07959_/X (sky130_fd_sc_hd__or2_4)
                                         _02574_ (net)
                  0.12    0.00    2.65 v _09231_/B (sky130_fd_sc_hd__nor3_4)
    19    0.12    1.14    0.93    3.57 ^ _09231_/Y (sky130_fd_sc_hd__nor3_4)
                                         _03619_ (net)
                  1.14    0.01    3.58 ^ _09260_/B (sky130_fd_sc_hd__nand2_4)
     8    0.04    0.21    0.21    3.79 v _09260_/Y (sky130_fd_sc_hd__nand2_4)
                                         _03640_ (net)
                  0.21    0.00    3.79 v _09305_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.19    0.21    4.00 ^ _09305_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _00792_ (net)
                  0.19    0.00    4.00 ^ CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.00   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.02   11.29   library setup time
                                 11.29   data required time
-----------------------------------------------------------------------------
                                 11.29   data required time
                                 -4.00   data arrival time
-----------------------------------------------------------------------------
                                  7.29   slack (MET)



==========================================================================
resizer report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
resizer max_slew_check_slack
--------------------------------------------------------------------------
0.34648385643959045

==========================================================================
resizer max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
resizer max_slew_check_slack_limit
--------------------------------------------------------------------------
0.2310

==========================================================================
resizer max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
resizer max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
resizer max_capacitance_check_slack
--------------------------------------------------------------------------
0.024533407762646675

==========================================================================
resizer max_capacitance_check_limit
--------------------------------------------------------------------------
0.031137999147176743

==========================================================================
resizer max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.7879

==========================================================================
resizer max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
resizer max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
resizer max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
resizer setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
resizer hold_violation_count
--------------------------------------------------------------------------
hold violation count 1263

==========================================================================
resizer report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.44    0.44 v CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.96    1.40 v _05790_/X (sky130_fd_sc_hd__or4_4)
   0.43    1.84 ^ _05791_/Y (sky130_fd_sc_hd__clkinv_16)
   0.21    2.04 v _07870_/Y (sky130_fd_sc_hd__o211ai_1)
   0.25    2.29 v _07871_/X (sky130_fd_sc_hd__o22a_1)
   0.35    2.64 v _07959_/X (sky130_fd_sc_hd__or2_4)
   0.93    3.57 ^ _09231_/Y (sky130_fd_sc_hd__nor3_4)
   0.22    3.79 v _09260_/Y (sky130_fd_sc_hd__nand2_4)
   0.21    4.00 ^ _09305_/Y (sky130_fd_sc_hd__o221ai_1)
   0.00    4.00 ^ CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           4.00   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock network delay (ideal)
  -0.60   11.31   clock uncertainty
   0.00   11.31   clock reconvergence pessimism
          11.31 ^ CPU_Xreg_value_a4[2][31]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.02   11.29   library setup time
          11.29   data required time
---------------------------------------------------------
          11.29   data required time
          -4.00   data arrival time
---------------------------------------------------------
           7.29   slack (MET)



==========================================================================
resizer report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_is_add_a2$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_is_add_a3$_DFF_P_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ CPU_is_add_a2$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.42    0.42 v CPU_is_add_a2$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.00    0.42 v CPU_is_add_a3$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
           0.42   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.95    0.95   clock uncertainty
   0.00    0.95   clock reconvergence pessimism
           0.95 ^ CPU_is_add_a3$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.07    1.02   library hold time
           1.02   data required time
---------------------------------------------------------
           1.02   data required time
          -0.42   data arrival time
---------------------------------------------------------
          -0.60   slack (VIOLATED)



==========================================================================
resizer critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
resizer critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
resizer critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
resizer critical path delay
--------------------------------------------------------------------------
3.9999

==========================================================================
resizer critical path slack
--------------------------------------------------------------------------
7.2876

==========================================================================
resizer slack div critical path delay
--------------------------------------------------------------------------
182.194555

==========================================================================
resizer report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.76e-03   7.40e-04   1.04e-08   5.50e-03  56.7%
Combinational          1.33e-03   2.86e-03   1.11e-08   4.19e-03  43.3%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  6.09e-03   3.60e-03   2.15e-08   9.69e-03 100.0%
                          62.8%      37.2%       0.0%
```

## CTS

CTS involves connecting the clock signal from the clock port to the clock pins of sequential cells while minimizing insertion delay and balancing skew. The clock network is typically categorized as a high fanout net, which requires special handling due to its significant power consumption

![image](https://github.com/user-attachments/assets/2be044ed-93b9-455f-8f57-2452d841e53e)

-> Clock Tree nets

![image](https://github.com/user-attachments/assets/39913658-1d2e-48cc-8703-904f72c8d180)

-> Clock Tree Connections

![image](https://github.com/user-attachments/assets/b6ad1e2e-5e63-44bd-b30f-0471c60420f3)

```

==========================================================================
cts final report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
cts final report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
cts final report_worst_slack
--------------------------------------------------------------------------
worst slack 6.55

==========================================================================
cts final report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.86 source latency CPU_src2_value_a3[4]$_DFF_P_/CLK ^
  -0.83 target latency CPU_Xreg_value_a4[13][27]$_SDFFE_PP0P_/CLK ^
   0.60 clock uncertainty
   0.00 CRPR
--------------
   0.63 setup skew


==========================================================================
cts final report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: CPU_dmem_rd_data_a5[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[5][11]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.37    0.38    0.36    0.36 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.38    0.00    0.36 ^ clkbuf_4_6_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.14    0.16    0.31    0.67 ^ clkbuf_4_6_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_6_0_clk (net)
                  0.16    0.00    0.67 ^ clkbuf_leaf_99_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.04    0.06    0.18    0.86 ^ clkbuf_leaf_99_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_99_clk (net)
                  0.06    0.00    0.86 ^ CPU_dmem_rd_data_a5[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.02    0.20    0.42    1.28 ^ CPU_dmem_rd_data_a5[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_dmem_rd_data_a5[11] (net)
                  0.20    0.00    1.28 ^ _08048_/A0 (sky130_fd_sc_hd__mux2_8)
    15    0.08    0.14    0.29    1.57 ^ _08048_/X (sky130_fd_sc_hd__mux2_8)
                                         _02662_ (net)
                  0.14    0.01    1.57 ^ _09533_/A (sky130_fd_sc_hd__nand2_1)
     1    0.00    0.05    0.06    1.64 v _09533_/Y (sky130_fd_sc_hd__nand2_1)
                                         _03823_ (net)
                  0.05    0.00    1.64 v _09534_/A2 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.08    0.13    1.77 ^ _09534_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _00866_ (net)
                  0.08    0.00    1.77 ^ CPU_Xreg_value_a4[5][11]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.77   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.37    0.38    0.36    0.36 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.38    0.00    0.36 ^ clkbuf_4_14_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.14    0.15    0.31    0.67 ^ clkbuf_4_14_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_14_0_clk (net)
                  0.15    0.00    0.67 ^ clkbuf_leaf_53_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.18    0.85 ^ clkbuf_leaf_53_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_53_clk (net)
                  0.06    0.00    0.85 ^ CPU_Xreg_value_a4[5][11]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.95    1.80   clock uncertainty
                          0.00    1.80   clock reconvergence pessimism
                         -0.04    1.77   library hold time
                                  1.77   data required time
-----------------------------------------------------------------------------
                                  1.77   data required time
                                 -1.77   data arrival time
-----------------------------------------------------------------------------
                                  0.00   slack (MET)



==========================================================================
cts final report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.37    0.38    0.36    0.36 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.38    0.00    0.36 ^ clkbuf_4_13_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.14    0.30    0.66 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13_0_clk (net)
                  0.14    0.00    0.66 ^ clkbuf_leaf_65_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.04    0.06    0.18    0.84 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_65_clk (net)
                  0.06    0.00    0.84 ^ CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.07    0.33    1.17 ^ CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_valid_load_a5 (net)
                  0.07    0.00    1.17 ^ _05790_/A (sky130_fd_sc_hd__or4_4)
    38    0.38    1.08    0.85    2.02 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  1.08    0.01    2.02 ^ _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.46    0.49    0.51    2.53 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.49    0.00    2.53 v _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.25    0.32    2.85 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.25    0.00    2.85 ^ _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.01    0.15    0.24    3.10 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.15    0.00    3.10 ^ _08747_/A_N (sky130_fd_sc_hd__nand2b_4)
     5    0.06    0.18    0.25    3.35 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
                                         _03295_ (net)
                  0.18    0.00    3.35 ^ _09140_/B (sky130_fd_sc_hd__or3_4)
    48    0.32    0.90    0.75    4.10 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
                                         _03559_ (net)
                  0.91    0.01    4.11 ^ _09171_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.13    0.08    4.19 v _09171_/Y (sky130_fd_sc_hd__nor2_1)
                                         _03580_ (net)
                  0.13    0.00    4.19 v _09172_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.13    0.14    4.33 ^ _09172_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _03581_ (net)
                  0.13    0.00    4.33 ^ hold1834/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.56    4.89 ^ hold1834/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1957 (net)
                  0.06    0.00    4.89 ^ _09173_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.10    0.04    4.92 v _09173_/Y (sky130_fd_sc_hd__nor2_1)
                                         _00747_ (net)
                  0.10    0.00    4.92 v hold1835/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.49 v hold1835/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1958 (net)
                  0.05    0.00    5.49 v CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.49   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     1    0.10    0.00    0.00   11.90 ^ clk (in)
                                         clk (net)
                  0.01    0.00   11.90 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.37    0.38    0.36   12.26 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.38    0.00   12.26 ^ clkbuf_4_7_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.14    0.15    0.31   12.57 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_clk (net)
                  0.15    0.00   12.57 ^ clkbuf_leaf_81_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.04    0.06    0.18   12.75 ^ clkbuf_leaf_81_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_81_clk (net)
                  0.06    0.00   12.75 ^ CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.60   12.16   clock uncertainty
                          0.00   12.16   clock reconvergence pessimism
                         -0.11   12.05   library setup time
                                 12.05   data required time
-----------------------------------------------------------------------------
                                 12.05   data required time
                                 -5.49   data arrival time
-----------------------------------------------------------------------------
                                  6.55   slack (MET)



==========================================================================
cts final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.37    0.38    0.36    0.36 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.38    0.00    0.36 ^ clkbuf_4_13_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.14    0.30    0.66 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13_0_clk (net)
                  0.14    0.00    0.66 ^ clkbuf_leaf_65_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.04    0.06    0.18    0.84 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_65_clk (net)
                  0.06    0.00    0.84 ^ CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.07    0.33    1.17 ^ CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_valid_load_a5 (net)
                  0.07    0.00    1.17 ^ _05790_/A (sky130_fd_sc_hd__or4_4)
    38    0.38    1.08    0.85    2.02 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  1.08    0.01    2.02 ^ _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.46    0.49    0.51    2.53 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.49    0.00    2.53 v _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.25    0.32    2.85 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.25    0.00    2.85 ^ _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.01    0.15    0.24    3.10 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.15    0.00    3.10 ^ _08747_/A_N (sky130_fd_sc_hd__nand2b_4)
     5    0.06    0.18    0.25    3.35 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
                                         _03295_ (net)
                  0.18    0.00    3.35 ^ _09140_/B (sky130_fd_sc_hd__or3_4)
    48    0.32    0.90    0.75    4.10 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
                                         _03559_ (net)
                  0.91    0.01    4.11 ^ _09171_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.13    0.08    4.19 v _09171_/Y (sky130_fd_sc_hd__nor2_1)
                                         _03580_ (net)
                  0.13    0.00    4.19 v _09172_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.13    0.14    4.33 ^ _09172_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _03581_ (net)
                  0.13    0.00    4.33 ^ hold1834/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.56    4.89 ^ hold1834/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1957 (net)
                  0.06    0.00    4.89 ^ _09173_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.10    0.04    4.92 v _09173_/Y (sky130_fd_sc_hd__nor2_1)
                                         _00747_ (net)
                  0.10    0.00    4.92 v hold1835/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.49 v hold1835/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1958 (net)
                  0.05    0.00    5.49 v CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.49   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     1    0.10    0.00    0.00   11.90 ^ clk (in)
                                         clk (net)
                  0.01    0.00   11.90 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.37    0.38    0.36   12.26 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.38    0.00   12.26 ^ clkbuf_4_7_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.14    0.15    0.31   12.57 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_clk (net)
                  0.15    0.00   12.57 ^ clkbuf_leaf_81_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.04    0.06    0.18   12.75 ^ clkbuf_leaf_81_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_81_clk (net)
                  0.06    0.00   12.75 ^ CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.60   12.16   clock uncertainty
                          0.00   12.16   clock reconvergence pessimism
                         -0.11   12.05   library setup time
                                 12.05   data required time
-----------------------------------------------------------------------------
                                 12.05   data required time
                                 -5.49   data arrival time
-----------------------------------------------------------------------------
                                  6.55   slack (MET)



==========================================================================
cts final report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
cts final max_slew_check_slack
--------------------------------------------------------------------------
0.33483871817588806

==========================================================================
cts final max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
cts final max_slew_check_slack_limit
--------------------------------------------------------------------------
0.2232

==========================================================================
cts final max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_capacitance_check_slack
--------------------------------------------------------------------------
0.024760598316788673

==========================================================================
cts final max_capacitance_check_limit
--------------------------------------------------------------------------
0.031137999147176743

==========================================================================
cts final max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.7952

==========================================================================
cts final max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
cts final max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
cts final max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
cts final setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
cts final hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
cts final report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.36    0.36 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.30    0.66 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.84 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.84 ^ CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.33    1.17 ^ CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.85    2.02 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
   0.51    2.53 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
   0.32    2.85 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
   0.24    3.10 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
   0.25    3.35 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
   0.75    4.10 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
   0.09    4.19 v _09171_/Y (sky130_fd_sc_hd__nor2_1)
   0.14    4.33 ^ _09172_/Y (sky130_fd_sc_hd__a21oi_1)
   0.56    4.89 ^ hold1834/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.04    4.92 v _09173_/Y (sky130_fd_sc_hd__nor2_1)
   0.57    5.49 v hold1835/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.00    5.49 v CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           5.49   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock source latency
   0.00   11.90 ^ clk (in)
   0.36   12.26 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31   12.57 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18   12.75 ^ clkbuf_leaf_81_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   12.75 ^ CPU_Xreg_value_a4[1][1]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.60   12.16   clock uncertainty
   0.00   12.16   clock reconvergence pessimism
  -0.11   12.05   library setup time
          12.05   data required time
---------------------------------------------------------
          12.05   data required time
          -5.49   data arrival time
---------------------------------------------------------
           6.55   slack (MET)



==========================================================================
cts final report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_dmem_rd_data_a5[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[5][11]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.36    0.36 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31    0.67 ^ clkbuf_4_6_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.86 ^ clkbuf_leaf_99_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.86 ^ CPU_dmem_rd_data_a5[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.42    1.28 ^ CPU_dmem_rd_data_a5[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.29    1.57 ^ _08048_/X (sky130_fd_sc_hd__mux2_8)
   0.07    1.64 v _09533_/Y (sky130_fd_sc_hd__nand2_1)
   0.13    1.77 ^ _09534_/Y (sky130_fd_sc_hd__a21oi_1)
   0.00    1.77 ^ CPU_Xreg_value_a4[5][11]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.77   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.36    0.36 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31    0.67 ^ clkbuf_4_14_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.85 ^ clkbuf_leaf_53_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.85 ^ CPU_Xreg_value_a4[5][11]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.95    1.80   clock uncertainty
   0.00    1.80   clock reconvergence pessimism
  -0.04    1.77   library hold time
           1.77   data required time
---------------------------------------------------------
           1.77   data required time
          -1.77   data arrival time
---------------------------------------------------------
           0.00   slack (MET)



==========================================================================
cts final critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path delay
--------------------------------------------------------------------------
5.4921

==========================================================================
cts final critical path slack
--------------------------------------------------------------------------
6.5547

==========================================================================
cts final slack div critical path delay
--------------------------------------------------------------------------
119.347790

==========================================================================
cts final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.67e-03   6.06e-04   1.04e-08   5.28e-03  36.3%
Combinational          1.73e-03   3.09e-03   2.44e-08   4.81e-03  33.1%
Clock                  2.45e-03   2.00e-03   2.20e-09   4.45e-03  30.6%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  8.85e-03   5.69e-03   3.70e-08   1.45e-02 100.0%
                          60.8%      39.2%       0.0%
```
## Route

#### Routing Flow

The routing flow consists of four main stages:

Global Routing: Divides the core area into global cells (gcells) and finds the shortest path for each net using algorithms like maze routing and Steiner tree. It assigns nets to specific gcells but does not define actual tracks.

Track Assignment: Assigns actual metal layers to global routes while fixing some design rule violations. However, many DRC, signal integrity, and timing violations still remain.

Detailed Routing: Completes the actual metal and via connections between pins. It fixes all remaining violations through multiple iterations. The block is divided into switch boxes (Sboxes) for routing.

Search and Repair: Performed after the first detailed routing iteration to locate and fix any remaining shorts or spacing violations[2][3].


![image](https://github.com/user-attachments/assets/875722ac-119c-4e42-bf9c-39371d159710)

-> Connectivity

![image](https://github.com/user-attachments/assets/e99fd93a-96cc-4e51-883e-cd3351df2196)

```

==========================================================================
global route report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
global route report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
global route report_worst_slack
--------------------------------------------------------------------------
worst slack 6.13

==========================================================================
global route report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.85 source latency CPU_src2_value_a3[9]$_DFF_P_/CLK ^
  -0.83 target latency CPU_src1_value_a3[10]$_DFF_P_/CLK ^
   0.60 clock uncertainty
   0.00 CRPR
--------------
   0.62 setup skew


==========================================================================
global route report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: CPU_dmem_rd_data_a5[10]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[14][10]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.11    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.36    0.36    0.34    0.34 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.36    0.01    0.35 ^ clkbuf_4_6_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.14    0.15    0.30    0.66 ^ clkbuf_4_6_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_6_0_clk (net)
                  0.15    0.00    0.66 ^ clkbuf_leaf_99_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.05    0.07    0.18    0.84 ^ clkbuf_leaf_99_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_99_clk (net)
                  0.07    0.00    0.85 ^ CPU_dmem_rd_data_a5[10]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.02    0.11    0.37    1.22 v CPU_dmem_rd_data_a5[10]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_dmem_rd_data_a5[10] (net)
                  0.11    0.00    1.22 v _08007_/A0 (sky130_fd_sc_hd__mux2_8)
    15    0.07    0.11    0.39    1.61 v _08007_/X (sky130_fd_sc_hd__mux2_8)
                                         _02622_ (net)
                  0.11    0.00    1.61 v _08954_/A (sky130_fd_sc_hd__nand2_1)
     1    0.00    0.05    0.08    1.69 ^ _08954_/Y (sky130_fd_sc_hd__nand2_1)
                                         _03437_ (net)
                  0.05    0.00    1.69 ^ _08955_/A2 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.04    0.06    1.76 v _08955_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _00673_ (net)
                  0.04    0.00    1.76 v CPU_Xreg_value_a4[14][10]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.76   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.11    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.36    0.36    0.34    0.34 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.36    0.01    0.35 ^ clkbuf_4_14_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.13    0.15    0.30    0.65 ^ clkbuf_4_14_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_14_0_clk (net)
                  0.15    0.00    0.66 ^ clkbuf_leaf_52_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.05    0.07    0.18    0.84 ^ clkbuf_leaf_52_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_52_clk (net)
                  0.07    0.00    0.84 ^ CPU_Xreg_value_a4[14][10]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.95    1.79   clock uncertainty
                          0.00    1.79   clock reconvergence pessimism
                         -0.05    1.75   library hold time
                                  1.75   data required time
-----------------------------------------------------------------------------
                                  1.75   data required time
                                 -1.76   data arrival time
-----------------------------------------------------------------------------
                                  0.01   slack (MET)



==========================================================================
global route report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.11    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.36    0.36    0.34    0.34 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.36    0.01    0.35 ^ clkbuf_4_13_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.14    0.30    0.65 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13_0_clk (net)
                  0.14    0.00    0.65 ^ clkbuf_leaf_65_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.05    0.07    0.18    0.83 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_65_clk (net)
                  0.07    0.00    0.84 ^ CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.10    0.35    1.19 ^ CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_dmem_rd_en_a4 (net)
                  0.10    0.00    1.19 ^ _05790_/D (sky130_fd_sc_hd__or4_4)
    38    0.40    1.13    0.88    2.06 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  1.13    0.01    2.08 ^ _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.47    0.51    0.52    2.59 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.51    0.01    2.61 v _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.29    0.36    2.96 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.29    0.00    2.96 ^ _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.02    0.16    0.26    3.22 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.16    0.00    3.22 ^ _08747_/A_N (sky130_fd_sc_hd__nand2b_4)
     5    0.06    0.18    0.26    3.48 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
                                         _03295_ (net)
                  0.18    0.00    3.48 ^ _09140_/B (sky130_fd_sc_hd__or3_4)
    48    0.35    0.98    0.80    4.28 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
                                         _03559_ (net)
                  0.98    0.03    4.31 ^ _09216_/B (sky130_fd_sc_hd__nor2_1)
     1    0.01    0.17    0.14    4.45 v _09216_/Y (sky130_fd_sc_hd__nor2_1)
                                         _03609_ (net)
                  0.17    0.00    4.45 v _09217_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.01    0.18    0.20    4.64 ^ _09217_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _03610_ (net)
                  0.18    0.00    4.64 ^ hold1846/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.01    0.09    0.61    5.25 ^ hold1846/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1969 (net)
                  0.09    0.00    5.25 ^ _09218_/B (sky130_fd_sc_hd__nor2_1)
     1    0.01    0.13    0.06    5.31 v _09218_/Y (sky130_fd_sc_hd__nor2_1)
                                         _00763_ (net)
                  0.13    0.00    5.31 v hold1847/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.59    5.90 v hold1847/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1970 (net)
                  0.06    0.00    5.90 v CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  5.90   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     1    0.11    0.00    0.00   11.90 ^ clk (in)
                                         clk (net)
                  0.01    0.00   11.90 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.36    0.36    0.34   12.24 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.36    0.01   12.25 ^ clkbuf_4_7_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.15    0.30   12.55 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_clk (net)
                  0.15    0.00   12.56 ^ clkbuf_leaf_76_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.05    0.07    0.18   12.74 ^ clkbuf_leaf_76_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_76_clk (net)
                  0.07    0.00   12.74 ^ CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                         -0.60   12.15   clock uncertainty
                          0.00   12.15   clock reconvergence pessimism
                         -0.11   12.03   library setup time
                                 12.03   data required time
-----------------------------------------------------------------------------
                                 12.03   data required time
                                 -5.90   data arrival time
-----------------------------------------------------------------------------
                                  6.13   slack (MET)



==========================================================================
global route report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.11    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.01    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.36    0.36    0.34    0.34 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.36    0.01    0.35 ^ clkbuf_4_13_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.14    0.30    0.65 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13_0_clk (net)
                  0.14    0.00    0.65 ^ clkbuf_leaf_65_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.05    0.07    0.18    0.83 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_65_clk (net)
                  0.07    0.00    0.84 ^ CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.10    0.35    1.19 ^ CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_dmem_rd_en_a4 (net)
                  0.10    0.00    1.19 ^ _05790_/D (sky130_fd_sc_hd__or4_4)
    38    0.40    1.13    0.88    2.06 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  1.13    0.01    2.08 ^ _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.47    0.51    0.52    2.59 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.51    0.01    2.61 v _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.29    0.36    2.96 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.29    0.00    2.96 ^ _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.02    0.16    0.26    3.22 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.16    0.00    3.22 ^ _08747_/A_N (sky130_fd_sc_hd__nand2b_4)
     5    0.06    0.18    0.26    3.48 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
                                         _03295_ (net)
                  0.18    0.00    3.48 ^ _09140_/B (sky130_fd_sc_hd__or3_4)
    48    0.35    0.98    0.80    4.28 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
                                         _03559_ (net)
                  0.98    0.03    4.31 ^ _09216_/B (sky130_fd_sc_hd__nor2_1)
     1    0.01    0.17    0.14    4.45 v _09216_/Y (sky130_fd_sc_hd__nor2_1)
                                         _03609_ (net)
                  0.17    0.00    4.45 v _09217_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.01    0.18    0.20    4.64 ^ _09217_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _03610_ (net)
                  0.18    0.00    4.64 ^ hold1846/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.01    0.09    0.61    5.25 ^ hold1846/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1969 (net)
                  0.09    0.00    5.25 ^ _09218_/B (sky130_fd_sc_hd__nor2_1)
     1    0.01    0.13    0.06    5.31 v _09218_/Y (sky130_fd_sc_hd__nor2_1)
                                         _00763_ (net)
                  0.13    0.00    5.31 v hold1847/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.59    5.90 v hold1847/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1970 (net)
                  0.06    0.00    5.90 v CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  5.90   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     1    0.11    0.00    0.00   11.90 ^ clk (in)
                                         clk (net)
                  0.01    0.00   11.90 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.36    0.36    0.34   12.24 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.36    0.01   12.25 ^ clkbuf_4_7_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.15    0.30   12.55 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_clk (net)
                  0.15    0.00   12.56 ^ clkbuf_leaf_76_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.05    0.07    0.18   12.74 ^ clkbuf_leaf_76_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_76_clk (net)
                  0.07    0.00   12.74 ^ CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                         -0.60   12.15   clock uncertainty
                          0.00   12.15   clock reconvergence pessimism
                         -0.11   12.03   library setup time
                                 12.03   data required time
-----------------------------------------------------------------------------
                                 12.03   data required time
                                 -5.90   data arrival time
-----------------------------------------------------------------------------
                                  6.13   slack (MET)



==========================================================================
global route report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
global route max_slew_check_slack
--------------------------------------------------------------------------
0.2381322979927063

==========================================================================
global route max_slew_check_limit
--------------------------------------------------------------------------
1.4951449632644653

==========================================================================
global route max_slew_check_slack_limit
--------------------------------------------------------------------------
0.1593

==========================================================================
global route max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
global route max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
global route max_capacitance_check_slack
--------------------------------------------------------------------------
0.023087244480848312

==========================================================================
global route max_capacitance_check_limit
--------------------------------------------------------------------------
0.1538189947605133

==========================================================================
global route max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.1501

==========================================================================
global route max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
global route max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
global route max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
global route setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
global route hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
global route report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.34    0.34 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31    0.65 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.83 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.84 ^ CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.35    1.19 ^ CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.88    2.06 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
   0.53    2.59 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
   0.37    2.96 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
   0.26    3.22 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
   0.26    3.48 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
   0.80    4.28 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
   0.16    4.45 v _09216_/Y (sky130_fd_sc_hd__nor2_1)
   0.20    4.64 ^ _09217_/Y (sky130_fd_sc_hd__a21oi_1)
   0.61    5.25 ^ hold1846/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.06    5.31 v _09218_/Y (sky130_fd_sc_hd__nor2_1)
   0.59    5.90 v hold1847/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.00    5.90 v CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_2)
           5.90   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock source latency
   0.00   11.90 ^ clk (in)
   0.34   12.24 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31   12.55 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.19   12.74 ^ clkbuf_leaf_76_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   12.74 ^ CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_2)
  -0.60   12.15   clock uncertainty
   0.00   12.15   clock reconvergence pessimism
  -0.11   12.03   library setup time
          12.03   data required time
---------------------------------------------------------
          12.03   data required time
          -5.90   data arrival time
---------------------------------------------------------
           6.13   slack (MET)



==========================================================================
global route report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_dmem_rd_data_a5[10]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[14][10]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.34    0.34 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31    0.66 ^ clkbuf_4_6_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.84 ^ clkbuf_leaf_99_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.85 ^ CPU_dmem_rd_data_a5[10]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.37    1.22 v CPU_dmem_rd_data_a5[10]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.39    1.61 v _08007_/X (sky130_fd_sc_hd__mux2_8)
   0.09    1.69 ^ _08954_/Y (sky130_fd_sc_hd__nand2_1)
   0.06    1.76 v _08955_/Y (sky130_fd_sc_hd__a21oi_1)
   0.00    1.76 v CPU_Xreg_value_a4[14][10]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.76   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.34    0.34 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31    0.65 ^ clkbuf_4_14_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.84 ^ clkbuf_leaf_52_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.84 ^ CPU_Xreg_value_a4[14][10]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.95    1.79   clock uncertainty
   0.00    1.79   clock reconvergence pessimism
  -0.05    1.75   library hold time
           1.75   data required time
---------------------------------------------------------
           1.75   data required time
          -1.76   data arrival time
---------------------------------------------------------
           0.01   slack (MET)



==========================================================================
global route critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
global route critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
global route critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
global route critical path delay
--------------------------------------------------------------------------
5.9000

==========================================================================
global route critical path slack
--------------------------------------------------------------------------
6.1338

==========================================================================
global route slack div critical path delay
--------------------------------------------------------------------------
103.962712

==========================================================================
global route report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.67e-03   6.43e-04   1.04e-08   5.31e-03  35.3%
Combinational          1.74e-03   3.39e-03   2.45e-08   5.13e-03  34.1%
Clock                  2.46e-03   2.16e-03   2.20e-09   4.62e-03  30.7%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  8.87e-03   6.20e-03   3.70e-08   1.51e-02 100.0%
                          58.9%      41.1%       0.0%
```

## Final

![image](https://github.com/user-attachments/assets/620c36e3-00be-4bd4-867d-5761d1fb509b)

![image](https://github.com/user-attachments/assets/07b0fb9a-0406-4c9e-b8be-2606fa12239a)

```

==========================================================================
finish report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
finish report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
finish report_worst_slack
--------------------------------------------------------------------------
worst slack 6.33

==========================================================================
finish report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.84 source latency CPU_src2_value_a3[4]$_DFF_P_/CLK ^
  -0.80 target latency CPU_imem_rd_addr_a1[1]$_SDFF_PP0_/CLK ^
   0.60 clock uncertainty
   0.00 CRPR
--------------
   0.63 setup skew


==========================================================================
finish report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: CPU_dmem_rd_data_a5[23]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[10][23]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.06    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.00    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.33    0.33 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.35    0.01    0.34 ^ clkbuf_4_2_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.14    0.16    0.31    0.65 ^ clkbuf_4_2_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_2_0_clk (net)
                  0.16    0.00    0.65 ^ clkbuf_leaf_9_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.18    0.83 ^ clkbuf_leaf_9_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_9_clk (net)
                  0.06    0.00    0.83 ^ CPU_dmem_rd_data_a5[23]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
     1    0.04    0.09    0.38    1.21 v CPU_dmem_rd_data_a5[23]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_2)
                                         CPU_dmem_rd_data_a5[23] (net)
                  0.09    0.01    1.22 v _08344_/B (sky130_fd_sc_hd__and3_4)
    15    0.06    0.09    0.26    1.48 v _08344_/X (sky130_fd_sc_hd__and3_4)
                                         _02945_ (net)
                  0.09    0.00    1.48 v _08347_/A2 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.08    0.15    1.63 ^ _08347_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _02948_ (net)
                  0.08    0.00    1.63 ^ _08348_/B1 (sky130_fd_sc_hd__o21ai_0)
     1    0.00    0.05    0.08    1.71 v _08348_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _00559_ (net)
                  0.05    0.00    1.71 v CPU_Xreg_value_a4[10][23]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.71   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.06    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.00    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.33    0.33 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.35    0.01    0.35 ^ clkbuf_4_10_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.13    0.14    0.30    0.64 ^ clkbuf_4_10_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_10_0_clk (net)
                  0.14    0.00    0.65 ^ clkbuf_leaf_37_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.18    0.82 ^ clkbuf_leaf_37_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_37_clk (net)
                  0.06    0.00    0.82 ^ CPU_Xreg_value_a4[10][23]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.95    1.78   clock uncertainty
                          0.00    1.78   clock reconvergence pessimism
                         -0.05    1.72   library hold time
                                  1.72   data required time
-----------------------------------------------------------------------------
                                  1.72   data required time
                                 -1.71   data arrival time
-----------------------------------------------------------------------------
                                 -0.02   slack (VIOLATED)



==========================================================================
finish report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.06    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.00    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.33    0.33 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.35    0.01    0.34 ^ clkbuf_4_13_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.12    0.13    0.29    0.63 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13_0_clk (net)
                  0.13    0.00    0.63 ^ clkbuf_leaf_65_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.04    0.06    0.17    0.81 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_65_clk (net)
                  0.06    0.00    0.81 ^ CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.08    0.34    1.15 ^ CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_dmem_rd_en_a4 (net)
                  0.08    0.00    1.15 ^ _05790_/D (sky130_fd_sc_hd__or4_4)
    38    0.41    1.15    0.87    2.01 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  1.15    0.02    2.03 ^ _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.46    0.51    0.50    2.53 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.51    0.01    2.54 v _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.28    0.35    2.90 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.28    0.00    2.90 ^ _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.01    0.13    0.24    3.13 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.13    0.00    3.13 ^ _08747_/A_N (sky130_fd_sc_hd__nand2b_4)
     5    0.06    0.18    0.24    3.38 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
                                         _03295_ (net)
                  0.18    0.00    3.38 ^ _09140_/B (sky130_fd_sc_hd__or3_4)
    48    0.38    1.08    0.85    4.23 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
                                         _03559_ (net)
                  1.08    0.04    4.27 ^ _09216_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.14    0.08    4.35 v _09216_/Y (sky130_fd_sc_hd__nor2_1)
                                         _03609_ (net)
                  0.14    0.00    4.35 v _09217_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.14    0.15    4.50 ^ _09217_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _03610_ (net)
                  0.14    0.00    4.50 ^ hold1846/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.07    0.57    5.08 ^ hold1846/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1969 (net)
                  0.07    0.00    5.08 ^ _09218_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.11    0.04    5.12 v _09218_/Y (sky130_fd_sc_hd__nor2_1)
                                         _00763_ (net)
                  0.11    0.00    5.12 v hold1847/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.69 v hold1847/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1970 (net)
                  0.05    0.00    5.69 v CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  5.69   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     1    0.06    0.00    0.00   11.90 ^ clk (in)
                                         clk (net)
                  0.00    0.00   11.90 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.33   12.23 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.35    0.01   12.25 ^ clkbuf_4_7_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.15    0.30   12.54 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_clk (net)
                  0.15    0.00   12.55 ^ clkbuf_leaf_76_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.05    0.07    0.18   12.73 ^ clkbuf_leaf_76_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_76_clk (net)
                  0.07    0.00   12.73 ^ CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                         -0.60   12.14   clock uncertainty
                          0.00   12.14   clock reconvergence pessimism
                         -0.11   12.03   library setup time
                                 12.03   data required time
-----------------------------------------------------------------------------
                                 12.03   data required time
                                 -5.69   data arrival time
-----------------------------------------------------------------------------
                                  6.33   slack (MET)



==========================================================================
finish report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.06    0.00    0.00    0.00 ^ clk (in)
                                         clk (net)
                  0.00    0.00    0.00 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.33    0.33 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.35    0.01    0.34 ^ clkbuf_4_13_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.12    0.13    0.29    0.63 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13_0_clk (net)
                  0.13    0.00    0.63 ^ clkbuf_leaf_65_clk/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.04    0.06    0.17    0.81 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_65_clk (net)
                  0.06    0.00    0.81 ^ CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.08    0.34    1.15 ^ CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         CPU_dmem_rd_en_a4 (net)
                  0.08    0.00    1.15 ^ _05790_/D (sky130_fd_sc_hd__or4_4)
    38    0.41    1.15    0.87    2.01 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
                                         _01035_ (net)
                  1.15    0.02    2.03 ^ _05791_/A (sky130_fd_sc_hd__clkinv_16)
    40    0.46    0.51    0.50    2.53 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _01036_ (net)
                  0.51    0.01    2.54 v _07870_/B1 (sky130_fd_sc_hd__o211ai_1)
     2    0.01    0.28    0.35    2.90 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _02486_ (net)
                  0.28    0.00    2.90 ^ _07871_/B2 (sky130_fd_sc_hd__o22a_1)
     3    0.01    0.13    0.24    3.13 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
                                         _02487_ (net)
                  0.13    0.00    3.13 ^ _08747_/A_N (sky130_fd_sc_hd__nand2b_4)
     5    0.06    0.18    0.24    3.38 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
                                         _03295_ (net)
                  0.18    0.00    3.38 ^ _09140_/B (sky130_fd_sc_hd__or3_4)
    48    0.38    1.08    0.85    4.23 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
                                         _03559_ (net)
                  1.08    0.04    4.27 ^ _09216_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.14    0.08    4.35 v _09216_/Y (sky130_fd_sc_hd__nor2_1)
                                         _03609_ (net)
                  0.14    0.00    4.35 v _09217_/B1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.14    0.15    4.50 ^ _09217_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _03610_ (net)
                  0.14    0.00    4.50 ^ hold1846/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.07    0.57    5.08 ^ hold1846/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1969 (net)
                  0.07    0.00    5.08 ^ _09218_/B (sky130_fd_sc_hd__nor2_1)
     1    0.00    0.11    0.04    5.12 v _09218_/Y (sky130_fd_sc_hd__nor2_1)
                                         _00763_ (net)
                  0.11    0.00    5.12 v hold1847/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.69 v hold1847/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1970 (net)
                  0.05    0.00    5.69 v CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  5.69   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     1    0.06    0.00    0.00   11.90 ^ clk (in)
                                         clk (net)
                  0.00    0.00   11.90 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.33   12.23 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_clk (net)
                  0.35    0.01   12.25 ^ clkbuf_4_7_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.13    0.15    0.30   12.54 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_clk (net)
                  0.15    0.00   12.55 ^ clkbuf_leaf_76_clk/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.05    0.07    0.18   12.73 ^ clkbuf_leaf_76_clk/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_76_clk (net)
                  0.07    0.00   12.73 ^ CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                         -0.60   12.14   clock uncertainty
                          0.00   12.14   clock reconvergence pessimism
                         -0.11   12.03   library setup time
                                 12.03   data required time
-----------------------------------------------------------------------------
                                 12.03   data required time
                                 -5.69   data arrival time
-----------------------------------------------------------------------------
                                  6.33   slack (MET)



==========================================================================
finish report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
finish max_slew_check_slack
--------------------------------------------------------------------------
0.1376684308052063

==========================================================================
finish max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
finish max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0918

==========================================================================
finish max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
finish max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
finish max_capacitance_check_slack
--------------------------------------------------------------------------
0.016519365832209587

==========================================================================
finish max_capacitance_check_limit
--------------------------------------------------------------------------
0.1538189947605133

==========================================================================
finish max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.1074

==========================================================================
finish max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
finish max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
finish max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
finish setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
finish hold_violation_count
--------------------------------------------------------------------------
hold violation count 8

==========================================================================
finish report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.33    0.33 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.30    0.63 ^ clkbuf_4_13_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.17    0.81 ^ clkbuf_leaf_65_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.81 ^ CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.34    1.15 ^ CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.87    2.01 ^ _05790_/X (sky130_fd_sc_hd__or4_4)
   0.52    2.53 v _05791_/Y (sky130_fd_sc_hd__clkinv_16)
   0.37    2.90 ^ _07870_/Y (sky130_fd_sc_hd__o211ai_1)
   0.24    3.13 ^ _07871_/X (sky130_fd_sc_hd__o22a_1)
   0.24    3.38 ^ _08747_/Y (sky130_fd_sc_hd__nand2b_4)
   0.85    4.23 ^ _09140_/X (sky130_fd_sc_hd__or3_4)
   0.12    4.35 v _09216_/Y (sky130_fd_sc_hd__nor2_1)
   0.15    4.50 ^ _09217_/Y (sky130_fd_sc_hd__a21oi_1)
   0.57    5.08 ^ hold1846/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.04    5.12 v _09218_/Y (sky130_fd_sc_hd__nor2_1)
   0.57    5.69 v hold1847/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.00    5.69 v CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_2)
           5.69   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock source latency
   0.00   11.90 ^ clk (in)
   0.33   12.23 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31   12.54 ^ clkbuf_4_7_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18   12.73 ^ clkbuf_leaf_76_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   12.73 ^ CPU_Xreg_value_a4[1][5]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_2)
  -0.60   12.14   clock uncertainty
   0.00   12.14   clock reconvergence pessimism
  -0.11   12.03   library setup time
          12.03   data required time
---------------------------------------------------------
          12.03   data required time
          -5.69   data arrival time
---------------------------------------------------------
           6.33   slack (MET)



==========================================================================
finish report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: CPU_dmem_rd_data_a5[23]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: CPU_Xreg_value_a4[10][23]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.33    0.33 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31    0.65 ^ clkbuf_4_2_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.83 ^ clkbuf_leaf_9_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.83 ^ CPU_dmem_rd_data_a5[23]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.38    1.21 v CPU_dmem_rd_data_a5[23]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_2)
   0.26    1.48 v _08344_/X (sky130_fd_sc_hd__and3_4)
   0.15    1.63 ^ _08347_/Y (sky130_fd_sc_hd__a21oi_1)
   0.08    1.71 v _08348_/Y (sky130_fd_sc_hd__o21ai_0)
   0.00    1.71 v CPU_Xreg_value_a4[10][23]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.71   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ clk (in)
   0.33    0.33 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.31    0.64 ^ clkbuf_4_10_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.82 ^ clkbuf_leaf_37_clk/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.82 ^ CPU_Xreg_value_a4[10][23]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.95    1.78   clock uncertainty
   0.00    1.78   clock reconvergence pessimism
  -0.05    1.72   library hold time
           1.72   data required time
---------------------------------------------------------
           1.72   data required time
          -1.71   data arrival time
---------------------------------------------------------
          -0.02   slack (VIOLATED)



==========================================================================
finish critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
finish critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
finish critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
finish critical path delay
--------------------------------------------------------------------------
5.6920

==========================================================================
finish critical path slack
--------------------------------------------------------------------------
6.3338

==========================================================================
finish slack div critical path delay
--------------------------------------------------------------------------
111.275474

==========================================================================
finish report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.67e-03   6.23e-04   1.04e-08   5.29e-03  35.9%
Combinational          1.73e-03   3.28e-03   2.45e-08   5.02e-03  34.1%
Clock                  2.45e-03   1.97e-03   2.20e-09   4.42e-03  30.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  8.85e-03   5.88e-03   3.70e-08   1.47e-02 100.0%
                          60.1%      39.9%       0.0%
```
-> IR Drop

![image](https://github.com/user-attachments/assets/f0f740a3-dcda-4312-ab54-2ac55b5650b8)

## Final QOR

![image](https://github.com/user-attachments/assets/d0b293ed-1fa8-42a4-bd33-8c1b1a8543f1)


</details>

<details>
<summary> vsdabysoc </summary>

## Understanding physical design steps and running flow for vsdbabysoc

The Directory structure for configuring vsdbabysoc is same as shown for rvmyth core design.

- First we need to create a directory vsdbabysoc inside
- Now copy the folders gds, include, lef and lib from the design folder in your system into this directory. - The gds folder would contain the files avsddac.gds and avsdpll.gds - The include folder would contain the files sandpiper.vh, sandpiper_gen.vh, sp_default.vh and sp_verilog.vh - The gds folder would contain the files avsddac.lef and avsdpll.lef - The lib folder would contain the files avsddac.lib and avsdpll.lib as shown above
- Copy the constraints file(constraints.sdc) from the design folder in your system into this directory.
- Copy the files(macro.cfg and pin_order.cfg) from the design folder in your system into this directory.

Following is the ```config.mk``` file

```
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd

export ADDITIONAL_LIBS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lib/avsddac.lib \
						 $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lib/avsdpll.lib

export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/vsdbabysoc.v \
					   $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/rvmyth_sne.v \
					   $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/include/clk_gate.v

export VERILOG_INCLUDE_DIRS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/include

export SDC_FILE      = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/constraint.sdc

export ADDITIONAL_LEFS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lef/avsddac.lef \
 						 $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/lef/avsdpll.lef

export ADDITIONAL_GDS = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/gds/avsddac.gds \
						$(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/gds/avsdpll.gds

export MACRO_PLACEMENT = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/cfg/macro.cfg

export DIE_AREA   = 0 0 1500 1500
# export CORE_AREA  = 20 20 2900 3500

export CORE_UTILIZATION = 45
export PLACE_DENSITY_LB_ADDON = 0.2

export REMOVE_ABC_BUFFERS = 1
```

Format for macro.cfg 

```
avsdpll 200 1200 N
avsddac 200 0 N
```

.sdc file

```
set_units -time ns
set PERIOD 11.9

// clock generation
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD

//clock constraints
set_clock_uncertainty [expr 0.05 * $PERIOD] -setup [get_clocks clk]
set_clock_uncertainty [expr 0.08 * $PERIOD] -hold [get_clocks clk]
set_clock_transition [expr 0.05 * $PERIOD] [get_clocks clk]

//input constraints
set_input_transition [expr $PERIOD * 0.08] [get_ports reset]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

## Synthesis

![image](https://github.com/user-attachments/assets/b1dd8070-896e-4d7b-ac6a-428b470fd2e1)

```

24. Printing statistics.

=== vsdbabysoc ===

   Number of wires:               7221
   Number of wire bits:           7221
   Number of public wires:        1416
   Number of public wire bits:    1416
   Number of ports:                  7
   Number of port bits:              7
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:               7077
     avsddac                         1
     avsdpll                         1
     sky130_fd_sc_hd__a2111oi_0     15
     sky130_fd_sc_hd__a2111oi_1      1
     sky130_fd_sc_hd__a2111oi_2      2
     sky130_fd_sc_hd__a211oi_1      20
     sky130_fd_sc_hd__a211oi_2       3
     sky130_fd_sc_hd__a21bo_2        1
     sky130_fd_sc_hd__a21boi_0       8
     sky130_fd_sc_hd__a21boi_2       1
     sky130_fd_sc_hd__a21o_1        30
     sky130_fd_sc_hd__a21oi_1      814
     sky130_fd_sc_hd__a221o_1        3
     sky130_fd_sc_hd__a221oi_1     101
     sky130_fd_sc_hd__a22o_1        95
     sky130_fd_sc_hd__a22oi_1      517
     sky130_fd_sc_hd__a311oi_1      41
     sky130_fd_sc_hd__a311oi_2       9
     sky130_fd_sc_hd__a31o_2         2
     sky130_fd_sc_hd__a31oi_1       18
     sky130_fd_sc_hd__a31oi_2        1
     sky130_fd_sc_hd__a32o_1         2
     sky130_fd_sc_hd__a41oi_1        2
     sky130_fd_sc_hd__and2_0         1
     sky130_fd_sc_hd__and2_1        12
     sky130_fd_sc_hd__and3_1        24
     sky130_fd_sc_hd__and3b_1        2
     sky130_fd_sc_hd__and4_1         2
     sky130_fd_sc_hd__buf_1         49
     sky130_fd_sc_hd__buf_2         10
     sky130_fd_sc_hd__buf_6          2
     sky130_fd_sc_hd__clkbuf_1     599
     sky130_fd_sc_hd__clkbuf_2       2
     sky130_fd_sc_hd__conb_1         1
     sky130_fd_sc_hd__dfxtp_1     1274
     sky130_fd_sc_hd__fa_1           3
     sky130_fd_sc_hd__ha_1         135
     sky130_fd_sc_hd__inv_1        110
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux2i_1       27
     sky130_fd_sc_hd__nand2_1     1407
     sky130_fd_sc_hd__nand2b_1      30
     sky130_fd_sc_hd__nand3_1      333
     sky130_fd_sc_hd__nand3b_1      38
     sky130_fd_sc_hd__nand4_1      120
     sky130_fd_sc_hd__nor2_1       319
     sky130_fd_sc_hd__nor2b_1       55
     sky130_fd_sc_hd__nor3_1        69
     sky130_fd_sc_hd__nor3b_1        7
     sky130_fd_sc_hd__nor4_1        34
     sky130_fd_sc_hd__nor4b_1        2
     sky130_fd_sc_hd__nor4bb_1       1
     sky130_fd_sc_hd__o2111a_1       2
     sky130_fd_sc_hd__o2111ai_1      3
     sky130_fd_sc_hd__o211a_1        3
     sky130_fd_sc_hd__o211ai_1      36
     sky130_fd_sc_hd__o211ai_2       1
     sky130_fd_sc_hd__o21a_1         5
     sky130_fd_sc_hd__o21ai_0      354
     sky130_fd_sc_hd__o21ai_1        9
     sky130_fd_sc_hd__o21ba_2        1
     sky130_fd_sc_hd__o21bai_1      25
     sky130_fd_sc_hd__o221a_2        1
     sky130_fd_sc_hd__o221ai_1      33
     sky130_fd_sc_hd__o22a_1        36
     sky130_fd_sc_hd__o22ai_1       73
     sky130_fd_sc_hd__o2bb2ai_1      1
     sky130_fd_sc_hd__o311a_1        2
     sky130_fd_sc_hd__o311ai_0       3
     sky130_fd_sc_hd__o311ai_1       3
     sky130_fd_sc_hd__o311ai_4       2
     sky130_fd_sc_hd__o31a_1         2
     sky130_fd_sc_hd__o31ai_1       34
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32ai_1        1
     sky130_fd_sc_hd__or2_2         10
     sky130_fd_sc_hd__or3_1         20
     sky130_fd_sc_hd__or4_1         10
     sky130_fd_sc_hd__or4_4          2
     sky130_fd_sc_hd__xnor2_1       39
     sky130_fd_sc_hd__xor2_1         7

   Area for cell type \avsddac is unknown!
   Area for cell type \avsdpll is unknown!

   Chip area for module '\vsdbabysoc': 56739.417600
     of which used for sequential elements: 25504.460800 (44.95%)

```
## Floorplan

The primary objectives of floorplanning are to minimize area, timing, wire length, and power consumption while ensuring easy routing and reliability.

Here are the key aspects of floorplanning:

#### Inputs for Floorplanning :

- Gate level netlist (.v)
- Physical & Logical Libraries. (.lefs & .libs for all standard cell,macros,IO Pads etc.)
- Synopsys design constraints (.sdc).
- RC Tech File (TLU+ file) - to determine RC values of interconnect layers/metal layers of technology node used in our design, and hence provide RC values for computation of wire delays.
- Technology File (.tf).
- Physical Partitioning information of the design.
- Floorplanning Parameters like height,width,aspect ratio etc.

#### Outputs of Floorplanning:

- Die/Core Area: The physical description of the ASIC design.
- IO Pad Information: The placement of I/O pins.
- Placed Macros Information: The placement of macros.
- Standard Cell Placement Areas: The areas where standard cells are placed.
- Power Grid Design: The power distribution plan.
- Blockages: The defined regions where cells cannot be placed.

#### Floorplan Control Parameters:

- Aspect Ratio: The ratio of the height to the width of the chip, which affects routing resources and congestion.
- Core Utilization: The percentage of the core area occupied by standard cells, macros, and blockages.

Standard Cell Row: The area where standard cells are placed, divided into rows with varying heights.
Flylines: Virtual connections between macros and IO pads, helping in logical placement and reducing routing resources.
Halo (Keep Out Margin): The region around fixed macros where other macros and standard cells cannot be placed.

#### Macro Placement Guidelines

- Grouping of macros as per hierarchy
- Analysis of  macro to input/output pins connection  
- Logical depth analysis among macros and macros to Input/Output pin
- Maximizing the core area
- Avoid notch formation
- Channel spacing 
- Macro abbutment
- IO pins to macro spacing
- Halo over macros
- Routing blockage over macros
- Partial placement blockage in the macro channels and macro to io pins region


![image](https://github.com/user-attachments/assets/aa38aa16-b39f-4f09-b71b-4ed69b61aa67)

![image](https://github.com/user-attachments/assets/286fe366-466d-4e85-be02-3b3aad67733b)

![image](https://github.com/user-attachments/assets/6556ed30-6b7e-4aed-9911-3cb06f3722af)

![image](https://github.com/user-attachments/assets/ad855b7b-9f35-4165-ba42-6550184d8647)


## Placement

#### Steps in Placement:

Placement can be divided into several key stages:
  
- Pre-Placement: This initial stage involves checks and the placement of physical-only cells (e.g., end-cap cells, well-tap cells) to ensure a clean design environment before standard cell placement begins.

- Global Placement: During this phase, the tool determines approximate locations for each standard cell based on various constraints such as timing, congestion, and power. This step does not enforce design rule checks (DRCs), allowing for overlaps.

- Legalization: After global placement, the tool adjusts the positions of cells to eliminate overlaps and ensure that all cells are in legal locations according to the design rules. This process may involve flipping cells to match power pin requirements.

- High Fanout Net Synthesis (HFNS): This step addresses nets with high fanout by distributing the load across multiple drivers and adding buffers as necessary to meet timing constraints[1][2].

- Post-Placement Optimization: After initial placement, further iterations are conducted to refine the design, focusing on timing, congestion, and power optimizations. This may include techniques like scan-chain reordering and tie cell insertion.

#### Techniques Used in Placement:

Placement can be driven by various priorities, including:
- Timing-Driven Placement: Prioritizes minimizing signal delays.
- Congestion-Driven Placement: Focuses on reducing routing congestion.
- Power-Driven Placement: Aims to minimize power consumption during operation.

#### Challanges in Placement

- Congestion : High density of cells in certain areas can lead to routing failures.
- Timing Closure : Ensuring all paths meet timing requirements after placement.
- Power Integrity : Uneven cell distribution can cause localized IR drop.

-> Standard Cell Placed
![image](https://github.com/user-attachments/assets/f4364f0f-3b7b-4eea-b042-da10d893b3ce)

![image](https://github.com/user-attachments/assets/eb33f4aa-9592-4508-a433-1ea8883cae84)

![image](https://github.com/user-attachments/assets/3a630785-68a3-40e1-8a22-70af7e12e3e4)

-> Resizing

![image](https://github.com/user-attachments/assets/c44dbf20-d94d-4821-ae85-e9abc65588c3)


```
==========================================================================
detailed place report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
detailed place report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
detailed place report_worst_slack
--------------------------------------------------------------------------
worst slack 6.48

==========================================================================
detailed place report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.42    0.42 v core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_rd_a2[1] (net)
                  0.03    0.00    0.42 v core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  0.42   data arrival time

                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.95    0.95   clock uncertainty
                          0.00    0.95   clock reconvergence pessimism
                                  0.95 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                          0.07    1.02   library hold time
                                  1.02   data required time
-----------------------------------------------------------------------------
                                  1.02   data required time
                                 -0.42   data arrival time
-----------------------------------------------------------------------------
                                 -0.60   slack (VIOLATED)



==========================================================================
detailed place report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.12    0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.12    0.00    0.49 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    0.58 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    0.94 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    1.68 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    2.31 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.27    0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.27    0.00    2.64 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.20    0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.20    0.00    2.79 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.42    0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.42    0.00    3.32 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    3.44 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.01    0.11    0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.11    0.00    3.62 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.03    0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.03    0.00    4.52 ^ _10942_/A1 (sky130_fd_sc_hd__mux2i_1)
     1    0.01    0.22    0.23    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
                                         core.CPU_src2_value_a2[29] (net)
                  0.22    0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.75   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.07   11.23   library setup time
                                 11.23   data required time
-----------------------------------------------------------------------------
                                 11.23   data required time
                                 -4.75   data arrival time
-----------------------------------------------------------------------------
                                  6.48   slack (MET)



==========================================================================
detailed place report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.12    0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.12    0.00    0.49 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    0.58 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    0.94 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    1.68 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    2.31 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.27    0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.27    0.00    2.64 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.20    0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.20    0.00    2.79 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.42    0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.42    0.00    3.32 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    3.44 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.01    0.11    0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.11    0.00    3.62 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.03    0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.03    0.00    4.52 ^ _10942_/A1 (sky130_fd_sc_hd__mux2i_1)
     1    0.01    0.22    0.23    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
                                         core.CPU_src2_value_a2[29] (net)
                  0.22    0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.75   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.07   11.23   library setup time
                                 11.23   data required time
-----------------------------------------------------------------------------
                                 11.23   data required time
                                 -4.75   data arrival time
-----------------------------------------------------------------------------
                                  6.48   slack (MET)



==========================================================================
detailed place report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
detailed place max_slew_check_slack
--------------------------------------------------------------------------
0.10627798736095428

==========================================================================
detailed place max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
detailed place max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0709

==========================================================================
detailed place max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_capacitance_check_slack
--------------------------------------------------------------------------
0.018443282693624496

==========================================================================
detailed place max_capacitance_check_limit
--------------------------------------------------------------------------
0.021067000925540924

==========================================================================
detailed place max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.8755

==========================================================================
detailed place max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
detailed place max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
detailed place max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
detailed place setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
detailed place hold_violation_count
--------------------------------------------------------------------------
hold violation count 1259

==========================================================================
detailed place report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
   0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
   0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
   0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
   0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
   0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
   0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
   0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
   0.24    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
   0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
           4.75   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock network delay (ideal)
  -0.60   11.31   clock uncertainty
   0.00   11.31   clock reconvergence pessimism
          11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.07   11.23   library setup time
          11.23   data required time
---------------------------------------------------------
          11.23   data required time
          -4.75   data arrival time
---------------------------------------------------------
           6.48   slack (MET)



==========================================================================
detailed place report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.42    0.42 v core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.00    0.42 v core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
           0.42   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.95    0.95   clock uncertainty
   0.00    0.95   clock reconvergence pessimism
           0.95 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.07    1.02   library hold time
           1.02   data required time
---------------------------------------------------------
           1.02   data required time
          -0.42   data arrival time
---------------------------------------------------------
          -0.60   slack (VIOLATED)



==========================================================================
detailed place critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path delay
--------------------------------------------------------------------------
4.7504

==========================================================================
detailed place critical path slack
--------------------------------------------------------------------------
6.4844

==========================================================================
detailed place slack div critical path delay
--------------------------------------------------------------------------
136.502189

==========================================================================
detailed place report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.78e-03   1.00e-03   1.04e-08   5.78e-03  53.7%
Combinational          1.48e-03   3.51e-03   1.11e-08   4.99e-03  46.3%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  6.26e-03   4.51e-03   2.15e-08   1.08e-02 100.0%
                          58.1%      41.9%       0.0%

```

## CTS

CTS involves connecting the clock signal from the clock port to the clock pins of sequential cells while minimizing insertion delay and balancing skew. The clock network is typically categorized as a high fanout net, which requires special handling due to its significant power consumption

Several structures of clock tree:

H-Tree Structure: A balanced tree structure that minimizes skew.

X-Tree Structure: Similar to the H-tree but optimized for different geometries.

Geometric Matching Algorithm (GMA): A method for optimizing the layout of the clock tree.

Pi Tree Structure: A structure that balances loads effectively.

Fishbone Structure: A more complex design that can handle varying loads and distances.

#### Inputs for CTS:

- Placement Database (DB): Contains the netlist after placement, including various technology files and specifications.
- Clock Tree Specification File: Defines the requirements and constraints for the clock tree.
- Library Files: Include information on clock buffers and inverters used in the design.

#### Outputs of CTS:

- A netlist that reflects the clock tree configuration.
- Timing reports detailing setup and hold times.
- Skew and latency reports to assess clock performance.

#### Quality Checks Post-CTS:

- Insertion Delay: Must meet target values.
- Skew Balancing: Should be within acceptable limits.
- Signal Integrity: Ensuring minimal crosstalk and other noise effects.
- Power Consumption: Evaluating the clock tree's power usage to ensure it aligns with design specifications.

-> Clock Nets

![image](https://github.com/user-attachments/assets/a8a20896-a104-4298-80e5-a5d9b147395a)

-> Clock Tree

![image](https://github.com/user-attachments/assets/9373c0d5-2f18-4141-bd11-862e691eeb03)


```

==========================================================================
detailed place report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
detailed place report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
detailed place report_worst_slack
--------------------------------------------------------------------------
worst slack 6.48

==========================================================================
detailed place report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.42    0.42 v core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_rd_a2[1] (net)
                  0.03    0.00    0.42 v core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  0.42   data arrival time

                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.95    0.95   clock uncertainty
                          0.00    0.95   clock reconvergence pessimism
                                  0.95 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                          0.07    1.02   library hold time
                                  1.02   data required time
-----------------------------------------------------------------------------
                                  1.02   data required time
                                 -0.42   data arrival time
-----------------------------------------------------------------------------
                                 -0.60   slack (VIOLATED)



==========================================================================
detailed place report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.12    0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.12    0.00    0.49 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    0.58 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    0.94 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    1.68 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    2.31 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.27    0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.27    0.00    2.64 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.20    0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.20    0.00    2.79 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.42    0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.42    0.00    3.32 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    3.44 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.01    0.11    0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.11    0.00    3.62 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.03    0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.03    0.00    4.52 ^ _10942_/A1 (sky130_fd_sc_hd__mux2i_1)
     1    0.01    0.22    0.23    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
                                         core.CPU_src2_value_a2[29] (net)
                  0.22    0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.75   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.07   11.23   library setup time
                                 11.23   data required time
-----------------------------------------------------------------------------
                                 11.23   data required time
                                 -4.75   data arrival time
-----------------------------------------------------------------------------
                                  6.48   slack (MET)



==========================================================================
detailed place report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.12    0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.12    0.00    0.49 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    0.58 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    0.94 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    1.68 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    2.31 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.27    0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.27    0.00    2.64 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.20    0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.20    0.00    2.79 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.42    0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.42    0.00    3.32 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    3.44 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.01    0.11    0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.11    0.00    3.62 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.03    0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.03    0.00    4.52 ^ _10942_/A1 (sky130_fd_sc_hd__mux2i_1)
     1    0.01    0.22    0.23    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
                                         core.CPU_src2_value_a2[29] (net)
                  0.22    0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.75   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.07   11.23   library setup time
                                 11.23   data required time
-----------------------------------------------------------------------------
                                 11.23   data required time
                                 -4.75   data arrival time
-----------------------------------------------------------------------------
                                  6.48   slack (MET)



==========================================================================
detailed place report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
detailed place max_slew_check_slack
--------------------------------------------------------------------------
0.10627798736095428

==========================================================================
detailed place max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
detailed place max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0709

==========================================================================
detailed place max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_capacitance_check_slack
--------------------------------------------------------------------------
0.018443282693624496

==========================================================================
detailed place max_capacitance_check_limit
--------------------------------------------------------------------------
0.021067000925540924

==========================================================================
detailed place max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.8755

==========================================================================
detailed place max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
detailed place max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
detailed place max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
detailed place setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
detailed place hold_violation_count
--------------------------------------------------------------------------
hold violation count 1259

==========================================================================
detailed place report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
   0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
   0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
   0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
   0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
   0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
   0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
   0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
   0.24    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
   0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
           4.75   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock network delay (ideal)
  -0.60   11.31   clock uncertainty
   0.00   11.31   clock reconvergence pessimism
          11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.07   11.23   library setup time
          11.23   data required time
---------------------------------------------------------
          11.23   data required time
          -4.75   data arrival time
---------------------------------------------------------
           6.48   slack (MET)



==========================================================================
detailed place report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.42    0.42 v core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.00    0.42 v core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
           0.42   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.95    0.95   clock uncertainty
   0.00    0.95   clock reconvergence pessimism
           0.95 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.07    1.02   library hold time
           1.02   data required time
---------------------------------------------------------
           1.02   data required time
          -0.42   data arrival time
---------------------------------------------------------
          -0.60   slack (VIOLATED)



==========================================================================
detailed place critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path delay
--------------------------------------------------------------------------
4.7504

==========================================================================
detailed place critical path slack
--------------------------------------------------------------------------
6.4844

==========================================================================
detailed place slack div critical path delay
--------------------------------------------------------------------------
136.502189

==========================================================================
detailed place report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.78e-03   1.00e-03   1.04e-08   5.78e-03  53.7%
Combinational          1.48e-03   3.51e-03   1.11e-08   4.99e-03  46.3%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  6.26e-03   4.51e-03   2.15e-08   1.08e-02 100.0%
                          58.1%      41.9%       0.0%

```

## Route

There are three main types of routing:

- Pre-routing (also known as power routing) which is done during power planning.
- Clock routing performed while building the clock tree in the clock tree synthesis (CTS) stage.
- Signal routing done after CTS to connect all signal pins.

#### Routing Flow

The routing flow consists of four main stages:

Global Routing: Divides the core area into global cells (gcells) and finds the shortest path for each net using algorithms like maze routing and Steiner tree. It assigns nets to specific gcells but does not define actual tracks.

Track Assignment: Assigns actual metal layers to global routes while fixing some design rule violations. However, many DRC, signal integrity, and timing violations still remain.

Detailed Routing: Completes the actual metal and via connections between pins. It fixes all remaining violations through multiple iterations. The block is divided into switch boxes (Sboxes) for routing.

Search and Repair: Performed after the first detailed routing iteration to locate and fix any remaining shorts or spacing violations[2][3].

#### Routing Constraints:

Key routing constraints include:

- Design rule constraints related to manufacturing
- Performance constraints to meet timing
- Routing density constraints to avoid congestion
- Constraints on off-grid routing
- Blocked routing regions

#### Routing Outputs:

The main outputs of routing are:

- Geometric layout of all nets in GDS format
- SPEF file for parasitics
- Updated SDC file with routed timing

-> Routed nets

![image](https://github.com/user-attachments/assets/2e63c158-37bb-4050-9b91-366aa5b895cd)

-> max path highlighted

![image](https://github.com/user-attachments/assets/eaaa7f09-624a-4d2a-9ffe-cdb563618bc0)

```

==========================================================================
detailed place report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
detailed place report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
detailed place report_worst_slack
--------------------------------------------------------------------------
worst slack 6.48

==========================================================================
detailed place report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.42    0.42 v core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_rd_a2[1] (net)
                  0.03    0.00    0.42 v core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  0.42   data arrival time

                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                          0.95    0.95   clock uncertainty
                          0.00    0.95   clock reconvergence pessimism
                                  0.95 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                          0.07    1.02   library hold time
                                  1.02   data required time
-----------------------------------------------------------------------------
                                  1.02   data required time
                                 -0.42   data arrival time
-----------------------------------------------------------------------------
                                 -0.60   slack (VIOLATED)



==========================================================================
detailed place report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.12    0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.12    0.00    0.49 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    0.58 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    0.94 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    1.68 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    2.31 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.27    0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.27    0.00    2.64 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.20    0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.20    0.00    2.79 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.42    0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.42    0.00    3.32 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    3.44 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.01    0.11    0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.11    0.00    3.62 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.03    0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.03    0.00    4.52 ^ _10942_/A1 (sky130_fd_sc_hd__mux2i_1)
     1    0.01    0.22    0.23    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
                                         core.CPU_src2_value_a2[29] (net)
                  0.22    0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.75   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.07   11.23   library setup time
                                 11.23   data required time
-----------------------------------------------------------------------------
                                 11.23   data required time
                                 -4.75   data arrival time
-----------------------------------------------------------------------------
                                  6.48   slack (MET)



==========================================================================
detailed place report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                  0.60    0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock network delay (ideal)
                  0.60    0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.12    0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.12    0.00    0.49 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    0.58 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    0.94 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    1.68 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    2.31 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.27    0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.27    0.00    2.64 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.20    0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.20    0.00    2.79 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.42    0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.42    0.00    3.32 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    3.44 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.01    0.11    0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.11    0.00    3.62 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.03    0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.03    0.00    4.52 ^ _10942_/A1 (sky130_fd_sc_hd__mux2i_1)
     1    0.01    0.22    0.23    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
                                         core.CPU_src2_value_a2[29] (net)
                  0.22    0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  4.75   data arrival time

                  0.60   11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock network delay (ideal)
                         -0.60   11.31   clock uncertainty
                          0.00   11.31   clock reconvergence pessimism
                                 11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.07   11.23   library setup time
                                 11.23   data required time
-----------------------------------------------------------------------------
                                 11.23   data required time
                                 -4.75   data arrival time
-----------------------------------------------------------------------------
                                  6.48   slack (MET)



==========================================================================
detailed place report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
detailed place max_slew_check_slack
--------------------------------------------------------------------------
0.10627798736095428

==========================================================================
detailed place max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
detailed place max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0709

==========================================================================
detailed place max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
detailed place max_capacitance_check_slack
--------------------------------------------------------------------------
0.018443282693624496

==========================================================================
detailed place max_capacitance_check_limit
--------------------------------------------------------------------------
0.021067000925540924

==========================================================================
detailed place max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.8755

==========================================================================
detailed place max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
detailed place max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
detailed place max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
detailed place setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
detailed place hold_violation_count
--------------------------------------------------------------------------
hold violation count 1259

==========================================================================
detailed place report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[29]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.49    0.49 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.09    0.58 v _05952_/Y (sky130_fd_sc_hd__inv_1)
   0.35    0.93 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
   0.75    1.68 v _08072_/X (sky130_fd_sc_hd__or4_2)
   0.63    2.31 v _08077_/X (sky130_fd_sc_hd__or4_2)
   0.32    2.64 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
   0.16    2.79 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
   0.53    3.32 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.12    3.44 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.19    3.62 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
   0.89    4.51 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
   0.24    4.75 v _10942_/Y (sky130_fd_sc_hd__mux2i_1)
   0.00    4.75 v core.CPU_src2_value_a3[29]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
           4.75   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock network delay (ideal)
  -0.60   11.31   clock uncertainty
   0.00   11.31   clock reconvergence pessimism
          11.31 ^ core.CPU_src2_value_a3[29]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.07   11.23   library setup time
          11.23   data required time
---------------------------------------------------------
          11.23   data required time
          -4.75   data arrival time
---------------------------------------------------------
           6.48   slack (MET)



==========================================================================
detailed place report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_rd_a2[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_rd_a3[1]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ core.CPU_rd_a2[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.42    0.42 v core.CPU_rd_a2[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.00    0.42 v core.CPU_rd_a3[1]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
           0.42   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.95    0.95   clock uncertainty
   0.00    0.95   clock reconvergence pessimism
           0.95 ^ core.CPU_rd_a3[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.07    1.02   library hold time
           1.02   data required time
---------------------------------------------------------
           1.02   data required time
          -0.42   data arrival time
---------------------------------------------------------
          -0.60   slack (VIOLATED)



==========================================================================
detailed place critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
detailed place critical path delay
--------------------------------------------------------------------------
4.7504

==========================================================================
detailed place critical path slack
--------------------------------------------------------------------------
6.4844

==========================================================================
detailed place slack div critical path delay
--------------------------------------------------------------------------
136.502189

==========================================================================
detailed place report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.78e-03   1.00e-03   1.04e-08   5.78e-03  53.7%
Combinational          1.48e-03   3.51e-03   1.11e-08   4.99e-03  46.3%
Clock                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  6.26e-03   4.51e-03   2.15e-08   1.08e-02 100.0%
                          58.1%      41.9%       0.0%

```

## Final

![image](https://github.com/user-attachments/assets/b43db3fe-f772-44c4-97f5-9ea0ea74a567)

### Heat Maps 

-> IR Drop

![image](https://github.com/user-attachments/assets/71ddc629-36e5-4ecc-9dbc-989a7d5e9e9f)

-> Final Routing

![image](https://github.com/user-attachments/assets/e1d8da41-a796-457f-be31-ae7d59dbb60d)

-> Placement Density

![image](https://github.com/user-attachments/assets/28fd25e1-8136-4b26-88e2-9cda8b93f0fb)

-> Estimated Congestion

![image](https://github.com/user-attachments/assets/ab5e76c7-dc0e-4041-9cfb-36ffa69b527e)


-> Final QOR

![image](https://github.com/user-attachments/assets/59a53d8a-232c-49c8-adaa-a6f6fe82fc5a)

```

==========================================================================
finish report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
finish report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
finish report_worst_slack
--------------------------------------------------------------------------
worst slack 6.28

==========================================================================
finish report_clock_skew
--------------------------------------------------------------------------
Clock clk
   1.33 source latency core.CPU_is_slti_a3$_DFF_P_/CLK ^
  -1.06 target latency core.CPU_Xreg_value_a4[7][2]$_SDFFE_PP1P_/CLK ^
   0.60 clock uncertainty
   0.00 CRPR
--------------
   0.87 setup skew


==========================================================================
finish report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_src2_value_a4[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Dmem_value_a5[15][1]$_SDFFE_PP1P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     2    0.20    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.24    0.12    0.12 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.03    0.06    0.20    0.32 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.06    0.00    0.33 ^ clkbuf_1_1_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     4    0.08    0.10    0.17    0.50 ^ clkbuf_1_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_1_1_0_CLK (net)
                  0.10    0.00    0.50 ^ clkbuf_3_7_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.06    0.08    0.18    0.68 ^ clkbuf_3_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_7_0_CLK (net)
                  0.08    0.00    0.68 ^ clkbuf_4_14__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.45    0.41    0.37    1.05 ^ clkbuf_4_14__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_14__leaf_CLK (net)
                  0.41    0.03    1.08 ^ clkbuf_leaf_78_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     9    0.04    0.06    0.24    1.32 ^ clkbuf_leaf_78_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_78_CLK (net)
                  0.06    0.00    1.32 ^ core.CPU_src2_value_a4[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_4)
    16    0.07    0.10    0.41    1.74 v core.CPU_src2_value_a4[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_4)
                                         core.CPU_dmem_wr_data_a4[1] (net)
                  0.10    0.00    1.74 v _06761_/A (sky130_fd_sc_hd__nand2_1)
     1    0.00    0.05    0.08    1.82 ^ _06761_/Y (sky130_fd_sc_hd__nand2_1)
                                         _01733_ (net)
                  0.05    0.00    1.82 ^ _06762_/C (sky130_fd_sc_hd__nand3b_1)
     1    0.00    0.06    0.08    1.89 v _06762_/Y (sky130_fd_sc_hd__nand3b_1)
                                         _00203_ (net)
                  0.06    0.00    1.89 v core.CPU_Dmem_value_a5[15][1]$_SDFFE_PP1P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.89   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     2    0.20    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.24    0.12    0.12 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.03    0.06    0.20    0.32 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.06    0.00    0.33 ^ clkbuf_1_0_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     4    0.11    0.12    0.19    0.52 ^ clkbuf_1_0_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_1_0_0_CLK (net)
                  0.12    0.01    0.53 ^ clkbuf_3_2_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.17    0.70 ^ clkbuf_3_2_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_2_0_CLK (net)
                  0.06    0.00    0.70 ^ clkbuf_4_5__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     7    0.13    0.15    0.21    0.91 ^ clkbuf_4_5__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_5__leaf_CLK (net)
                  0.15    0.00    0.92 ^ clkbuf_leaf_136_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.03    0.06    0.17    1.09 ^ clkbuf_leaf_136_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_136_CLK (net)
                  0.06    0.00    1.09 ^ core.CPU_Dmem_value_a5[15][1]$_SDFFE_PP1P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.95    2.04   clock uncertainty
                          0.00    2.04   clock reconvergence pessimism
                         -0.06    1.98   library hold time
                                  1.98   data required time
-----------------------------------------------------------------------------
                                  1.98   data required time
                                 -1.89   data arrival time
-----------------------------------------------------------------------------
                                 -0.09   slack (VIOLATED)



==========================================================================
finish report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     2    0.20    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.24    0.12    0.12 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.03    0.06    0.20    0.32 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.06    0.00    0.33 ^ clkbuf_1_1_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     4    0.08    0.10    0.17    0.50 ^ clkbuf_1_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_1_1_0_CLK (net)
                  0.10    0.00    0.50 ^ clkbuf_3_5_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.08    0.10    0.19    0.69 ^ clkbuf_3_5_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_5_0_CLK (net)
                  0.10    0.00    0.70 ^ clkbuf_4_11__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.19    0.20    0.27    0.97 ^ clkbuf_4_11__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_11__leaf_CLK (net)
                  0.20    0.01    0.97 ^ clkbuf_leaf_68_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.04    0.06    0.19    1.17 ^ clkbuf_leaf_68_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_68_CLK (net)
                  0.06    0.00    1.17 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.11    0.36    1.53 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.11    0.00    1.53 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.08    1.61 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    1.61 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    1.97 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    1.97 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    2.72 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    2.72 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    3.35 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    3.35 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.28    0.33    3.68 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.28    0.00    3.68 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.21    0.16    3.84 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.21    0.00    3.84 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.41    0.52    4.37 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.41    0.00    4.37 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    4.48 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    4.48 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.02    0.12    0.19    4.67 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.12    0.00    4.68 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.11    0.94    5.62 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.11    0.03    5.65 ^ _09216_/A1 (sky130_fd_sc_hd__o21ai_0)
     1    0.01    0.20    0.28    5.93 v _09216_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _00757_ (net)
                  0.20    0.00    5.93 v core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.93   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     2    0.20    0.00    0.00   11.90 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.24    0.12   12.02 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.03    0.06    0.20   12.22 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.06    0.00   12.23 ^ clkbuf_1_0_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     4    0.11    0.12    0.19   12.42 ^ clkbuf_1_0_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_1_0_0_CLK (net)
                  0.12    0.01   12.43 ^ clkbuf_3_1_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.06    0.08    0.18   12.61 ^ clkbuf_3_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_1_0_CLK (net)
                  0.08    0.00   12.61 ^ clkbuf_4_2__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     7    0.10    0.11    0.20   12.81 ^ clkbuf_4_2__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_2__leaf_CLK (net)
                  0.11    0.00   12.81 ^ clkbuf_leaf_43_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.04    0.06    0.17   12.98 ^ clkbuf_leaf_43_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_43_CLK (net)
                  0.06    0.00   12.98 ^ core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.60   12.38   clock uncertainty
                          0.00   12.38   clock reconvergence pessimism
                         -0.18   12.21   library setup time
                                 12.21   data required time
-----------------------------------------------------------------------------
                                 12.21   data required time
                                 -5.93   data arrival time
-----------------------------------------------------------------------------
                                  6.28   slack (MET)



==========================================================================
finish report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     2    0.20    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.24    0.12    0.12 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.03    0.06    0.20    0.32 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.06    0.00    0.33 ^ clkbuf_1_1_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     4    0.08    0.10    0.17    0.50 ^ clkbuf_1_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_1_1_0_CLK (net)
                  0.10    0.00    0.50 ^ clkbuf_3_5_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.08    0.10    0.19    0.69 ^ clkbuf_3_5_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_5_0_CLK (net)
                  0.10    0.00    0.70 ^ clkbuf_4_11__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.19    0.20    0.27    0.97 ^ clkbuf_4_11__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_11__leaf_CLK (net)
                  0.20    0.01    0.97 ^ clkbuf_leaf_68_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.04    0.06    0.19    1.17 ^ clkbuf_leaf_68_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_68_CLK (net)
                  0.06    0.00    1.17 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     3    0.01    0.11    0.36    1.53 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_src1_value_a3[11] (net)
                  0.11    0.00    1.53 ^ _05952_/A (sky130_fd_sc_hd__inv_1)
     2    0.01    0.06    0.08    1.61 v _05952_/Y (sky130_fd_sc_hd__inv_1)
                                         _05526_ (net)
                  0.06    0.00    1.61 v _11479_/B (sky130_fd_sc_hd__ha_2)
     9    0.04    0.12    0.35    1.97 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
                                         _05528_ (net)
                  0.12    0.00    1.97 v _08072_/A (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.75    2.72 v _08072_/X (sky130_fd_sc_hd__or4_2)
                                         _02699_ (net)
                  0.14    0.00    2.72 v _08077_/D (sky130_fd_sc_hd__or4_2)
     2    0.01    0.14    0.63    3.35 v _08077_/X (sky130_fd_sc_hd__or4_2)
                                         _02704_ (net)
                  0.14    0.00    3.35 v _08078_/B1 (sky130_fd_sc_hd__a211oi_4)
     2    0.02    0.28    0.33    3.68 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _02705_ (net)
                  0.28    0.00    3.68 ^ _08288_/A3 (sky130_fd_sc_hd__o311ai_4)
     3    0.02    0.21    0.16    3.84 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
                                         _02906_ (net)
                  0.21    0.00    3.84 v _08482_/A2 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.41    0.52    4.37 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03092_ (net)
                  0.41    0.00    4.37 ^ _08483_/C1 (sky130_fd_sc_hd__a2111oi_2)
     1    0.01    0.11    0.12    4.48 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
                                         _03093_ (net)
                  0.11    0.00    4.48 v _08484_/B (sky130_fd_sc_hd__xnor2_2)
     1    0.02    0.12    0.19    4.67 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
                                         _03094_ (net)
                  0.12    0.00    4.68 v _08501_/A2 (sky130_fd_sc_hd__a211oi_4)
    17    0.10    1.11    0.94    5.62 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
                                         _03111_ (net)
                  1.11    0.03    5.65 ^ _09216_/A1 (sky130_fd_sc_hd__o21ai_0)
     1    0.01    0.20    0.28    5.93 v _09216_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _00757_ (net)
                  0.20    0.00    5.93 v core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.93   data arrival time

                         11.90   11.90   clock clk (rise edge)
                          0.00   11.90   clock source latency
     2    0.20    0.00    0.00   11.90 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.24    0.12   12.02 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.03    0.06    0.20   12.22 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.06    0.00   12.23 ^ clkbuf_1_0_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     4    0.11    0.12    0.19   12.42 ^ clkbuf_1_0_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_1_0_0_CLK (net)
                  0.12    0.01   12.43 ^ clkbuf_3_1_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.06    0.08    0.18   12.61 ^ clkbuf_3_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_1_0_CLK (net)
                  0.08    0.00   12.61 ^ clkbuf_4_2__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     7    0.10    0.11    0.20   12.81 ^ clkbuf_4_2__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_2__leaf_CLK (net)
                  0.11    0.00   12.81 ^ clkbuf_leaf_43_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.04    0.06    0.17   12.98 ^ clkbuf_leaf_43_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_43_CLK (net)
                  0.06    0.00   12.98 ^ core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.60   12.38   clock uncertainty
                          0.00   12.38   clock reconvergence pessimism
                         -0.18   12.21   library setup time
                                 12.21   data required time
-----------------------------------------------------------------------------
                                 12.21   data required time
                                 -5.93   data arrival time
-----------------------------------------------------------------------------
                                  6.28   slack (MET)



==========================================================================
finish report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
finish max_slew_check_slack
--------------------------------------------------------------------------
0.012871704064309597

==========================================================================
finish max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
finish max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0086

==========================================================================
finish max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
finish max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
finish max_capacitance_check_slack
--------------------------------------------------------------------------
0.01334045734256506

==========================================================================
finish max_capacitance_check_limit
--------------------------------------------------------------------------
0.5346779823303223

==========================================================================
finish max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.0250

==========================================================================
finish max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
finish max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
finish max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
finish setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
finish hold_violation_count
--------------------------------------------------------------------------
hold violation count 30

==========================================================================
finish report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_src1_value_a3[11]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.32    0.32 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.17    0.50 ^ clkbuf_1_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.69 ^ clkbuf_3_5_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.27    0.97 ^ clkbuf_4_11__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.20    1.17 ^ clkbuf_leaf_68_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    1.17 ^ core.CPU_src1_value_a3[11]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.36    1.53 ^ core.CPU_src1_value_a3[11]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.09    1.61 v _05952_/Y (sky130_fd_sc_hd__inv_1)
   0.35    1.97 v _11479_/SUM (sky130_fd_sc_hd__ha_2)
   0.75    2.72 v _08072_/X (sky130_fd_sc_hd__or4_2)
   0.63    3.35 v _08077_/X (sky130_fd_sc_hd__or4_2)
   0.33    3.68 ^ _08078_/Y (sky130_fd_sc_hd__a211oi_4)
   0.16    3.84 v _08288_/Y (sky130_fd_sc_hd__o311ai_4)
   0.53    4.37 ^ _08482_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.12    4.48 v _08483_/Y (sky130_fd_sc_hd__a2111oi_2)
   0.19    4.67 v _08484_/Y (sky130_fd_sc_hd__xnor2_2)
   0.94    5.62 ^ _08501_/Y (sky130_fd_sc_hd__a211oi_4)
   0.31    5.93 v _09216_/Y (sky130_fd_sc_hd__o21ai_0)
   0.00    5.93 v core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           5.93   data arrival time

  11.90   11.90   clock clk (rise edge)
   0.00   11.90   clock source latency
   0.00   11.90 ^ pll/CLK (avsdpll)
   0.32   12.22 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19   12.42 ^ clkbuf_1_0_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19   12.61 ^ clkbuf_3_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.20   12.81 ^ clkbuf_4_2__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.17   12.98 ^ clkbuf_leaf_43_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   12.98 ^ core.CPU_Xreg_value_a4[1][29]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.60   12.38   clock uncertainty
   0.00   12.38   clock reconvergence pessimism
  -0.18   12.21   library setup time
          12.21   data required time
---------------------------------------------------------
          12.21   data required time
          -5.93   data arrival time
---------------------------------------------------------
           6.28   slack (MET)



==========================================================================
finish report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_src2_value_a4[1]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Dmem_value_a5[15][1]$_SDFFE_PP1P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.32    0.32 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.17    0.50 ^ clkbuf_1_1_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.68 ^ clkbuf_3_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.37    1.05 ^ clkbuf_4_14__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.27    1.32 ^ clkbuf_leaf_78_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    1.32 ^ core.CPU_src2_value_a4[1]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_4)
   0.41    1.74 v core.CPU_src2_value_a4[1]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_4)
   0.08    1.82 ^ _06761_/Y (sky130_fd_sc_hd__nand2_1)
   0.08    1.89 v _06762_/Y (sky130_fd_sc_hd__nand3b_1)
   0.00    1.89 v core.CPU_Dmem_value_a5[15][1]$_SDFFE_PP1P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.89   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.32    0.32 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.52 ^ clkbuf_1_0_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.70 ^ clkbuf_3_2_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.22    0.91 ^ clkbuf_4_5__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    1.09 ^ clkbuf_leaf_136_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    1.09 ^ core.CPU_Dmem_value_a5[15][1]$_SDFFE_PP1P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.95    2.04   clock uncertainty
   0.00    2.04   clock reconvergence pessimism
  -0.06    1.98   library hold time
           1.98   data required time
---------------------------------------------------------
           1.98   data required time
          -1.89   data arrival time
---------------------------------------------------------
          -0.09   slack (VIOLATED)



==========================================================================
finish critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
finish critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
finish critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
finish critical path delay
--------------------------------------------------------------------------
5.9293

==========================================================================
finish critical path slack
--------------------------------------------------------------------------
6.2787

==========================================================================
finish slack div critical path delay
--------------------------------------------------------------------------
105.892770

==========================================================================
finish report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             4.70e-03   7.53e-04   1.04e-08   5.45e-03  32.3%
Combinational          1.97e-03   3.94e-03   2.28e-08   5.92e-03  35.0%
Clock                  3.09e-03   2.43e-03   2.73e-09   5.52e-03  32.7%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  9.76e-03   7.13e-03   3.59e-08   1.69e-02 100.0%
                          57.8%      42.2%       0.0%
```

#### The final reports are created when the pll is not connected to power grid. It is due to unconnectivity of VDD and VSS pins

</details>

</details>

<details>

<summary> References </summary>

* https://makerchip.com/sandbox
* https://github.com/Subhasis-Sahu/BabySoC_Simulation.git
* https://github.com/stevehoover
* https://github.com/YosysHQ/yosys.git
* https://github.com/The-OpenROAD-Project/OpenSTA.git
* https://github.com/kunalg123
* https://www.vsdiat.com
* https://github.com/manili/VSDBabySoC.git

</details>
