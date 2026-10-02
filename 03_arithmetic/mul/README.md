# MUL Operation

This folder contains two programs that demonstrate the `MUL` instruction and how it affects the EFLAGS register.

## Program 1: mul1.asm

### Operation

The program multiplies:

**25 × 10 = 250**

For an 8-bit multiplication, the result is stored in the `AX` register.

The result is:

```text
AX = 00FAh
AH = 00h
AL = FAh
```

### EFLAGS

| Flag | Status    | Reason                                                                               |
| ---- | --------- | ------------------------------------------------------------------------------------ |
| CF   | Cleared   | The upper half of the result (`AH`) is zero, so the result fits in the lower 8 bits. |
| ZF   | Undefined | `MUL` does not define the Zero Flag.                                                 |
| SF   | Undefined | `MUL` does not define the Sign Flag.                                                 |
| OF   | Cleared   | The upper half of the result is zero, so there is no unsigned overflow.              |
| PF   | Undefined | `MUL` does not define the Parity Flag.                                               |
| AF   | Undefined | `MUL` does not define the Auxiliary Carry Flag.                                      |

### Explanation

The multiplication gives 250, which fits inside an unsigned 8-bit value. The upper half of the result (`AH`) is zero, so CF and OF are cleared.

The other flags are **undefined** after `MUL`. This means we should not use the values displayed by GDB for ZF, SF, PF or AF as meaningful results.

## Program 2: mul2.asm

### Operation

The program multiplies:

**3000 × 200 = 600000**

For a 16-bit multiplication, the result is stored in `DX:AX`.

The result is:

```text
DX:AX = 0009:27C0h
```

### EFLAGS

| Flag | Status    | Reason                                                                                                  |
| ---- | --------- | ------------------------------------------------------------------------------------------------------- |
| CF   | Set       | The upper half of the result (`DX`) is not zero, meaning the result does not fit completely in `AX`.    |
| ZF   | Undefined | `MUL` does not define the Zero Flag.                                                                    |
| SF   | Undefined | `MUL` does not define the Sign Flag.                                                                    |
| OF   | Set       | The upper half of the result is non-zero, indicating that the result is larger than can fit in 16 bits. |
| PF   | Undefined | `MUL` does not define the Parity Flag.                                                                  |
| AF   | Undefined | `MUL` does not define the Auxiliary Carry Flag.                                                         |

### Explanation

The result is 600000, which is larger than the maximum 16-bit unsigned value of 65535. Therefore, the result needs both `DX` and `AX` to store it.

Because `DX` is not zero, CF and OF are set. The other flags are undefined because the `MUL` instruction does not define them.
