This is a great transition. You are asking about the fundamental ways computers represent text and how C++ categorizes data expressions. These two concepts—Encodings and Value Categories—are pillars of low-level programming.

---

### Part 1: Wide Characters & Encodings

The standard `char` in C++ is **1 byte** (8 bits).1 This limits it to **256** possible values.

- **The Problem:** This is fine for English (ASCII uses only 127 values), but impossible for languages like Hindi, Japanese, or Chinese, which have thousands of characters.
    

#### 1. `wchar_t` (Wide Character)

To solve this, C++ introduced `wchar_t`.

- **Size:** It is wider than `char`. (Usually 2 bytes on Windows, 4 bytes on Linux/macOS).2
    
- **Usage:** It allows holding larger numbers that map to complex characters.3
    
- **Syntax:** You mark a wide string literal with an **L**.4
    
    C++
    
    ```
    char  c = 'a';      // 1 byte
    wchar_t w = L'あ';  // 2 or 4 bytes (Japanese 'a')
    
    std::string s = "Hello";
    std::wstring ws = L"Hello"; // Wide string class
    ```
    

#### 2. Character Encodings (The Map)

Computer memory only stores numbers. "Encoding" is the map that tells the computer "Number 65 = 'A'".5

- **ASCII:** The old standard. 0-127. Only English.
    
- **Unicode:** The modern standard. It assigns a unique number (Code Point) to every character in human history (including Emojis).6
    
    - **UTF-8 (The King):** Variable length. It uses 1 byte for English (looks like ASCII) but expands to 2, 3, or 4 bytes for other languages. _Most of the web and Linux uses this._
        
    - **UTF-16:** Uses 2 or 4 bytes.7 Used heavily by Windows and Java.
        
    - **UTF-32:** Fixed 4 bytes per character. Simple to calculate length, but wastes a lot of memory.
        

---

### Part 2: L-Values vs. R-Values

This concept explains exactly why your `int* x = 8` code failed earlier. In C++, every expression is either an **L-value** or an **R-value**.

#### 1. L-Value (Locator Value)

Think "L" for **Location** (or Left-side).

- **Definition:** An object that occupies an identifiable location in memory (has an address).
    
- **The Test:** If you can take its address using `&`, it is an L-value.
    
- **Lifespan:** It persists beyond a single expression.
    

C++

```
int a = 10; // 'a' is an L-value.
a = 20;     // Valid: 'a' is on the Left side.
int* p = &a; // Valid: We can take the address of 'a'.
```

#### 2. R-Value (Read Value)

Think "R" for **Right-side** only.

- **Definition:** A temporary value that does not have a persistent memory location. It exists only for a split second during computation.
    
- **The Test:** You usually **cannot** take its address.
    
- **Lifespan:** Dies immediately after the line of code finishes.
    

C++

```
int x = 10;
int y = 20;

// (x + y) evaluates to 30.
// That 30 is a temporary R-value.
int z = x + y; 

// & (x + y); // ERROR: You cannot take the address of a temporary calculation.
// 10 = x;    // ERROR: 10 is an R-value, it can't be on the left.
```

### 3. The "String Literal" Exception

This connects back to your previous question.

- **Integer Literal (`8`):** Pure **R-value**. It has no address. You cannot say `int* p = &8`.
    
- **String Literal (`"jatin"`):** This is a weird exception. It is technically an **L-value**.
    
    - Because string literals are stored in the global read-only memory, they _do_ have a permanent address. That is why `char* p = "jatin"` works (the pointer `p` takes the address of that L-value).
        

### Summary Table

|**Concept**|**L-Value**|**R-Value**|
|---|---|---|
|**Meaning**|Identity / Location|Contents / Temporary|
|**Position**|Left or Right of `=`|Usually Right of `=`|
|**Address (`&`)**|**Allowed**|**Forbidden**|
|**Examples**|Variables (`x`), Array elements (`arr[0]`), String Literals (`"abc"`)|Numbers (`5`), Math results (`x+y`), Function returns (`getVal()`)|

---

