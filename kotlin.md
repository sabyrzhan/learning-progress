# Kotlin lang
### Immutable and mutable variable declarations
```
// This is the immutable variable
val value = "This is the immutable"
// This is the mutable variable
var value2 = "This is the variable"
```
### String interpolation with variable value and expression
```
val str = "Some string"
print("This is the string $str and its length is ${str.length}")
```
### `switch/case` like statement in Kotlin is `when`. Unlike `switch/case` in other languages it has various ways of handling the conditions:
```
// Example 1
var season = 3
when (season) {
  1 -> println("Spring")
  2 -> println("Summer")
  3 -> {
    println("Fall")
    println("Autumn")
  }
  4 -> println("Winter")
  else -> println("Invalid seasons")
}

// Example 2
var month = 3
when (month) {
  in 3..5 -> println("Spring")
  in 6..8 -> println("Summer")
  in 9..11 -> println("Fall")
  12, 1, 2 -> println("Winter")
  else -> println("Invalid season")
}

// Example 3
var age = 21
when (age) {
  !in 0..20 -> println("now you may drink in the US")
  in 18..20 -> println("you may vote now")
  16,17 -> println("you may drive now")
  else -> println("you're too young")
}

// Example 4
var x: Any = 13.37
when (x) {
  is Int -> println("$s is an Int")
  !is Double -> println("$x is not a Double")
  is String -> println("$x is a String")
  else -> println("$x is none of the above")
}
```
### `for` loop
```
// Example 1
for (num in 1..5) {
  println("Simple $num")
}

// Example 2 (excludes 10)
for (num in 1 until 10) {
  println("Until: $num")
}

// Example 3
for (num in 10 downTo 1) {
  println("Downto: $num")
}

// Example 4 with step
for (num in 1..10 step 2) {
  println("With step: $num")
}

// Example 5 as method way 1
for (num in 1.rangeTo(10).step(2)) {
  println("Method way 1: $num")
}

1.rangeTo(10).step(2).forEach {
  println("Method way 2: $it")
}
```
5. Function/method signature
```
// Example
fun add(a: Int, b: Int): Int {
    return a + b
}
```
### Nullables
```
// Example
var name: String = "Name"
// name = null  <-- Compilation error

var nullableName: String? = "Nullable name"
nullableName = null // OK

var len = name.length
var nullableLength = nullableName?.length
println("Len: $len, nullableLen: $nullableLen")

// Safecall operator with let
nullableName?.let {
  println("Nullable length is: ${nullableName.length}")
}
```
7. Elvis operator
```
// Example
var nullableName: String? = "Name"
var nonNullableName = nullableName ?: "guest"
```
### Not-Null assertion operator `!!`
```
var nullableName: String? = "Name"
var expectedNotNullName = nullableName!!
```
9. Class with constructor and initializer
```
// Example: defining class with constructor with default values and initializer
class Person constructor(name: String = "DEFAULT_NAME", surname: String = "DEFAULT_SURNAME") {
  init {
    println("This block is executed once object is created")
  }
}

val person = Person("Name", "Surname")
val defaultPerson = Person()
val personWithName = Person(name = "Anothername")
val personWithSurname = Person(surname = "Anothersurname")
```
### Shadowing
```
// The parameter variable is immutable. To set the value it must be redefined thus overriding parameter value
fun add(a: Int, b: Int): Int {
  var a = 100
  return a + b
}
```
### Secondary constructor
```
// Example:
// Primary constructor
// If you put var/val modifier to parameters then member fields with the same name will be defined. Otherwise they are used only in `init`
class Car(var make: String, var model: String) {
    // Secondary constructor. Always must inherit primary constructor
    constructor(): this("NONAME_MAKE", "NONAME_MODEL") {
    }

    fun printDetails() {
        println("Make is ${this.make} and model is ${this.model}")
    }
}

class Animal(name: String) {
    var name: String? = null
    
    // To add logic to promary constructor, you must always define `init`
    init {
        println("Initializing animal with name=$name")
        this.name = name
    }

    fun printDetails() {
        println("Animal is $name")
    }
}
```
### `getter/setter`
```
// Example
class Car {
  var make: String? = null
  get() = field.lowercase()
  set(value) {
    field = "updated: $value"
  }
  
  var model: String? = null
  get() = "Value is: $field"
  set(value) {
    field = "model is: $value"
  }
}
```
### Data classes
```
// Example. Data classes must have at least 1 parameter in a constructor
data class User(id: Int, name: String)
```

### Inheritence
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

### Collections
There are 2 types of collections: same type and different type collections.

#### Same type collection classses
`IntArray`, `BooleanArray`, `ByteArray`, `FloatArray`, `LongArray` etc. Also created using `arrayOf<CLASS>(value1, value2, ...)`

### Different array collections
Created using `array(values)` function. For example, `array(1, "2', "tree", 4.0)`. Take into account the collections created this way are immutable.
