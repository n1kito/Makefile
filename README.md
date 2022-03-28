<p align="center">
<img src="img/Makefile.png" width="300px">
</p>

The make program uses the makefile data base and the last-modification times of the files to decide which of the files need to be updated.

**Concequences of updating files**
+ When make recompiles the editor, each changed C source file must be recompiled.  
+ If a header file has changed, each C source file that includes the header file must be recompiled to be safe. Each compilation produces an object file corresponding to the source file.  
+ Finally, if any source file has been recompiled, all the object files, whether newly made or saved from previous compilations, must be linked together to produce the new executable editor.

<details>
<summary><b>Understanding the compilation process</b></summary>
<br>

> Compilation is the process of translating the code you write into a language that is native to the machine you are targeting.

## Compilation steps

```mermaid
flowchart LR;
A[.c Code]-- Preprocessing -->B(["Pre-processed File (.i)"])
B-- Compilation -->C(["Assembly Code (.s)"])
C-- Assembly -->D(["Machine Code (.o, .obj)"])
D-- Linking -->E["Executable (.out)"]
```

## Preprocessing
In pre-processed (.i) files:
+ Header files are included
+ All Macros are resolved (replaced with their values)
+ Comments are removed.

You can see what your .i file looks like by running the following command to pre-process it:
```shell
$ gcc -E yourFile.c
```

This main.c file:
```C
#include "main.h"

#define MESSAGE "Bof."

/* This program prints a happy message */
int	main(void)
{
	printf("%s", MESSAGE);
	return (0);
}
```

and its corresponding .h file:
```C
#ifndef MAIN_H
# define MAIN_H

#include <stdio.h>

#endif
```

Would be pre-processed into this main.i file:
```C
# 400 "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/stdio.h" 2 3 4
# 5 "./main.h" 2
# 2 "main.c" 2

int main(void)
{
 printf("%s", "Bof.");
 return (0);
}
```

## Compilation

Compilation takes pre-processed files as input, and converts this high-level source code into **assembly instructions**, which are **specific to the target processor architecture**.

It generates a .s file.

You can generate this file with the following command:
```shell
$ gcc -S yourFile.c
# This will run the preprocessor (cpp) over yourFile.c, perform the initial compilation and then stop before the assembler is run.
```

The .s file of my main.c looks like this:
```c
	.section	__TEXT,__text,regular,pure_instructions
	.build_version macos, 12, 0	sdk_version 12, 0
	.globl	_main                           ## -- Begin function main
	.p2align	4, 0x90
_main:                                  ## @main
	.cfi_startproc
## %bb.0:
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register %rbp
	subq	$16, %rsp
	movl	$0, -4(%rbp)
	leaq	L_.str(%rip), %rdi
	leaq	L_.str.1(%rip), %rsi
	movb	$0, %al
	callq	_printf
	xorl	%eax, %eax
	addq	$16, %rsp
	popq	%rbp
	retq
	.cfi_endproc
                                        ## -- End function
	.section	__TEXT,__cstring,cstring_literals
L_.str:                                 ## @.str
	.asciz	"%s"

L_.str.1:                               ## @.str.1
	.asciz	"Bof."

.subsections_via_symbols
```

## Assembly

Converts the assembly instructions that resulted from the compilation, and generated **object code** with .o extension.

The output consists of actual instructions to be run by the target processor.

You can generate .o files with the following command:

```shell
$ gcc -c yourFile.c
```

The following file might look like something like this:

![.o file](img/o_file.jpg)

## Linking

Until now we have only included header files, which contain function declarations, like the `printf()` used in our program, which still has no definition.  
It will be defined in the corresponding library files.

So **Linking** is the final stage of compilation, it takes all the **static libraries** and **object files** and generates a **single executable**.  
This executable extension will be `.out` on Linux platforms, and `.exe` on windows platforms.

Sources:  
[This video](https://www.youtube.com/watch?v=8XBsNtx6Wyk)

</details>


# Resources

[Apparently all I need to know is in here.](https://www.gnu.org/software/make/manual/make.html)  
I might die.