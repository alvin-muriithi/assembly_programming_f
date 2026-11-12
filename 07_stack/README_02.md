# 02.asm – Function with a Stack Frame (64-bit)

```asm
_start:
    call work
    mov rax, 60
    xor rdi, rdi
    syscall

work:
    push rbp
    mov rbp, rsp
    sub rsp, 16
    mov qword [rbp - 8], 100
    mov qword [rbp - 16], 200
    mov rax, [rbp - 8]
    mov rbx, [rbp - 16]
    nop
    mov rsp, rbp
    pop rbp
    ret
```

## How the stack works in this program

At the start, RSP was `0x7fffffff70d0`.

`call work` pushes the return address onto the stack and jumps to `work`. RSP dropped to `0x7fffffff70c8`, and the value stored there was `0x401005`, which is the address of `mov rax, 60` in `_start`. This is how the program knows where to come back to.

Inside `work`, `push rbp` saves the old RBP (which was 0) and RSP drops to `0x7fffffff70c0`. `mov rbp, rsp` then sets RBP to this same address, so RBP becomes a fixed reference point for the function. `sub rsp, 16` moves RSP down to `0x7fffffff70b0`, making room for two 8-byte local variables.

The two `mov qword` instructions write 100 at `rbp - 8` and 200 at `rbp - 16`. These values are then read back into RAX and RBX. None of these instructions move RSP; they only read and write memory using RBP as the base.

To leave the function, `mov rsp, rbp` brings RSP back up to `0x7fffffff70c0`, which removes the local variables in one step. `pop rbp` restores RBP to 0 and RSP becomes `0x7fffffff70c8`. Finally `ret` pops `0x401005` and jumps back to `_start`, leaving RSP at `0x7fffffff70d0`. The program then exits with code 0.

## Observations

- The return address pushed by `call` matched the address of the instruction right after the call.
- RBP stayed fixed while the function was running, so the local variables could always be found at `rbp - 8` and `rbp - 16`.
- `sub rsp, 16` and `mov rsp, rbp` moved RSP by 16 at once, without using push or pop.
- The cleanup happened in the reverse order of the setup, which is why `ret` found the correct return address.
- RSP ended at the same address it started at.
