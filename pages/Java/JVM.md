## [[GC - garbage collection]]
	- JVM starts 2 threads for garbage collection:
		- 1. Search Thread - for searching garbages - one per JVM
		- 2. GC Thread - for cleaning garbages - can be many per JVM
- ## [[Search Thread]]
	- There are 2 algorithms used by JVM used to search the dead (garbage) objects:
		- **Reference counter (old)** - this algorithm was initially used to search garbage. It is simple to implement and understand, but it does not solve object's circular references. Every time object is used by another object, object's reference is increased or decreased by 1. If the counter is equal to 0, then the object is considered as garbage.
		- **Rechability analysis (currently used)** - currently used algorithm by JVM. The algorithm traces the objects by chain of references starting from "root" objects. The "root" is GC root here and the path from root to certain object is called "reference chain". If the object is not reachable from any "root", then the object will be considered as garbage.
- ## [[HotSpot JVM Heap Structure]]
	- Heap memory - is the place of the memory where all Java objects reside. It consists from following regions:
		- **Young generation** - stores small size and newly created objects. It consists from eden and S0/S1 survivor spaces.
			- **Eden** - objects are moved first here
			- **S0** - then here
			- **S1** - then here
		- **Old generation (aka Tenured)** - stores big size objects existing for long time. After surviving young generation objects are moved here. Big objects immediately passed to this region bypassing young generation.
		- **Metaspace (aka PermGen in old Java)** - used by JVM for internal purposes
- ## [[GC algorithms]]
	- **Copy collector** - moves objects from one region to another, for example `Eden -> S0 -> S1 -> OldGen` and frees old space for new objects
	- **Mark Sweep** - similar to copy collector, but it also can not to move objects to another region and mark old spaces for new objects. So old objects will remain on their own address and new objects will reside on old object addresses.
	- **Mark Sweep Compact** - does the same as mark sweep. But additionally it defragments memory space for better object allocation.
- ## [[Garbage collectors]]
	- **Scavenge/Serial GC** (old) - does very simple operations: uses copy collector algorithm on young generation space (fast operation) and uses mark sweep compact on old generation space (slow operation).
	- **Parallel GC** (old but can used as default on client JVMs) - does the same operation as Scavange GC, but using multiple threads. Threads can be defined by flag or use the same amount as CPU which is used by default.
	- **Concurrent Mark Sweep (CMS)** (old) - it uses mulitple steps to search garbage and then delete the gargbage. It uses multiple JVM pauses to search for objects and mark them and then concurrently removes garbages at real time.
	- **Garbage First (G1)** - it divides memory regions into multiple subregions so multiple GCs can clean each region faster and independently. It does multiple stop-the-worlds for marking regions and concurrently searches the garbage. And only after that cleanes the garbage. Usecase: in systems with large memory but small objects.
- ## [[JFR - Java Flight Recorder]]
	- Used to analyze the GC statistics and heap usage. For example, GC runs, heap size before and after GC run, VM options and other statistics. What it actually does is: writes the statistics to file or memory while application is runnings for specified time. After the time is elapsed, it will write to `.jfr` file to specified path. To start the recording you have to pass following VM option when running the application:
	  * `-XX:StartFlightRecording=disk=false,filename=rec.jfr` - to write the file to memory
	  * `-XX:StartFlightRecording=filename=rec.jfr` - to save the file to disk
	  
	  After generating the file you can read it using one of the profilers or mission control center, for example Azul Mission Control.
- ## [[JVM Flags]]
	- ### `OutOfMemory` flags
		- `-XX:HeapDumpPath=<file_path>.hprof` - heap dump to file
		- `-XX:OnOutOfMemoryError="<command_to_execute>"` - execute shell cmd on OOM
		- `-XX:+UseGCOverheadLimit` - if you are setting heapdump, usage of this option is encauraged, because it increases the chance of dumping the heap memory before shutting down the JVM because of OOM. That is it starts dumping the memory when 98% of time is spent for garbage collection and 2% only is being recovered, which is the moment when `OutOfMemoryException` will be thrown.
	- ### `InitialRAMPercentage`, `MinRAMPercentage`, `MaxRAMPercentage`
		- `-XX:InitialRAMPercentage` is used to compute the initial heap size of your java application. Say suppose you are configuring -XX:InitialRAMPercentage=25 and overall physical memory (or container memory) is 1GB then your java application’s heap size will be ~250MB (i.e., 25% of 1GB).
		- Both `-XX:MaxRAMPercentage` and `-XX:MinRAMPercentage` are used to determine the maximum Java heap size.
			- `-XX:MinRAMPercentage` JVM argument will be used to compute Java heap size only if your overall available memory’s size in the physical server (or in the container) is less than 250MB (approximately). Say suppose you are configuring -XX:MinRAMPercentage=50 and overall physical memory (or container) memory is 100MB, then your java application’s max heap size will be set to 50MB (i.e., 50% of 100MB).
			- `-XX:MaxRAMPercentage` JVM argument will be used to compute Java heap size only if your overall available memory’s size in the physical server (or in container) is more than 250MB(approximately). Say suppose you are configuring -XX:MaxRAMPercentage=75 and overall physical server (or container) memory is 1GB, then your java application’s max heap size will be set to 750MB (i.e., 75% of 1GB).
		- Source: https://blog.gceasy.io/2020/11/05/difference-between-initialrampercentage-minrampercentage-maxrampercentage/
		- ```
		  For example:
		  docker run -m 1GB eclipse-temurin:22-jdk \
		      java -XX:MaxRAMPercentage=25 -XX:MinRAMPercentage=50 \
		      -XshowSettings:vm -version
		  will output 250MB since the container RAM size is greater than 250MB
		  
		  
		  docker run -m 100Mb eclipse-temurin:22-jdk \
		  	java -XX:MaxRAMPercentage=25 -XX:MinRAMPercentage=50 \
		      -XshowSettings:vm -version
		  will output 50MB since the ram size is less than 250MB
		  ```
-