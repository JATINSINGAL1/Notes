Inter Process communication (IPC)
mechanism that allow to Process to communicate 

#### Shared Memory 
common memory is allocated to process where they exchange information by storing certain information in these variables( shared memory). Some process write whereas some process read that info directly. This method is fast but requires syncronization mechanism like(==semaphores==) to avoid conflicts during multiple read/writes. 

#### Message Passing
sending and receiving messages to exchange data. 
can be achieved using methods like Sockets , message queue or pipes. 

for some cases kernel act as intermediate information is send and receive through kernel.

**Simple and safer than shared memory as there is no risk of overwriting shared data , but it incurs more overhead due to kernel involvement.**


## Process Synchronization 
mechanism in operating systems used to manage the execution of multiple processes that access shared resources. 

why process sync needed : 
for data consistency 
to prevent race condition : ensures processes don't access ahared data at the same time. 
to prevent deadlock avoid circular waits 
Fairness 
Mutual exclusion : only one process allowed in the critical section at a time. 

based on syn process can be divided into two : 
independent 
cooperative 

more formal define 
It's the coordination of multiple cooperative processes in a system to ensure controlled access to shared resources. 

few few problems of improper sync : 
==Deadlock== two or more processes get stuck, each waiting for the other to release a resource. 
Loss of Data , Inconsistency 

##### types of process sync 
competitive : if and only if the process compete for the accessibility of shared resources. lack of sync lead to inconsistency of data. 
cooperative : if they get affected by each other i.e. execution of one process affects the other process. Lead to ==deadlock== if not taken care of. Here output for one process is input for one. 

#### conditions that require process sync 
- critical section : a code segment that can be accessed by only one process at a time. It contains shared variables that need to be synchronized to maintain the conisistency of data variables. 
- race condition : occurs in critical section. This happen when the result of process / thread execution in the critical section differs according to the order in which the threads execute.
- preemption : This is important as mainly issues arise when a **process has not finished its job on shared resources and got preempted.** The other process might end up reading inconsistent value. 

## Race conditions // 

prevention techniques: 
	Mutex: 
	Semaphores:
	Monitors:
	Atomic Operations
	Disable Interrupts 
	Proper Scheduling 


Peterson's Algorithm in Process Synchronization
it ensures mutual exclusion between two processses, thus preventing race conditions. 
Uses two shared variables: 
`flag[i] : to shows whether process i wants to enter the critical section 
`turn :indicates whose turn it is to enter if both processes want to access the critical section at the same time. 

## The Algorithm

****For process Pi:****

> `do {`  
> `flag[i] = true; // Pi wants to enter`  
> `turn = j; // Give turn to Pj`
> `while (flag[j] && turn == j); // Wait if Pj also wants to enter`
> `// Critical Section`
> `flag[i] = false; // Pi leaves critical section`
> `// Remainder Section`  
> `} while (true);`

use case theoritically : 
Accessing a shared Printer
Reading and writing to a shared file:
Competing for a shared resources: when competing for limited resources, such as network connections or critical hardware, sol ensures to avoid conflicts. 
==in practice modern system use hardware instructions or higher-level concurrency primitives== 

## Hardware Based Solution 
use special instructions which are fast and efficient making them ideal for systems with advanced hardware support.
- Test and set 
- swap 

#### TAS 
TAS is an atomic instruction that reads a variable’s old value and sets it to true in a single indivisible step.
```
 boolean lock = false; // Shared lock variable
 
 boolean TestAndSet(boolean &target) {  
 boolean rv = target; // Step 1: Read old value  
 target = true; // Step 2: Set lock (mark busy)  
 return rv; // Step 3: Return old value  
 }  
 while (1) {
 
 while (TestAndSet(lock)); // Entry Section → Busy wait until lock is free  
 // ---- Critical Section ----  
 lock = false; // Exit Section → Release lock  
 // ---- Remainder Section ----  
 }
```


#### Swap 
as the name suggest must be something involving swaps-> a key is defined to a process and when key = true means the process want to enters the critical section so a while loop performs a swap making key = false and lock(var) =  true ; 
entering the critical condition 
and after exiting the lock = false ; 
```
> boolean lock = false; // Shared variable
> 
> boolean key; // Local per-process variable
> 
> void swap(boolean &a, boolean &b) {  
> boolean temp = a;  
> a = b  
> b = temp;  
> }
> 
> while (1) {  
> key = true; // Process wants to enter  
> while (key) // Entry Section  
> swap(lock, key); // Keep swapping until lock becomes true & key false  
> // ---- Critical Section ----  
> lock = false; // Exit Section → Release lock  
> }
```

Compare and Swap (enhancement)
CAS automatically compares a variable with an expected value and updates if they only match. 
```
> int lock = 0; // 0 = free, 1 = busy  
>   
> boolean CompareAndSwap(int &target, int expected, int new_val) {  
> int old = target;  
> if (target == expected)  
> target = new_val;  
> return old == expected; // true if swap succeeded  
> }  
>   
> while (1) {  
> while (!CompareAndSwap(lock, 0, 1)); // Try to acquire lock  
>   
> // ---- Critical Section ----  
>   
> lock = 0; // Exit Section → Release lock  
> }
```

- ****Drawbacks****:
    - Both Swap and CAS suffer from ****busy waiting****.
    - No ****bounded waiting guarantee**** → some processes may starve.

Spin Lock 
nothing just higher level abstraction build upon either TAS or CAS


## Semaphores  
###### a system of sending messages by holding the arms or two flags or [poles](https://www.google.com/search?client=firefox-b-d&sa=X&sca_esv=e8c8f12bd7e52d54&biw=1520&bih=800&sxsrf=AE3TifM3M4Lk9Mi8pdldzdvGJQtIBNBoIw:1762244597813&q=poles&si=AMgyJEvCiuN81CuVzBIsHJFq8TP0aSTrlPasptf1ynASNcJBFFJOBYHcpyWeCmp16gzDD6nspDKD_iz3doWBioWvXqG0yqKd-g%3D%3D&expnd=1&ved=2ahUKEwi3-YnaiNiQAxVp1TgGHdYyK4MQyecJegQIIBAc) in certain positions according to an [alphabetic](https://www.google.com/search?client=firefox-b-d&sa=X&sca_esv=e8c8f12bd7e52d54&biw=1520&bih=800&sxsrf=AE3TifM3M4Lk9Mi8pdldzdvGJQtIBNBoIw:1762244597813&q=alphabetic&si=AMgyJEt_i95eqLH3KOj-Ut-VGJJ7P-4jgE6rZpmekWmGuCUI05KGIOSzjQJEXITBv5FvqxQxJ8rBQ1ykSCWlIUcTcWZgEybymRMjyEXZkfqrl37907N_jEM%3D&expnd=1&ved=2ahUKEwi3-YnaiNiQAxVp1TgGHdYyK4MQyecJegQIIBAd) code.

A Semaphore is simply a variable (integer) used to control access to a shared resource by multiple processes in a concurrent system. It ensures that only the allowed number of processes can use a resource at a given time.

two main operations 
- Wait 
- Signal 

#### Types of Semaphores

Semaphores are mainly of two Types:

****1. Counting Semaphore****
- Used when multiple instances of a resource exist.
- The semaphore value can range over an unrestricted domain (0 to N).
- Example: Managing access to a pool of 5 printers.

 ****2. Binary Semaphore****
- Special case of counting semaphore with only two values: 0 and 1.
- Works like a lock: either the resource is free (1) or busy (0).
- Example: Managing access to a single critical section.

semaphore in my words 
consider it as class which contains a variable which will define the if a process get access to a resource or not , with two important functions called wait and signal : 

![[semaphore_workflow.webp]]
let consider an example P1 tries to access the resource while S == 1 , the process will get access and will get into critical section making S=1 now when P2 tries to enter the critical section the wait will stop it from doing that also , when P1 exit using signal it increment making s=1 again empty the resource for P2.

#### counting Semaphore 
useful when multiple identical resources. 
The value of S represent no of available resources. 

****Pseudocode:****

Semaphore structure:
```
struct semaphore{
> 
> int count; // number of available resources  
> queue q; // waiting processes
> 
> };
```


wait() method:
```
void wait(semaphore s) {
> 
> s.count--  
> if( s.count <=0 ){  
> 
> }
> 
> }
```
> 

- If s.count > 0 → resource available, a process can access it.
- If s.count <= 0 → no resource available, processes is added to the queue.

signal() method:
```
void signal(semaphore s) {
> 
> s.count++  
> if( s.count >0 ){  
> // assign the resource process queue
> 
> }
```

source code is simple to understand when you get that process is entering the queue to run latter as resources are not empty

### Limitations of Semaphores

- Priority Inversion: A low-priority process holding a semaphore can block a high-priority one.
- Deadlock: Processes may wait on each other’s semaphores in a cycle, causing indefinite blocking.
- Complex to Manage: The OS must carefully track wait and signal calls; misuse can cause errors.
- Busy Waiting: In basic implementations, processes may keep checking the semaphore value, wasting CPU time.

### Mutex
Mutual Exclusion Object. Used to provide UE to specific part of code that the process can execute and work with a particular section of the code at a particular time.




## Monitors 
mechanism that simplify process and thread synchronization. 
They are build on locks and mostly used in multithreading systems like java. 

monitors combine shared data and the operations on that data inside a single structure, making synchronization safer and easier to manage.

only one thread can execute inside a monitor at a time, ensuring automatic mutual exclusion. 

##### Monitors are implemented at the programming language level, not directly at Os. 

 There are three main condition variables:

- ****wait()****: temporarily releases the monitor lock and puts the thread to sleep until it is signaled.
- ****signal()****: wakes up one waiting thread (if any).
- ****broadcast()**** (in some languages): wakes up all waiting threads.

```
class AccountUpdate {
    private int bal; // shared resource accessed only by one thread at a time.

    void synchronized deposit(int n) {
        bal = bal + n;
    }

    void synchronized withdraw(int n) {
        bal = bal - n;
    }
}
Use of synchronized – Makes the methods act like monitor procedures, guaranteeing mutual exclusion.
```

Limitations : 
mainly it is quite language , compiler depended. 
we can't add it like an external library. 

### Priority Inversion 
A scheduling Problem where a low-priority task holds a resource required by a high-priority task.

Types of priority Inversion 

Bounded PI : the delay is predictable and limited to the time the lower -priority task holds the resource. In this Tasks M are defined and fix time of execution + execution time of Task L turns out to be the waiting time for Task H . 


Unbounded PI : the delay is unpredictable or unlimited amount of time due to repeated preemption by intermediate priority tasks. Without special scheduling protocols, unbounded priority inversion can compromise system reliability and responsiveness.

#### Sol to Priority Inversion
lead to significant delays and system inefficiencies.
- ****Priority Inheritance****: Temporarily elevates the priority of the low-priority task holding the resource to match that of the highest-priority waiting task, ensuring timely resource release .
- ****Priority Ceiling Protocol****: Assigns a maximum priority to each resource, preventing tasks with lower priorities from acquiring resources needed by higher-priority tasks .
- ****Avoiding Blocking****: Utilizes non-blocking algorithms or designs systems to minimize shared resource usage, thereby reducing the chances of priority inversion .


## Classical IPC Problems 
https://www.geeksforgeeks.org/operating-systems/classical-ipc-problems/
// crazy learning from this article as you tackle real life problems here ... and implement things directly related to OS    Readers Preference – give readers priority, making writers wait.
    Writers Preference – give writers priority, ensuring timely updates.
Producer-Consumer Problem:
buffer issue : 
buffer overflow : producer tries to add when the buffer is already full , 
vice versa in Buffer Underflow : consumer tries to remove when the buffer is empty . 

solution use : semaphores or mutexes 
Read More https://www.geeksforgeeks.org/operating-systems/producer-consumer-problem-using-semaphores-set-1/


Readers-Writers Problem : 
challenges : Allow many readers to access simultaneously 
ensures that only one writer writes at a time. 
Prevent readers from reading while a write is writing. 

 ##### Solution 
 - ****Readers Preference**** – give readers priority, making writers wait.
- ****Writers Preference**** – give writers priority, ensuring timely updates.

Dining Philosophers Problem: 
The problem models philosophers seated around a table, each needing two chopsticks to eat. Chopsticks are shared between neighours creating potential conflicts. 

Use of semaphores or monitors to coordinate 

Sleeping Barber Problem 
challenges : 
prevent deadlock where no one gets served. 
ensures fairness so no customer starves waiting too long. 