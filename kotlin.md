# Kotlin lang
1. Immutable and mutable variable declarations:
```
// This is the immutable variable
val value = "This is the immutable"
// This is the mutable variable
var value2 = "This is the variable"
```
2. String interpolation with variable value and expression
```
val str = "Some string"
print("This is the string $str and its length is ${str.length}")
```
3. `switch/case` like statement in Kotlin is `when`. Unlike `switch/case` in other languages it has various ways of handling the conditions:
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
4. `for` loop
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
6. Nullables
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
8. Not-Null assertion operator `!!`
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
