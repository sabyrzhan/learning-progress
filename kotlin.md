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
3. `switch/case` statement in Kotlin is `when`. Unlike `switch/case` in other languages it has various ways of handling the conditions:
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
  in 16,17 -> println("you may drive now")
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
