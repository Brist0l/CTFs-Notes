the write syscall has it's number set to 1.You also need to
specify the FD in which it is going to write in the args, it's
the first argument.
The second arg is "where" the data and the third arg is "how
much" the data is.
The args are specified in the following registers in order:
- `rdi`
- `rsi
- `rdx`
- `r10
- `r8`
- `r9`

eg:![[Pasted image 20250214212841.png]]

You can chain syscalls by modifying the registers,eg:![[Pasted image 20250214213443.png]]
![[Pasted image 20250214214043.png]]
