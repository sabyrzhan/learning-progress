## [[GoF Patterns]]
	- ### Strategy pattern
		- Used to define multiple algorithms that differ only in behavior and use them interchangeably at run-time. It is similar to Dependency Injection but the difference is - DI is used for code coupling but not for interchanging implementations at run-time.
	- ### Observer pattern
		- It itilizes pub-sub mechanism for notifying messages to all the subscribed objects. Implementing system consists from following components:
			- Subject - object responsible for subscribing, unsubscribing observers and notifying messages.
			- Observer - notification receiver who actually has method - **notify**.
	- ### Factory Method Pattern
		- Used to create objects inheriting the same object. The pattern more about inheritence compared to abstract factory pattern.
		- Let class or interface subcalsses decide which concrete object to create
	- ### Abstract Factory Pattern
		- Also known as - **factory of factories** pattern. Used to create objects from the same family. The pattern uses composition to built final objects.
		- The factory class creates other factory classes which themselves create and return abstract objects.
	- ### Prototype Pattern
		- Create new object by cloning existing object. Thus eliminating overhead of creating new  object and setting each field manually.
	- ### Bridge Pattern
	  collapsed:: true
		- **Problem**:
		  Adding a new functionality to system produces a cross product of components and functionalities. This type of problem occurs in a system with OOP design and adding new functionalities using inheritance. In the end system becomes a single hierarchy flat tree system with tightly coupled classes.
		- **Solution**:
		  Use composition instead of inheritance by dividing the single hierarchy into multiple hierarchies which can independently be developed. Interaction with other hierarchy can be done over provided interfaces. This eliminates tight coupling to specific implementation.
		- **Pattern**:
		  Bridge pattern produces following components after your refactor using this pattern:
			- **Abstraction***:
			  The top layer or component that client uses to interact with your system. The layer does not know anything about implementation and interacts with other hierarchy or component strictly over interface provided by it. This layer can be extended using so called **Refined Abstraction**, but still keep using interfaces as a bridge to interact with other component.
			- **Implementor**:
			  This is the main contract or interface that is exported to **Abstraction** layer and single point of interaction with this component or hierarchy.
			- **ConcreteImplementor**:
			  Concrete implementation of the interface. Strictly follows the contract when implemented and is prohibited to be used directly by the abstraction. When the **Implementor** functionality is extended, the concrete implementor is forced to implement the functionality accordingly.
		- **Key points**:
		  *Decouple abstraction from implementation*, *strong encapsulation*, *separate hierarchy*
		- **Resources**:
			- https://www.pentalog.com/blog/design-patterns/bridge-design-patterns/
	- ### Composite Pattern
		- **Problem**:
		  You have a set of objects of similar type. By using the same contract you can perform operation on each of them. But instead of that, you would like the functionality that would perform the same operation on all of them using the same contract. For example:
			- **File manager**: you have files and folders. Folders themselves can be nested and contain other files and folders. You can get the size of each of them. But by getting the size of the folder, you can get the sum of sizes of nested child files and folders.
			- **Batch operation**: You have cron jobs in Kubernetes that perform various tasks. Each task take different time to perform. You want to get not only single task average time, but also all tasks average time.
		- **Solution**:
		  We can use Composite pattern for that. It contains collection of objects as tree structure that all comply to the same contract. We can add or remove objects using Composite and in the end by using the same contract perform the same operation on all of them as if on single one.
		- **Pattern**:
		  It consists from following components:
			- **Component**
			  The main interface that all the objects must comply to including Composite intself
			- **Leaf**
			  The object that implements Component.
			- **Composite**
			  The class that implements Composite pattern and aggregates all the items in a tree like structure. This also implements Component in order to operate on each item using single contract.
			- **Client**
			  User who is performing operation on Composite class.
	- ### Decorator pattern
		- **Problem**:
		  You want to extend the class by adding multiple functionalities, keeping the OCP and SRP principles at the time. By inheriting it can bring multiple subclasses that will bring class hierarchy explosion. Also the class can be final that will prohibit to extend it. For example:
			- **HTTP client**: to add functionalities like request metrics, logging, data encryption, retry you must make a lot of subclasses. If there has to be combinations then the size becomes even more.
			- **File Reader and Writer**: we can have handler of various file types. And if initially the class contains basic functionality, then to extend it with other features like encryption, faster storage type support, large file support etc, here also have to make combinations with just inheritance
		- **Solution**:
		  Decorator pattern is to the rescue. It wraps the target class complying to the same interface as the target one and provide additional features. Besides that you can use multiple stacks of decorators, by passing as the argument. Since all of them will implement the same contract. This looks similar to Composite, but the difference is instead of tree of objects it only uses single wrapped target object.
		- **Pattern**
		  Even though we could implement this using inheritance that is not the only or main way. When the quantity of functionalities become large, managing subclasses produce "explosion of class hierarchy". Since you have to deal with combinations of the features, the number becomes even more. Because of that using composition is the primary way, since it lets to comply to OCP and SRP principles. Also makes it easy to test by injecting fake or  stubs.