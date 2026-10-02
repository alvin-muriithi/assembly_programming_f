# DIV Operation

This folder contains two programs that demonstrate the `DIV` instruction.

Unlike `ADD`, `SUB`, and `MUL`, the `DIV` instruction does not define the arithmetic flags in EFLAGS.

## Program 1: div1.asm

### Operation

The program divides:

**100 ÷ 7**

The result is:

```text
Quotient  = 14
Remainder = 2
```

For an 8-bit division:

* The quotient is stored in `AL`.
* The remainder is stored in `AH`.

Therefore:

```text
AL = 14
AH = 2
```

### EFLAGS

| Flag | Status    | Reason                                          |
| ---- | --------- | ----------------------------------------------- |
| CF   | Undefined | `DIV` does not define the Carry Flag.           |
| ZF   | Undefined | `DIV` does not define the Zero Flag.            |
| SF   | Undefined | `DIV` does not define the Sign Flag.            |
| OF   | Undefined | `DIV` does not define the Overflow Flag.        |
| PF   | Undefined | `DIV` does not define the Parity Flag.          |
| AF   | Undefined | `DIV` does not define the Auxiliary Carry Flag. |

### Explanation

The division produces a quotient of 14 and a remainder of 2. The important result of this instruction is the quotient and remainder, not the EFLAGS values.

The arithmetic flags are **undefined after `DIV`**. Therefore, any values that GDB displays for these flags should not be interpreted as being set or cleared by the division.

## Program 2: div2.asm

### Operation

The program divides:

**50000 ÷ 300**

The result is:

```text
Quotient  = 166
Remainder = 200
```

For this 16-bit division:

* The quotient is stored in `AX`.
* The remainder is stored in `DX`.

Therefore:

```text
AX = 166
DX = 200
```

### EFLAGS

| Flag | Status    | Reason                                          |
| ---- | --------- | ----------------------------------------------- |
| CF   | Undefined | `DIV` does not define the Carry Flag.           |
| ZF   | Undefined | `DIV` does not define the Zero Flag.            |
| SF   | Undefined | `DIV` does not define the Sign Flag.            |
| OF   | Undefined | `DIV` does not define the Overflow Flag.        |
| PF   | Undefined | `DIV` does not define the Parity Flag.          |
| AF   | Undefined | `DIV` does not define the Auxiliary Carry Flag. |

### Explanation

The division produces a quotient of 166 and a remainder of 200. Both values are valid for the division.

The EFLAGS arithmetic flags are undefined after `DIV`. This means that GDB may show values for these flags, but those values should not be described as being set or cleared by the `DIV` instruction.
