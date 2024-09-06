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

## Writing C program, compiling it with GCC and RISC-V compiler and compairing the output
 
</details>


