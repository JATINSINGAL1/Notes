a process is a program in execution, a single program when run multiple times create multiple process to run. 

how process look inside memory , it is divided into multiple sections 
stack temporary data like runtime variables , return addresses 
heap dynamic memory is allocated to the process 
data global variable 
text initial instruction is here read only file

Attribute of process 
structure storing these attributes of process is called PCB -> Process control block 
attributes include : 
PID : unique id assign to a process 
state of process : running , Halt , stopped different states a process can be 
Priority and other process scheduling information : The information which tells the OS about the priority of the process and all .
I/O information , information about I/O devices the process is connected 
file descriptor the information of the file and network port the process is interacting 
Accounting information : The information about the time process has run 
memory management information : information about the resources used up by the memory , memory address allocated to the process. Memory layout (heap , stack)


During a state transistion a os updates the process table which is just the collection of multiple Pcb 

PCB also have registers, when any process is switched the Cpu registers of the process get stored in these register and vice versa. 

The Process Control Block (PCB) is stored in a special part of memory that normal users can't access. This is because it holds important information about the process. Some operating systems place the PCB at the start of the kernel stack for the process, as this is a safe and secure spot.

there are several roles played by process tables and PCB


Core functionality of Os 
Process Management 
creating , scheduling and coordinating process to make optimal utilization of the cpu 

single task systems : easy to manage only single process run at a time.
Mutiprogramming and Mutlitasking : multi process need to share recourses quite complex to handle . 
resource sharing : Active process might share memory / resources which need to be managed 

Cpu bound vs I/o bound 
the I/o processes are genearlly long and cpu can't sit idle during that state so the cpu is assigned another process.

Process Management Tasks 
creating : PID , seeting up PCB , termination , by os or parent process. termination involves clearing all the allocated resources. 

CPU scheduling 
DeadLock Handling : to ensure that system does not reach a state where the tow or more processes can't proceed due to cyclic dependency on each other. 

Inter Process communication : Os provide feature like shared memory and message passing for cooperating processes to communcate . hmm 
sync of process so that resoureces are shared such that they are accessed in a control manner. 


#### context Switching of process 

loading and unloading process from the running state to the ready state. 
##### TLB
Translation look aside buffer TLB is a cpu cache that memory management hardware uses to improve virtual address translation speed. They have fixed number of slots that contain page table entries which map virtual addresses to physical addresses . on a context switch some tlb entries can becom invalid ==since the virtayal to physical mapping is different==. The simplest way to deal is to flush entire TLB .  

Process switching involves a mode switch as context switching happens only in kernel mode only. 

## States of Process in Os 
during its life time process goes through multiple states. 

2 state model : 
Running 
Not Running : waiting for anything or simply just paused. 
Thing called Dispatcher who checks availability of CPU , 
scheduler role is to decide which process to run next. pick on bais of set of rules 

Five state model 
new : newly created process which is yet to start , not loaded in main memory but its PCB is created .
ready : as soon as CPU is available process will start to execute  
running 
blocked / waiting : can't continue , waiting for an event to happen (i/o task or reading data from the disk)
exit or terminated  : finished or stopped by user , released from os , removed form memory . 


Seven State model 
![[state.webp]]New state : process is about to be created , can say a program present in secondary memory that will be picked by OS to create the process 

ready loaded into main memory . These processes are maintained in a queue called ready queue . to be executed. 

Blocked state : wait in the main memory 

suspended ready : process was ready but swapped out of main memory placed onto external storage by scheduler 

Based on resoures and execution status the process change it's state. 
****Running to Ready:**** When a running process is preempted by the operating system, it moves to the ready state. For example, if a higher-priority process becomes ready, the operating system may preempt the running process and move it to the ready state.

Types of Schedulers 

Long term Scheduler: decides how many process to stay on ready state. this decide the degree of multiiprogramming. once the decision is taken it lasts for a long time which means it runs ingrequently.  Loads a process from disk to main memory for execution. It controls number of process present in ready state or in main memory. 
==In Time sharing systems like Windows , there is no long term scheduler. Instead every new process is directly added to memory for the short term scheduler to handle.== 

Short Term Scheduler : which process is to be executed next and then will call the dispatcher. runs frequently. also called CPU Scheduler. 
Time taken by dispatcher is called dispatch latency or process context switch time.

#### Dispatcher
	module that hands over control of the cpu to the process that has been selected.
	switching context 
	switching to userMode: make sure that process run in user mode. 
	jump to correct location in user program from where the program can be restarted. 
dispatch latency delay that occur during context switching and control transfer. 

Medium Scheduler suspension decision are taken by it . is used for swapping which is moving main memory to secondary and vice versa. 
 ==swapping is done to reduce degree of multiprogramming==

A running process may become suspended if it makes an I/O request. A suspended processes cannot make any progress towards completion. In this condition, to remove the process from memory and make space for other processes, the suspended process is moved to the secondary storage. This process is called swapping and the process is said to be ==swapped out or rolled out==. Swapping may be necessary to improve the process mix (of CPU bound and IO bound)

some other schedulers ; 
I/o schedulers : 
real time schedulers : they ensures critical tasks to complete within a specified time frame, they can priortize and schedule tasks using algo like EDF ( ealiest deadline first)  or rate monotonic . 
## MutliProgramming 
we have many processes ready to run . 2 type fo mp : 

preemption : forcefully removed from CPU , also called timesharing or multitasking . process is switched even before completion. This switching happens because the CPU may give other processes priority and substitute the currently active process for the higher priority process.

example : 
	round robin 
	shortest remaining time first 
	priority scheduling 
widely used in modern os 
better average response time in multi use systems. 
disadvantage : 
Risk of concurrency issues if preempted during shared resources access.


Non-preemption : processes are not removed until they complete the excecution . Once the control is given to CPU for a process execution , till the cpu releases the control itself, control can't be taken back forcibly from the CPU. 
FCFS 
shortest job first 
adv : minimal scheduling burden 
less computatianl resources are used ; 

disadvantage : 
open to ddos attack : like malicious process can take cpu forever . 
no round robin avg response time becomes less . You can't implement round-robin for non-preemptive scheduling because ==round-robin's core principle is to **preempt** a process after a fixed time quantum to give the CPU to the next process==.

## CPU scheduling Algorithms 
minimize the response and waiting time of the process . 

Terminlogy 
Arrival time : the time at which the process arrives the ready queue 
Completion Time : the time at which the process completes its execution 
Burst Time time require by a process for a cpu execution. 
Turn around Time : time differnce completion time and arrival time. 

waiting time = turn around time - Burst time . 

designing of cpu scheduling algo : 

cpu utilizations : keep cpu busy. Theoretically usage can range from 0 to 100 but in real time systems varies from 40 to 90% depending on the system load. 

Throughput : avg cpu performance === defined as no of processes executed in unit time. 
Turn around time : conversion time is time elasped from the time of arrival to completion. 

In operating systems,

**response time** is ==the time elapsed between a user's request or a process's submission and the system's first response==,

Various Algo : 

FCFS 
SJF
SRTF shortest remaining time first sjf with preemptive 
RR 
Priority Preemptive 
Priority non Preemptive 
MLQ Multi Queue 
MFLQ Mutilvevel feedback queue scheduling 


### Starvation and Aging in Os 
#### simple think not getting your turn to execute. 
occurs in priority scheduling when a low priority process keeps waiting cause higher priority tasks keep comming up in cpu . 

this problem arrives in heavily loaded systems where resources are preocuppied by higher priority tasks leaving some process starved. 
To prevent this os use ==aging== a technique that gradually increases the priority of waiting process ensuring fair execution. 
causes : 
not fair scheduling : some how due to random pick a process is always missed. leading to starvation . 
limited resources : 

Aging is a scheduling technique used to prevent starvation by gradually increasing the priority of processes waiting too long in the system. 
ensures fairness as long waiting process eventually get cpu time. 
often combined with scheduling algos to balance short term efficiency with long term fairness. 
For example, if priorities range from 127 (low) to 0 (high), a waiting process can move up one level every 15 minutes, ensuring even the lowest-priority process eventually gets executed.


