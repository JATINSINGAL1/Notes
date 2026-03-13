it is an operator. main function it return an address of the datatype we want to create. 
new doesn't only allocate the memory it also call the constructor. 

`Entity* e = new Entity(); `
`Entity *e = (Entity*)malloc(sizeof(Entity))` this allocates space and return address for that memory. 
The only difference between the above two line is earlier one call the constructor as well. 

`int x = new int();`
This specific line of code, as written, is **incorrect** and will cause a **Compile Error** in C++.

You are trying to fit a "Memory Address" into a variable designed to hold a "Simple Number."

Here is the breakdown of why it fails and what you likely meant to write.

### 1. The Error

`int x = new int();`

- **`new int()`**: This requests memory from the **Heap**. It creates a new integer and returns its **address** (a pointer, `int*`).
- **`int x`**: This is a standard integer variable. It expects a value like `5`, `10`, or `-1`.
- **The Conflict:** You are trying to shove a pointer (address) into a regular integer variable.

> **Analogy:** You are trying to store a whole "House" (or the address of the house) inside a small "Mailbox."

---

### 2. The Correction (What you likely meant)

If you are using `new`, you are doing **Dynamic Memory Allocation**, which means you need a **pointer** to hold the result.

**Correct Syntax:**

C++

```
int* x = new int(); 
```

Here is what this corrected line does, step-by-step:

1. **`new int`**: The computer goes to the **Heap** (the large pool of free memory) and finds space for 1 integer (4 bytes).
2. **`()`**: The parentheses tell the computer to **initialize** that value to `0`. (If you wrote just `new int` without parentheses, the value would be garbage).
3. **Returns Address**: The `new` keyword returns the address of that new block (e.g., `0x800`).
4. **`int* x`**: You create a pointer named `x` on the **Stack**.
5. **`=`**: The address `0x800` is stored inside `x`.

---

### 3. Another Possibility (Stack Allocation)

If you didn't want a pointer and just wanted an integer initialized to 0, you don't use `new`.

**Correct Syntax:**

C++

```
int x = int(); // Creates an int on the stack and sets it to 0
// OR simply:
int x = 0;
```

### Summary

- **`new`** always returns a **Pointer**.
- Therefore, the variable on the left side **must** be a pointer type (`int*`), not a regular type (`int`).

---


# If you use new keyword you have to use Delete to free that memory. 
`delete var_name ;` does free(sizeof(var_name)); as well as call the destructor . 


# Placement new 