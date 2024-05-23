# VSDSquadron Mini Research Internship

## Compile C Program using RISC-V Compiler


- If leafpad isn't installed, install it using the command `sudo apt install leafpad`
- Now we will write a program in C which sums the numbers from 1 to n.
```bash
$ leafpad sum1ton.c &  
```
<p align="center">
  <img width="1000" height="" src="/images/1.png">
</p>

- Now we Compile the C program `sum1ton.c` using the GCC compiler and it generates an executable named `a.out`.
- `./a.out`command executes the program we compiled.

```bash
$ gcc sum1ton.c 
$ ./a.out
```
<p align="center">
  <img width="1000" height="" src="/images/2.png">
</p>

#### Now let's compile the same program using RISC-V gcc compiler

<p align="center">
  <img width="1000" height="" src="/images/10.png">
</p>


```bash
$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
<p align="center">
  <img width="1000" height="" src="/images/3.png">
</p>

This instruction  translate C program `sum1ton.c`into an object file `sum1ton.o`
Let's understand each part of the instruction
- `riscv64-unknown-elf-gcc` :  It's the GNU compiler for the RISC-V 64-bit architecture, targeting an ELF (Executable and Linkable Format) . The "unknown" part indicates that the target system (operating system) is not explicitly defined
- `-O1` : This is an optimization flag instructing the compiler to perform basic optimizations for both size and speed.
- `-mabi=lp64` : This option specifies the ABI (Application Binary Interface) as LP64, which is a 64-bit calling convention.
- `-march=rv64i` : This tells the compiler to generate code for the base integer instruction set of the RV64 architecture.
- `-o sum1ton.o`: This is the output option. The compiler will generate an object file named sum1ton.o.
- `sum1ton.c`: This is the input file, a C source code file named sum1ton.c that contains the program to be compile


Now let's analyze the object file generated using the instruction 👇
```bash
$ riscv64-unknown-elf-objdump -d sum1ton.o
```
<p align="center">
  <img width="1000" height="" src="/images/4.png">
</p>

- `riscv64-unknown-elf-objdump`: This is the command itself. It's a tool called `objdump` for the RISC-V 64-bit architecture and targets ELF object files. The "unknown" part again indicates that the target system is not explicitly defined.
- `-d` : The -d flag instructs objdump to disassemble the object file. Disassembly is the process of converting machine code instructions stored in the object file back into a human-readable format (assembly language).
- `sum1ton.o` : the name of the object file that has to be disassemble

<p align="center">
  <img width="1000" height="" src="/images/5.png">
</p>

```bash
$ riscv64-unknown-elf-objdump -d sum1ton.o | less
```
<p align="center">
  <img width="1000" height="" src="/images/6.png">
</p>

-  `| less` : The pipe (|) symbol directs the output of objdump (the disassembled instructions) to the `less` command. `less` is a pager utility that allows you to view the output one page at a time, for long outputs.
  
  
<p align="center">
  <img width="1000" height="" src="/images/7.png">
</p>

- Let's calculate the number of instruction that the main() is using
```
Number of instrustions = (101c0 -10184)/4
                       = (3C)/4 = F
```
- So, total 15 instructions are present in main() function.

- Let's change the optimization flag from `-01` to `-Ofast` and observe the changes in output
```bash
$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c   
```
- `-Ofast`: This is an optimization flag that tells the compiler to perform aggressive optimizations for speed.
<p align="center">
  <img width="1000" height="" src="/images/8.png">
</p>

```bash
$ riscv64-unknown-elf-objdump -d sum1ton.o | less
```

<p align="center">
  <img width="1000" height="" src="/images/9.png">
</p>

```
Number of instrustions = (101e0 -101b0)/4
                       = (30)/4 = C
```
- So, total 12 instructions are present in main() function.

Therefore, the number of instructions is different for `-O1` and `-Ofast` i.e, 15 and 12, when n=100.
