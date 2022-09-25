# Swift
1. `break` in `switch` is optional, because it stops code execution until meets next `case` statement. To continue execution as in Java/C/C++ 
language use `fallthrough` statement. For example:
```
let fallthroughSwitch = 10 
switch fallthroughSwitch { 
case 0..<20:
    print("Number is between 0 and 20") 
    fallthrough 
case 0..<30: 
    print("Number is between 0 and 30") 
default: 
    print("Number is something else") 
}
```
will print:
```
Number is between 0 and 20
Number is between 0 and 30
```
