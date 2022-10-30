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