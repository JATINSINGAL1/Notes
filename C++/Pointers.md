Pointers are nothing but **Variable** that carry or stores address of the memory in form of Integer. 
Consider two blocks 11 , 12. These are the address of these blocks. The block 12 contains the data let say 100. 

Okay the setup is ready, Now think how the CPU would know to access this memory block it doesn't know anything about it. Here comes the pointer to the rescue. 11 would store the address of 12 nothing but block 11 would be initialized with value 12. 

Now you would say How CPU knows about 11, so consider it as special block whose address is known to CPU.

THIS is how memory is actually accessed. 


```
void* ptr = NULL ; 
```

Question
1. What is data type of ptr block ? -> INTEGER
2. What is the value it store? NULL -> 0
3. What does void define here? (actually it is for syntactical purpose which defines the data type of block whose address we are storing inside the ptr).


```
int var = 8 ;
void* ptr = &var ;  
cout<<&var;
```

Simply Print the address of var block. ==Use of &== 

You know the type in pointer are used for just read and write purpose to make sure The integrity of the data block is maintained. 


### Dereferencing the Pointer

`*ptr`

The above lines means nothing but var so it implies we can directly access or change the data. 

Now the clear Explanation of type comes:
when we want to change the data lets say `*ptr = 10` how does our compiler know how many bytes to change. 
Cause the address defined is the starting point of the smallest unit and our data can comprise of multiple such small unit (byte), type actually help us to tell when to stop from that starting point. 

---
## Meaning of Type for Pointer 

This is a fantastic question. You are absolutely right: physically, all pointers are just memory addresses, and on a 64-bit system, every pointer (whether `int*`, `char*`, or `double*`) usually takes up exactly the same amount of space (8 bytes).

However, the compiler needs the data type for three critical reasons: **Dereferencing (Size)**, **Interpretation**, and **Arithmetic**.

Here is the breakdown of why the type definition is mandatory.

---

### 1. Dereferencing: How many bytes to read?

When you just hold the pointer `ptr`, you hold the starting address of the data. But when you _dereference_ it (use `*ptr`), you are asking the computer to "go to this address and read the value."

Without the type, the computer knows where to start, but it doesn't know when to **stop**.

- **`char* ptr`:** The computer goes to the address and reads **1 byte**.
- **`int* ptr`:** The computer goes to the address and reads **4 bytes** (usually).
- **`double* ptr`:** The computer goes to the address and reads **8 bytes**.

If you stored an `int` (4 bytes) but used a `char*` to point to it, when you dereferenced it, you would only get the first 8 bits of the number, resulting in garbage data.

### 2. Interpretation: How to read the bits?

Even if two variables have the same size, the computer reads their binary data differently based on type.

Imagine you have 4 bytes of data at an address.

- If it is an `int*`, the computer reads the binary using **Two's Complement** (standard integer math).
- If it is a `float*`, the computer reads the _exact same binary_ using **IEEE 754 floating point standard** (exponents and mantissas).

If you didn't specify the type, the CPU wouldn't know if the binary `01000001` represents the number `65` or the letter `'A'`.

### 3. Pointer Arithmetic: How big is a "Step"?

This is often the most confusing part for beginners, but it is the most important for arrays and iteration.

When you write `ptr + 1`, you are not adding the number 1 to the memory address. You are telling the pointer to "move to the **next** element."

The "step size" depends entirely on the data type:

- **`char* ptr`:** `ptr + 1` adds **1** to the actual memory address.
- **`int* ptr`:** `ptr + 1` adds **4** to the actual memory address.
- **`Structure* ptr`:** If you have a custom struct that is 100 bytes large, `ptr + 1` adds **100** to the address.

**Visual Example:**

> Imagine a street where houses are different sizes.
> 
> - **Char Street:** Houses are 1 meter wide. If you walk to the "next" house, you walk 1 meter.
> - **Int Street:** Houses are 4 meters wide. If you walk to the "next" house, you must walk 4 meters to get past the current one.

---

### Summary Comparison

Here is a quick look at what happens to the same memory address `0x1000` based on how you define the pointer:

|**Feature**|**char* ptr = 0x1000**|**int* ptr = 0x1000**|
|---|---|---|
|**Storage Size**|Pointer itself takes 8 bytes.|Pointer itself takes 8 bytes.|
|**`*ptr` (Read)**|Reads memory `0x1000` only.|Reads memory `0x1000` to `0x1003`.|
|**`ptr + 1` (Math)**|Result is `0x1001`.|Result is `0x1004`.|

### The Exception: `void*`

There is actually a type of pointer that behaves exactly as you described in your question: the **`void` pointer** (`void* ptr`).

- It stores an address and nothing else.
- Because it has no type, **you cannot dereference it** (`*ptr` throws an error) and **you cannot do arithmetic** (`ptr++` throws an error).
- To use it, you must cast it to a specific type first so the compiler knows how to handle the data.

## Dynamic Memory Allocation 

```
int* buffer = new int[8] ; 
memset(buffer, 0 , 8); 
delete[] buffer ; 
```

what we did is **Created an array** of size 8 whose first address of first block is stored inside buffer. 
Using memset  we initialized it with 0 ; 
and deleted it later . 

### Doubt 1 
`int a = 5; int var = &a; `

The above code through compile error why? 

#### 1. The "Physical" Reason: The Box is Too Small

This is the most practical reason.

- On almost all modern computers (64-bit systems), an integer (`int`) is usually **4 bytes**.
- A memory address (pointer) is **8 bytes**.

You are trying to pour an **8-liter bucket** into a **4-liter bottle**.

If the compiler allowed this, the top half of the memory address would be sliced off (truncated). The value stored in `var` would be a corrupted, useless number that points to nowhere.

#### 2. The "Logical" Reason: Type Safety

Even if you are on an old 32-bit system where pointers and integers are the same size (4 bytes), C++ still forbids this without a specific "cast."

C++ is **Strongly Typed**. It treats `int` (a number for math) and `int*` (a coordinate for memory) as completely different species.

- **`int`**: Supports `+`, `-`, `/`, `*` (multiplication).
- **`int*`**: Supports dereferencing (`*`), pointing (`->`).

If the compiler let you silently store a pointer in an `int`, you might accidentally try to divide your memory address by 5, which is nonsensical logic. The compiler forces you to admit, "I know what I am doing," before it allows the conversion.

# Dangling Pointer 
 dangling pointer is ==a pointer that points to a memory location that has been deallocated or freed==. This creates a situation where the pointer still holds the address, but the memory at that address is no longer valid or in use. Attempting to access this memory can lead to undefined behavior, such as crashes, incorrect results, or data corruption.

# Smart Pointers 
```
class Player{
int x ; 
/// further code 
};
class ScopedPtr{
private : 
Player * m_ptr; 
public : 
ScopedPtr(Entity* ptr)
: m_ptr(ptr)
{} // constructor 

~ScopedPtr(){
delete m_ptr ; 
}

};

int main(){
{
ScopedPtr e = new Player();
}
}
```

or Scoped Pointer is just a class wrapper around a pointer which upon construction heap allocates the pointer and then upon destruction deletes the pointer.  

Explanation: basically the ScopedPtr object got created on stack when the scope ended the object got deleted. 