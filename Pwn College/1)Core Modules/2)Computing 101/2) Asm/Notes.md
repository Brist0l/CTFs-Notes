moving a value into rax and then invoking `syscall` will ask the
OS to call that syscall , for exit the number in rax should
be 60.
![[Pasted image 20250213122112.png]]

Now these syscalls can take multiple args, although exit only
takes 1 which is the exit code . If we specify the exit code
in `rdi` it will exit with that code.
![[Pasted image 20250213122508.png]]

In order to execute these program we must assemble the `.s` file
and link it. We will be assembling it with `as` and we need to
specify the asm dialect before we start assembling it.That can
be done by `.intel_syntax noprefix` .`.intel_syntax noprefix` tells the 
assembler that you will be using Intel assembly syntax, and 
specifically the variant of it where you don't have to add 
extra prefixes to every instruction. We'll talk about these 
later, but for now, we'll let the assembler figure it out!

`as -o asm.o asm.s` is used to assemble it.
`ld -o exe asm.o` is used to link it.

To access a memory address use `[]` . e.g.:
`mov rdi,[6969]`
