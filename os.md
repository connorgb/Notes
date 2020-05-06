Text Notes

CH 2 Intro
A typical OS exports a few hundred system calls that are available to applications: a standard library.
An OS is a resource manager.
CH 4 Processes
A process is a running program; instructions and maybe running data.
How to provide the illusion of many CPUs?
Virtualisation: running one process then stopping it and running another.
Time sharing
Requires low-level mechanisms and higher-level OS policies, like a scheduling policy.
Mechanisms are the 'how', policies are the 'which', as in: which process should run?
A machine state is what a program can read or update.
The address space is part of the process.
Also includes registers.
Program counter (instruction pointer) which says which instruction to execute next, stack pointer and frame pointer, which manage the stack for function parameters, local variables, and return addresses, are examples.
Process API
Create
Destroy
Wait
Misc Control
Status
How does the OS get a program running?
First load the code into memory from storage.
Early OSs did this eagerly ie. all at once, it is now done lazily ie. as needed (paging and swapping).
Allocate memory for the program's run time stack.
C uses stack for local vars, parameters and return addresses.
Initialize the stack with arguments (main(): argc and argv array)
Allocate memory for the heap.
C uses the heap for explicitly requested dynamically-allocated data; programs request space by calling malloc() and free it with free().
Needed for data structures (linked list, hash tables, trees etc.)
OS may expand heap as needed.
Other initialisation tasks (file descriptors).
Start process on main(), transferring control to the program.
Process states
Running
Executing instructions.
Ready
Not running for some reason.
Blocked
Not ready to run until some other event takes place.
Moving from ready to running (and back) is scheduling/descheduling.
Data structures in the OS
Process list
Also handle blocked processes.
Register list
Hold contents for stopped registers (context switch).
// the different states a process can be in

enum proc_state { UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE };

// the information xv6 tracks about each process

// including its register context and state

struct proc {

char *mem; // Start of process memory

uint sz; // Size of process memory

char *kstack; // Bottom of kernel stack for thisprocess

enum proc_state state; // Process state

int pid; // Process ID

struct proc *parent; // Parent process

void *chan; // If !zero, sleeping on chan

int killed; // If !zero, has been killed

struct file *ofile[NOFILE]; // Open files

struct inode *cwd; // Current directory

struct context context; // Switch here to run process

struct trapframe *tf; // Trap frame for the current

};

CH 5 Process API
fork() system call
CH 6 Direct Execution
Time sharing without affecting performance while still retaining control
Limited direct execution
Run process directly on CPU.
What happens with restricted operations like IO or accessing more memory?
Requires a new process mode: user mode, which is restricted compared to kernel mode.
System calls allow the kernel to expose key functionality to the user program, like file access, creating/destroying processes and talking with other processes, allocating memory.
Trap and return-from-trap instructions allow programs to 'trap' into kernel, and location of trap table.
Trap table contains trap handlers which tell hardware locations of kernel-level events (keyboard or storage interrupts, IO etc.)
Hardware must save enough of the caller's registers (program counter, flags)
Push into a per-process kernel stack
Limited Direct Execution Protocol

OS in boot (kernel mode)	Hardware	Program (user mode)
Initialize trap table		
| | | | Remember address of syscall handler | | | OS in run (kernel mode) | | | | Create entry for process list Allocate memory for program Load program into memory Setup user stack into argv Fill kernel stack with reg/PC Return-from-trap | | | | | Restore regs (from kernel stack) Move to user mode Jump to main | | | | | Run main() … Call system call Trap into OS | | | Save regs (to kernel stack) Move to kernel mode Jump to trap handler | | | Handle trap Do work of syscall return-from-trap | | | | | Restore regs (from kernel stack) Move to kernel mode Jump to PC after trap | | | | | ... Return from main Trap (via exit()) | | Free memory of process Remove from process list | | |

A system-call number is assigned to a system call and saved to a user mode register
Write() system calls specify address of a buffer. If a user passes an address to kernel space (a 'bad' address), the OS has to reject it.
Applications transfer control to OS via system calls or traps from illegal inputs
OS takes control from application via timer interrupt [M+63]
Passes to interrupt handler
OS must inform hardware of which code to run when the timer interrupt occurs
Timer can be turned off (concurrency)
LDE Protocol (Timer Interrupt)

OS in boot (kernel mode)	Hardware	Program (user mode)
Initialize trap table		
| | | | Remember addresses of syscall handler, timer handler | | | Start interrupt timer | | | | | Start timer Interrupt CPU in x ms | | | OS in run (kernel mode) | | | | | | Process A ... | | | Timer interrupt Save regs(A) -&gt; k-stack(A) Move to kernel mode Jump to trap handler | | | Handle the trap Call switch() routine Save regs(A) -&gt; proc_t(A) Restore regs(B) &lt;- proc_t(B) Switch to k-stack(B)Return-from-trap (into B) | | | | | Restore regs(B) &lt;- k-stack(B) Move to user mode Jump to B's PC | | | | | Process B … |

OS can also disable interrupts while one is being handled
Locking schemes prevent concurrent access to internal data structures
The OS can effectively baby-proof the CPU
CH 7 Scheduling Intro
Turnaround time is a scheduling metric
Completion time - arrival time
A performance metric
Jain's Fairness Index
First in First out method
Small jobs wait for large ones
Shortest Job First method
If we no longer assume jobs come at once, incoming small jobs get stuck behind current large job
Shortest Time-to-Completion method
No longer assume jobs run to completion, a scheduler [CK68] can determine which job would end first
Response time metric
Firstrun time - arrival time
Round Robin method
Runs a job for a time-slice (must be a multiple of the timer-interrupt period)
Enlarge the time slice to amortize cost of context-switching
Good response time, bad turnaround time
Overlap allows other jobs to run while first job does IO
CH 8 Scheduling Multi-Level Feedback Queue
[+62]
Assigns priority tiers to jobs
Equal priority jobs run RR
Jobs are given highest priority as they arrive
Priority goes down if entire time slice is used up
Priority stays if job gives up CPU before time
Since IO requires CPU, IO jobs stay in priority
Priority still goes down when total time slice is used up
If too many jobs, long running jobs get no CPU time
Vulnerable to 'gaming the scheduler'
We have to periodically move all jobs to top priority
High priority queues given short time slice
FreeBSD scheduler [LM+89] bases priority on CPU use, which generally decays over time, providing the priority boost (Epema)
CH 9 Scheduling Proportional Share
Each job guaranteed a portion of CPU time. Lottery schedule. (p1)
Percent of tickets a process has represents its share of the system resource.
Choosing lottery winner can be random or deterministic with stride scheduling. (p6)
Stride = number of tickets / some large number
Pass value is incremented by stride value each time process is run. (p7)
Has global state: New processes are given pass values of 0 and will monopolize the CPU.
Lottery scheduling has no global state, it simply adds a new process with allotted tickets and updates the single global variable to track total tickets.
Schedulers take up minimum of 5% CPU time.
Linux's Completely Fair Scheduler [J09]
Virtual runtime (vruntime) (p8).
Sched_latency: divide by number of processes
Min_granularity: prevents timeslices being divided to small (p9)
CFS is based on a periodic timer.
Niceness table: high priority given low niceness (-20 to 15)
Time_slice_k = (weight_k / weight_i) * sched_latency
Vruntime_i = vruntime_i + (weight_0 / weight_i) * runtime_i (p10)
Niceness table conserves CPU proportionality ratios when difference in values is constant
Red-Black trees
Unlike binary trees, balanced trees maintain low depths and log time
CFS alters the runtime of sleeping programs, setting them to the minimum value of the tree (p11)
How do we know how many tickets the browser should be allocated?
CH 10 Multiprocessor Scheduling
CPUs have hardware caches to help them run faster (p2)
Caches hold popular data for later
Temporal locality and spatial locality (p3)
Temporal: when data is accessed, it is likely to be used again soon
Spatial: when data location is accessed, nearby data is likely to be used soon
Cache coherence between multiple CPUs (p4)
Bus snooping: CPUs watch data transfers between cache and other processors
Mutual exclusion primitives (locks)
Pthread_mutex_tm, lock(&m) and unlock(&m) (p5)
Cache affinity: Keeping processes on the same processor
Single Queue Scheduling (p6)
Simple
Lots of locking on multiple processors
Cache affinity
Multi Queue Scheduling (p7)
More scalable
Load imbalance: uneven load portioning (ex: between 3 jobs on 2 schedules) (p8)
Migration (p9)
How to efficiently move processes between queues?
Work stealing: queues low on jobs can 'peek' at other queues (p10)
Has its own overhead
3 major schedulers
O(1)
Multi-queue
Priority-based
Completely Fair Scheduler
Multi-queue
Deterministic, proportional-share
BF Scheduler
Single queue
Proportional-share (earliest eligible virtual deadline)
CH 13 Address Spaces
Giving one process all memory, then stopping it and doing the same for another is expensive (p2)
Allocating memory space for sharing requires protection so processes cannot read another process' memory
Address space is used to map memory (p3)
Top of address space is program memory, then heap below it (malloc, dynamic data structures), stack on bottom (local variables, arguments, return values) (p4)
The OS needs transparency when it virtualizes memory for multiple processes: virtual memory should seem identical to physical memory (p5)
CH 14 Memory API
2 types of memory
Stack or 'automatic' memory
Alloc and dealloc are managed implicitly, upon variable declaration
Heap memory
Alloc and dealloc are explicit (p2)
int *x = (int *) malloc(sizeof(int));

- malloc() requests space on the heap, an address is returned which is stored on the stack
Malloc()
sizeof() is a compile time operator rather than a function call since the size of the int is determined and allocated at compile time (p3)
Strings require malloc(strlen(s) + 1) to make room for the eos char
Malloc returns a pointer to type void (p4)
The casting helps the compiler
Free()
Takes a pointer returned by malloc
The size of the allocated region must be tracked by the memory allocation library
Forgetting to allocate memory
char *src = "hello";

char *dst; // unallocated

strcpy(dst, src); // segfault!

Instead: (p5)

char *src = "hello";

char *dst = (char *) malloc(strlen(src) + 1;

strcpy(dst, src); // this works!

Not allocating enough memory
char *src = "hello";

char *dst = (char *) malloc(strlen(src)); // too small!

strcpy(dst, src); // may still work

Even though this runs correctly once, it may still overwrite some other variable some other time
A buffer overflow
Forgetting to initialize allocated memory (p6)
Call malloc without filling in values for your new data type
Will encounter an 'uninitialized read', where it reads from the heap some data of unknown value
Forgetting to free memory
Memory leak
If a chunk of memory is still referenced, even garbage-collection would not help
Memory management is done both at the process level and the OS level
Even if you don't call free(), when a process exits the OS reclaims that heap memory
Freeing memory before you are done with it
Dangling pointer
Can overwrite valid memory (free then call malloc on the freed space)
Free memory repeatedly
Double free
Call free incorrectly (p7)
Free expects you only to pass pointers received from malloc
Purify and valgrind are tools for helping you locate memory issues
malloc() and free() are library calls, not system calls (p8)
Malloc manages virtual address space, and is built on system calls that call the OS for more memory
Brk changes the location of the end of the heap
Do not call this!
nmap() creates an anonymous memory region
swap space; treated like a heap
Other calls
calloc() allocates memory and zeroes it, in case you forget to initialize memory space
realloc() makes a new larger portion of memory, copies the old region into it, and returns a new pointer
CH 15 Address Translation
How do we efficiently and flexibly virtualize memory?
Maintain control over which memory locations an application can access?
Hardware changes each memory access from a virtual address into a physical address (p2)
The OS has to manage memory
By virtualizing, the hardware will interpose on each memory access (p3)
Dynamic reallocation (base and bounds) (p5)
Two hardware registers in the CPU
The OS sets the base value for the register for each process (p6)
Physical address = virtual address + base
This happens at runtime (p7)
Processor checks if the memory reference is within bounds
Happens on the memory management unit (MMU)
Modifying the base and bounds registers is privileged obviously (p8)
Hardware requirements: (p9)
Privileged mode
base/bounds registers
Translate virtual addresses and check bounds
Privileged update instructions
Privileged registration of exception handler instructions
Ability to raise exceptions
OS requirements (p10)
Memory management
Allocate, reclaim, free list
base/bounds management
Exception handling
Limited Direct Execution (Dynamic Relocation) @ Boot (p11)

OS @ boot (kernel mode)	Hardware	(No Program Yet)
Initialize trap table		
| | | | Remember addresses of… System call handler Timer handler Illegal mem-access handler Illegal instruction handler | | | Start interrupt timer | | | | | Start timer; interrupt after X ms | | | Initialize process table** Initialize free list** | | |

Limited Direct Execution (Dynamic Relocation) @ Runtime (p13)

OS @ run (kernel mode)	Hardware	Program (user mode)
To start process A: Allocate entry in process table Allocate memory for process Set base/bound registers Return-from-trap (into A)		
| | | | Restore registers of A Move to user mode Jump to A's (initial) PC | | | | | Process A runs Fetch instruction | | | Translate virtual address Perform fetch | | | | | Execute instruction | | | If explicit load/store: Then ensure address is legal Translate virtual address Perform load/store | | | | | (A runs…) | | | Timer interrupt Move to kernel mode Jump to handler | | | Handle timer Decide: stop A, run B Call switch() routine Save regs(A) to proc-struct(A) (including base/bounds) Return-from-trap (into B) | | | | | Restore registers of B Move to user mode Jump to B's PC | | | | | Process B runs Execute bad load | | | Load is out of bounds; move to kernel mode Jump to trap handler | | | Handle the trap Decide to kill process B Deallocate B's memory Free B's entry in process table | | |

Internal fragmentation in relocated processes
When the process stack and heap are smaller than allocated memory
Address this using segmentation
CH 16 Segmentation
