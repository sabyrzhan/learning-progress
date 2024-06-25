### Single Responsibility Principle (SRP)
	- Object must have only single responsibility and have only one reason to change.
	  For example it is a bad design to mix the data management functionality with data persistence functionality. Better to separate into 2 different classes, where persistency will operate on data management class.
- ### Open-Closed Principle (OCP)
	- Object or module must be open for extension and closed for modification
	  If there is a new requirement to add or update the classes it is better to extend for example using extra design patterns. For example, let's say we have products in our store. We can add product filter functionality by creating separate FilterSpec interface with `isSatisfiedBy(product)` signature. Here we are using Specification design pattern for that. Whenever we want to add new filter we just create new one by implementing FilterSpec. More over we can create CompositeFilter using [[Composite Pattern]] to combine multiple filter and apply as a single unit.
- ### Liskov Substitution Principle (LSP)
	- Child classes must be substitutable without changing the parent class behavior when state changes.
	  Popular example of the violation of the LSP is Rectangle-Square problem. When we have rectangle and square which extends it, square changes rectangle's behavior by mutating both width and height fields when one of the field is changed. In the end this produces different results when other common methods outcomes depend on these fields. The correct approach would be to separate the fields into their own struct, so they affect their own behavior not common the ones.
- ### Interface Segregation Principle (ISP)
	- ISP states interface should not provide too many functions, but segregated into multiple separate interfaces. This will let classes to implement the functions that they really need instead of implementing all of them or leaving with empty default body that are not required.
- ### Dependency Inversion Principle (DIP)
	- DIP states high level classes or modules should not depend on low level classes or modules. They must interact using abstractions. For example, we have a DB storage that provides access to data through DB driver. Instead of accessing directly through driver specific or low level API, we should access it using separate abstraction layer like interface. This way we wont depend on specific low level storage. If the storage type will be file or network, we can swap the implementation and still communicate through abstract interfaces.