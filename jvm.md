# JVM
## GC - garbage collection
JVM starts 2 threads for garbage collection:
1. Search Thread - for searching garbages - one per JVM
2. GC Thread - for cleaning garbages - can be many per JVM

## Search Thread
There are 2 algorithms used by JVM used to search the dead (garbage) objects:
* **Reference counter (old)** - this algorithm was initially used to search garbage. It is simple to implement and understand, but it does not solve object's circular references. Every time object is used by another object, object's reference is increased or decreased by 1. If the counter is equal to 0, then the object is considered as garbage.
* **Rechability analysis (currently used)** - currently used algorithm by JVM. The algorithm traces the objects by chain of references starting from "root" objects. The "root" is GC root here and the path from root to certain object is called "reference chain". If the object is not reachable from any "root", then the object will be considered as garbage.

## HotSpot JVM Heap Structure
Heap memory - is the place of the memory where all Java objects reside. It consists from following regions:
1. **Young generation** - stores small size and newly created objects. It consists from eden and S0/S1 survivor spaces.
    1. **Eden** - objects are moved first here
    2. **S0** - then here
    3. **S1** - then here
2. **Old generation (aka Tenured)** - stores big size objects existing for long time. After surviving young generation objects are moved here. Big objects immediately passed to this region bypassing young generation.
3. **Metaspace (aka PermGen in old Java)** - used by JVM for internal purposes

## GC algorithms
1. **Copy collector** - moves objects from one region to another, for example `Eden -> S0 -> S1 -> OldGen` and frees old space for new objects
2. **Mark Sweep** - similar to copy collector, but it also can not to move objects to another region and mark old spaces for new objects. So old objects will remain on their own address and new objects will reside on old object addresses.
3. **Mark Sweep Compact** - does the same as mark sweep. But additionally it defragments memory space for better object allocation.

## Garbage collectors
1. **Scavenge/Serial GC** (old) - does very simple operations: uses copy collector algorithm on young generation space (fast operation) and uses mark sweep compact on old generation space (slow operation).
2. **Parallel GC** (old but can used as default on client JVMs) - does the same operation as Scavange GC, but using multiple threads. Threads can be defined by flag or use the same amount as CPU which is used by default.
3. **Concurrent Mark Sweep (CMS)** (old) - it uses mulitple steps to search garbage and then delete the gargbage. It uses multiple JVM pauses to search for objects and mark them and then concurrently removes garbages at real time. 
4. **Garbage First (G1)** - it divides memory regions into multiple subregions so multiple GCs can clean each region faster and independently. It does multiple stop-the-worlds for marking regions and concurrently searches the garbage. And only after that cleanes the garbage. Usecase: in systems with large memory but small objects.

## JFR - Java Flight Recorder
Used to analyze the GC statistics and heap usage. For example, GC runs, heap size before and after GC run, VM options and other statistics. What it actually does is: writes the statistics to file or memory while application is runnings for specified time. After the time is elapsed, it will write to `.jfr` file to specified path. To start the recording you have to pass following VM option when running the application:
* `-XX:StartFlightRecording=disk=false,filename=rec.jfr` - to write the file to memory
* `-XX:StartFlightRecording=filename=rec.jfr` - to save the file to disk

After generating the file you can read it using one of the profilers or mission control center, for example Azul Mission Control.
