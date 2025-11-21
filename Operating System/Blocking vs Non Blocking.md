
To master this, you must separate **Control Flow** (Sync/Async) from **OS Behavior** (Blocking/Non-Blocking).

#### The Axes
1. **Synchronous/Asynchronous (Control Flow):**
    - **Sync:** The caller waits for the callee to finish before moving to the next line of code.
    - **Async:** The caller moves to the next line of code immediately. The result comes later (via callback, promise, or event).
2. **Blocking/Non-Blocking (OS/Kernel State):**
    - **Blocking:** The thread is put to sleep by the OS. It consumes no CPU while waiting.
    - **Non-Blocking:** The function returns immediately, usually with a status like "Data not ready." The thread keeps running.

| **Combination**                                             | **Technical Behavior**                                                                                             | **The "Food" Analogy**                                                                                                                                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Sync Blocking**<br><br>  <br><br>_(Classic)_           | You call a function. Your thread sleeps until the data arrives. You do nothing else.                               | **The Standard Queue:** You stand at the counter. You do not move, check your phone, or talk to anyone until the cashier hands you the burger.                                                |
| **2. Sync Non-Blocking**<br><br>  <br><br>_(Busy Wait)_     | You call a function. It returns immediately ("Not ready"). You loop and check again. You burn CPU cycles checking. | **The Impatient Customer:** You order. You stand at the counter asking "Is it ready?" every 3 seconds. You aren't doing anything else (like sitting down), you are just busy checking.        |
| **3. Async Non-Blocking**<br><br>  <br><br>_(Modern Async)_ | You call a function. It returns a "Promise." You go do other work. You are notified (callback) when done.          | **The Buzzer:** You order. They give you a buzzer. You go sit down, read a book, or talk to friends. When the buzzer vibrates (callback), you go get the food.                                |
| **4. Async Blocking**<br>_(Anti-Pattern)_                   | You trigger an async task, but immediately pause execution to wait for the result.                                 | **Ordering Online & Staring:** You order on an App (Async mechanism), but then you stand by the door staring at the street (Blocking) until the driver arrives, refusing to do anything else. |


#### Async Blocking (The "Await" Model)

This sounds contradictory, but it is very common in modern programming (e.g., `await` in JavaScript/Python).

- It uses an **Async** mechanism (Promises/Futures) under the hood.
    
- But the code _looks_ **Blocking** because you pause that specific function until the data arrives.
    
- _Note:_ In strictly technical terms, `await` is often Non-Blocking for the _thread_ (the thread goes to handle other requests), but Blocking for the _function scope_ (this specific function stops here). 
---


Blocking : when execution begins if it require response from another process it get blocked and doesn't perform the next task until the response is received. 

Non Blocking : On the other hand when when the process require response from the other process it doesn't wait, if the response is not received ; it just keeps on checking (does i get response ) this way 

## Synchronous and Asynchronous

Sync : The task need to be done in sequence like second will be done only when first is completed. 

Sync and Blocking appear to be same : but they are different sync just talk about following the task in the sequence. 
That's why we have : 
Sync Blocking : the primitive sync that we know next task will be executed only when previous is done. 

Sync Non-Blocking : the request for previous is made , our execution thread doesn't wait for the completion of the process it ask through polling or check if response is received. Till then next task start to execute . 
**Sync Non-Blocking:** You keep calling the restaurant (polling) regularly to ask if your food is ready while doing other things in between, processing in order but not being idle or stuck.

Async : The task doesn't require to be done in sequence. The execution thread can shift to another task without completing the previous task. 
This time when the previous task is completed we get notified through call back function 

Async blocking: But when the task get stuck due to blocking that's response is required from the process.
**Async Blocking:** You order food online (async task), but you wait by the door (blocking) until the delivery arrives before doing anything else.

Async Non-blocking: The primitive async we know the call back function provide us the response for the async function 