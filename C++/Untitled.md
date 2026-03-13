You are absolutely spot on regarding your intuition about the string literal.

Here is the technical breakdown of why `char*` behaves differently than `int*` in this context.

### 1. The `char* name = "jatin"` Case

You are correct: **"jatin" is stored in a specific memory location.**

- **Storage:** When the compiler sees `"jatin"`, it stores those 6 bytes (5 chars + null terminator `\0`) in the **Global/Static Read-Only Memory** (often called the `.rodata` section).
    
- **Evaluation:** In C++, an array name decays into a pointer. So, the literal `"jatin"` evaluates to the **memory address** of the first character 'j' (e.g., `0x5004`).
    
- **Assignment:** When you write `char* name = "jatin";`, you are assigning that address (`0x5004`) to the pointer variable `name`.
    

**Visual Representation:**

> **Important Warning:** In modern C++, `char* name = "jatin"` is actually deprecated (and invalid in C++11 onwards) because the memory is read-only. You should strictly use `const char* name = ...` to ensure you don't try to modify that read-only memory (which causes a crash).

---

### 2. The `int* x = 8` Case

This fails because **8 is a value, not a memory address.**

- **The Literal:** The number `8` is an integer literal (an "r-value"). It likely exists directly inside the CPU instruction code, not in a specific addressable memory slot you are meant to reference.
    
- **Type Mismatch:** The variable `x` expects a type of `int*` (a memory address). You are providing an `int` (a number). The compiler prevents this implicit conversion to stop you from shooting yourself in the foot.
    
- **What if you force it?** If you cast it: `int* x = (int*)8;`, the code will compile. However, `x` will now point to the **memory address 0x00000008**. This is reserved operating system memory. If you try to access it (`*x`), your program will crash immediately (Segmentation Fault).