# ADD Operation

This folder contains two programs that demonstrate the `ADD` instruction and how it affects the EFLAGS register.

## Program 1: add1.asm

### Operation

The program adds:

**120 + 10 = 130**

The result is `130`, which is `82h` in hexadecimal.

### EFLAGS

| Flag | Status  | Reason                                                                                |
| ---- | ------- | ------------------------------------------------------------------------------------- |
| CF   | Cleared | There was no carry out of the most significant bit because the result fits in 8 bits. |
| ZF   | Cleared | The result is 130, not zero.                                                          |
| SF   | Set     | The most significant bit of `82h` is 1.                                               |
| OF   | Set     | 130 is outside the signed 8-bit range of -128 to 127.                                 |
| PF   | Set     | `82h` is `10000010`, which contains two 1-bits. The number of 1-bits is even.         |
| AF   | Set     | There was a carry from the lower 4 bits to the upper 4 bits during the addition.      |

### Explanation

The addition does not produce an unsigned carry, so CF is cleared. However, when the result is considered as a signed 8-bit number, 130 cannot be represented, so OF is set. The result has its highest bit set, so SF is also set.

## Program 2: add2.asm

### Operation

The program adds:

**32000 + 500 = 32500**

The result is `32500`, which is `7EF4h` in hexadecimal.

### EFLAGS

| Flag | Status  | Reason                                                      |
| ---- | ------- | ----------------------------------------------------------- |
| CF   | Cleared | The result fits within the 16-bit unsigned range.           |
| ZF   | Cleared | The result is not zero.                                     |
| SF   | Cleared | The most significant bit of `7EF4h` is 0.                   |
| OF   | Cleared | 32500 is within the signed 16-bit range of -32768 to 32767. |
| PF   | Cleared | The low byte `F4h` has five 1-bits, which is odd parity.    |
| AF   | Cleared | There was no carry from bit 3 to bit 4.                     |

### Explanation

The addition produces a positive result that fits within the 16-bit signed and unsigned ranges. Therefore, there is no carry or signed overflow. The sign bit is also 0, so SF is cleared.
