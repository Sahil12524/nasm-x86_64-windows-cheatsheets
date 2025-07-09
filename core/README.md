# 🧠 NASM x86-64 Assembly Cheat Sheet (Windows)

> Focusing on core integer operations and system programming. (SSE/AVX moved to separate guide.)

---

## 1⃣ General Purpose Registers

| Register | Purpose                                               | Preserved? |
| -------- | ----------------------------------------------------- | ---------- |
| RAX      | Accumulator (return values, multiplication, division) | No         |
| RBX      | Base register                                         |            |

| **Yes** |                                     |         |
| ------- | ----------------------------------- | ------- |
| RCX     | Counter (loops, `rep` instructions) | No      |
| RDX     | Data register (I/O, multiplication) | No      |
| RSI     | Source index (string ops)           | No      |
| RDI     | Destination index (string ops)      | No      |
| RBP     | Base pointer (stack frames)         | **Yes** |
| RSP     | Stack pointer                       | **Yes** |
| R8-R11  | Additional arguments/temporary      | No      |
| R12-R15 | General purpose                     | **Yes** |

> ℹ️ *Windows ABI:* Volatile registers may be destroyed by function calls. Preserved registers must be restored.

---

## 2⃣ Core Data Movement

| Instruction      | Description                     | Example                 |
| ---------------- | ------------------------------- | ----------------------- |
| `mov dst, src`   | Copy data                       | `mov rax, rbx`          |
| `lea dst, [mem]` | Load effective address          | `lea rcx, [rbx+8]`      |
| `xchg a, b`      | Swap values                     | `xchg rax, rdx`         |
| `push src`       | Push to stack (decrements RSP)  | `push rbp`              |
| `pop dst`        | Pop from stack (increments RSP) | `pop rdi`               |
| `movzx dst, src` | Zero-extend                     | `movzx eax, byte [rcx]` |
| `movsx dst, src` | Sign-extend                     | `movsx rdx, word [rsi]` |

---

## 3⃣ Arithmetic Operations

| Instruction     | Description                              | Flags Affected       |
| --------------- | ---------------------------------------- | -------------------- |
| `add dst, src`  | Addition                                 | OF, SF, ZF, CF       |
| `sub dst, src`  | Subtraction                              | OF, SF, ZF, CF       |
| `inc dst`       | Increment by 1                           | OF, SF, ZF (not CF!) |
| `dec dst`       | Decrement by 1                           | OF, SF, ZF (not CF!) |
| `neg dst`       | Two's complement negation                | OF, SF, ZF, CF       |
| `imul dst, src` | Signed multiply                          | OF, CF               |
| `mul src`       | Unsigned multiply (RAX × src → RDX\:RAX) | OF, CF               |
| `idiv src`      | Signed divide (RDX\:RAX ÷ src)           | -                    |
| `div src`       | Unsigned divide                          | -                    |

---

## 4⃣ Bitwise Operations

| Instruction    | Description            | Example           |
| -------------- | ---------------------- | ----------------- |
| `and dst, src` | Bitwise AND            | `and rax, 0xFF`   |
| `or dst, src`  | Bitwise OR             | `or rbx, rcx`     |
| `xor dst, src` | Bitwise XOR            | `xor rax, rax`    |
| `not dst`      | Bitwise NOT            | `not qword [rsp]` |
| `shl dst, cnt` | Shift left (unsigned)  | `shl rdx, 3`      |
| `shr dst, cnt` | Shift right (unsigned) | `shr rax, 1`      |
| `sal dst, cnt` | Arithmetic shift left  | (Same as `shl`)   |
| `sar dst, cnt` | Arithmetic shift right | `sar rbx, 4`      |
| `rol dst, cnt` | Rotate left            | `rol al, 4`       |
| `ror dst, cnt` | Rotate right           | `ror rdx, 8`      |

---

## 5⃣ Control Flow

### Comparisons

| Instruction | Description                        | Flags Set      |
| ----------- | ---------------------------------- | -------------- |
| `cmp a, b`  | Compute `a - b` (discard result)   | OF, SF, ZF, CF |
| `test a, b` | Bitwise `a AND b` (discard result) | SF, ZF, PF     |

### Jumps

| Instruction | Condition                  | Notes       |
| ----------- | -------------------------- | ----------- |
| `jmp label` | Unconditional              | -           |
| `je/jz`     | ZF=1 (equal/zero)          | After `cmp` |
| `jne/jnz`   | ZF=0 (not equal)           | -           |
| `jg/jnle`   | SF=OF and ZF=0 (signed >)  | -           |
| `jge/jnl`   | SF=OF (signed ≥)           | -           |
| `jl/jnge`   | SF≠OF (signed <)           | -           |
| `jle/jng`   | ZF=1 or SF≠OF (signed ≤)   | -           |
| `ja/jnbe`   | CF=0 and ZF=0 (unsigned >) | -           |
| `jb/jnae`   | CF=1 (unsigned <)          | -           |

---

## 6⃣ Function Calls (Windows ABI)

### Calling Convention

1. **Integer/pointer args**: `RCX`, `RDX`, `R8`, `R9`
2. **Additional args**: Push to stack (right-to-left)
3. **Stack alignment**: Must be 16-byte aligned *before* `call`
4. **Shadow space**: Reserve 32 bytes even if less than 4 args

```asm
sub rsp, 40      ; 32 (shadow) + 8 (align) = 40
mov rcx, arg1    ; 1st arg
mov rdx, arg2    ; 2nd arg
call function
add rsp, 40      ; Cleanup
```

### Prologue/Epilogue

```asm
my_function:
    push rbp          ; Save frame pointer
    mov rbp, rsp      ; New frame
    sub rsp, 32       ; Local variables
    ...
    leave             ; ≡ mov rsp, rbp; pop rbp
    ret
```

---

## 7⃣ System Calls (Windows API Examples)

| Function       | Usage                             |
| -------------- | --------------------------------- |
| `ExitProcess`  | `mov rcx, code; call ExitProcess` |
| `GetStdHandle` | `mov rcx, -11; call GetStdHandle` |
| `WriteFile`    | Requires handle, buffer, length   |

```asm
extern ExitProcess
section .data
    msg db "Hello", 0

section .text
    mov rcx, msg
    call printf
    mov rcx, 0
    call ExitProcess
```

---

## 8⃣ String Operations

| Instruction | Description                 | Registers Used |
| ----------- | --------------------------- | -------------- |
| `movsb`     | Copy byte `[rsi]` → `[rdi]` | RSI, RDI       |
| `rep movsb` | Repeat `RCX` times          | RCX, RSI, RDI  |
| `cmpsb`     | Compare `[rsi]` vs `[rdi]`  | Sets ZF        |
| `scasb`     | Scan for `AL` in `[rdi]`    | RDI, AL        |
| `stosb`     | Store `AL` → `[rdi]`        | RDI, AL        |

---

## 9⃣ Essential Directives

```asm
section .data           ; Initialized data
    msg db "Hello", 0   ; Null-terminated string

section .bss            ; Uninitialized data
    buffer resb 64      ; Reserve 64 bytes

section .text           ; Code
global main             ; Export symbol
main:                   ; Entry point
    ...
```

---

## 📂 Stack Operations in x86-64 Assembly

The stack is a **LIFO** (Last-In-First-Out) data structure managed via the **RSP** (Stack Pointer). On x86-64, the stack **grows downward** (toward lower addresses).

### 1⃣ Basic Stack Operations

| Instruction | Description         | Effect on RSP | Example    |
| ----------- | ------------------- | ------------- | ---------- |
| `push src`  | Push onto stack     | `RSP -= 8`    | `push rax` |
| `pop dst`   | Pop from stack      | `RSP += 8`    | `pop rbx`  |
| `pushf`     | Push FLAGS register | `RSP -= 8`    | `pushf`    |
| `popf`      | Pop into FLAGS      | `RSP += 8`    | `popf`     |

### 2⃣ Stack Frame Management

| Instruction    | Description          | Usage                           |
| -------------- | -------------------- | ------------------------------- |
| `enter n, 0`   | Allocate stack frame | `enter 32, 0` (32-byte reserve) |
| `leave`        | Release stack frame  | `mov rsp, rbp; pop rbp`         |
| `mov rbp, rsp` | Manual frame setup   | Used in prologues               |

### Example Prologue/Epilogue

```asm
my_function:
    push rbp
    mov rbp, rsp
    sub rsp, 32
    ...
    leave
    ret
```

### 3⃣ Stack in Function Calls (Windows ABI)

```asm
sub rsp, 40        ; 32 (shadow) + 8 (align)
mov rcx, arg1      ; First argument
mov rdx, arg2      ; Second argument
push arg5          ; Push args 5+, right-to-left
push arg4
call some_function
add rsp, 40        ; Cleanup
```

### 4⃣ Stack Tricks

| Technique           | Code Example             | Use Case               |
| ------------------- | ------------------------ | ---------------------- |
| Preserve registers  | `push rax; ...; pop rax` | Save/restore registers |
| Access stack args   | `mov rax, [rbp+16]`      | Access 2nd argument    |
| Allocate space      | `sub rsp, 16`            | Scratch space          |
| Dynamic stack alloc | `sub rsp, rax`           | Variable space         |

**Example: Swapping Registers via Stack**

```asm
push rax
push rbx
pop rax   ; rax = old rbx
pop rbx   ; rbx = old rax
```

### 5⃣ Stack Pitfalls

- ❌ **Misalignment**: Ensure 16-byte alignment before `call`
- ❌ **Unbalanced pushes/pops**: Crashes or corrupted data
- ❌ **Overwriting return address**: Crashes on `ret`

**Debug Tip**: Use `sub rsp,8` temporarily to inspect values in a debugger.

---

Made with ❤️ for systems programmers.

