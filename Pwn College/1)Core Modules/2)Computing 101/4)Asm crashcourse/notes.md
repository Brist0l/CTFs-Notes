mul -> unsigned multiplication
imul -> signed multiplication
For the instruction `div reg`, the following happens:

- `rax = rdx:rax / reg`
- `rdx = remainder`

`rdx:rax` means that `rdx` will be the upper 64-bits of the 128-bit dividend and `rax` will be the lower 64-bits of the 128-bit dividend.
https://www.aldeid.com/wiki/X86-assembly/Instructions/div

MSB                                    LSB
+----------------------------------------+
|                   rax                  |
+--------------------+-------------------+
                     |        eax        |
                     +---------+---------+
                               |   ax    |
                               +----+----+
                               | ah | al |
                               +----+----+
We can use a math trick to optimize the modulo operator (`%`). 
Compilers use this trick a lot.If we have `x % y`, and `y` is a 
power of 2, such as `2^n`, the result will be the lower `n` bits 
of `x`.
![[Pasted image 20250215153759.png]]

![[Pasted image 20250215155023.png]]
same size registers can be moved into one another with `mov`
Take, for instance, `al`, the lowest 8 bits of `rax`.

The value in `al` (in bits) is:

```
rax = 10001010
```

If we shift once to the left using the `shl` instruction:

```
shl al, 1
```

The new value is:

```
al = 00010100
```

Everything shifted to the left, and the highest bit fell off while a new 0 was added to the right side.

![[Pasted image 20250215161808.png]]
solution of byte extraction

https://en.wikipedia.org/wiki/XOR_swap_algorithm, This may be 
used when `mov` isn't allowed.

![[Pasted image 20250216000948.png]]
this is the solution to check-even .(tip: it helps to write
down what is going wrong in the code and what the desired out
put must be)

It is worth noting, as you may have noticed, that values are stored in reverse order of how we represent them.

As an example, say:

```
[0x1330] = 0x00000000deadc0de
```

If you examined how it actually looked in memory, you would see:

```
[0x1330] = 0xde
[0x1331] = 0xc0
[0x1332] = 0xad
[0x1333] = 0xde
[0x1334] = 0x00
[0x1335] = 0x00
[0x1336] = 0x00
[0x1337] = 0x00
```

This format of storing things in 'reverse' is intentional in x86, and it's called "Little Endian".

`rsp` always stores the memory address of the top of the stack, i.e., the memory address of the last value pushed.And as alway
s remember that the stack will grow downwards.
![[Pasted image 20241210161506 1.png]]
when you reference an address like [rsp+x] , here x is in byte
and not in bits.


In x86, absolute jumps (jump to a specific address) are accomplished by first putting the target address in a register `reg`, then doing `jmp reg`.

For all jumps, there are three types:

- Relative jumps: jump + or - the next instruction.
- Absolute jumps: jump to a specific address.
- Indirect jumps: jump to the memory address specified in a register.
![[Pasted image 20250216155218.png]]
solution for relative jump. The labels are basically memory
addresses which can be acceded repetitively. the directive
`.rept 0x51` repeats the instructions till `.endr` 0x51 times.

In absolute jmp you must first move the address in a register
and then jump.

```assembly
; assume rdi = x, rax is output
; rdx = rdi mod 2
mov rax, rdi
mov rsi, 2
div rsi
; remainder is 0 if even
cmp rdx, 0
; jump to not_even code if it's not 0
jne not_even
; fall through to even code
mov rbx, 1
jmp done
; jump to this only when not_even
not_even:
mov rbx, 0
done:
mov rax, rbx
; more instructions here
```

jmp just adds to the instruction pointer.

The order of the labels is important:
![[Pasted image 20250217095958.png]]
A function is a callable segment of code that does not destroy control flow.

Functions use the instructions "call" and "ret".

The "call" instruction pushes the memory address of the next instruction onto the stack and then jumps to the value stored in the first argument.

Let's use the following instructions as an example:

```
0x1021 mov rax, 0x400000
0x1028 call rax
0x102a mov [rsi], rax
```

1. `call` pushes `0x102a`, the address of the next instruction, onto the stack.
2. `call` jumps to `0x400000`, the value stored in `rax`.

The "ret" instruction is the opposite of "call".

`ret` pops the top value off of the stack and jumps to it.

Let's use the following instructions and stack as an example:

```
Stack ADDR  VALUE
0x103f mov rax, rdx         RSP + 0x8   0xdeadbeef
0x1042 ret                  RSP + 0x0   0x0000102a
```

Here, `ret` will jump to `0x102a`.