# 04.asm – Function with a Stack Frame (32-bit)

```asm
BITS 32
_start:
    call work
    mov eax, 1
    xor ebx, ebx
    int 0x80

work:
    push ebp
    mov ebp, esp
    sub esp, 8
    mov dword [ebp - 4], 100
    mov dword [ebp - 8], 200
    mov eax, [ebp - 4]
    mov ebx, [ebp - 8]
    nop
    mov esp, ebp
    pop ebp
    ret
```

## How the stack works in this program

This is the 32-bit version of `02.asm`. It uses ESP and EBP instead of RSP and RBP, and every push or pop moves 4 bytes instead of 8.

At the start, ESP was `0xffff6530`. `call work` pushes the return address `0x08049005` (the address of `mov eax, 1`) and ESP drops to `0xffff652c`.

In `work`, `push ebp` saves the old EBP (0) and ESP drops to `0xffff6528`. `mov ebp, esp` makes EBP point to this address. `sub esp, 8` moves ESP down to `0xffff6520`, making room for two 4-byte local variables.

The value 100 is stored at `ebp - 4` and 200 at `ebp - 8`. These are then loaded into EAX and EBX. ESP does not move during this part.

`mov esp, ebp` brings ESP back to `0xffff6528`, removing the local variables. `pop ebp` restores EBP to 0 and ESP becomes `0xffff652c`. `ret` pops the return address and jumps back, leaving ESP at `0xffff6530`. The program then exits using `int 0x80` with exit code 0.

## Observations

- Each push, pop, call and ret changed ESP by 4 bytes.
- Only 8 bytes were needed for two local variables, compared with 16 bytes in the 64-bit version.
- The stack addresses (`0xffff....`) are shorter than the 64-bit ones because this is a 32-bit program.
- `xor ebx, ebx` overwrites the 200 left in EBX, because in 32-bit Linux EBX holds the exit code.
- ESP ended at the same address it started at.
