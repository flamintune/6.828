# RISC-V assembly (easy)

make fs.img 获得 汇编代码 .asm

## read code (call.asm)

- function g
- function f
- function main

Which registers contain arguments to functions? For example, which register holds 13 in main's call to `printf`?

a2 a1

Where is the call to function `f` in the assembly code for main? Where is the call to `g`? (Hint: the compiler may inline functions.)

addi s0,sp,16

addi s0,sp,16

At what address is the function `printf` located?

630 ra + 1536

ra = sp + 8

What value is in the register `ra` just after the `jalr` to `printf` in `main`?

ra = pc + 4



In the following code, what is going to be printed after `'y='`? (**note: the answer is not a specific value.**) Why does this happen?

```c
	printf("x=%d y=%d", 3);
```

it print x=3 y=1

why?

## Some question

