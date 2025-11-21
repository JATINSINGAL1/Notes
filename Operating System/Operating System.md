Define : It is an interface to interact with hardware. 
- #####  abstraction 
	hides hardware details and make it simple to interact with wide range of hardware my hiding it's complexity.
- ##### multiplexing 
	multitasking, keep track of multiple processes and ensures the optimum usage of hardware by sharing resources.
#### OS as Virtual Machine 
the primary way the OS does this is through a general technique that we call virtualization. That is, the OS takes a physical resource (such as the processor, or memory, or a disk) and transforms it into a more general, powerful, and easy-to-use virtual form of itself. Thus, we sometimes refer to the operating system as a virtual machine.
##### Os provides APIs we can call :
- System Calls : Application makes interact with hardware through os using these ==system calls==. Using these software create new process, run program, access memory and devices. 
- Interrupt : OS can also alert applications about change in hardware (I/O response , network packet received ) using ==interrupt==.

Policies and Mechanisms (improve this).
- policies is the set of rules based on which os take decisions for different mechanisms. 
### Virtualizing Memory 
In an example we ran two different processes to our surprise OS provided the same memory address to each process (should not have happened). Instead actually each process has it's own ==private virtual address== space (sometimes just called its ==address space==), which the OS somehow maps onto the physical memory of the machine.[[More to Read]][1]
### Concurrency 
as we can observe there are multiple processes inside a single program that an Os has to work on with juggling between multi programs surely this leads to deep and interesting problems. 
###### Multi Threaded Programs 
running a process on multiple threads 

the result of every process depends on how the instructions are executed which is one at a time , in our example where we were running a loop for a long number, three instructions: one to load the value of the counter from memory into a register, one to increment it, and one to store it back into memory. Because these three instructions do not execute ==atomically== (all at once), strange things can happen.
#### Persistence 
file system is the software that manages the storing of data for long period of time using SSD and Hard dive. 

For performance reasons, most file systems first delay such writes for a while, hoping to batch them into larger groups. To handle the problems of system crashes during writes, most file systems incorporate some kind of intricate write protocol, such as ==journaling or copy-on-write==, carefully ordering writes to disk to ensure that if a failure occurs during the write sequence, the system can recover to reasonable state afterwards. To make different common operations efficient, file systems employ many different data structures and access methods, from simple lists to complex b-trees.



# Read Again 
The key difference between a system call and a procedure call is that a system call transfers control (i.e., jumps) into the OS while simultaneously raising the hardware privilege level. User applications run in what is referred to as user mode which means the hardware restricts what applications can do; for example, an application running in ==user mode== can’t typically initiate an I/O request to the disk, access any physical memory
page, or send a packet on the network. When a system call is initiated (usually through a special hardware instruction called a **trap**), the hardware transfers control to a pre-specified trap handler (that the OS set up previously) and simultaneously raises the privilege level to kernel mode. In kernel mode, the OS has full access to the hardware of the system and thus can do things like initiate an I/O request or make more memory
available to a program. When the OS is done servicing the request, it passes control back to the user via a special return-from-trap instruction, which reverts to user mode while simultaneously passing control back to where the application left off.