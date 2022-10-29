- ## Classes and OOP #classes #oop
  collapsed:: true
	- ### Nested and Inner classes #[[nested and inner classes]]
	  collapsed:: true
		- #### Nested classes
		  Nested classes are static by default. Nested classes cannot access data of outer class.
		  ```
		  class OuterClass {
		    private var name: String = "Mr X"
		    
		    class NestedClass {
		      var description: String = "code inside the inner class"
		      private var id: Int = 101
		      
		      fun foo() {
		        // println("name is $name") // ERROR: cannot access outer class data
		        println("ID is $id")
		      }
		    }
		    
		    fun main() {
		      println(OuterClass.NestedClass().description)
		      OuterClass.NestedClass().foo()
		    }
		  }
		  ```
		- #### Inner classes
		  Inner classes are the same nested classes but with keyword inner. Inner classes cannot be declared inside interfaces and non-inner nested classes. The difference than nested classes are it can access outer class data
		  ```
		  class OuterClass {
		    private var name: String = "Mr X"
		    
		    inner class InnerClass {
		      var description: String = "code inside the inner class"
		      private var id: Int = 101
		      
		      fun foo() {
		        println("name is $name") // can access private member of outer class
		        println("ID is $id")
		      }
		    }
		    
		    fun main() {
		      println(OuterClass().NestedClass().description)
		      OuterClass().NestedClass().foo()
		    }
		  }
		  ```
	- ### Inheritance #inheritance
	  Classes are `final` by default. To make the class extendable use `open` modifier. #[[open class]]
	  Use `override` modifier when overriding method
	  ```
	  //Example. To extend the class must be `open`
	  open class Car(make: String, model: String) {
	  fun drive() { }
	  }
	  
	  class Toyota(make: String, model: String): Car(make, model) {
	  override fun drive() { }
	  }
	  ```
	- #### Safe and Unsafe casting #casting
		- #### Unsafe casting
		  It is done using inflix operator `as`.
		  ```
		  fun main() {
		    val obj: Any? = null
		    val str: String = obj as String
		    println(str) // ERROR: throws NullPointerException because cannot be cast
		    
		    // this will work since source and target are nullables
		    val str2: String? = obj as String?
		    println(str2) // prints `null`
		  }
		  ```
		- #### Safe casting
		  For this one `as?` is used but it expects the result type to be nullable. So if the casting fails it will return `null`.
		  ```
		  fun main() {
		    val obj: Any? = null
		    val str2: String? = obj as? String
		    println(str2) // prints `null`
		  }
		  ```
- ## Collections #collections
  collapsed:: true
  There are 2 types of collections: same type and different type collections.
	- ### Same type collection classses
	  `IntArray`, `BooleanArray`, `ByteArray`, `FloatArray`, `LongArray` etc. Also created using `arrayOf<CLASS>(value1, value2, ...)`
	- ### Different array collections #[[immutable collections]]
	  Created using `arrayOf(values)` function. For example, `arrayOf(1, "2', "tree", 4.0)`. Take into account the collections created this way are immutable.
	- ### Mutable collections #[[mutable collections]]
		- Mutable list    - `ArrayList`, `arrayListOf`, `mutableListOf`
		- Mutable set   - `mutableSetOf`, `hashSetOf`
		- Mutable map - `HashMap`, `hashMapOf`, `mutableMapOf`
	- Array examples:
		- ```
		  // Example
		  val numbers: IntArray = intArrayOf(1,2,3,4)
		  print(numbers.contentToString())
		  numbers[0] = numbers[0] + 2
		  print(numbers.contentToString())
		  
		  val items: Array<Any> = arrayOf(1, "2", "three", 4.0f)
		  ```
	- List examples:
		- ```
		  // Lists
		  val months = listOf("January", "February", "March")
		  println(months)
		  val mutableMonths = months.toMutableList()
		  mutableMonths[0] = mutableMonths[0] + " 1"
		  println(mutableMonths)
		  ```
	- Set examples
		- ```
		  // Sets
		  val fruits = setOf("apple", "mango", "banana", "apple")
		  println(fruits)
		  val mutableFruits = fruits.toMutableSet()
		  mutableFruits.add("grapefruit")
		  mutableFruits.add("orange")
		  mutableFruits.add("orange")
		  println(mutableFruits)
		  ```
	- Map examples
		- ```
		  // Maps
		  val daysOfTheWeek = mapOf(1 to "Mon", 2 to "Tue", 3 to "Wed", 4 to "Thur", 5 to "Fri", 6 to "Sat", 7 to "Sun")
		  println(daysOfTheWeek)
		  ```
- ## Lambda functions #lambda
  Lambda syntax: `{ variables -> function body}`
  ```
  val sum: (Int, Int) -> Int = { a: Int, b: Int -> a + b }
  println(sum(1,2))
  
  val sum2 = { a: Int, b: Int -> a + b }
  println(sum2(1,2))
  ```
- ## Access modifiers #[[access modifiers]]
  collapsed:: true
  There are 4 of them: `public, private, protected, internal`. The default is `public`.
	- `public` - accessable from everywhere
	- `private` - accessable inside the package or class
	- `internal` - accessable inside module where it is implemented
	- `protected` - allow visibility to its subclass only
	- ```
	  open class Base {
	    var a = 1 //public by default
	    private var b = 2 //private to Base class
	    protected open val c = 3 //visible to Base and Derives classes
	    internal val d = 4 //visible inside the same module
	    protected fun e() { } //visible to the Base and Derived classes
	  }
	  
	  class Derived: Base {
	    // a, c, d, e() of the Base class are visible
	    // b is not visible
	    override val c = 9 //c is protected
	  }
	  
	  fun main() {
	    val base = Base()
	    // base.a and base.d are visible
	    // base.b, base.c, base.e() are not visible
	    val derived = Derived()
	    // derived.c is not visible
	  }
	  ```
- ## Exception handing #[[exceptions]]
  collapsed:: true
	- General approach to throw exception: `throw MyException()`
	- The syntax:
	  ```
	  try {
	   // code to execute
	  } catxh(e: MyException) {
	  
	  } finally {
	  
	  }
	  ```
	- Example:
	  ```
	  val str = parseNumber("10")
	  println(str)
	  
	  fun parseNumber(str: String): Int {
	    return try {
	      Integer.parseInt(str)
	    } catch(e: ArithmeticException) {
	      0
	    }
	  }
	  ```