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

  ## Write C program and compile it with GCC and RISC-V compiler and compare the outputs
  
</details>


