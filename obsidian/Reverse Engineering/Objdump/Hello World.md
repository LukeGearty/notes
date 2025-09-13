```C

#include <stdio.h>

int main() {
	printf("Hello, World!\n");
return 0;

}
```

Compiled
```
gcc hello_world.c -o hello
```

 ```
 objdump -d hello
 ```
Output
```

hello:	file format mach-o 64-bit x86-64

Disassembly of section __TEXT,__text:

0000000100000470 <_main>:
100000470: 55                          	pushq	%rbp
100000471: 48 89 e5                    	movq	%rsp, %rbp
100000474: 48 83 ec 10                 	subq	$0x10, %rsp
100000478: c7 45 fc 00 00 00 00        	movl	$0x0, -0x4(%rbp)
10000047f: 48 8d 3d 16 00 00 00        	leaq	0x16(%rip), %rdi        ## 0x10000049c <_printf+0x10000049c>
100000486: b0 00                       	movb	$0x0, %al
100000488: e8 09 00 00 00              	callq	0x100000496 <_printf+0x100000496>
10000048d: 31 c0                       	xorl	%eax, %eax
10000048f: 48 83 c4 10                 	addq	$0x10, %rsp
100000493: 5d                          	popq	%rbp
100000494: c3                          	retq

Disassembly of section __TEXT,__stubs:

0000000100000496 <__stubs>:
100000496: ff 25 64 0b 00 00           	jmpq	*0xb64(%rip)            ## 0x100001000 <_printf+0x100001000>


```

# Line by Line:

```
100000470: 55                          	pushq	%rbp
```
Value of base pointer register %rbp (holds the previous function's) is pushed onto the stack. Decrease stack point %rsp by 8 bytes and copy %rbp there. Part of the 
function prologue

https://gregfoletta.gitbooks.io/assembly-constructs/content/functions.html
https://www.cs.nmsu.edu/~jcook/posts/basic-x86-64-assembly/


```
100000471: 48 89 e5                    	movq	%rsp, %rbp
```
movq = move quadword (8 bytes)
Copies the current value of the stack pointer register %rsp into the base pointer register %rbp
https://cs61.seas.harvard.edu/site/2019/Asm/
https://courses.cs.washington.edu/courses/cse351/16sp/lectures/06-procedures_16sp.pdf

```
100000474: 48 83 ec 10    subq   $0x10, %rsp

```
subq - subtract quadword (8 bytes)
$0x10 - the immediate constant 0x10 (hex, 16 in decimal)
%rsp - stack pointer register
Subtracts 16 bytes from %rsp
https://stackoverflow.com/questions/61416433/x86-explanation-number-of-function-arguments-and-local-variables

```
100000478: c7 45 fc 00 00 00 00        	movl	$0x0, -0x4(%rbp)

```
movl - suffix indicates a "long" operand size - 4 bytes (32 bits)
$0x0 - immediate constant 0
-0x4(%rbp) - take value in register %rbp and subtracting 4
Stores 32 bit value 0 at memory location %rbp - 4

https://web.stanford.edu/class/archive/cs/cs107/cs107.1166/guide_x86-64.html
https://stackoverflow.com/questions/48300784/trouble-understanding-this-assembly-code

```
10000047f: 48 8d 3d 16 00 00 00        	leaq	0x16(%rip), %rdi        ## 0x10000049c <_printf+0x10000049c>

```
leaq - load effective address - compute the address specified by an expression without accesing memory
