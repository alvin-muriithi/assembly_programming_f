# 01.asm – Push and Pop (64-bit)

```asm
mov rax, 100
mov rbx, 200
push rax
push rbx
nop
pop rcx
pop rdx
nop
mov rax, 60
xor rdi, rdi
syscall
```

## How the stack works in this program

When the program starts, RSP was at `0x7fffffff70d0`, and the value on top of the stack was `1`. This is placed there by Linux and is the number of command-line arguments.

The first two `mov` instructions load 100 into RAX and 200 into RBX. They do not touch the stack, so RSP stays the same.

`push rax` lowers RSP by 8 to `0x7fffffff70c8` and stores 100 there. `push rbx` lowers RSP by another 8 to `0x7fffffff70c0` and stores 200 there. At this point 200 is on top and 100 is just under it.

`pop rcx` reads the top value (200) into RCX and raises RSP by 8 back to `0x7fffffff70c8`. `pop rdx` reads the next value (100) into RDX and raises RSP to `0x7fffffff70d0`, which is where it started. The program then exits with code 0.

## Observations

- The values came out in reverse order. RCX got 200 and RDX got 100, even though 100 was pushed first. This shows the stack is Last In, First Out.
- RSP changed by exactly 8 bytes on every push and pop.
- There were two pushes and two pops, so the stack ended up balanced at the starting address.
- RAX and RBX still held 100 and 200 after the pops, because pop only changes the register being popped into.
