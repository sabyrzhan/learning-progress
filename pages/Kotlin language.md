- ## Inheritence #oop
  Use `override` modifier when overriding method
  ```
  //Example
  class Car(make: String, model: String) {
  fun drive() { }
  }
  
  class Toyota(make: String, model: String): Car(make, model) {
  override fun drive() { }
  }
  ```
- ## Collections #collections
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