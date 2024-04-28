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