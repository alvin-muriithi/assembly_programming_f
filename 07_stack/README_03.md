# 03.asm – Call and Ret (64-bit)

```asm
_start:
    mov rax, 10
    call calculate

after_call:
    nop
    mov rax, 60
    xor rdi, rdi
    syscall

calculate:
    add rax, 5
    ret
```

## How the stack works in this program

This program has no push or pop instructions. The stack is only used by `call` and `ret`.

At the start, RSP was `0x7fffffff70d0`. `mov rax, 10` sets RAX to 10 and does not affect the stack.

`call calculate` pushes the address of the next instruction onto the stack and then jumps to `calculate`. RSP dropped by 8 to `0x7fffffff70c8`, and the value stored there was `0x40100a`, which is the address of the `after_call` label.

Inside `calculate`, `add rax, 5` changes RAX to 15. RSP does not change here.

`ret` pops `0x40100a` off the stack and jumps to it. RSP goes back up to `0x7fffffff70d0` and the program continues at `after_call`, then exits with code 0.

## Observations

- `call` works like a push of the return address followed by a jump.
- `ret` works like a pop into the instruction pointer.
- RAX was 15 after returning, so the function passed its result back in a register, not on the stack.
- The function did not need its own stack frame because it had no local variables.
- If the function had pushed something and not popped it, `ret` would have used the wrong value as the return address and the program would crash.
