# 05.asm – Saving and Restoring Registers (64-bit)

```asm
mov rax, 0x1122334455667788
mov rbx, 0x1234
push rax
push rbx
nop
pop rbx
pop rax
mov rax, 60
xor rdi, rdi
syscall
```

## How the stack works in this program

At the start, RSP was `0x7fffffff70d0`. The two `mov` instructions load a large 8-byte value into RAX and a small value into RBX, without touching the stack.

`push rax` lowers RSP to `0x7fffffff70c8` and stores `0x1122334455667788`. `push rbx` lowers RSP to `0x7fffffff70c0` and stores `0x1234`. Even though `0x1234` is small, it still takes a full 8-byte slot and is stored as `0x0000000000001234`.

The pops are done in reverse order. `pop rbx` takes `0x1234` back into RBX and RSP rises to `0x7fffffff70c8`. `pop rax` takes `0x1122334455667788` back into RAX and RSP returns to `0x7fffffff70d0`. The program then exits with code 0.

## Observations

- Because the registers were popped in reverse order, both got back their original values. This is the usual way to save and restore registers.
- In `01.asm` the values were popped into different registers and came out swapped. Here nothing is swapped because the order is reversed on purpose.
- Looking at the stack byte by byte showed `88 77 66 55 44 33 22 11` for RAX and `34 12 00 00 00 00 00 00` for RBX. The lowest byte is stored first (little-endian), but the value is correct again once it is popped.
- RSP moved by 8 on every push and pop and ended at its starting address.
