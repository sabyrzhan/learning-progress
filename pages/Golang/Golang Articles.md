## [[Slices vs Pointers to slices]]
	- Date: Jul 14, 2022
	- Source: https://stackoverflow.com/a/72979409
	- ### A bit of theory
	  collapsed:: true
		- There's no "right" or "wrong" or "correct" and "incorrect": you can have a pointer to a slice, and you can have a pointer to a pointer to a slice, and you can add levels of such indirection endlessly.
		  collapsed:: true
		  
		  What to do depends on what you need in a particular case.
		  
		  To help you with the reasoning, I'll try to provide a couple of facts and draw some conclusions.
		  
		  The first two things to understand about types and values in Go are:
			- Everything in Go, ever, always, is passed by value.
			  
			  This means variable assignments (`=` and `:=`), passing values to function and method calls, and copying memory which happens internally such as when reallocating backing arrays of slices or rebalancing maps.
			  
			  Passing by value means that actual bits of the value which is assigned are physically copied into the variable which "receives" the value.
			- Types in Go—both built-in and user-defined (including those defined in the standard library)—can have value semantics and reference semantics when it comes to assignment.
			  
			  This one is a bit tricky, and often leads to novices incorrectly assuming that the first rule explained above does not hold.
			  
			  "The trick" is that if a type contains a pointer (an adderss of a variable) or consists of a single pointer, the value of this pointer is copied when the value of the type is copied.
			  
			  What does this mean? Pretty simple: if you assign the value of a variable of type `int` to another variable of type `int`, both variables contain identical bits but they are completely independent: change the content of any of them, and another will be unaffected. If you assign a variable containing a pointer (or consisting of a single pointer) to another one, they both, again, will contain identical bits and are independent in the sense that changing those bits in any of them will not affect the other. But since the pointer in both these variables contains the address of the same memory location, using those pointers to modify the contents of the memory location they point at will modify the same memory.
			  In other words, the difference is that an `int` does not *reference* anything while a pointer naturally *references* another memory location—because it contains its address. Hence, if a type contains at least a single pointer (it may do so by containing a field of another type which itself contains a pointer, and so on—to any nesting level), values of this type will have reference assignment semantics: if you assign a value to another variable, you end up with two values referencing the same memory location.
			  
			  That is why maps, slices and strings have reference semantics: when you assign variables of these types both variables point to the same underlying memory location.
			  
			  Let's move on to slices.
	- ### Slices vs pointers to slices
	  collapsed:: true
		- A slice, logically, is a `struct` of three fields: a pointer to the slice's backing array which actually contains the slice's elements, and two `int`s: the capacity of the slice and its length. When you pass around and assign a slice value, these `struct` values are copied: a pointer and two integers. As you can see, when you pass a slice value around the backing array is *not* copied—only a pointer to it.
		  collapsed:: true
		  
		  Now let's consider when you want to use a plain slice or a pointer to a slice.
		  
		  If you're concerned with performance (memory allocation and/or CPU cycles needed to copy memory), these concerns are unfounded: copying three integers when passing around a slice is dirt-cheap on today's hardware. Using a pointer to a slice would make copying a tiny bit faster—a single integer rather than three—but these savings will be easily offset by two facts:
			- The slice's variable (a pointer to which you will juggle around) will almost certainly end up being allocated on the heap so that the compiler can be sure its value will survive crossing boundaries of the function calls—so you will pay for using the memory manager, and the garbage collector will have more work.
			- Using a level of indirection reduces [data locality](https://en.wikipedia.org/wiki/Locality_of_reference): accessing RAM is slow so CPUs have caches which prefetch data at the addresses following the one at which the data is being read. If the control flow immediately reads memory at another location, the prefetched data is thrown away: cache trashing.
		- OK, so is there a case when you would want a pointer to a slice? Yes. For instance, the built-in [`append`](https://pkg.go.dev/builtin#append) function could have been defined as
		  
		  ```
		  func append(*[]T, T...)
		  ```
		  
		  instead of
		  
		  ```
		  func append([]T, T...) []T
		  ```
		  
		  (N.B. the `T` here actually means "any type" because `append` is not a library fuction and cannot be sensibly defined in plain Go; so it's sort of pseudocode.)
		  
		  That is, it could accept a pointer to a slice and possibly *replace* the slice pointed to by the pointer, so you'd call it as `append(&slice, element)` and not as `slice = append(slice, element)`.
		  
		  But honestly, in real-world Go projects I have dealt with, the only case of using pointers to slices which I can remember was about pooling slices which are heavily reused—to save on memory reallocations. And that sole case was only due to [`sync.Pool`](https://pkg.go.dev/sync#Pool) keeping elements of type `interface{}` which may be more effective when using pointers¹.
	- ### Slices of values vs slices of pointers to values
	  collapsed:: true
		- Exactly the same logic described above applies to the reasoning about this case.
		  
		  When you put a value in a slice that value is copied. When the slice needs to grow its backing array, the array will be reallocated, and reallocation means physically copying all existing elements into the new memory location.
		  
		  So, two considerations:
			- Are elements reasonably small so that copying them is not going to press on memory and CPU resources?
			  
			  (Note that "small" vs "big" also heavily depens on the frequency of such copying in a working program: copying a couple of megabytes once in a while is not a big deal; copying even tens of kilobytes in a tight time-critical loop can be a big deal.)
			- Is your program OK with multiple copies of the same data? (For instance, values of certain types like [`sync.Mutex`](https://pkg.go.dev/sync#Mutex) must not be copied after first use.²)
		- If the answer to either question is "no", you should consider keeping pointers in the slice. But when you consider keeping pointers, also think about data locality explained above: if a slice contains data intended for time-critical number-crunching, it's better to not have the CPU to chase pointers.
		  
		  To recap: when you ask about a "correct" or "right" way of doing something, the question has no sense without specifying the set of criteria according to which we could classify all possible solutions to a problem. Still, there are considerations which must be performed when designing the way you're going to store and manipulate data, and I have tried to explain these considerations.
		- In general, a rule of thumb regarding slices could be:
			- Slices are designed to be passed around "as is"—as values, not pointers to variables containing their values.
			  
			  There are legitimate reasons to have pointers to slices, though.
			- Most of the time you keep values in the slice's elements, not pointers to variables with these values.
			  Exceptions to this general rule:
				- Values you intend to store in a slice occupy too much space so that it looks like the envisioned pattern of using slices of them would involve excessive memory churn and spending CPU time to copying data around.
				- Types of values you intend to store in a slice require they must not be copied but rather only referenced, existing as a single instance each. A good example are types containing/embedding a field of type [`sync.Mutex`](https://pkg.go.dev/sync#Mutex) (or, actually, a variable of any other type from the [`sync`](https://pkg.go.dev/sync) package except those which itself have reference semantics such as [`sync.Pool`](https://pkg.go.dev/sync#Pool))².
	- ### A note of caution on correctness vs performance
	  collapsed:: true
		- The text above contains a lot of performance considerations. I've presented them because Go is a reasonably low-level language: not *that* low-level as C and C++ and Rust but still providing the programmer with plenty of wiggle-room to use when performance is at stake. Still, you should very well understand that at this point on your learning curve, correctness *must* be your top—if not the sole—objective: please take no offence, but if you were after tuning some Go code to shave off some CPU time to execute it, you weren't asking your question in the first place. In other words, please consider all of the above as a set of facts and considerations to guilde you in your learning and exploration of the subject but do not fall into the trap of trying to think about performance first. Make your programs correct and easy to read and modify.
	- ¹ An interface value is a pair of pointers: to the variable containing the value you have put into the interface value and to a special data structure inside the Go runtime which describes the type of that variable. So while you can put a slice value into a variable of type `interface{}` directly—in the sense that it's perfectly fine in the language—if the value's type is not itself a single pointer, the compiler will have to allocate on the heap a variable to contain a copy of your value there, and store a pointer to that new variable into the value of type `interface{}`. This is needed to hold that "everything is always passed by value" semantics of the Go assignments.
	  Consequently, if you put a slice value into a variable of type `interface{}`, you will end up with a copy of that value on the heap. Because of this, keeping pointers to slices in data structures such as [`sync.Map`](https://pkg.go.dev/sync#Mutex) makes code uglier but results in lesser memory churn.
	- ² All synchronization primitives, when compiled down to the machine code, work on memory locations–that is, all parts of the running program which need to synchronize on the same primitive are actually using the same address of a memory block representing that primitive. Hence, consider that if you lock a mutex, *copy its value* to a new variable (that means–a distinct memory location) and then unlock the copy, the initially locked copy won't notice, and all other parts of the program which use it for synchronization won't notice, too, which means you have a grave bug in your code.
-