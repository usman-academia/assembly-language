# Assembly Language Cheat Sheet, Rebuilt for Beginners
## NASM + DOSBox + AFD, with C++ side-by-side comparisons

This version is written for a beginner who wants the **idea** behind each instruction, not just a memorized syntax list.  
It also corrects a few common mistakes from the original notes, especially around `DIV`, `LOOP`, `CMP`, and register usage.

---

## How to use this sheet

Read one section at a time.  
For each example:

1. Read the explanation.
2. Compare the assembly with the C++ version.
3. Type the code yourself.
4. Run it.
5. Break it on purpose and watch what changes in AFD.

---

# 1) Getting Started

## What DOSBox is

DOSBox is a small emulator that pretends to be an old DOS machine.  
We use it because this style of 16-bit assembly is easiest to learn there.

### Basic workflow

<table>
<tr>
<th>Assembly workflow</th>
<th>C++ workflow analogy</th>
</tr>
<tr>
<td>

```text
1. Write the .asm file
2. Assemble it with NASM
3. Run the .com file
4. Debug with AFD
```

</td>
<td>

```cpp
// C++ feels more like:
1. write .cpp
2. compile it
3. run the program
4. debug with an IDE
```

</td>
</tr>
</table>

---

## Useful DOSBox commands

| Command | Meaning |
|---|---|
| `mount c c:\assembly` | Make a real folder available as drive C: |
| `c:` | Switch to the mounted drive |
| `dir` | List files |
| `cd folder` | Enter a folder |
| `cd ..` | Go back one folder |
| `cls` | Clear screen |
| `exit` | Close DOSBox |

---

## Assemble and run

```text
nasm hello.asm -o hello.com
hello
afd hello.com
```

---

# 2) The CPU Basics

Assembly talks almost directly to the CPU.  
The CPU mainly works with:

- **registers**: tiny fast storage inside the CPU
- **memory**: where variables and code live
- **flags**: tiny status bits the CPU sets automatically

---

## Registers you should know first

| Register | Simple meaning | C++ idea |
|---|---|---|
| `AX` | main 16-bit working register | `int` or `short` temporary |
| `BX` | general-purpose register | another temporary variable |
| `CX` | counter register | loop counter like `for (int i=...)` |
| `DX` | extra/data register | helper for division and I/O |
| `SP` | stack pointer | top of the stack |
| `BP` | stack base pointer | reference point for stack data |
| `SI` | source index | pointer-like helper |
| `DI` | destination index | pointer-like helper |

Each 16-bit register can be split into two 8-bit parts:

- `AX = AH + AL`
- `BX = BH + BL`
- `CX = CH + CL`
- `DX = DH + DL`

---

## Flags that matter most

| Flag | Meaning | Easy meaning |
|---|---|---|
| `ZF` | Zero Flag | result became zero |
| `CF` | Carry Flag | overflow in unsigned math |
| `SF` | Sign Flag | result looks negative |
| `OF` | Overflow Flag | signed overflow happened |

---

## Numbers in assembly

| Type | Example | Notes |
|---|---|---|
| Decimal | `10` | normal human number |
| Hexadecimal | `0Ah` | common in assembly |
| Binary | `00001010b` | useful for bit tricks |

---

## ASCII idea

A character is just a number.

| Character | ASCII code |
|---|---|
| `'0'` | `30h` |
| `'7'` | `37h` |
| `'A'` | `41h` |
| `'a'` | `61h` |
| space | `20h` |
| Enter / CR | `0Dh` |
| Line feed / LF | `0Ah` |
| `$` | `24h` |

---

# 3) Basic Assembly Syntax

## A .COM program

For this course, we use `.COM` programs.  
They start at memory offset `100h`.

```nasm
org 100h

; your code here

mov ax, 4C00h
int 21h
```

### Why `org 100h`?

A `.COM` file loads at `100h`.  
So your labels and addresses must be based on that start position.

---

## Comments and data

| Assembly | Meaning | C++ analogy |
|---|---|---|
| `; comment` | ignored by NASM | `// comment` |
| `db` | define byte | `char` / `uint8_t` |
| `dw` | define word | `short` / `uint16_t` |
| `equ` | constant | `const` or `constexpr` |

```nasm
myByte   db 10
myWord   dw 1000
myText   db 'Hello$', 0
count    equ 5
```

---

# 4) Your First Output Programs

## Print one character

<table>
<tr>
<th>Assembly</th>
<th>C++ equivalent idea</th>
</tr>
<tr>
<td>

```nasm
org 100h

mov ah, 02h
mov dl, 'A'
int 21h

mov ax, 4C00h
int 21h
```

</td>
<td>

```cpp
#include <iostream>
int main() {
    std::cout << 'A';
}
```

</td>
</tr>
</table>

### What is happening?

- `AH = 02h` tells DOS: “print one character”
- `DL` holds the character
- `int 21h` calls DOS

---

## Print a string

<table>
<tr>
<th>Assembly</th>
<th>C++ equivalent idea</th>
</tr>
<tr>
<td>

```nasm
org 100h
jmp start

msg db 'Hello, World!', 13, 10, '$'

start:
    mov ah, 09h
    lea dx, [msg]
    int 21h

    mov ax, 4C00h
    int 21h
```

</td>
<td>

```cpp
#include <iostream>
int main() {
    std::cout << "Hello, World!" << std::endl;
}
```

</td>
</tr>
</table>

### Important
For `AH = 09h`, the string must end with `$`.

---

# 5) Moving Data Around

## `mov`, `push`, `pop`, `xchg`, `lea`

| Instruction | Meaning | C++ idea |
|---|---|---|
| `mov ax, 5` | put 5 into AX | `ax = 5;` |
| `mov bx, ax` | copy AX into BX | `bx = ax;` |
| `xchg ax, bx` | swap two values | `swap(ax, bx);` |
| `push ax` | save AX on stack | save variable temporarily |
| `pop bx` | restore from stack into BX | restore saved value |
| `lea dx, [msg]` | load address of `msg` | pointer to `msg` |

### Example: copy a value

```nasm
mov ax, 10
mov bx, ax
```

```cpp
int ax = 10;
int bx = ax;
```

---

# 6) Arithmetic

## Add, subtract, increment, decrement

<table>
<tr>
<th>Assembly</th>
<th>C++ equivalent</th>
</tr>
<tr>
<td>

```nasm
mov al, 5
add al, 3      ; AL = 8
sub al, 2      ; AL = 6
inc al         ; AL = 7
dec al         ; AL = 6
```

</td>
<td>

```cpp
int al = 5;
al += 3;
al -= 2;
al++;
al--;
```

</td>
</tr>
</table>

---

## Addition example with memory

```nasm
org 100h
jmp start

numA   db 20
numB   db 15
result db 0

start:
    mov al, [numA]
    add al, [numB]
    mov [result], al

    mov ax, 4C00h
    int 21h
```

### Think of it like this

```cpp
unsigned char numA = 20;
unsigned char numB = 15;
unsigned char result = numA + numB;
```

---

## `mul` and `div` are special

### `MUL`
- unsigned multiply
- uses fixed registers
- result may be wider than the input

### `DIV`
- unsigned division
- also uses fixed registers
- quotient and remainder go into different registers

#### 8-bit version

- `mul bl` uses `AL * BL`, result goes to `AX`
- `div bl` divides `AX` by `BL`
  - quotient goes to `AL`
  - remainder goes to `AH`

### Example

```nasm
mov al, 10
mov bl, 6
mul bl          ; AX = 60

mov bl, 4
div bl          ; AL = 15, AH = 0
```

### C++ idea

```cpp
int x = 10 * 6;
int q = x / 4;
int r = x % 4;
```

---

## Printing a digit

If a register contains the number `7`, that is not the same as the character `'7'`.

To print a single digit, add `30h`.

```nasm
mov al, 7
add al, 30h     ; now AL = ASCII '7'
mov ah, 02h
mov dl, al
int 21h
```

```cpp
int digit = 7;
std::cout << digit;
```

---

## Printing a two-digit number

This is one of the most confusing beginner topics, so here it is slowly.

### Idea

Suppose the number is `35`.

- divide by `10`
- quotient = `3`
- remainder = `5`
- print `3`, then print `5`

### Assembly

```nasm
org 100h

mov ax, 35      ; number to print
xor dx, dx      ; important before 16-bit DIV
mov bx, 10
div bx          ; AX = 3, DX = 5

push dx         ; save remainder

add al, 30h     ; tens digit to ASCII
mov ah, 02h
mov dl, al
int 21h

pop dx
mov al, dl      ; low byte of remainder
add al, 30h
mov ah, 02h
mov dl, al
int 21h

mov ax, 4C00h
int 21h
```

### C++ idea

```cpp
int n = 35;
std::cout << n;
```

### Correct rule for `DIV`
For **16-bit division**:
- dividend is in `DX:AX`
- divisor is the operand
- quotient goes to `AX`
- remainder goes to `DX`

This is why `xor dx, dx` is used before dividing 35 by 10.

---

# 7) Logic and Bitwise Instructions

## `and`, `or`, `xor`, `not`

| Instruction | Meaning | C++ idea |
|---|---|---|
| `and` | bitwise AND | `&` |
| `or` | bitwise OR | `|` |
| `xor` | bitwise XOR | `^` |
| `not` | flips all bits | `~` |

### Example

```nasm
mov al, 6      ; 00000110b
and al, 1      ; 00000000b or 00000001b depending on input
```

### Useful trick

```nasm
xor ax, ax     ; AX = 0
```

That is a very common way to clear a register.

### C++ analogy

```cpp
ax = 0;
```

---

# 8) Comparing and Jumping

## `cmp` does not store the result

`cmp a, b` is like doing `a - b` only to set the flags.

### Basic if-statement pattern

<table>
<tr>
<th>Assembly</th>
<th>C++ equivalent</th>
</tr>
<tr>
<td>

```nasm
cmp al, 5
je equal
; not equal code here
jmp done

equal:
; equal code here

done:
```

</td>
<td>

```cpp
if (al == 5) {
    // equal code
} else {
    // not equal code
}
```

</td>
</tr>
</table>

---

## Common jumps

| Jump | Meaning | Use when |
|---|---|---|
| `je` / `jz` | equal / zero | result was zero |
| `jne` / `jnz` | not equal / not zero | result was not zero |
| `jg` | greater, signed | signed numbers |
| `jl` | less, signed | signed numbers |
| `jge` | greater or equal, signed | signed numbers |
| `jle` | less or equal, signed | signed numbers |
| `ja` | above, unsigned | unsigned numbers |
| `jb` | below, unsigned | unsigned numbers |

### Important beginner rule
Use **signed jumps** (`jg`, `jl`, `jge`, `jle`) for signed values.  
Use **unsigned jumps** (`ja`, `jb`, `jae`, `jbe`) for unsigned values.

---

## Example: check if zero

```nasm
org 100h
jmp start

num db 0
msg1 db 'Zero', 13, 10, '$'
msg2 db 'Not zero', 13, 10, '$'

start:
    mov al, [num]
    cmp al, 0
    je is_zero

    mov ah, 09h
    lea dx, [msg2]
    int 21h
    jmp done

is_zero:
    mov ah, 09h
    lea dx, [msg1]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

### C++ idea

```cpp
if (num == 0) {
    // zero
} else {
    // not zero
}
```

---

# 9) Keyboard Input

## Read one key

```nasm
mov ah, 01h
int 21h        ; AL gets the pressed key
```

### C++ idea

```cpp
char ch;
std::cin >> ch;
```

---

## Read and echo a character

```nasm
org 100h
jmp start

prompt db 'Type a key: $'
reply  db 13, 10, 'You typed: $'

start:
    mov ah, 09h
    lea dx, [prompt]
    int 21h

    mov ah, 01h
    int 21h
    mov bl, al

    mov ah, 09h
    lea dx, [reply]
    int 21h

    mov ah, 02h
    mov dl, bl
    int 21h

    mov ax, 4C00h
    int 21h
```

---

# 10) Loops

## `loop` instruction

`loop label` does three things:

1. decreases `CX` by 1
2. checks whether `CX != 0`
3. jumps if it is not zero

### Very important
`loop` always uses **CX**.  
Do not expect it to use `BL`, `CL`, or any other register by itself.

---

## Counted loop

<table>
<tr>
<th>Assembly</th>
<th>C++ equivalent</th>
</tr>
<tr>
<td>

```nasm
mov cx, 5
print_loop:
    mov ah, 02h
    mov dl, '*'
    int 21h
    loop print_loop
```

</td>
<td>

```cpp
for (int i = 0; i < 5; i++) {
    std::cout << '*';
}
```

</td>
</tr>
</table>

---

## Print stars five times

```nasm
org 100h

mov cx, 5

again:
    mov ah, 02h
    mov dl, '*'
    int 21h
    loop again

mov ax, 4C00h
int 21h
```

---

## While-style loop

Sometimes you do not know the count in advance.

```nasm
start:
    mov al, 9

loop_here:
    ; do something
    dec al
    cmp al, 0
    jge loop_here
```

### C++ idea

```cpp
int al = 9;
while (al >= 0) {
    al--;
}
```

---

## Nested loops

A loop inside another loop.

### Example: 3 rows and 4 stars per row

```nasm
org 100h

mov cx, 3          ; outer loop counter

outer:
    push cx
    mov cx, 4      ; inner loop counter

inner:
    mov ah, 02h
    mov dl, '*'
    int 21h
    loop inner

    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    pop cx
    loop outer

mov ax, 4C00h
int 21h
```

### C++ idea

```cpp
for (int row = 0; row < 3; row++) {
    for (int col = 0; col < 4; col++) {
        std::cout << '*';
    }
    std::cout << '\n';
}
```

---

# 11) Procedures and the Stack

## Procedures are like functions

```nasm
call myProc
ret
```

### C++ idea

```cpp
myProc();
```

---

## Stack basics

The stack is a place to save temporary values.

| Assembly | Meaning | C++ idea |
|---|---|---|
| `push ax` | save AX | save a temporary |
| `pop ax` | restore AX | get saved value back |

### Rule
Every `push` should have a matching `pop`, in reverse order.

---

## Save and restore a register

```nasm
push ax
; code that changes AX
pop ax
```

### C++ idea

```cpp
int saved = ax;
// code changes ax
ax = saved;
```

---

## Simple procedure example

### Assembly

```nasm
org 100h
jmp start

newline:
    push ax
    push dx

    mov ah, 02h
    mov dl, 13
    int 21h
    mov dl, 10
    int 21h

    pop dx
    pop ax
    ret

start:
    call newline
    call newline

    mov ax, 4C00h
    int 21h
```

### C++ idea

```cpp
void newline() {
    std::cout << '\r' << '\n';
}
```

---

## Procedure that returns a value

```nasm
add_bytes:
    mov al, bl
    add al, cl
    ret
```

### C++ idea

```cpp
int add_bytes(int bl, int cl) {
    return bl + cl;
}
```

---

# 12) Arrays

An array is just a block of consecutive memory locations.

## Basic idea

```nasm
myArray db 3, 7, 1, 9, 5
```

Think of it like:

```cpp
int myArray[] = {3, 7, 1, 9, 5};
```

---

## Read array elements with a pointer

```nasm
org 100h
jmp start

arr db 3, 7, 1, 9, 5
len equ 5

start:
    lea bx, [arr]
    mov cx, len

next_item:
    mov al, [bx]
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h

    mov ah, 02h
    mov dl, ' '
    int 21h

    inc bx
    loop next_item

    mov ax, 4C00h
    int 21h
```

### C++ idea

```cpp
for (int i = 0; i < 5; i++) {
    std::cout << arr[i] << ' ';
}
```

---

## Sum an array

```nasm
org 100h
jmp start

arr   db 10, 20, 5, 15
len   equ 4
total dw 0

start:
    lea bx, [arr]
    mov cx, len
    xor ax, ax

sum_loop:
    xor ah, ah
    mov al, [bx]
    add word [total], ax
    inc bx
    loop sum_loop

    mov ax, 4C00h
    int 21h
```

### C++ idea

```cpp
int total = 0;
for (int i = 0; i < 4; i++) {
    total += arr[i];
}
```

---

# 13) Common Beginner Programs

## Print `ABC`

```nasm
org 100h

mov ah, 02h
mov dl, 'A'
int 21h

mov dl, 'B'
int 21h

mov dl, 'C'
int 21h

mov ax, 4C00h
int 21h
```

### C++ idea

```cpp
std::cout << "ABC";
```

---

## Print `35`

```nasm
org 100h

mov ax, 35
xor dx, dx
mov bx, 10
div bx

push dx

add al, 30h
mov ah, 02h
mov dl, al
int 21h

pop dx
mov al, dl
add al, 30h
mov ah, 02h
mov dl, al
int 21h

mov ax, 4C00h
int 21h
```

### C++ idea

```cpp
std::cout << 35;
```

---

## Check odd or even

```nasm
org 100h
jmp start

num db 13
odd_msg  db 'Odd', 13, 10, '$'
even_msg db 'Even', 13, 10, '$'

start:
    mov al, [num]
    and al, 1
    jz even

    mov ah, 09h
    lea dx, [odd_msg]
    int 21h
    jmp done

even:
    mov ah, 09h
    lea dx, [even_msg]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

### C++ idea

```cpp
if (num % 2 == 0) {
    // even
} else {
    // odd
}
```

---

# 14) Common mistakes to avoid

| Mistake | Why it is wrong | Better way |
|---|---|---|
| Using `loop` with the wrong register | `loop` uses `CX` only | load count into `CX` |
| Forgetting `$` for DOS string print | `AH=09h` stops at `$` | end strings with `$` |
| Using `DIV` without clearing the high part | wrong dividend value | clear `DX` for 16-bit division |
| Confusing `AL` with `'0'` | number 0 is not ASCII `'0'` | add/subtract `30h` |
| Forgetting `push`/`pop` pairs | stack becomes unbalanced | restore in reverse order |
| Mixing signed and unsigned jumps | wrong comparisons | use the right jump type |

---

# 15) One-page quick reference

## Core patterns

```nasm
; set to zero
xor ax, ax

; print char in DL
mov ah, 02h
int 21h

; print string at DX
mov ah, 09h
int 21h

; read key into AL
mov ah, 01h
int 21h

; compare
cmp al, 5
je  label

; loop
mov cx, 10
again:
    ; body
    loop again

; exit
mov ax, 4C00h
int 21h
```

---

# 16) Final mental model

Assembly feels hard because it exposes the machine directly.

A good beginner way to think about it is:

- **registers** are tiny variables inside the CPU
- **memory** is where data lives
- **instructions** are tiny commands
- **flags** are the CPU’s status lights
- **C++** is the high-level version of the same ideas

If a C++ line is:

```cpp
x = x + 1;
```

the assembly idea is:

```nasm
inc x
```

If a C++ line is:

```cpp
if (a == b) { ... }
```

the assembly idea is:

```nasm
cmp a, b
je  equal
```

If a C++ loop is:

```cpp
for (int i = 0; i < 5; i++) { ... }
```

the assembly idea is:

```nasm
mov cx, 5
again:
    ; body
    loop again
```

---

# 17) Clean corrected notes from the original file

These are the main corrections that matter for a beginner:

1. `loop` uses **CX**, not just any register.
2. For `DIV`, the dividend must be prepared correctly:
   - 8-bit division uses `AX`
   - 16-bit division uses `DX:AX`
3. `AH=09h` prints until it sees `$`.
4. `cmp` changes flags only; it does **not** store the subtraction result.
5. `xor reg, reg` is a common way to clear a register.
6. `push` and `pop` should match in reverse order.
7. Signed and unsigned jumps are not interchangeable.
8. Printing numbers needs ASCII conversion.

---

If you want to keep learning from this sheet, the best next step is to rewrite each example once without looking, then compare your version to the reference.
