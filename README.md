## 📖 Cheatsheet and YouTube Video Course in Urdu/Hindi
> **[Open the Assembly Cheatsheet →](https://assembly-ist.netlify.app/)**

> **[Assembly Language Full Course](https://www.youtube.com/watch?v=GCefp01AzCU&list=PLQR3mV3wWgCo47E18bpdsISc5mmpxnlwv "My Channel")**
---

# 🖥️ Complete Assembly Language Course
### Using DOSBox + NASM + AFD (Debugger)
**Instructor Guide & Student Reference**

---

> **How to use this course:** Read every section carefully. Type every program yourself — do NOT copy-paste. Run every program. Break things on purpose. That is how you learn assembly.

---

# 📦 PART 1: DOSBox Basics

## 1.1 What is DOSBox?

DOSBox is a program that **emulates an old DOS (Disk Operating System) environment** on your modern computer. We use it because NASM assembly programs for 16-bit x86 architecture run in a DOS environment.

Think of DOSBox as a time machine that takes your computer back to the 1980s.

---

## 1.2 Starting DOSBox

1. Open DOSBox from your desktop or Start Menu.
2. You will see a black screen with a prompt like:

```
Z:\>
```

This means DOSBox is running and waiting for your command.

---

## 1.3 Mounting a Drive

DOSBox does NOT automatically see your Windows/Linux folders. You must **mount** a folder as a drive.

```
mount c c:\assembly
```

This mounts the folder `C:\assembly` on your real computer as drive `C:` inside DOSBox.

Then switch to it:

```
c:
```

Now your prompt will show `C:\>`

> **Best Practice:** Create a folder called `assembly` on your Desktop or C drive, and always mount that.

---

## 1.4 Essential DOSBox / DOS Commands

| Command | What It Does | Example |
|---|---|---|
| `mount c c:\folder` | Mounts a real folder as drive C | `mount c c:\assembly` |
| `c:` | Switch to drive C | `c:` |
| `dir` | List files and folders | `dir` |
| `dir /w` | List files in wide format | `dir /w` |
| `cd foldername` | Change into a folder | `cd programs` |
| `cd ..` | Go back one folder level | `cd ..` |
| `md foldername` | Make a new directory | `md lesson1` |
| `rd foldername` | Remove a directory | `rd lesson1` |
| `del filename` | Delete a file | `del hello.asm` |
| `copy a b` | Copy file a to b | `copy hello.asm backup.asm` |
| `ren oldname newname` | Rename a file | `ren old.asm new.asm` |
| `type filename` | Print a file's contents | `type hello.asm` |
| `cls` | Clear the screen | `cls` |
| `exit` | Close DOSBox | `exit` |

---

## 1.5 Assembling and Running a Program

This is the **workflow** you will repeat for every single program in this course:

**Step 1 — Write your code** using a text editor (Notepad, VS Code, etc.) and save it with a `.asm` extension inside your mounted folder.

**Step 2 — Assemble it** (convert assembly code to a `.com` file):

```
nasm hello.asm -o hello.com
```

**Step 3 — Run it:**

```
hello
```

**Step 4 — Debug it (optional but useful):**

```
afd hello.com
```

---

## 1.6 Using AFD (Assembly Full-screen Debugger)

AFD is your debugger. It lets you watch your program execute **one instruction at a time**.

| Key | Action |
|---|---|
| `F8` | Step Over — execute one instruction |
| `F7` | Step Into — go inside a function call |
| `F2` | Set/remove breakpoint at cursor |
| `F9` | Run until breakpoint |
| `F10` | Run to end |
| `Tab` | Switch between panels |
| `Alt+X` or `Esc` | Exit AFD |

**The AFD Screen has 4 panels:**
- **Code panel** — shows your instructions
- **Register panel** — shows AX, BX, CX, DX, etc.
- **Flags panel** — shows Zero Flag, Carry Flag, etc.
- **Data/Stack panel** — shows memory contents

> Always use AFD when your program behaves unexpectedly. Watch the registers change as each line executes.

---

# 🔧 PART 2: Computer Architecture Basics (The Foundation)

Before writing a single line of code, you must understand **what you are programming**.

---

## 2.1 The CPU and Memory

Your CPU (Central Processing Unit) executes instructions. It has tiny storage locations called **registers**. RAM (memory) stores data and your program's code.

Assembly language lets you **directly control the CPU** — there is no middle man like C++ or Python. This is why it is fast, and why it is unforgiving.

---

## 2.2 The x86 16-bit Registers

Registers are like tiny variables that live inside the CPU. They are extremely fast.

### General Purpose Registers

| Register | Full Name | Common Use |
|---|---|---|
| `AX` | Accumulator | Math operations, function return values |
| `BX` | Base | Memory addressing, base pointer |
| `CX` | Counter | Loop counters |
| `DX` | Data | I/O operations, multiply/divide overflow |

Each 16-bit register is split into two 8-bit halves:
- `AX` = `AH` (high byte) + `AL` (low byte)
- `BX` = `BH` + `BL`
- `CX` = `CH` + `CL`
- `DX` = `DH` + `DL`

### Segment Registers

| Register | Full Name | Use |
|---|---|---|
| `CS` | Code Segment | Points to current code |
| `DS` | Data Segment | Points to data section |
| `SS` | Stack Segment | Points to the stack |
| `ES` | Extra Segment | Extra data operations |

### Pointer and Index Registers

| Register | Use |
|---|---|
| `SP` | Stack Pointer — top of stack |
| `BP` | Base Pointer — stack frame base |
| `SI` | Source Index — string operations |
| `DI` | Destination Index — string operations |
| `IP` | Instruction Pointer — next instruction address |

### Flags Register

Contains single bits (flags) that the CPU sets automatically:

| Flag | Meaning |
|---|---|
| `ZF` (Zero Flag) | Set when result is zero |
| `CF` (Carry Flag) | Set when arithmetic carries/borrows |
| `SF` (Sign Flag) | Set when result is negative |
| `OF` (Overflow Flag) | Set when signed overflow occurs |
| `PF` (Parity Flag) | Set when result has even number of 1-bits |

---

## 2.3 Number Systems

Assembly programmers must know three number systems:

| System | Base | Digits Used | Example | NASM Syntax |
|---|---|---|---|---|
| Decimal | 10 | 0-9 | 65 | `65` |
| Hexadecimal | 16 | 0-9, A-F | 41h | `0x41` or `41h` |
| Binary | 2 | 0, 1 | 01000001b | `01000001b` |

**ASCII Table (commonly used):**

| Character | Decimal | Hex |
|---|---|---|
| `0`-`9` | 48-57 | 30h-39h |
| `A`-`Z` | 65-90 | 41h-5Ah |
| `a`-`z` | 97-122 | 61h-7Ah |
| Space | 32 | 20h |
| Enter (CR) | 13 | 0Dh |
| Newline (LF) | 10 | 0Ah |
| `$` (string terminator) | 36 | 24h |

---

# 📝 PART 3: NASM Assembly Language Syntax

## 3.1 Structure of a .COM Program

For this course, we write `.COM` programs — the simplest executable format in DOS.

```nasm
; Every .COM program starts at memory offset 100h
org 100h

; Code goes here

; Program must end with:
mov ax, 4C00h       ; Function 4Ch = Exit program, AL=00 = exit code 0
int 21h             ; Call DOS interrupt
```

---

## 3.2 Sections of a Program

```nasm
org 100h            ; Origin: tells NASM where code starts in memory

; Data section: define your variables and strings here
; (in .COM files, data and code can be in the same segment)

; Code starts here
```

---

## 3.3 Comments

```nasm
; This is a comment — the semicolon starts a comment
; Comments are ignored by NASM
; Use comments to explain EVERY line for this course
mov ax, 5   ; This is an inline comment
```

---

## 3.4 Defining Data

```nasm
myByte   db 10          ; Define Byte — 1 byte (0 to 255)
myWord   dw 1000        ; Define Word — 2 bytes (0 to 65535)
myMsg    db 'Hello$'    ; String ending with $ (for DOS printing)
myArray  db 1,2,3,4,5   ; Array of 5 bytes
```

---

# ⚙️ PART 4: Assembly Instructions — The Complete Reference

## 4.1 Data Movement Instructions

```nasm
mov ax, 5        ; AX = 5 (load constant into register)
mov bx, ax       ; BX = AX (copy register to register)
mov al, [bx]     ; AL = memory at address BX (load from memory)
mov [bx], al     ; Memory at address BX = AL (store to memory)

xchg ax, bx      ; Swap AX and BX
push ax          ; Push AX onto the stack
pop bx           ; Pop top of stack into BX
lea bx, [myVar]  ; Load Effective Address of myVar into BX
```

---

## 4.2 Arithmetic Instructions

```nasm
add ax, 5        ; AX = AX + 5
add ax, bx       ; AX = AX + BX
sub ax, 3        ; AX = AX - 3
sub ax, bx       ; AX = AX - BX
inc ax           ; AX = AX + 1 (increment)
dec ax           ; AX = AX - 1 (decrement)

mul bx           ; AX = AX * BX (unsigned, result in DX:AX)
imul bx          ; AX = AX * BX (signed)
div bx           ; AX = DX:AX / BX, remainder in DX (unsigned)
idiv bx          ; Signed division

neg ax           ; AX = -AX (negate)
```

---

## 4.3 Logical Instructions

```nasm
and ax, bx       ; AX = AX AND BX (bitwise)
or  ax, bx       ; AX = AX OR BX
xor ax, bx       ; AX = AX XOR BX
not ax           ; AX = NOT AX (flip all bits)

; XOR trick: xor ax, ax is the fastest way to set AX to 0
xor ax, ax       ; AX = 0
```

---

## 4.4 Shift and Rotate Instructions

```nasm
shl ax, 1        ; Shift Left by 1 (multiply by 2)
shr ax, 1        ; Shift Right by 1 (divide by 2, unsigned)
sar ax, 1        ; Arithmetic Shift Right (divide by 2, signed)
rol ax, 1        ; Rotate Left
ror ax, 1        ; Rotate Right
```

---

## 4.5 Comparison and Jump Instructions

```nasm
cmp ax, bx       ; Compare AX and BX (sets flags, does NOT change values)
cmp ax, 0        ; Compare AX with 0
test ax, ax      ; AND ax,ax — sets ZF if AX is zero (non-destructive)
```

**Unconditional Jump:**
```nasm
jmp label        ; Always jump to label
```

**Conditional Jumps (after CMP):**

| Instruction | Meaning | Condition |
|---|---|---|
| `je` / `jz` | Jump if Equal / Zero | ZF=1 |
| `jne` / `jnz` | Jump if Not Equal / Not Zero | ZF=0 |
| `jg` / `jnle` | Jump if Greater (signed) | ZF=0 and SF=OF |
| `jl` / `jnge` | Jump if Less (signed) | SF not equal OF |
| `jge` / `jnl` | Jump if Greater or Equal (signed) | SF=OF |
| `jle` / `jng` | Jump if Less or Equal (signed) | ZF=1 or SF not equal OF |
| `ja` | Jump if Above (unsigned) | CF=0 and ZF=0 |
| `jb` / `jc` | Jump if Below / Carry (unsigned) | CF=1 |
| `jae` | Jump if Above or Equal (unsigned) | CF=0 |
| `jbe` | Jump if Below or Equal (unsigned) | CF=1 or ZF=1 |
| `js` | Jump if Sign (negative) | SF=1 |
| `jns` | Jump if Not Sign (positive) | SF=0 |

---

## 4.6 Procedure (Function) Instructions

```nasm
call myFunc      ; Push return address, jump to myFunc
ret              ; Pop return address, jump back (return from function)
```

---

## 4.7 DOS Interrupt INT 21h — Your I/O Toolkit

INT 21h is the DOS system call. You set `AH` to the function number, and call `int 21h`.

| AH Value | Function | Input | Output |
|---|---|---|---|
| `01h` | Read character from keyboard (with echo) | — | AL = character |
| `02h` | Print one character | DL = character | — |
| `09h` | Print a string (ends with `$`) | DX = address of string | — |
| `0Ah` | Read a string from keyboard | DX = buffer address | Buffer filled |
| `4Ch` | Exit program | AL = exit code | — |

---

# 💻 PART 5: Programming — Simple Programs

## 5.1 Printing a Single Character

The simplest output operation — print one character using INT 21h function 02h.

---

### Program 5.1.1 — Print the Letter 'A'

```nasm
; ============================================================
; Program: print_a.asm
; Purpose: Print the letter 'A' to the screen
; Assemble: nasm print_a.asm -o print_a.com
; Run     : print_a
; ============================================================

org 100h            ; .COM programs always start at offset 100h

mov ah, 02h         ; AH = 02h means "print a character" (DOS function 2)
mov dl, 'A'         ; DL = 'A' = ASCII 65 = 41h (the character to print)
int 21h             ; Call DOS — it will print whatever character is in DL

mov ax, 4C00h       ; AH = 4Ch (exit function), AL = 00h (exit code 0)
int 21h             ; Call DOS to terminate the program cleanly
```

**Assemble and run:**
```
nasm print_a.asm -o print_a.com
print_a
```
**Expected output:** `A`

---

### Program 5.1.2 — Print Three Letters in a Row

```nasm
; ============================================================
; Program: print_abc.asm
; Purpose: Print the letters A, B, and C on the screen
; Concept : Calling INT 21h multiple times
; ============================================================

org 100h

; --- Print 'A' ---
mov ah, 02h         ; Set AH = 02h: DOS "print character" function
mov dl, 'A'         ; Load character 'A' into DL (65 in decimal)
int 21h             ; Trigger DOS interrupt — prints DL

; --- Print 'B' ---
mov ah, 02h         ; AH must be set again before each call (good habit)
mov dl, 'B'         ; Load character 'B' into DL (66 in decimal)
int 21h             ; Prints 'B'

; --- Print 'C' ---
mov ah, 02h
mov dl, 'C'         ; Load character 'C' into DL (67 in decimal)
int 21h             ; Prints 'C'

; --- Exit the program ---
mov ax, 4C00h       ; AH=4Ch exit function, AL=0 exit code
int 21h
```

**Expected output:** `ABC`

---

### Program 5.1.3 — Print a Digit and a Symbol With Newline

```nasm
; ============================================================
; Program: print_digit_sym.asm
; Purpose: Print digit '7', '!', then move to a new line
; Concept : Characters are just numbers (ASCII values)
; ============================================================

org 100h

; --- Print digit '7' ---
; '7' in ASCII is 55 decimal = 37h hex
mov ah, 02h
mov dl, '7'         ; Single quotes work in NASM for ASCII values
int 21h

; --- Print exclamation mark '!' ---
; '!' in ASCII is 33 decimal = 21h hex
mov ah, 02h
mov dl, '!'
int 21h

; --- Print newline (CR + LF) ---
; A newline = Carriage Return (13) followed by Line Feed (10)
mov ah, 02h
mov dl, 13          ; Carriage Return: moves cursor to column 0
int 21h
mov ah, 02h
mov dl, 10          ; Line Feed: moves cursor down one row
int 21h

; --- Exit ---
mov ax, 4C00h
int 21h
```

**Expected output:**
```
7!
```

---

## 5.2 Printing Strings

Printing one character at a time is tedious. INT 21h function 09h prints a whole string. The string **must end with a `$`** character — that is the DOS string terminator.

---

### Program 5.2.1 — Hello, World!

```nasm
; ============================================================
; Program: hello.asm
; Purpose: Print "Hello, World!" — the traditional first program
; Concept : INT 21h function 09h to print a null-terminated string
; ============================================================

org 100h

; Jump past the data so it isn't executed as code
jmp start

; --- Data Section ---
; The string must end with '$' — that tells DOS where to stop printing
message db 'Hello, World!', 13, 10, '$'
;                           ^    ^    ^
;                           CR   LF   terminator

; --- Code Section ---
start:
    mov ah, 09h         ; AH = 09h: DOS "print string" function
    lea dx, [message]   ; DX = address (offset) of the string in memory
    int 21h             ; DOS prints from [DX] until it hits the '$'

    mov ax, 4C00h       ; Exit cleanly
    int 21h
```

**Expected output:**
```
Hello, World!
```

---

### Program 5.2.2 — Print Multiple Lines of Text

```nasm
; ============================================================
; Program: multiline.asm
; Purpose: Print three separate lines of text
; Concept : Multiple strings, each with their own CR+LF+'$'
; ============================================================

org 100h

jmp start

; Each string has CR (13), LF (10), and '$' at the end
line1 db 'Assembly Language is powerful!', 13, 10, '$'
line2 db 'We are programming the CPU directly.', 13, 10, '$'
line3 db 'Welcome to the course!', 13, 10, '$'

start:
    ; --- Print line 1 ---
    mov ah, 09h
    lea dx, [line1]
    int 21h

    ; --- Print line 2 ---
    mov ah, 09h
    lea dx, [line2]
    int 21h

    ; --- Print line 3 ---
    mov ah, 09h
    lea dx, [line3]
    int 21h

    ; --- Exit ---
    mov ax, 4C00h
    int 21h
```

**Expected output:**
```
Assembly Language is powerful!
We are programming the CPU directly.
Welcome to the course!
```

---

### Program 5.2.3 — Print a Text Box Using ASCII Characters

```nasm
; ============================================================
; Program: textbox.asm
; Purpose: Draw a simple box using + | - characters
; Concept : Multiple string prints to form a visual pattern
; ============================================================

org 100h

jmp start

; Define the three row types of our box
top_bot db '+----------+', 13, 10, '$'   ; Top and bottom border
side    db '|          |', 13, 10, '$'   ; Side rows (empty inside)
content db '|  Hello!  |', 13, 10, '$'  ; A content row

start:
    ; Print top border
    mov ah, 09h
    lea dx, [top_bot]
    int 21h

    ; Print empty side row
    mov ah, 09h
    lea dx, [side]
    int 21h

    ; Print content row
    mov ah, 09h
    lea dx, [content]
    int 21h

    ; Print another empty side row
    mov ah, 09h
    lea dx, [side]
    int 21h

    ; Print bottom border
    mov ah, 09h
    lea dx, [top_bot]
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:**
```
+----------+
|          |
|  Hello!  |
|          |
+----------+
```

---

# ⚡ PART 6: Working with Registers and Arithmetic

## 6.1 Basic Arithmetic Operations

---

### Program 6.1.1 — Addition and Storing the Result

```nasm
; ============================================================
; Program: addition.asm
; Purpose: Add two numbers and store the result in a variable
; Concept : MOV, ADD, reading/writing memory variables
; Tip     : Open with AFD and press F8 to watch AL change
; ============================================================

org 100h

jmp start

; --- Variables ---
numA   db 20        ; First number (1 byte, value 20)
numB   db 15        ; Second number (1 byte, value 15)
result db 0         ; Will hold the result (20+15 = 35)

start:
    ; Load first number into AL register
    mov al, [numA]      ; AL = 20  (brackets mean: read from memory)

    ; Add second number to AL
    add al, [numB]      ; AL = 20 + 15 = 35

    ; Store the result back into the 'result' variable in memory
    mov [result], al    ; memory[result] = 35

    ; At this point, [result] = 35. Check in AFD!

    ; Exit
    mov ax, 4C00h
    int 21h
```

> **AFD Tip:** Step through with F8. After `mov al, [numA]`, the register panel shows AL = 14h (20 in hex). After `add al, [numB]`, AL = 23h (35 in hex).

---

### Program 6.1.2 — Subtraction, INC, and DEC

```nasm
; ============================================================
; Program: arithmetic2.asm
; Purpose: Show SUB, INC (increment), and DEC (decrement)
; Concept : Arithmetic instructions on 16-bit word variables
; ============================================================

org 100h

jmp start

valA dw 100         ; 16-bit word variable, value = 100
valB dw 40          ; 16-bit word variable, value = 40

start:
    ; Load valA into the 16-bit AX register
    mov ax, [valA]      ; AX = 100

    ; Subtract valB from AX
    sub ax, [valB]      ; AX = 100 - 40 = 60

    ; Increment: add 1 to AX (faster than ADD AX, 1)
    inc ax              ; AX = 60 + 1 = 61

    ; Decrement: subtract 1 from AX
    dec ax              ; AX = 61 - 1 = 60

    ; Store the final result back to memory
    mov [valA], ax      ; valA = 60

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

### Program 6.1.3 — Multiplication and Division

```nasm
; ============================================================
; Program: muldiv.asm
; Purpose: Demonstrate MUL (multiply) and DIV (divide)
; Concept : MUL always uses AX; DIV divides AX by operand
;           8-bit MUL: AX = AL * operand
;           8-bit DIV: AL = AX / operand, AH = remainder
; ============================================================

org 100h

jmp start

factor db 6         ; Multiplication factor
divsor db 4         ; Division divisor

start:
    ; =====================
    ; MULTIPLICATION: 10 * 6
    ; =====================
    mov al, 10          ; AL = 10  (MUL uses AL as first operand)
    mov bl, [factor]    ; BL = 6
    mul bl              ; AX = AL * BL = 10 * 6 = 60
                        ; After 8-bit MUL, result is ALWAYS in AX

    ; AX = 60 (003Ch in hex) — verify this in AFD

    ; =====================
    ; DIVISION: 60 / 4
    ; =====================
    ; AX = 60 (already set from above)
    mov bl, [divsor]    ; BL = 4
    div bl              ; AL = AX / BL = 60 / 4 = 15 (quotient)
                        ; AH = 60 mod 4 = 0 (remainder)

    ; AL = 15 (0Fh), AH = 0 — check in AFD!

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

## 6.2 Printing Numbers (Converting to ASCII)

A register holds a raw binary number. To **print** it, you must convert it to its ASCII representation. For digits 0-9: add `30h` to the digit.

Why? Because ASCII '0' = 48 = 30h. So digit 7 + 30h = 55 = ASCII '7'.

---

### Program 6.2.1 — Compute and Print a Single Digit Result

```nasm
; ============================================================
; Program: print_result.asm
; Purpose: Compute 3+4=7 and print the result on screen
; Concept : Digit to ASCII conversion: digit + 30h = ASCII char
; ============================================================

org 100h

start:
    ; --- Compute 3 + 4 ---
    mov al, 3           ; AL = 3
    add al, 4           ; AL = 3 + 4 = 7

    ; --- Convert number to ASCII character ---
    add al, 30h         ; AL = 7 + 30h = 37h = ASCII '7'
                        ; Now AL holds the printable character '7'

    ; --- Print the character ---
    mov ah, 02h         ; DOS function: print character
    mov dl, al          ; DL = '7' (character to print)
    int 21h             ; Output appears on screen

    ; --- Exit ---
    mov ax, 4C00h
    int 21h
```

**Expected output:** `7`

---

### Program 6.2.2 — Compute and Print Two Different Results

```nasm
; ============================================================
; Program: two_results.asm
; Purpose: Compute 4*2 and 9-3, print both results
; Concept : Two separate calculations, each digit printed
; ============================================================

org 100h

start:
    ; ========================
    ; First calculation: 4 * 2 = 8
    ; ========================
    mov al, 4           ; AL = 4
    mov bl, 2           ; BL = 2
    mul bl              ; AX = 4 * 2 = 8 (result in AX for 8-bit mul)

    ; Convert AL (= 8) to ASCII and print
    add al, 30h         ; AL = 38h = ASCII '8'
    mov ah, 02h
    mov dl, al
    int 21h             ; Prints '8'

    ; Print a space between results
    mov ah, 02h
    mov dl, ' '         ; Space character (ASCII 32)
    int 21h

    ; ========================
    ; Second calculation: 9 - 3 = 6
    ; ========================
    mov al, 9           ; AL = 9
    sub al, 3           ; AL = 9 - 3 = 6

    ; Convert and print
    add al, 30h         ; AL = 36h = ASCII '6'
    mov ah, 02h
    mov dl, al
    int 21h             ; Prints '6'

    ; --- Exit ---
    mov ax, 4C00h
    int 21h
```

**Expected output:** `8 6`

---

### Program 6.2.3 — Print a Two-Digit Number

```nasm
; ============================================================
; Program: twodigit.asm
; Purpose: Print the number 35 by separating tens and units
; Concept : Use DIV to break a number into individual digits
; ============================================================

org 100h

start:
    ; We want to print 35
    mov ax, 35          ; AX = 35 (the number we want to display)

    ; --- Separate the tens and units digits ---
    ; Dividing 35 by 10:
    ;   Quotient  = 3 (tens digit)  -> goes to AL
    ;   Remainder = 5 (units digit) -> goes to AH
    mov bl, 10          ; BL = 10 (the divisor)
    div bl              ; AL = 3 (tens), AH = 5 (units)

    ; Save the units digit before it gets overwritten by INT 21h
    mov cl, ah          ; CL = 5 (save remainder safely in CL)

    ; --- Print tens digit ---
    add al, 30h         ; Convert 3 to ASCII '3' (33h + 30h = 33h... = 33h, which is '3')
    mov ah, 02h         ; DOS print function
    mov dl, al          ; DL = '3'
    int 21h             ; Prints '3'

    ; --- Print units digit ---
    add cl, 30h         ; Convert 5 to ASCII '5'
    mov ah, 02h
    mov dl, cl          ; DL = '5'
    int 21h             ; Prints '5'

    ; --- Exit ---
    mov ax, 4C00h
    int 21h
```

**Expected output:** `35`

---

# 🔀 PART 7: Conditions (If-Else Logic)

Conditions in assembly use **CMP** to compare values (it sets flags) followed by a **conditional jump** to redirect execution.

**The basic if-else pattern:**
```nasm
    cmp ax, bx          ; Compare AX and BX (subtracts internally, keeps flags)
    je  equal_label     ; Jump IF equal (ZF=1)
    ; --- else branch ---
    jmp done
equal_label:
    ; --- if branch ---
done:
```

---

## 7.1 Simple If Condition

---

### Program 7.1.1 — Check if a Number is Zero

```nasm
; ============================================================
; Program: is_zero.asm
; Purpose: Check if a number equals zero and print a message
; Concept : CMP followed by JE (Jump if Equal)
; Test    : Change 'number db 0' to other values and reassemble
; ============================================================

org 100h

jmp start

; --- Data ---
number  db 0                            ; Try: 0, 5, 255

msg_yes db 'The number is zero!', 13, 10, '$'
msg_no  db 'The number is NOT zero!', 13, 10, '$'

start:
    mov al, [number]        ; AL = value of number variable

    cmp al, 0               ; Compare AL with 0
                            ; Internally: AL - 0, result not stored, only flags set
    je  is_zero             ; If AL == 0, Zero Flag (ZF) = 1, so jump

    ; --- This runs if number is NOT zero ---
    mov ah, 09h
    lea dx, [msg_no]
    int 21h
    jmp done                ; Must jump over the is_zero block

is_zero:
    ; --- This runs if number IS zero ---
    mov ah, 09h
    lea dx, [msg_yes]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

---

### Program 7.1.2 — Check if a Number is Positive, Negative, or Zero

```nasm
; ============================================================
; Program: sign_check.asm
; Purpose: Determine sign of a number: positive, negative, zero
; Concept : JZ (zero), JS (sign flag set = negative result)
; ============================================================

org 100h

jmp start

number   db 5       ; Try: 0, 5, 200 (200 as signed byte = -56)

msg_pos  db 'Positive', 13, 10, '$'
msg_neg  db 'Negative', 13, 10, '$'
msg_zero db 'Zero',     13, 10, '$'

start:
    mov al, [number]    ; Load the number into AL

    ; Check for zero first (most specific case)
    cmp al, 0
    jz  zero_case       ; JZ = Jump if Zero Flag set (same as JE)

    ; Check for negative (Sign Flag is set when MSB of result is 1)
    js  neg_case        ; JS = Jump if Sign Flag set (negative)

    ; If we reach here, number is positive
    mov ah, 09h
    lea dx, [msg_pos]
    int 21h
    jmp finish

neg_case:
    mov ah, 09h
    lea dx, [msg_neg]
    int 21h
    jmp finish

zero_case:
    mov ah, 09h
    lea dx, [msg_zero]
    int 21h

finish:
    mov ax, 4C00h
    int 21h
```

---

### Program 7.1.3 — Find the Larger of Two Numbers

```nasm
; ============================================================
; Program: larger.asm
; Purpose: Compare two numbers, print which is larger
; Concept : CMP + JG (signed greater), JE (equal)
; ============================================================

org 100h

jmp start

numA   db 25                            ; Change these to test
numB   db 18

msg_a  db 'A is larger', 13, 10, '$'
msg_b  db 'B is larger', 13, 10, '$'
msg_eq db 'Both are equal', 13, 10, '$'

start:
    mov al, [numA]      ; AL = A
    mov bl, [numB]      ; BL = B

    ; Compare A and B
    cmp al, bl          ; Compute AL - BL and set flags

    je  equal_branch    ; Jump if A == B (ZF = 1)
    jg  a_bigger        ; Jump if A > B (signed: ZF=0, SF=OF)

    ; If neither je nor jg triggered, B must be larger
    mov ah, 09h
    lea dx, [msg_b]
    int 21h
    jmp done

a_bigger:
    mov ah, 09h
    lea dx, [msg_a]
    int 21h
    jmp done

equal_branch:
    mov ah, 09h
    lea dx, [msg_eq]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

---

## 7.2 If-Else with Keyboard Input

---

### Program 7.2.1 — Read a Key and Respond Differently

```nasm
; ============================================================
; Program: key_check.asm
; Purpose: Read one key, print a tailored message per key
; Concept : INT 21h 01h reads char into AL; compare with ASCII
; ============================================================

org 100h

jmp start

prompt    db 'Press Y or N: $'
msg_yes   db 13, 10, 'You said YES!', 13, 10, '$'
msg_no    db 13, 10, 'You said NO!', 13, 10, '$'
msg_other db 13, 10, 'That is not Y or N!', 13, 10, '$'

start:
    ; Print prompt to ask user
    mov ah, 09h
    lea dx, [prompt]
    int 21h

    ; Read one character from keyboard (it echoes to screen automatically)
    mov ah, 01h         ; DOS function 01h = read character
    int 21h             ; AL = the ASCII code of the pressed key

    ; Check if user pressed 'Y' (uppercase)
    cmp al, 'Y'
    je  pressed_yes

    ; Check if user pressed 'y' (lowercase)
    cmp al, 'y'
    je  pressed_yes

    ; Check if user pressed 'N' (uppercase)
    cmp al, 'N'
    je  pressed_no

    ; Check if user pressed 'n' (lowercase)
    cmp al, 'n'
    je  pressed_no

    ; None of the valid keys
    mov ah, 09h
    lea dx, [msg_other]
    int 21h
    jmp done

pressed_yes:
    mov ah, 09h
    lea dx, [msg_yes]
    int 21h
    jmp done

pressed_no:
    mov ah, 09h
    lea dx, [msg_no]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

---

### Program 7.2.2 — Grade Checker

```nasm
; ============================================================
; Program: grade.asm
; Purpose: Classify marks into a letter grade
; Concept : Multiple CMP+JGE checks — order matters!
;           Must check from highest to lowest
; ============================================================

org 100h

jmp start

marks db 75         ; Try: 95, 80, 65, 52, 30

grade_a db 'Grade A - Excellent (90+)', 13, 10, '$'
grade_b db 'Grade B - Good (75-89)', 13, 10, '$'
grade_c db 'Grade C - Average (60-74)', 13, 10, '$'
grade_d db 'Grade D - Below Average (50-59)', 13, 10, '$'
grade_f db 'Grade F - Fail (below 50)', 13, 10, '$'

start:
    mov al, [marks]     ; AL = marks value

    ; Check from highest grade downward
    cmp al, 90
    jge get_A          ; >= 90 -> Grade A

    cmp al, 75
    jge get_B          ; >= 75 -> Grade B

    cmp al, 60
    jge get_C          ; >= 60 -> Grade C

    cmp al, 50
    jge get_D          ; >= 50 -> Grade D

    ; If none of the above — it's F
    mov ah, 09h
    lea dx, [grade_f]
    int 21h
    jmp done

get_A:
    mov ah, 09h
    lea dx, [grade_a]
    int 21h
    jmp done

get_B:
    mov ah, 09h
    lea dx, [grade_b]
    int 21h
    jmp done

get_C:
    mov ah, 09h
    lea dx, [grade_c]
    int 21h
    jmp done

get_D:
    mov ah, 09h
    lea dx, [grade_d]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

---

### Program 7.2.3 — Odd or Even Checker

```nasm
; ============================================================
; Program: odd_even.asm
; Purpose: Determine if a number is odd or even
; Concept : Bitwise AND with 1 isolates the least significant bit
;           Even numbers have LSB = 0; Odd numbers have LSB = 1
; ============================================================

org 100h

jmp start

number   db 13          ; Try: 2, 7, 100, 33, 128

msg_even db 'Number is Even', 13, 10, '$'
msg_odd  db 'Number is Odd',  13, 10, '$'

start:
    mov al, [number]    ; Load the number into AL

    ; AND AL with 00000001b — only the last bit survives
    and al, 01h         ; If last bit = 0 -> even, if 1 -> odd
                        ; AND also sets Zero Flag if result is 0

    jz  even_case       ; Jump if Zero Flag set (result was 0 -> even)

    ; Odd case (result was 1, ZF not set)
    mov ah, 09h
    lea dx, [msg_odd]
    int 21h
    jmp done

even_case:
    mov ah, 09h
    lea dx, [msg_even]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

---

# 🔁 PART 8: Loops

Loops in assembly use a counter and a jump back to the beginning. The `LOOP` instruction is the built-in shortcut.

**How LOOP works:**
1. Decrements CX by 1
2. If CX is not zero, jumps to the label
3. If CX becomes zero, falls through to the next instruction

```nasm
    mov cx, 5       ; Set loop count
myloop:
    ; ... body ...
    loop myloop     ; CX--; if CX != 0, jump to myloop
```

---

## 8.1 Counting Loops with LOOP

---

### Program 8.1.1 — Print Stars Using LOOP

```nasm
; ============================================================
; Program: stars.asm
; Purpose: Print exactly 5 star (*) characters using a loop
; Concept : LOOP instruction with CX as the counter
; ============================================================

org 100h

start:
    mov cx, 5           ; CX = 5 — we want 5 iterations

print_loop:
    ; --- Loop body ---
    mov ah, 02h         ; DOS function: print character
    mov dl, '*'         ; The character to print
    int 21h             ; Print it

    loop print_loop     ; CX = CX - 1; if CX != 0 jump back

    ; Print newline after the stars
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `*****`

---

### Program 8.1.2 — Print Numbers 1 to 9

```nasm
; ============================================================
; Program: count9.asm
; Purpose: Print the digits 1 through 9 with spaces
; Concept : Loop body increments a counter, converts to ASCII
; ============================================================

org 100h

start:
    mov cx, 9           ; Loop 9 times (for digits 1 through 9)
    mov bl, 1           ; BL = current number (starts at 1)

number_loop:
    ; Convert current number in BL to ASCII and print it
    mov al, bl          ; AL = current digit (1 through 9)
    add al, 30h         ; Convert to ASCII: 1->'1', 2->'2', etc.
    mov ah, 02h
    mov dl, al
    int 21h             ; Print the digit

    ; Print a space separator
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Move to the next number
    inc bl              ; BL = BL + 1

    ; Repeat
    loop number_loop    ; CX-- ; if CX != 0, repeat

    ; Newline at the end
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `1 2 3 4 5 6 7 8 9`

---

### Program 8.1.3 — Print the Alphabet A to Z

```nasm
; ============================================================
; Program: alphabet.asm
; Purpose: Print all 26 uppercase letters of the alphabet
; Concept : Loop with register as character, increment each time
; ============================================================

org 100h

start:
    mov cx, 26          ; 26 letters in the alphabet
    mov al, 'A'         ; AL = 65 decimal = ASCII code for 'A'
                        ; We will print AL, then increment it

alpha_loop:
    ; Print the current letter (already in ASCII form)
    mov ah, 02h
    mov dl, al          ; DL = current letter
    int 21h             ; Print it

    ; Advance to the next letter
    inc al              ; AL: A->B->C->...->Z

    loop alpha_loop     ; Repeat 26 times total

    ; Newline
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `ABCDEFGHIJKLMNOPQRSTUVWXYZ`

---

## 8.2 While-Style Loops (CMP + Conditional Jump)

Sometimes you need a loop that doesn't use a fixed count — it runs until a condition becomes false.

---

### Program 8.2.1 — Countdown from 9 to 0

```nasm
; ============================================================
; Program: countdown.asm
; Purpose: Count down from 9 to 0 and print each digit
; Concept : Decrement register, compare with 0, jump back
; ============================================================

org 100h

start:
    mov al, 9           ; AL = 9 (start of countdown)

countdown_loop:
    ; Print current digit
    mov bl, al          ; Save AL in BL (INT 21h may change AH)
    add al, 30h         ; Convert digit to ASCII
    mov ah, 02h
    mov dl, al
    int 21h             ; Print current digit

    ; Print space separator
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Restore and decrement
    mov al, bl          ; Restore original digit value
    dec al              ; Count down by 1

    ; Check: should we keep looping?
    cmp al, 0
    jge countdown_loop  ; If AL >= 0, go back and print it

    ; Newline at end
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `9 8 7 6 5 4 3 2 1 0`

---

### Program 8.2.2 — Sum of Numbers 1 to 10

```nasm
; ============================================================
; Program: sum1to10.asm
; Purpose: Calculate 1+2+3+...+10 = 55
; Concept : Accumulator pattern — add each number to a running total
; ============================================================

org 100h

jmp start

total dw 0          ; Result stored here (needs 16 bits: 55 > 255 max byte)

start:
    xor ax, ax          ; AX = 0 — this is our sum accumulator
                        ; xor reg,reg is the fastest way to zero a register
    mov bl, 1           ; BL = current number (1 to 10)

add_loop:
    ; Add current number to the sum
    xor bh, bh          ; BH = 0, so BX = BL (extend to 16 bits)
    add ax, bx          ; AX = AX + BX (running sum)

    ; Move to next number
    inc bl              ; BL++

    ; Check: have we passed 10?
    cmp bl, 11          ; If BL == 11, we're done (we've added 1 through 10)
    jne add_loop        ; If BL != 11, loop again

    ; AX = 55 (the answer)
    mov [total], ax     ; Store it

    ; Print "55" — two digits
    xor dx, dx
    mov bx, 10
    div bx              ; AX=5 (tens), DX=5 (units)
    push dx

    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '5'

    pop dx
    add dl, 30h
    mov ah, 02h
    int 21h             ; Print '5'

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `55`

---

### Program 8.2.3 — Multiplication Using Repeated Addition

```nasm
; ============================================================
; Program: mult_repeat.asm
; Purpose: Multiply 7 x 8 by adding 7 eight times
; Concept : Multiplication = repeated addition (shows the concept)
; ============================================================

org 100h

jmp start

numA   db 7         ; Number to repeatedly add
numB   db 8         ; How many times to add it
result db 0

start:
    xor al, al          ; AL = 0 (accumulator)
    mov bl, [numA]      ; BL = 7 (value to add each iteration)
    mov cl, [numB]      ; CL = 8 (loop counter)

add_loop:
    add al, bl          ; AL = AL + 7 (repeated 8 times)
    loop add_loop       ; CL-- ; repeat until CL = 0

    ; AL = 7 * 8 = 56
    mov [result], al    ; Store the result

    ; Print result "56"
    xor ah, ah          ; AH = 0, so AX = AL = 56
    mov bl, 10
    div bl              ; AL = 5 (tens), AH = 6 (units)
    push ax

    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '5'

    pop ax
    add ah, 30h
    mov al, ah
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '6'

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `56`

---

## 8.3 Nested Loops

A loop inside another loop. The outer loop runs N times. For each outer iteration, the inner loop runs M times. Total executions = N × M.

> **Critical:** You cannot use CX for both loops simultaneously. Use different registers for inner and outer counters, or save/restore CX on the stack.

---

### Program 8.3.1 — Print a Grid of Stars (3 Rows × 4 Columns)

```nasm
; ============================================================
; Program: star_grid.asm
; Purpose: Print a 3-row by 4-column grid of stars
; Concept : Nested loops; inner counter in CH, outer in CL
; ============================================================

org 100h

start:
    mov cl, 3           ; CL = number of rows (outer counter)

row_loop:
    mov ch, 4           ; CH = number of columns (inner counter)
                        ; Reset to 4 for every new row

col_loop:
    ; Print one star
    mov ah, 02h
    mov dl, '*'
    int 21h

    ; Print a space after each star
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Inner loop: count down columns
    dec ch              ; CH--
    jnz col_loop        ; If CH != 0, print another star in this row

    ; End of row — print newline
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Outer loop: move to next row
    dec cl              ; CL--
    jnz row_loop        ; If CL != 0, do another row

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:**
```
* * * * 
* * * * 
* * * * 
```

---

### Program 8.3.2 — Print a Number Triangle

```nasm
; ============================================================
; Program: num_triangle.asm
; Purpose: Print:  1
;                  1 2
;                  1 2 3
;                  1 2 3 4
;                  1 2 3 4 5
; Concept : Outer = row (1-5), inner = column (1 to row number)
; ============================================================

org 100h

start:
    mov cl, 1           ; CL = row counter (1 to 5)

outer_loop:
    mov ch, 1           ; CH = column counter (resets to 1 each row)

inner_loop:
    ; Print current column number
    mov al, ch          ; AL = column number
    add al, 30h         ; Convert to ASCII
    mov ah, 02h
    mov dl, al
    int 21h

    ; Print space
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Advance column
    inc ch              ; CH++

    ; Inner loop: run while ch <= cl
    cmp ch, cl
    jle inner_loop      ; If ch <= cl, keep printing this row

    ; End of row: print newline
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Advance row
    inc cl              ; CL++

    ; Outer loop: run while cl <= 5
    cmp cl, 6
    jl outer_loop

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:**
```
1 
1 2 
1 2 3 
1 2 3 4 
1 2 3 4 5 
```

---

### Program 8.3.3 — Print a Star Pyramid

```nasm
; ============================================================
; Program: pyramid.asm
; Purpose: Print a pyramid of stars with 5 rows
;          Row 1: 1 star, Row 2: 3 stars, Row 3: 5 stars...
; Concept : Outer loop for rows, inner loop for stars
;           Stars per row = 2*row - 1
; ============================================================

org 100h

start:
    mov cl, 1           ; CL = current row (1 to 5)

pyramid_row:
    ; Calculate stars for this row: stars = 2*CL - 1
    mov al, cl
    shl al, 1           ; AL = CL * 2 (shift left = multiply by 2)
    dec al              ; AL = 2*CL - 1 (stars in this row)
    mov ch, al          ; CH = star count for inner loop

star_in_row:
    mov ah, 02h
    mov dl, '*'
    int 21h

    dec ch
    jnz star_in_row     ; Keep printing stars for this row

    ; Newline after row
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Next row
    inc cl
    cmp cl, 6           ; Done when CL > 5
    jl pyramid_row

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:**
```
*
***
*****
*******
*********
```

---

# 📞 PART 9: Procedures (Functions)

Procedures let you write code once and reuse it many times. This is the foundation of structured programming.

**Key rules:**
- Write the procedure **before or after** the main code (use `jmp start` to skip it)
- Use `CALL label` to execute a procedure
- Use `RET` to return to where you came from
- **Always** save registers you use inside a procedure with PUSH/POP

---

## 9.1 Simple Procedures

---

### Program 9.1.1 — Reusable Newline Procedure

```nasm
; ============================================================
; Program: proc_newline.asm
; Purpose: Demonstrate a reusable procedure to print a newline
; Concept : CALL/RET, register preservation with PUSH/POP
; ============================================================

org 100h

jmp start           ; Skip over procedures and data, go to main

; --- Data ---
line1 db 'First line$'
line2 db 'Second line$'
line3 db 'Third line$'

; ============================================================
; PROCEDURE: newline
; Purpose  : Prints CR + LF (moves cursor to a new line)
; Inputs   : None
; Outputs  : None
; Changes  : AH and DL (both saved and restored)
; ============================================================
newline:
    push ax             ; Save AX because we will change AH
    push dx             ; Save DX because we will change DL

    mov ah, 02h
    mov dl, 13          ; Print Carriage Return (move to column 0)
    int 21h

    mov ah, 02h
    mov dl, 10          ; Print Line Feed (move down one row)
    int 21h

    pop dx              ; Restore DX (in REVERSE order of push)
    pop ax              ; Restore AX
    ret                 ; Return to the instruction after CALL newline

; ============================================================
; MAIN PROGRAM
; ============================================================
start:
    ; Print line 1 + newline using the procedure
    mov ah, 09h
    lea dx, [line1]
    int 21h
    call newline        ; Jump into newline procedure, then come back

    ; Print line 2 + newline
    mov ah, 09h
    lea dx, [line2]
    int 21h
    call newline

    ; Print line 3 + newline
    mov ah, 09h
    lea dx, [line3]
    int 21h
    call newline

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

### Program 9.1.2 — Procedure to Print a Single Digit

```nasm
; ============================================================
; Program: proc_print_digit.asm
; Purpose: Create a procedure to print any digit 0-9
; Concept : Passing parameters via registers (convention: AL = input)
; ============================================================

org 100h

jmp start

; ============================================================
; PROCEDURE: print_digit
; Purpose  : Prints a digit (0-9) to the screen
; Input    : AL = the digit as a NUMBER (0-9), not ASCII
; Output   : None
; Registers: Saves and restores AX and DX
; ============================================================
print_digit:
    push ax             ; Save AX (we will modify AL)
    push dx             ; Save DX (we will modify DL)

    add al, 30h         ; Convert number to ASCII (e.g., 7 -> '7')
    mov ah, 02h         ; DOS: print character function
    mov dl, al          ; DL = ASCII character
    int 21h             ; Print it!

    pop dx              ; Restore DX
    pop ax              ; Restore AX
    ret                 ; Return to caller

; ============================================================
; MAIN PROGRAM
; ============================================================
start:
    ; Print digit 3
    mov al, 3           ; Pass parameter: AL = 3
    call print_digit    ; Call the procedure

    ; Print digit 7
    mov al, 7           ; Pass parameter: AL = 7
    call print_digit

    ; Print digit 0
    mov al, 0           ; Pass parameter: AL = 0
    call print_digit

    ; Print digit 5
    mov al, 5
    call print_digit

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `3705`

---

### Program 9.1.3 — Procedure That Returns a Value

```nasm
; ============================================================
; Program: proc_add.asm
; Purpose: Procedure adds two numbers and returns the result
; Concept : Input via BL and CL registers, output via AL
;           This is the "calling convention" we define ourselves
; ============================================================

org 100h

jmp start

; ============================================================
; PROCEDURE: add_bytes
; Purpose  : Adds two 8-bit numbers
; Input    : BL = first number
;            CL = second number
; Output   : AL = BL + CL (sum)
; Note     : Caller should save/restore BL and CL if needed
; ============================================================
add_bytes:
    mov al, bl          ; AL = first number
    add al, cl          ; AL = AL + second number
    ret                 ; Return with result in AL

; ============================================================
; MAIN PROGRAM
; ============================================================
start:
    ; --- Calculate 3 + 6 = 9 ---
    mov bl, 3
    mov cl, 6
    call add_bytes      ; AL = 9 after call

    ; Print result
    call print_digit_AL ; (using the print_digit_AL procedure below)

    ; Print space
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; --- Calculate 4 + 5 = 9 ---
    mov bl, 4
    mov cl, 5
    call add_bytes      ; AL = 9

    call print_digit_AL

    ; Exit
    mov ax, 4C00h
    int 21h

; ============================================================
; Helper procedure to print digit in AL
; ============================================================
print_digit_AL:
    push dx
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h
    pop dx
    ret
```

**Expected output:** `9 9`

---

## 9.2 Advanced Procedures

---

### Program 9.2.1 — Procedure to Print Any Number 0-99

```nasm
; ============================================================
; Program: print_num_proc.asm
; Purpose: Reusable procedure to print any number from 0 to 99
; Concept : Division to extract digits, conditional print of tens
; ============================================================

org 100h

jmp start

; ============================================================
; PROCEDURE: print_num
; Purpose  : Prints a number between 0 and 99
; Input    : AL = the number (0 to 99)
; Output   : Number displayed on screen
; Saves    : BX, CX, DX (all restored on exit)
; ============================================================
print_num:
    push ax
    push bx
    push cx
    push dx

    xor ah, ah          ; AH = 0 so AX = AL (zero-extend to 16-bit)
    mov bl, 10
    div bl              ; AL = tens digit, AH = units digit

    mov cl, ah          ; Save units digit in CL

    ; Print tens digit (skip it if it's 0 to avoid leading zero)
    cmp al, 0
    je  skip_tens

    add al, 30h         ; Convert tens to ASCII
    mov ah, 02h
    mov dl, al
    int 21h

skip_tens:
    ; Always print units digit
    mov al, cl
    add al, 30h         ; Convert units to ASCII
    mov ah, 02h
    mov dl, al
    int 21h

    pop dx
    pop cx
    pop bx
    pop ax
    ret

; ============================================================
; MAIN PROGRAM
; ============================================================
start:
    ; Print 7
    mov al, 7
    call print_num
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Print 42
    mov al, 42
    call print_num
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Print 99
    mov al, 99
    call print_num
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Print 0
    mov al, 0
    call print_num

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `7 42 99 0`

---

### Program 9.2.2 — Procedure: Find Maximum of Three Numbers

```nasm
; ============================================================
; Program: max_three.asm
; Purpose: Find the largest of three numbers using a procedure
; Concept : Nested comparisons in procedure, result returned in AL
; ============================================================

org 100h

jmp start

; ============================================================
; PROCEDURE: max3
; Purpose  : Returns the maximum of three 8-bit values
; Input    : AL = number1, BL = number2, CL = number3
; Output   : AL = the maximum value
; ============================================================
max3:
    ; Step 1: Find max of AL and BL, put winner in AL
    cmp al, bl          ; Compare AL and BL
    jge step2           ; If AL >= BL, AL is already winner, go to step 2
    mov al, bl          ; Otherwise BL is bigger — copy it to AL

step2:
    ; Step 2: Compare current winner (AL) with CL
    cmp al, cl
    jge max_found       ; If AL >= CL, AL is the maximum
    mov al, cl          ; Otherwise CL is the biggest

max_found:
    ret                 ; Return with AL = maximum value

; ============================================================
; MAIN PROGRAM
; ============================================================
start:
    ; Find max of 12, 45, 30
    mov al, 12          ; First number
    mov bl, 45          ; Second number
    mov cl, 30          ; Third number
    call max3           ; AL = 45 (the maximum)

    ; Print result using print_num
    call print_num

    ; Newline
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Find max of 99, 1, 50
    mov al, 99
    mov bl, 1
    mov cl, 50
    call max3           ; AL = 99

    call print_num

    ; Exit
    mov ax, 4C00h
    int 21h

; Reuse from previous program:
print_num:
    push ax
    push bx
    push cx
    push dx
    xor ah, ah
    mov bl, 10
    div bl
    mov cl, ah
    cmp al, 0
    je skip_t
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h
skip_t:
    mov al, cl
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h
    pop dx
    pop cx
    pop bx
    pop ax
    ret
```

**Expected output:**
```
45
99
```

---

### Program 9.2.3 — Iterative Factorial Using a Procedure

```nasm
; ============================================================
; Program: factorial.asm
; Purpose: Compute N! using a loop inside a procedure
; Concept : Loop-based factorial, 16-bit result returned in AX
; Note    : Maximum safe N = 7 (7! = 5040 fits in 16-bit AX)
;           8! = 40320, still fits. 9! = 362880 overflows!
; ============================================================

org 100h

jmp start

; ============================================================
; PROCEDURE: factorial
; Purpose  : Computes N! iteratively
; Input    : CL = N (the number, 0 to 8)
; Output   : AX = N! (the factorial)
; ============================================================
factorial:
    push bx
    push cx

    mov ax, 1           ; AX = 1 (result accumulator, starts at 1)

    ; Edge case: 0! = 1
    cmp cl, 0
    je  fact_done       ; Return 1 immediately if N=0

fact_loop:
    xor bh, bh          ; BH = 0
    mov bl, cl          ; BL = current N value
    mul bx              ; AX = AX * BX (16-bit multiply)

    dec cl              ; N--
    jnz fact_loop       ; Repeat while N > 0

fact_done:
    pop cx
    pop bx
    ret                 ; AX = N!

; ============================================================
; MAIN PROGRAM — computes 6! = 720
; ============================================================
start:
    mov cl, 6           ; N = 6
    call factorial      ; AX = 720

    ; Print AX (720 is a 3-digit number)
    ; Hundreds digit: 720 / 100 = 7 remainder 20
    mov bx, 100
    xor dx, dx
    div bx              ; AX = 7 (hundreds), DX = 20 (remainder)
    push dx             ; Save remainder (20)

    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '7'

    ; Process remainder 20
    pop ax              ; AX = 20
    xor ah, ah
    mov bl, 10
    div bl              ; AL = 2 (tens), AH = 0 (units)
    push ax

    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '2'

    pop ax
    mov al, ah          ; AL = units digit (0)
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '0'

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `720`

---

# 🗄️ PART 10: The Stack

The stack is a **LIFO (Last In, First Out)** memory area. The CPU automatically manages the Stack Pointer (SP).

```
PUSH AX → SP = SP - 2; memory[SP] = AX   (stack grows DOWN)
POP  AX → AX = memory[SP]; SP = SP + 2   (stack shrinks UP)
```

**Golden Rule: Every PUSH must have exactly one matching POP, in reverse order.**

---

### Program 10.1 — Demonstrating PUSH and POP

```nasm
; ============================================================
; Program: stack_demo.asm
; Purpose: Show how PUSH saves registers and POP restores them
; Concept : Stack as register backup storage
; Use AFD : Watch SP register decrease on PUSH, increase on POP
; ============================================================

org 100h

start:
    mov ax, 111         ; AX = 111
    mov bx, 222         ; BX = 222
    mov cx, 333         ; CX = 333

    ; --- Save registers on stack ---
    push ax             ; Stack (top to bottom): [111]
    push bx             ; Stack: [222, 111]
    push cx             ; Stack: [333, 222, 111]
    ; SP has decreased by 6 bytes total

    ; --- Simulate the registers being changed ---
    mov ax, 0           ; Wipe AX
    mov bx, 0           ; Wipe BX
    mov cx, 0           ; Wipe CX

    ; --- Restore from stack (MUST be in reverse push order!) ---
    pop cx              ; CX = 333 (last pushed, first popped — LIFO!)
    pop bx              ; BX = 222
    pop ax              ; AX = 111
    ; SP is back to original value

    ; All three registers are restored correctly!
    ; Verify in AFD

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

### Program 10.2 — Using Stack to Preserve CX in Nested Loops

```nasm
; ============================================================
; Program: stack_cx.asm
; Purpose: Show how to use the stack to save CX for nested loops
; Problem : LOOP uses CX — if we call LOOP inside LOOP, they clash
; Solution: Save outer CX with PUSH before inner loop, POP after
; ============================================================

org 100h

start:
    mov cx, 3           ; Outer loop: 3 rows

outer:
    push cx             ; SAVE outer CX before inner loop modifies it!

    mov cx, 4           ; Inner loop: 4 stars per row

inner:
    mov ah, 02h
    mov dl, '*'
    int 21h
    loop inner          ; CX-- for inner loop

    ; Print newline after each row
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    pop cx              ; RESTORE outer CX so outer LOOP works correctly
    loop outer          ; CX-- for outer loop

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:**
```
****
****
****
```

---

### Program 10.3 — Stack for Temporary Storage

```nasm
; ============================================================
; Program: stack_temp.asm
; Purpose: Use stack as temporary storage while calling procedures
; Concept : Push before call, pop after, to preserve values
; ============================================================

org 100h

jmp start

; A "destructive" procedure — changes AX for its own purposes
bad_proc:
    mov ax, 9999        ; Clobbers AX!
    ret

start:
    mov ax, 42          ; AX = 42 — we need this value after the call

    ; We want to call bad_proc but keep AX = 42
    push ax             ; Save AX on the stack
    call bad_proc       ; AX is now 9999 inside, but we saved ours
    pop ax              ; Restore: AX = 42 again

    ; Verify: print AX (should be 42)
    mov bl, 10
    xor ah, ah
    div bl              ; AL = 4 (tens), AH = 2 (units)
    push ax

    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '4'

    pop ax
    add ah, 30h
    mov al, ah
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '2'

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `42`

---

# 📥 PART 11: Reading Input from the Keyboard

## 11.1 Single Character Input

---

### Program 11.1.1 — Read and Echo a Character

```nasm
; ============================================================
; Program: echo_char.asm
; Purpose: Read one character from keyboard, print it back
; Concept : INT 21h function 01h reads character into AL
; ============================================================

org 100h

jmp start

prompt   db 'Type any key: $'
newline  db 13, 10, '$'
reply    db 13, 10, 'You typed: $'

start:
    ; Show prompt
    mov ah, 09h
    lea dx, [prompt]
    int 21h

    ; Read character from keyboard
    ; Function 01h echoes the character automatically
    mov ah, 01h
    int 21h             ; AL = ASCII code of pressed key

    ; Save the character
    mov bl, al          ; BL = pressed character

    ; Print reply
    mov ah, 09h
    lea dx, [reply]
    int 21h

    ; Print the stored character
    mov ah, 02h
    mov dl, bl          ; DL = our saved character
    int 21h

    ; Print newline
    mov ah, 09h
    lea dx, [newline]
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

### Program 11.1.2 — Read a Digit and Print Its Double

```nasm
; ============================================================
; Program: double_digit.asm
; Purpose: Read a single digit, print its double value
; Concept : ASCII to number, arithmetic, number to ASCII back
; ============================================================

org 100h

jmp start

prompt db 'Enter a digit (0-4): $'
result db 13, 10, 'Double = $'
nl     db 13, 10, '$'

start:
    ; Print prompt
    mov ah, 09h
    lea dx, [prompt]
    int 21h

    ; Read one key
    mov ah, 01h
    int 21h             ; AL = ASCII digit (e.g., '3' = 51 = 33h)

    ; Convert ASCII to actual number
    sub al, 30h         ; AL = 3 (strip the ASCII offset)

    ; Double it
    shl al, 1           ; AL = 6 (shift left 1 = multiply by 2)
    ; Alternatively: add al, al

    ; Save result
    mov bl, al          ; BL = 6

    ; Print result label
    mov ah, 09h
    lea dx, [result]
    int 21h

    ; Print the result digit (it's max 8 for input 0-4, single digit)
    mov al, bl
    add al, 30h         ; Convert back to ASCII
    mov ah, 02h
    mov dl, al
    int 21h

    ; Newline
    mov ah, 09h
    lea dx, [nl]
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

### Program 11.1.3 — Simple Menu System

```nasm
; ============================================================
; Program: menu.asm
; Purpose: Display a menu, respond to user's choice
; Concept : Input reading + multi-branch if-else with CMP
; ============================================================

org 100h

jmp start

menu_text db '=== MENU ===', 13, 10,
           db '1. Say Hello', 13, 10,
           db '2. Say Goodbye', 13, 10,
           db '3. Exit', 13, 10,
           db 'Choice: $'

opt1 db 13, 10, 'Hello, Student!', 13, 10, '$'
opt2 db 13, 10, 'Goodbye! See you next class!', 13, 10, '$'
opt3 db 13, 10, 'Exiting...', 13, 10, '$'
bad  db 13, 10, 'Invalid choice!', 13, 10, '$'

start:
    ; Show menu
    mov ah, 09h
    lea dx, [menu_text]
    int 21h

    ; Read choice
    mov ah, 01h
    int 21h             ; AL = pressed key

    ; Branch based on choice
    cmp al, '1'
    je  choice_1

    cmp al, '2'
    je  choice_2

    cmp al, '3'
    je  choice_3

    ; Invalid input
    mov ah, 09h
    lea dx, [bad]
    int 21h
    jmp done

choice_1:
    mov ah, 09h
    lea dx, [opt1]
    int 21h
    jmp done

choice_2:
    mov ah, 09h
    lea dx, [opt2]
    int 21h
    jmp done

choice_3:
    mov ah, 09h
    lea dx, [opt3]
    int 21h

done:
    mov ax, 4C00h
    int 21h
```

---

# 🧩 PART 12: Arrays

An array in assembly is a sequence of consecutive bytes in memory. You access elements using a **base address + offset**.

---

## 12.1 Working with Arrays

---

### Program 12.1.1 — Print All Elements of an Array

```nasm
; ============================================================
; Program: array_print.asm
; Purpose: Define an array of digits and print them all
; Concept : Base address in BX, increment BX to move through array
; ============================================================

org 100h

jmp start

; Array of 5 single-digit numbers
myArray  db 3, 7, 1, 9, 5
arrLen   equ 5              ; EQU defines a constant (not stored in memory)

start:
    lea bx, [myArray]   ; BX = base address (pointer to first element)
    mov cx, arrLen      ; CX = number of elements to print

print_loop:
    ; Read current element from memory
    mov al, [bx]        ; AL = *BX (dereference the pointer)

    ; Convert to ASCII and print
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h

    ; Print space separator
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Advance pointer to next element (each db element = 1 byte)
    inc bx              ; BX++

    loop print_loop

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `3 7 1 9 5`

---

### Program 12.1.2 — Sum All Elements of an Array

```nasm
; ============================================================
; Program: array_sum.asm
; Purpose: Add all numbers in an array and print the total
; Concept : Pointer traversal with accumulator
; ============================================================

org 100h

jmp start

data   db 10, 20, 5, 15, 30, 8, 12   ; 7 elements, sum = 100
count  equ 7
total  dw 0                            ; 16-bit result (sum may exceed 255)

start:
    lea bx, [data]      ; BX = pointer to array start
    mov cx, count       ; CX = element count
    xor ax, ax          ; AX = 0 (accumulator)

sum_loop:
    xor ah, ah          ; Clear AH (extend byte to word)
    mov al, [bx]        ; AL = current byte
    add ax, [bx]        ; But since AH=0, this works: AX += element
    inc bx              ; Next element
    loop sum_loop

    ; AX = 100 (total)
    mov [total], ax

    ; Print "100"
    ; Hundreds digit: 100 / 100 = 1 remainder 0
    mov bx, 100
    xor dx, dx
    div bx              ; AX = 1, DX = 0
    push dx

    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '1'

    pop ax              ; AX = 0 (remainder)
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '0'
    int 21h             ; Print '0' again (both zero)

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `100`

---

### Program 12.1.3 — Find Minimum and Maximum in an Array

```nasm
; ============================================================
; Program: array_minmax.asm
; Purpose: Scan array to find smallest and largest values
; Concept : Track min and max in registers while looping
; ============================================================

org 100h

jmp start

data   db 14, 55, 3, 88, 21, 7, 66
len    equ 7
minVal db 0
maxVal db 0

start:
    lea bx, [data]      ; BX = array pointer
    mov cx, len         ; CX = count
    mov al, [bx]        ; AL = first element = initial min AND max

    ; Initialize both min and max with the first element
    mov [minVal], al
    mov [maxVal], al

    inc bx              ; Move past first element
    dec cx              ; One less element to scan

scan_loop:
    mov al, [bx]        ; AL = current element

    ; Check if current < minVal
    cmp al, [minVal]
    jge not_new_min
    mov [minVal], al    ; Update minimum

not_new_min:
    ; Check if current > maxVal
    cmp al, [maxVal]
    jle not_new_max
    mov [maxVal], al    ; Update maximum

not_new_max:
    inc bx
    loop scan_loop

    ; Print min and max
    ; (Both are two-digit numbers for our test data)
    mov al, [minVal]    ; Min = 3
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '3'

    mov ah, 02h
    mov dl, ' '
    int 21h

    mov al, [maxVal]    ; Max = 88
    xor ah, ah
    mov bl, 10
    div bl
    push ax
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '8'
    pop ax
    add ah, 30h
    mov al, ah
    mov ah, 02h
    mov dl, al
    int 21h             ; Print '8'

    ; Exit
    mov ax, 4C00h
    int 21h
```

**Expected output:** `3 88`

---

# 🎮 PART 13: Complete Programs

---

## Program 13.1 — Number Guessing Game

```nasm
; ============================================================
; Program: guess_game.asm
; Purpose: A complete interactive guessing game
; Concept : Loop + input + conditions + feedback messages
; Rules   : Player enters a single digit to guess the secret
; ============================================================

org 100h

jmp start

secret   db 5           ; The hidden answer (change to test)

banner   db '=== GUESS THE NUMBER (0-9) ===', 13, 10, '$'
prompt   db 'Your guess: $'
correct  db 13, 10, '*** CORRECT! You Win! ***', 13, 10, '$'
hi_msg   db 13, 10, 'Too HIGH, guess lower.', 13, 10, '$'
lo_msg   db 13, 10, 'Too LOW, guess higher.', 13, 10, '$'

start:
    ; Print banner
    mov ah, 09h
    lea dx, [banner]
    int 21h

game_loop:
    ; Print prompt
    mov ah, 09h
    lea dx, [prompt]
    int 21h

    ; Read one digit from keyboard
    mov ah, 01h
    int 21h             ; AL = ASCII of pressed digit

    ; Convert ASCII to number
    sub al, 30h         ; AL = digit as number (0-9)

    ; Compare guess with secret
    mov bl, [secret]    ; BL = secret number
    cmp al, bl          ; Compare guess with secret

    je  won             ; Equal -> winner!
    jg  too_high        ; Guess > secret -> too high

    ; If we reach here, guess < secret
    mov ah, 09h
    lea dx, [lo_msg]
    int 21h
    jmp game_loop       ; Let them try again

too_high:
    mov ah, 09h
    lea dx, [hi_msg]
    int 21h
    jmp game_loop

won:
    mov ah, 09h
    lea dx, [correct]
    int 21h
    ; Game ends, fall through to exit

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

## Program 13.2 — Fibonacci Sequence (First 8 Terms)

```nasm
; ============================================================
; Program: fibonacci.asm
; Purpose: Print first 8 Fibonacci numbers: 0 1 1 2 3 5 8 13
; Concept : Track previous two terms, add them for the next
; ============================================================

org 100h

jmp start

f0 db 0         ; Fibonacci term F(n-2)
f1 db 1         ; Fibonacci term F(n-1)
fn db 0         ; Current Fibonacci term F(n)

start:
    ; Print F(0) = 0
    mov al, [f0]
    call print_num

    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Print F(1) = 1
    mov al, [f1]
    call print_num

    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Compute and print F(2) through F(7) = 6 more terms
    mov cx, 6

fib_loop:
    ; fn = f0 + f1
    mov al, [f0]
    add al, [f1]        ; AL = f0 + f1
    mov [fn], al        ; Store as current term

    ; Print current term
    call print_num
    mov ah, 02h
    mov dl, ' '
    int 21h

    ; Shift: f0 = f1, f1 = fn
    mov al, [f1]
    mov [f0], al        ; f0 becomes old f1
    mov al, [fn]
    mov [f1], al        ; f1 becomes new fn

    loop fib_loop

    ; Newline at end
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Exit
    mov ax, 4C00h
    int 21h

; ============================================================
; PROCEDURE: print_num
; Prints AL as a number (0-99)
; ============================================================
print_num:
    push ax
    push bx
    push cx
    push dx

    xor ah, ah          ; AX = AL (zero extend)
    mov bl, 10
    div bl              ; AL=tens, AH=units
    mov cl, ah

    cmp al, 0
    je skip_t2
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h

skip_t2:
    mov al, cl
    add al, 30h
    mov ah, 02h
    mov dl, al
    int 21h

    pop dx
    pop cx
    pop bx
    pop ax
    ret
```

**Expected output:** `0 1 1 2 3 5 8 13`

---

## Program 13.3 — Full Star Pyramid with Input

```nasm
; ============================================================
; Program: input_pyramid.asm
; Purpose: Ask user for number of rows, print a star pyramid
; Concept : Input, conversion, nested loops with PUSH/POP for CX
; ============================================================

org 100h

jmp start

prompt db 'Enter number of rows (1-9): $'
nl     db 13, 10, '$'

start:
    ; Print prompt
    mov ah, 09h
    lea dx, [prompt]
    int 21h

    ; Read number of rows from user
    mov ah, 01h
    int 21h             ; AL = ASCII digit
    sub al, 30h         ; Convert to actual number
    mov cl, al          ; CL = total rows

    ; Print a newline first
    mov ah, 09h
    lea dx, [nl]
    int 21h

    ; --- OUTER LOOP: one iteration per row ---
    ; We already have CL = row count
    ; We'll use a different approach: use BL as row counter (1 to CL)
    mov bl, 1           ; BL = current row number
    mov bh, cl          ; BH = total rows (save it)

row_loop:
    ; --- INNER LOOP: print BL stars ---
    mov cl, bl          ; CL = number of stars for this row

star_loop:
    mov ah, 02h
    mov dl, '*'
    int 21h
    loop star_loop      ; Print BL stars (CL decrements)

    ; Print newline after each row
    mov ah, 02h
    mov dl, 13
    int 21h
    mov ah, 02h
    mov dl, 10
    int 21h

    ; Move to next row
    inc bl              ; BL++

    ; Check if done (BL > total rows)
    cmp bl, bh
    jle row_loop        ; While BL <= total rows

    ; Exit
    mov ax, 4C00h
    int 21h
```

---

# 📚 PART 14: Quick Reference Summary

## Instruction Cheat Sheet

```
======================================================
DATA MOVEMENT
======================================================
mov  dst, src      Copy data from src to dst
xchg dst, src      Swap values between two operands
push src           Push value onto stack (SP -= 2)
pop  dst           Pop value from stack (SP += 2)
lea  dst, [addr]   Load effective address (pointer)

======================================================
ARITHMETIC
======================================================
add  dst, src      dst = dst + src
sub  dst, src      dst = dst - src
inc  dst           dst = dst + 1
dec  dst           dst = dst - 1
mul  src           AX = AL * src  (8-bit unsigned)
div  src           AL = AX / src, AH = AX mod src
neg  dst           dst = -dst (two's complement)

======================================================
LOGIC
======================================================
and  dst, src      dst = dst AND src  (bitwise)
or   dst, src      dst = dst OR src
xor  dst, src      dst = dst XOR src
not  dst           dst = NOT dst (flip all bits)

xor ax, ax         Fastest way to zero a register!

======================================================
SHIFTS
======================================================
shl  dst, n        Shift left n bits (x 2^n)
shr  dst, n        Shift right n bits (/ 2^n, unsigned)
sar  dst, n        Arithmetic shift right (signed)

======================================================
COMPARE AND JUMP
======================================================
cmp  a, b          Compute a-b, set flags (don't store)
test a, b          Compute a AND b, set flags

je   / jz   lbl    Jump if equal / zero (ZF=1)
jne  / jnz  lbl    Jump if not equal / not zero (ZF=0)
jg   / jnle lbl    Jump if greater (signed)
jl   / jnge lbl    Jump if less (signed)
jge  / jnl  lbl    Jump if >= (signed)
jle  / jng  lbl    Jump if <= (signed)
ja          lbl    Jump if above (unsigned)
jb   / jc   lbl    Jump if below / carry (unsigned)
js          lbl    Jump if negative (SF=1)
jmp         lbl    Unconditional jump

loop        lbl    CX-- ; if CX!=0 jump to lbl

======================================================
PROCEDURES
======================================================
call lbl           Push IP, jump to procedure
ret                Pop IP, return to caller

======================================================
INT 21h FUNCTIONS
======================================================
AH=01h   Read keyboard char into AL (echoes it)
AH=02h   Print char in DL to screen
AH=09h   Print string at [DX] until '$'
AH=4Ch   Exit program (AL = exit code)
```

---

## Common Patterns Reference

```nasm
; ---- Set register to zero (fastest way) ----
xor ax, ax          ; AX = 0

; ---- Print a newline (CR + LF) ----
mov ah, 02h  /  mov dl, 13  /  int 21h     ; Carriage Return
mov ah, 02h  /  mov dl, 10  /  int 21h     ; Line Feed

; ---- Convert digit NUMBER to ASCII CHARACTER ----
add al, 30h         ; 7 -> '7'

; ---- Convert ASCII CHARACTER back to NUMBER ----
sub al, 30h         ; '7' -> 7

; ---- Print a string (must end with '$') ----
mov ah, 09h
lea dx, [myString]
int 21h

; ---- Read one key from keyboard ----
mov ah, 01h
int 21h             ; AL = character

; ---- Save and restore registers (stack) ----
push ax  /  push bx     ; Save (in this order)
...code that changes AX and BX...
pop bx   /  pop ax      ; Restore (REVERSE order!)

; ---- Count-controlled loop ----
mov cx, N
myloop:
    ...body...
    loop myloop         ; CX-- ; repeat if CX != 0

; ---- While loop (CMP + JMP) ----
whileloop:
    cmp al, limit
    jge endloop         ; Exit condition
    ...body...
    jmp whileloop
endloop:

; ---- If-Else structure ----
    cmp al, value
    je  true_branch
    ; false code here
    jmp endif
true_branch:
    ; true code here
endif:

; ---- Two-digit number print ----
xor ah, ah          ; AX = AL (zero extend)
mov bl, 10
div bl              ; AL = tens, AH = units
push ax
add al, 30h  /  mov ah,02h  /  mov dl,al  /  int 21h   ; tens
pop ax
add ah, 30h  /  mov al, ah  /  mov ah,02h  /  mov dl,al  /  int 21h  ; units
```

---

## Complete Workflow Summary

```
STEP 1: Write code
    - Use Notepad or any text editor
    - Save file as: myprogram.asm
    - Put it in: C:\assembly\

STEP 2: Mount drive in DOSBox
    mount c c:\assembly
    c:

STEP 3: Assemble
    nasm myprogram.asm -o myprogram.com
    (if no error appears, assembly succeeded)

STEP 4: Run
    myprogram

STEP 5: Debug (if something is wrong)
    afd myprogram.com
    F8 = step one instruction at a time
    Tab = switch between panels
    Watch: registers, flags, memory
    Alt+X = exit AFD

STEP 6: Edit → Reassemble → Rerun
    Repeat until program works correctly
```

---

> **A Final Word to Students:**
>
> Assembly language punishes laziness and rewards patience. Unlike Python or Java, there are no safety nets — no garbage collector, no bounds checking, no helpful error messages. You must think like the CPU.
>
> The secret to mastering this course:
> 1. **Type every program yourself.** Never copy-paste.
> 2. **Run it through AFD.** Watch every register change with F8.
> 3. **Change one thing.** See what breaks. Fix it.
> 4. **Comment every line** until you understand why it is there.
>
> Struggle is normal. Confusion is normal. Assembly is hard because it is **real**. When your program finally works, you will have earned it.
>
> **Good luck.**

---
*End of Complete Assembly Language Course — DOSBox + NASM + AFD*
*Total Topics: DOSBox basics, Architecture, Registers, Instructions, I/O, Arithmetic, Conditions, Loops, Procedures, Stack, Arrays, Complete Programs*
