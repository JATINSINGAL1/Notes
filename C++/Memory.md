# Stack vs Heap 
**Stack vs. Heap Memory in C++ (Condensed Notes)**

- **Location:** Both Stack and Heap reside in physical **RAM**, but are managed differently.
- **The Stack (Fast & Automatic):**
    - **Allocation:** Involves moving a stack pointer (often just **1 CPU instruction**).
    - **Performance:** Extremely fast; memory is contiguous, leading to fewer CPU cache misses.
    - **Lifetime:** Automatic. Variables are "popped" off and freed instantly when the scope (function/loop) ends.
    - **Size:** Small, predefined limit (e.g., ~2MB).
    
- **The Heap (Slow & Manual):**
    - **Allocation:** Uses `new`/`malloc`. Requires searching a "free list," bookkeeping, and potentially asking the OS for more RAM.
    - **Performance:** Significantly slower due to allocation overhead and memory fragmentation (causing cache misses).
    - **Lifetime:** Manual. Requires explicit `delete` or smart pointers to prevent memory leaks.
    - **Size:** Can grow dynamically; limited by physical RAM.

[must Watch](https://www.youtube.com/watch?v=wJ1L2nSIV1s&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=54)
