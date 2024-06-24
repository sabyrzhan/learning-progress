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
			- **Rendering rows of table**: instead of creating each sell for table with a value, it is better to reuse the cell to generate the dynamic content
			- **HTTP connection**: opening new connection every time to send the data to destination, can beat the performance of the system. Instead we can keep the same connection and reuse it to send other data until we finish the upload.
		- **Solution**
		  Flyweight pattern can be used to save RAM memory and reuse the pool of objects for multiple usages. It has similarity with Singleton pattern, the difference is in the number of instances. It is better to use it mostly to solve the problems with memory and to increase the performance of the system by saving it.
		- **Pattern components**
			- **Pool of objects**: reusable instances. It consists from 2 states:
				- ***Intrinsic***: the state only related to object and does not depend to external context or other objects. The state is immutable to them.
				- ***Extrinsic***: the one that is changed and dependent to external context. It need to be passed to the object in order to perform operation.
			- **Factory**: the one that picks the object by some part of the intrinsic state and creates new object if does not exist.
			- **Client or Context**: the owner of the contextual data. The client passes the contextual data to flyweight object to perform operation. Client sees the object as a template and uses Factory to get the it instead of creating it directly.
	- ### Proxy Pattern
	  collapsed:: true
		- **Problem**
		  Let's say we have an HTTP client to remote REST service. It is a really basic client that does not have much functionality related to distributed environment: like better logging, retrier, pooling, caching etc. How can we add such functionality to each method without breaking SRP and OCP? Also keeping the same interface and make it swappable in our system?
		- **Solution**
		  We can use Proxy pattern for that. By aggregating the target client and implementing the common interface we can add our function before and after calling the target method. The purpose of the pattern is not to change the behavior but extend the existing methods with supporting functionality and forward the request to target method.
	- ### Chain of Responsibility Pattern
	  collapsed:: true
		- **Problem**
		  There can be a requirement to make multiple steps of validations or even trace the full lifecycle of the request. It is possible to make all these validations within one function or class. But if it happens within a framework or a system where the steps are evolved and they are added over time, it will be hard to maintain it. If you want to remove or reorder the steps the maintenance becomes a nightmare. In the end the class will violate OCP and SRP principles and will require testing on every change since it can bring new bugs.
		- **Solution**
		  With Chain of Responsibility (CoR) pattern you can divide the steps into multiple separate classes or methods. Each class will perform its own responsibility and pass the outcome or data to next class in the chain. Each can be reordered and maintained separately. The implementation is also very simple, which can be achieved using single-linked list. Each next step will have a pointer to its next step and so forth. For any step there is no requirement to have the reference to the next step. It can either handle it or break the chain which will finish the processing.
	- ### Command Pattern
	  collapsed:: true
		- **Problem**
		  How can we use all the request parameters to a service/REST API as a standalone entity with its context and other related data? Let's imagine a REST API client that makes requests to external systems. We can call request methods directly, in each caller methods. But this will bring performance issues if we use it the same way in distributed environment. If we start using it in an event driven service, how can we exchange the requests between system components in a reusable and self-contained object?
		- **Solution**
		  Solution for such problem has its name as a command design pattern. It wraps all the request data, context and other related data into an object and lets to perform single responsibility operation over it. Since each command is an independent entity, it can be reused and combined with other patterns like Composite pattern or Chain of Responsibility. One of the popular use cases are GUI applications where each mouse click or keyboard press can be handled using Command pattern. Also, in text editor or database transactions, where each command besides main operation can provide undo/rollback operation.
		- **Pattern components**
			- **Command**: main interface. There can be also a base class that contains all the boilerplate common fields and methods.
			- **Concrete command**: implementation of the Command that performs main operation. The command class should be very light in scope of SRP and OCP to ovoid complexity.
			- **Sender**: the owner of the command that initiates the command operation. It does not know any specific implementation and operates using only command contract.
			- **Receiver**: delegated service that perform the main business logic. Mostly the command wraps the operation of the receiver, since by itself command does not perform any business operations.
			- **Client**: the creator of the concrete command. Client is the only who configures the relationships between command and sender/receiver and  who knows about concrete commands and manages them.
	- ### Iterator Pattern
	  collapsed:: true
		- **Problem**
		  Sometimes just looping over the objects might not be enough. Instead you want to delegate looping to other object that could manage internal state. For example, there might be a case that while looping over the collection you want to remove the element and keep looping or replace the object in a slice. Or even you would want to allow concurrent modification of the slice or maps while looping it.
		- **Solution**
		  You can utilize so called iterator pattern that facilitates looping and changing the collection objects. Separate object which is called an iterator will be responsible for looping over it and manage internal state. By using current value you can access the current value of iterator. Iterator also provides the end of the loop or flag to identify is there a next value.
	- ### Mediator Pattern
	  collapsed:: true
		- **Problem**
		  collapsed:: true
		  You have components that need to exchange data between other components. Setting each component to be aware of each other, will make the system complex when the number of components grow. It makes components strongly coupled and whenever the components are added or removed dynamically, you have to transfer this knowledge to each of them. For example:
			- **Chat system**: each user is added and removed dynamically from the room. Making each user aware of other user will force to loop over one to send/receive message or join/remove the user.
			- **CICD Runner**: submitting a job to a runner directly will not be balanced and it becomes client's responsibility. Knowledge of each other makes it tightly coupled and less scalable.
			- **Aircrafts**: communication between each flying aircraft makes it risky for pilots planning to land the aircraft. Updating information between each other constantly makes it unreliable because the communication can be lost or message delivery occur with latency. This makes landing catastrophic if communication is lost.
		- **Solution**
		  Using mediator pattern we can create a central system that acts as a message broker to all components. All communications are done through one system, which makes them loosely coupled between each other. Mediator is aware of all the components and their interactions between each other. For chat system separate room server will act as user manager and message broker, CICD runner server will register runners and distribute their job and in case of aircraft, separate control tower will make it easier for pilots to manage landing and flying directions.
		- **Pattern components**
			- **Mediator**
			  Composition object that facilitates communication between each component. It has knowledge of all the components and can even manage their lifecycle.
			- **Component**
			  The component is the main part of the system that performs the business logic and which has zero knowledge about other components. Each component has a reference to mediator, but not to other components.
	- ### Memento Pattern
	  collapsed:: true
		- **Problem**
		  You want to save the states of the object after change in order to revert later. At the same time you want to fully encapsulate state data so the owner have access to it. Its applications can be, text editors to Undo/Redo the change, rollback to previous record in DB transactions, make database snapshots etc.
		- **Solution**
		  Memento pattern stores the snapshot of the data and lets to revert to it. Most of the time you make it immutable, so you pass the data you want to save as constructor parameter. Caretaker can be used to fully delegate the state management like storing and restoring the state. Also, keep in mind memento is just the storage and does not provide additional functions for state modifications.
		- **Pattern components**
			- **Memento**
			  Value class that stores the state. Can be directly 1-to-1 mapped fields or object itself
			- **Caretaker**
			  Delegate class that manages the state (store, restore state)
			- **Origin**
			  Owner of the state. For example, in case of text editor - editor is the owner of the state, database transaction - persistence manager is the owner. State management is done via caretaker or caretaker acts as the state provider externally.
	- ### Observer Pattern
	  collapsed:: true
		- **Problem**
		  You have an object that changes its state dynamically. At the same time you have objects that must listen to object changes. Also those objects must be able to watch and un-watch the changes at any time.
		- **Solution**
		  Observer lets to subscribe listeners to subject changes. The subject provides 2 methods like `subscribe` and `unsubscribe` for observers. And listener provides single method: `update` or `notify`, which invokes specific actions when event occurs.
		- **Pattern components**
			- **Observer/Subscriber**
			  Object that monitors the changes in subject. Provides mostly single method like `notify` or `update` to perform action when event occurs.
			- **Observed/Publisher**
			  Main subject when its state changes, notifies subscribers. Provides mostly methods like `subscribe` and `unsubscribe` for observers.
	- ### State Pattern
	  collapsed:: true
		- **Problem**
		  Throughout the lifecycle, application can transition to multiple states. In each state it can behave differently. To solve the state management we can use `if-else` or `switch` statements to handle the operations. But this becomes unscalable when we introduce more and more states and application becomes dynamic and sensitive to states.
		- **Solution**
		  The State pattern delegates each state behavior to its own class. Centralized context class owns the state, but at the same time each state class is able to change the state using context. There must be a separate base interface and abstract class that all the states extend and with default implementation if needed. This is to allow the context to change the state without knowing specific state implementations.
		- **Pattern components**
			- **Context**
			  State owner or state machine. This can be totally separate class or client that owns and manages the state.
			- **State**
			  Base interface that all state classes implement. The interface must be simple and generally contains single method for handling the behavior. The method can accept the context in order to change the state.
			- **Base state**
			  Although not needed, but can also be used to set default methods for all the states.
			- **Concrete state**
			  Specific state implementation. Each implementation is light and handles the required behavior.
	- ### Strategy Pattern
	  collapsed:: true
		- **Problem**
		  We can have a case we have to use different algorithms to solve the same problem. For example, to increase the speed or way of processing data. Moreover we have to change it at run-time based on the user request. The data that we provide does not change, but the algorithm does.
		- **Solution**
		  We can provide an high-level interface which all specific algorithm classes must implement. Main application or context can swap the algorithm at run-time. Specific algorithm is provided by the client, so the the context is not aware about the implementations. It only knows the interface.
		- **Pattern components**
			- **Context**
			  Main app or strategy owner that has a reference to specific algorithm. Strategy is swapped in this class.
			- **Strategy**
			  High-level base interface that other algorithms must implement. Context is only aware about this interface only.
			- **Concrete strategy**
			  Specific implementation of the strategy interface. It is chosen by action type and by the client.
	- ### Template method Pattern
	  collapsed:: true
		- **Problem**
		  You have a fat method that processes the data of various document types like Word, PDF, CSV etc. The logic has common functionalities for different types, but has many if-else statements for some specific cases. The logic becomes unscalable when you start adding many other document types. You have to add additional if-else statements and create similar logic.
		- **Solution**
		  With template method pattern you can extract the specific functions to abstract methods to make them overridable. In the main method you call those abstract functions in specific order. The main function is left final and the owner class will become abstract. Making the all methods as abstract is not required though. You can make them as overridable, but with default functionality. It is very similar to Strategy pattern, but the difference is algorithm is chosen at compile time, whereas in Strategy it is swapped at run-time.
		- **Pattern components**
			- **Abstract class**
			  Main method owner class that calls all the abstract or overridable methods as steps in specific order.
			- **Specific classes**
			  Classes that extend the abstract class and implements specific functions.
	- ### Visitor Pattern
		- **Problem**
		  You have a list of classes that implement the same interface. Then you have a task to perform object specific actions. The standard way would be to loop over each one and check each one with `instanceOf` and then perform the action. This will work for small amounts of subclasses like 2-5. But when it gets more, maintaining cost also increases. Because you have to take into account each `if-else` or `switch` statements performed on all the objects in order not to miss.
		- **Solution**
		  We can solve such problem with Visitor pattern using Double Dispatch technique. Instead of performing action on each object ourself, we can let object to accept the delegate (visitor object). We will force objects to implement separate interface that accepts visitor parameter. This way visitor will be aware about each specific class to perform logic on them.
		- **Pattern components**
			- **Visitor**
			  Visitor interface that can be aware about each element that it processes. It can have many overloaded methods to accept each of them.
			- **Concrete Visitor**
			  Concrete visitor implementation
			- **Element**
			  Elements that accept visitor. Base interface or abstract class for subclasses.
			- **Concrete element**
			  Concrete implementation of the element which accepts the visitor. Thanks to Double dispatch this object will be redirected to visitor.