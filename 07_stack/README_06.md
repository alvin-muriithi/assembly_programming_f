# 06.asm – Saving and Restoring Registers (32-bit)

```asm
BITS 32
mov eax, 0x11223344
mov ebx, 0x1234
push eax
push ebx
nop
pop ebx
pop eax
mov eax, 1
xor ebx, ebx
int 0x80
```

## How the stack works in this program

This is the 32-bit version of `05.asm`, so it uses ESP and each push or pop moves 4 bytes.

At the start, ESP was `0xffff6530`. EAX is set to `0x11223344` and EBX to `0x1234`.

`push eax` lowers ESP to `0xffff652c` and stores `0x11223344`. `push ebx` lowers ESP to `0xffff6528` and stores `0x00001234`.

`pop ebx` takes `0x1234` back into EBX and ESP rises to `0xffff652c`. `pop eax` takes `0x11223344` back into EAX and ESP returns to `0xffff6530`. Both registers now have their original values.

After that, EAX is set to 1 (the exit system call) and EBX to 0 (the exit code), and `int 0x80` ends the program.

## Observations

- ESP changed by 4 bytes each time, compared with 8 bytes in `05.asm`.
- Popping in reverse order restored both registers correctly.
- The small value `0x1234` was still stored as a full 4-byte slot.
- The restored values were overwritten straight away for the exit call, since 32-bit Linux uses EAX and EBX for the system call number and exit code.
- ESP ended at the same address it started at.
