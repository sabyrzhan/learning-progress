## [[GoF Patterns]]
	- ### Strategy pattern
	  collapsed:: true
		- Used to define multiple algorithms that differ only in behavior and use them interchangeably at run-time. It is similar to Dependency Injection but the difference is - DI is used for code coupling but not for interchanging implementations at run-time.
	- ### Observer pattern
	  collapsed:: true
		- It itilizes pub-sub mechanism for notifying messages to all the subscribed objects. Implementing system consists from following components:
			- Subject - object responsible for subscribing, unsubscribing observers and notifying messages.
			- Observer - notification receiver who actually has method - **notify**.
	- ### Factory Method Pattern
	  collapsed:: true
		- Used to create objects inheriting the same object. The pattern more about inheritence compared to abstract factory pattern.
		- Let class or interface subcalsses decide which concrete object to create
	- ### Abstract Factory Pattern
	  collapsed:: true
		- Also known as - **factory of factories** pattern. Used to create objects from the same family. The pattern uses composition to built final objects.
		- The factory class creates other factory classes which themselves create and return abstract objects.
	- ### Prototype Pattern
	  collapsed:: true
		- Create new object by cloning existing object. Thus eliminating overhead of creating new  object and setting each field manually.
	- ### Bridge Pattern
	  collapsed:: true
		- **Problem**:
		  Adding a new functionality to system produces a cross product of components and functionalities. This type of problem occurs in a system with OOP design and adding new functionalities using inheritance. In the end system becomes a single hierarchy flat tree system with tightly coupled classes.
		- **Solution**:
		  Use composition instead of inheritance by dividing the single hierarchy into multiple hierarchies which can independently be developed. Interaction with other hierarchy can be done over provided interfaces. This eliminates tight coupling to specific implementation.
		- **Pattern components**:
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
	  collapsed:: true
		- **Problem**:
		  You have a set of objects of similar type. By using the same contract you can perform operation on each of them. But instead of that, you would like the functionality that would perform the same operation on all of them using the same contract. For example:
			- **File manager**: you have files and folders. Folders themselves can be nested and contain other files and folders. You can get the size of each of them. But by getting the size of the folder, you can get the sum of sizes of nested child files and folders.
			- **Batch operation**: You have cron jobs in Kubernetes that perform various tasks. Each task take different time to perform. You want to get not only single task average time, but also all tasks average time.
		- **Solution**:
		  We can use Composite pattern for that. It contains collection of objects as tree structure that all comply to the same contract. We can add or remove objects using Composite and in the end by using the same contract perform the same operation on all of them as if on single one.
		- **Pattern components**:
			- **Component**
			  The main interface that all the objects must comply to including Composite intself
			- **Leaf**
			  The object that implements Component.
			- **Composite**
			  The class that implements Composite pattern and aggregates all the items in a tree like structure. This also implements Component in order to operate on each item using single contract.
			- **Client**
			  User who is performing operation on Composite class.
	- ### Decorator pattern
	  collapsed:: true
		- **Problem**:
		  You want to extend the class by adding multiple functionalities, keeping the OCP and SRP principles at the time. By inheriting it can bring multiple subclasses that will bring class hierarchy explosion. Also the class can be final that will prohibit to extend it. For example:
			- **HTTP client**: to add functionalities like request metrics, logging, data encryption, retry you must make a lot of subclasses. If there has to be combinations then the size becomes even more.
			- **File Reader and Writer**: we can have handler of various file types. And if initially the class contains basic functionality, then to extend it with other features like encryption, faster storage type support, large file support etc, here also have to make combinations with just inheritance
		- **Solution**:
		  Decorator pattern is to the rescue. It wraps the target class also being complied to the same interface as the target one and provide additional features. Besides that you can use multiple stacks of decorators, by passing as the argument, since all of them implement the same contract. This looks similar to Composite, but the difference is instead of tree of objects it only uses single wrapped target object. Even though we could implement this using inheritance that is not the only or main way. When the quantity of functionalities become large, managing subclasses produce "explosion of class hierarchy". Since you have to deal with combinations of the features, the number becomes even more. Because of that using composition is the primary way, since it lets to comply to OCP and SRP principles. Also makes it easy to test by injecting fake or  stubs.
	- ### Facade Pattern
	  collapsed:: true
		- **Problem**:
		  You have a system that is very complicated to use for regular user due to involvement of many dependent services. Or a framework that consists from many libraries, in which in order to create just a simple REST endpoint, developer has to first instantiate and register bunch objects from various libraries. For example:
			- **P2P payment API**: in order to make simple p2p payment from one account to another account using phone number, you have to instantiate subsystems like AccountService, CurrencyService, User3DSecureService, DatabaseService etc, also in right order just to perform SendMoney in the end.
			- **Spring Framework**: Spring Framework itself consists from many core libraries like spring-web, spring-mvc, spring-jdbc, spring-data, spring-security etc. To just create REST API and start the service, the developer must know which beans to instantiate first and start the server. Thankfully Spring Boot simplifies all these process using dependency autodiscovery and autoconfiguration.
		- **Solution**:
		  Using Facade pattern you can aggregate all the dependent service into single API and provide simple or fluent API to the user. All the heavy and complicated jobs, communications between the components are performed inside the Facade class and is hidden from the user.
	- ### Flyweight Pattern
	  collapsed:: true
		- **Problem**
		  Generating huge amount of objects at run-time can blow up the RAM memory. Moreover, if the object contains values that are repeated in each object, then it would be better to store them somewhere an reuse or refer to the common data instead of Copy/Past-ing them. For example:
			- **Rendering rows of table**: instead of creating each sell for with value, it is better to reuse the cell to generate the dynamic content
			- **HTTP connection**: opening new connection every time you send the packet of data to destination can beat the performance of the system. Instead we can keep the same connection and reuse for all packet until we finish the upload.
		- **Solution**
		  Flyweight pattern can be used to save RAM memory and reuse the pool of objects for multiple usages. It has similarity with Singleton pattern, the difference is in the number of instances. It is better to use it mostly to solve the problems with memory and to increase the performance of the system by saving it.
		- **Pattern components**
			- **Pool of objects**: reusable instances. It consists from 2 states:
				- ***Intrinsic***: the state only related to object and does not depend to external context or other objects. The state is immutable to them.
				- ***Extrinsic***: the one that is changed and dependent to external context. It need to be passed to the object in order to perform operation.
			- **Factory**: the one that picks the object by some part of the intrinsic state and creates new object if does not exist.
			- **Client or Context**: the owner of the contextual data. The client passes the contextual data to flyweight object to perform operation. Client sees the object as a template and uses Factory to get the it instead of creating it directly.