# Concurrency
## Threads vs Processes
* threads are seperated concurrent tasks run in a process also called LWP (light-weight processes)
* multiple threads can be created in a process
* threads share memory in a process, whereas processes mostly dont share memory (depends on hardware or OS)
* threads have their own state, stack and registers
* threads in application are abstractions provided by OS
* there are different kinds of threads:
    1. Hardware threads
        a. independent execution streams maintained by hardware
        b. used in SMT (hyper-threaded) processor cores
        c. one SMT core can execute several hardware threads
    2. Kernel (system) threads
        a. LWP managed by OS
        b. they do system and user tasks
        c. does not relate to access privileges, since program instructions are performed in any of these threads
* User vs Application threads
    1. User threads - abstraction provided by the OS
    2. Application threads (fibers) - abstractions provided by the application/process

