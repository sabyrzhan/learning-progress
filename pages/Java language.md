- ## Java 19
	- ### Pattern matching Preview 3 #[[java pattern matching]]
	  collapsed:: true
		- `&&` operator in case  is replaced with `when`:
		  ```
		  // Before Java 19
		  Object x = 100;
		  switch (x) {
		    case Integer i && i > 10 -> ...
		    default -> ...
		  }
		  
		  // Java 19
		  Object x = 100;
		  switch (x) {
		    case Integer i when i > 10 -> ...
		    default -> ...
		  }
		  ```
		- Support of `null` matching. Note that if `case null` is absent, NPE will be thrown right at `switch` operator.
		  ```
		  // Before Java 19
		  Object x = null;
		  switch (x) {
		    case Integer i -> ... // NPE
		    default -> ...
		  }
		  
		  // Java 19
		  Object x = null;
		  switch (x) {
		    case null -> ...
		    case Object x -> ...
		    default -> ...
		  }
		  ```
	- ### Record Patterns Preview #[[java record patterns]]
	  collapsed:: true
		- Possible to match records with parameters
		  ```
		  var point = createRecord();
		  if (point instanceof Point(int x, int y)) {
		    System.out.println("X: " + x);
		    System.out.println("Y: " + y);
		  }
		  ```
		- Possible to match using named variable
		  ```
		  var point = createRecord();
		  if (point instanceof Point(int x, int y) p) {
		    System.out.println("X: " + p.x);
		    System.out.println("Y: " + p.y);
		  }
		  ```
		- Matching using also another custom type
		  ```
		  var coloredPoint = createColoredRecord();
		  if (coloredPoint instanceof ColoredPoint(Point(var x, var y) point, var color)) {
		    System.out.println("Point: " + point);
		    System.out.println("Color: " + color);
		  }
		  ```
		- Record patterns in Pattern matching
		  ```
		  var simplePoint = createRecord();
		  switch(simplePoint) {
		    case ColoredPoint(Point(var x, var y) point, Color color) when x >= 10 && color == Color.GREEN -> System.out.println("This is the colored point");
		    case Point(var x, var y) when y > 15 -> System.out.println("This is the simple point with y = " + y);
		    default -> System.out.println("Unknown point");
		  }
		  ```
	- ### Virtual Threads Preview #[[java virtual threads]]
		- VTs are themselves instances of `java.lang.Thread` class
		- Previously it was called `Fiber`
		- Thread on which VTs are running is called carrier thread
		- Platform threads rely to OS scheduler for scheduling, whereas VTs rely on `ForkJoinPool`
		- If VT is processing blocking operation, then carrier thread unmounts it to process other VT, which scales concurrency very well
		- However in 2 cases the VT cannot be unmounted: when `synchronized` and JNI codes are processed. So, if you want high scalability with VT, better to avoid using `synchronized` and JNI blocks
		- To create new VT you can use following APIs: #[[java virtual thread api]]
			- `Thread.Builder` - as its name says.  To create new VT you can you following factory method. For example, `Thread.ofVirtual().name("name").unstarted(runnable)`
			- `Thread.startVirtualThread(Runnable)` - created and immediately runs VT
			- `Thread.isVirtual()` - checks whether thread is virtual
			- `Executors.newVirtualThreadPerTaskExecutor()` - create new executor service that creates VT per new task.
		- JVM TI and Java Flight Recorder can be used to debug VTs in applications #[[java virtual threads debug]]
		- You must keep in mind that virtual threads are IO-bound rather than CPU-bound. It scales very well when working with blocking tasks such disk, network requests.
		- Usage of executors or schedulers for threads is antipattern when creating virtual threads. Sure, anyway we have to use factory executor like `Executors.newVirtualThreadPerTaskExecutor`, but using cached or 1-thread per task pools will be overkill. #[[virtual thread executor]]
		- Virtual threads are very lightweight and memory footprint for starts out at only a few hundred bytes, and is expanded and shrunk automatically as the call stack expands and shrinks.
		- Virtual threads are an alternative implementation of  `java.lang.Thread` which store their stack frames in Javas garbage-collected heap rather than in monolithic blocks of memory allocated by the operating system. #[[virtual thread memory]]
		- Virtual threads are garbage collected, meaning if the virtual thread is not bound to any carrier thread or is out of sight from any scheduler, then it killed and removed from heap. #[[java gc]]
		- They are better suited for short-lived blocking tasks. Because of its nature how it works with memory and its memory footprint, you can create almost unlimited amount of it. For example, 1 million virtual threads consume only 1G of RAM memory. #[[virtual thread memory]]
		- When VT is in waiting state and unmounted, it is paged to heap memory. This means the OS only cares about consuming memory of the carrier thread, but everything else related to VT is handles totally by JVM. #jvm #[[virtual thread memory]]