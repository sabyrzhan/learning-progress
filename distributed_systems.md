# Distributed Systems
## Fallacies of distributed systems
There at least 8 common false assumptions that developers make when developing systems:
1. The network is reliable - since most of the time we use TCP for communication, it makes us believe that network is reliable and never fail.
2. Latency is zero - when developing apps that access data locally and remote we have to understand there are latency differences. The latency increases even more when we try to access intercontinent services.
3. Badwidth is infinite - even though network bandwidth has been significantly increasesed in last decade, we must not forget that we even more have to design our systems to use it efficiently to send responses with lower latencies.
4. The network is secure - security fails can happen anytime: bugs, attacks, XSS scripts and so on. As a developer and designer of the system you have to do anything at your best to secure your system.
5. Topology does not change - there are numerious topologies and changed all the time. System must be able to adapt to change with low downtime and latency.
6. There is one administrator - single administrator can exist in scope of small or personal project. But in distributed environment they can even be divided to continents, customer specific where they even can be divided by roles. Even each registered user to the system can be an administrator of resources that belongs to the user.
7. Transport cost is zero - the data amount transferred, the network mesh that needs to be supported, stuff who is supported the network infrastructure, security, routing and many more are not zero cost. So in distributed when sending data from point A to point B, many factors are taken into account before calculating its cost.
8. The network is homogeneous - simply put pc or smartphone which is communicating with the backend. Whereas the backend itself working with various protocols and interacting other services before responding back the to the user. As we can see the system heterogeneous and it is also as topology always changed and system also has to be adapted with lower downtime and latency.

## Distributed system properties
1. Network asynchrony - we will never know how much time the request to resource will take all the time. We can just give probably or max/min time range, because there multiple systems being interacted.
2. Partial failure - in contract to service running on single computer, in distributed environment components can fail. Because of that system can degrade or partially work.
3. Concurrency - the system han handle multiple operations at the same time. Even working on the same data.

## Measure of correctness
There are 2 properties of measurement:
1. Safety property - something that will never happen in a correct system
2. Liveness property - something that eventually happen in a correct system.

Even though both of these properties define correctness, the safety property is always considered and taken into account first that the second. That is because most of the times it is not possible to fully apply both of the properties and the same time.

## Distributed system categories
1. Synchronous - every call to services are split into rounds, thus the caller is locked to the response from the called node.
2. Asynchronous - each call to service is not bound to time when it receives response, so every node could run at independent rates.

## Multiple deliveries of a message
There might be cases when system A fails to send a message to system B. But to mitigate this issue, system A can re-send the message by implementing retry mechanism. So in order to avoid double processing of the same message the systems can use following approaches:
* idempotency - no matter how much time system A sends the message, system B responds with success status ensuring it is processed only once.
* de-duplication - it is the same as idempotency, except that unique ID is assigned to a message before sending it. So by that ID system ensures the message was processed once.

The difference between the two above is idempotency adds additional restrictions to the system. Whereas in second case we can make sure by the message ID, which means not only receiver, but also sender takes part in it. Specifically for ID generation.

I have also want to point out, that there are differences in delivery and processing:
* delivery - can occur multiple times and we means delivery to the endpoint at hardware level;
* processing - occurs mostly at software level.

When designing the system we should assume that **delivery** happens multiple times, and **processing** should occur only one-time. These themselves implement such semantics like **at-most-once** and **at-least-once**.

## Failure
Failure is hard to detect in distributed environment. One of the reason for that - because of the asynchronous nature of the network. It makes harder to differentiate the failed node node respoding with latency.

Thus timeout is main reason we mostly use to identify the failure. However it has its own trade-offs. For example, short timeout has make us believe the node is dead. Whereas long timeouts of the service call can make us to belieave the service is healthy.

### Types of failures
1. Fail-stop - node halted permanently and is detected very easily (f.e., with monitors)
2. Crash - node halted silently (f.e., the system was not monitored). Can only be identified by sending only sending request.
3. Omission - node fails to respond to incoming requests. Node is alive, but service is dead.
4. Byzantine - node's behavior is anomalic. The server is sending different responses to the same request. It might be due to bugs, malicious actor or smth similar.

### Failure detector
It is a seperate independent component of the node to identify the failure. 2 properies are used to categories it:
* Completeness - 
* Accuracy - 


## Three pillars of observability
1. Metrics
2. Logging
3. Tracing
