# SUB Operation

This folder contains two programs that demonstrate the `SUB` instruction and how it affects the EFLAGS register.

## Program 1: sub1.asm

### Operation

The program subtracts:

**50 - 80 = -30**

The 8-bit representation of `-30` is `E2h`.

### EFLAGS

| Flag | Status  | Reason                                                                             |
| ---- | ------- | ---------------------------------------------------------------------------------- |
| CF   | Set     | A borrow is needed because 50 is smaller than 80 when treated as unsigned numbers. |
| ZF   | Cleared | The result is not zero.                                                            |
| SF   | Set     | The most significant bit of `E2h` is 1.                                            |
| OF   | Cleared | -30 is within the signed 8-bit range of -128 to 127.                               |
| PF   | Set     | `E2h` is `11100010`, which has four 1-bits.                                        |
| AF   | Cleared | There is no borrow from the lower 4 bits.                                          |

### Explanation

The result is negative, so the sign flag is set. Since 50 is smaller than 80, an unsigned borrow occurs and CF is set. However, -30 can be represented normally as a signed 8-bit number, so there is no signed overflow.

## Program 2: sub2.asm

### Operation

The program subtracts:

**1000 - 2000 = -1000**

The 16-bit representation of `-1000` is `FC18h`.

### EFLAGS

| Flag | Status  | Reason                                                      |
| ---- | ------- | ----------------------------------------------------------- |
| CF   | Set     | A borrow is needed because 1000 is smaller than 2000.       |
| ZF   | Cleared | The result is not zero.                                     |
| SF   | Set     | The most significant bit of `FC18h` is 1.                   |
| OF   | Cleared | -1000 is within the signed 16-bit range.                    |
| PF   | Set     | The low byte `18h` contains two 1-bits, giving even parity. |
| AF   | Cleared | There is no borrow between the lower and upper nibbles.     |

### Explanation

The result is -1000, so the sign flag is set. Since the first number is smaller than the second, the subtraction requires an unsigned borrow, which sets CF. The result is still within the signed 16-bit range, so OF remains cleared.
