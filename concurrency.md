# Concurrency
## Threads vs Processes
* threads are seperated concurrent tasks run in a process also called LWP (light-weight processes)
* multiple threads can be created in a process
* threads share memory in a process, whereas processes mostly dont share memory (depends on hardware or OS)
* threads have their own state, stack and registers
* threads in application are abstractions provided by OS
* OS defines different thread models
