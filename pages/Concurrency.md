- ## Threads vs Processes
	- threads are separated concurrent tasks run in a process also called LWP (light-weight processes)
	- multiple threads can be created in a process
	- threads share memory in a process, whereas processes mostly dont share memory (depends on hardware or OS)
	- threads have their own state, stack and registers
	- threads in application are abstractions provided by OS
	- there are different kinds of threads:
		- Hardware threads
			- independent execution streams maintained by hardware
			- used in SMT (hyper-threaded) processor cores
			- one SMT core can execute several hardware threads
		- Kernel (system) threads
			- LWP managed by OS
			- they do system and user tasks
			- does not relate to access privileges, since program instructions are performed in any of these threads
			- User vs Kernel threads
				- Kernel threads - is "what really happens" - independent instructions scheduled and executed by OS
				- User threads - abstraction provided by the OS. They are executed by one or more kernel threads. There is also application threads (fibers) - abstractions provided by the application/process - is the subtype of user threads.
			- There are different threading models:
				- N:M - N user threads map to M kernel threads, also called hyper-threading model. Hard to achieve working model.
				- N:1 - N user threads map to 1 kernel thread
				- 1:1 - 1 user thread map to 1 kernel thread (e.g., POSIX threads)
- ## Memory model
  collapsed:: true
	- Concurrency needs some guarantess from compilers. It is hard or impossible to write multithreaded application without compiler support. For instance, if no support compiler can optimize regions of the code which should be thread safe by hoisting common subexpression (also called "Loop invariant code motion")
	- Memory model describes the interaction of threads through memory. It includes following constraints:
		- Memory ordering: any guarantess for relative order of memory reads and writes? For example can READ and WRITE operations be ordered?
			- on X86 - WR can be reordered (RR, WW, RW are ordered)
			- on ARM - on the operations can be reordered
			- Synchronization events: at which point memory access is synchronized?
			- Consistency model: what memory state is guaranteed to be seen the same by multiple threads?
		- Memory model is the property of the entire system
			- Hardware has memory model
			- Languages also define memory model
			- Compiler may provide additional guarantees
			- Compilers avoid transformation or optimization that could violate language memory model
			- Compulers must use hardware features to guarantee the memory model defined by the language
- ## Data synchronization
  collapsed:: true
	- data sharing costs more on large systems (many CPUs)
	- group shared variables accessed together
	- mutexes are easy to use but have high overhead
	- spinlocks have low overhead if waiting time is short
	- spinlocks must use nanoseconds or another pause
	- spinlocks can use existing shared data as access tokens
	- atomic operations can sometimes replace locks
- ## Atomicity vs Locks
  collapsed:: true
	- **Locks/Unlocks** are used to avoid data race conditions in a multithreaded app thus protecting shared data from other threads to access in unsafe manner. However it is a slow process and degrades performance.
	- **Atomicity** is light version of lock/unlock and used to perform operations (often algebraic operations) in one transaction. In contrast to locks/unlocks, atomic operations are provided by hardware.
	- **False data sharing** occurs because of cache line size. Speed of accessing/mutating one data adjacent to another data (e.g, array elements) might not differ because of cache line size.
	- **SpinLock**
	  SpinLocks use pauses to hint the CPU to give more resources to other threads by using pauses. Pauses can be implemented in various ways, for example, 1 nanosecond sleep, by sending "PAUSE" or "no-op" instructions to CPU. The pause should not take very long time but be very quick.
- ## Cache line size
	- When CPU is instructed to read data from memory, it does not actually load only requested data, but array of data with size of **cache line size**. For example, if cache size (depends on CPU architecture) is 16 bytes and CPU is instructed to load 8 byte data, CPU in this case loads whole 16 bytes of data which also contains 8 byte data. This means that 2 independent shared variables on the same cache line behave as if they were shared.