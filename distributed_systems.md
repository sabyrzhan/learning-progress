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

## Types of services
- Stateless - depend on direct and indirect inputs to return any results.
- Statefull - depend on the state saved in the past and which directly relate to the result.

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
* Completeness - percentage of identified failed nodes in a certain period of time
* Accuracy - number of mistakes made in a certain period of time

## Scalability
### Partitioning
Partitioning - is the process of splitting a dataset into multiple smaller datasets and assigning responsibility of storing and processing on a different node. It mainly comes from relational database systems, but also used when designing distributed systems.

There are 2 variants of partitioning:
* Vertical - splitting a table columns into multiple tables, which will require table joins if you want to fetch more data.
* Horizontal - splitting a table rows into multiple tables, which will require access multiple tables if you want to fetch range of data. Also requires the knowledge of the node if data must be retrieved with specific criteria. Another disadvantage is the loss of ACID property, because of the complexity to achieve between different nodes.

### Horizontal partitioning alogrithms
1. **Range based partitioning** - range criteria is used to partition data. For example, by using alphabet letters.
    1. Pros:
        * Implementation simplicity using partition key
        * Range query within single partition
        * Good performance for small amount of data
        * Re-partitioning is easier
    2. Cons
        * Inability to range query without partition key
        * Bad performance for big data
        * Uneven distribution between the nodes
2. **Hash based partitining** - using hash function on search criterias to find the correct node, most commonly with `mod` operator.
    1. Pros:
        * ability to calcuate node at runime without storing any statfull information about the location
        * more changes the data is evenly distributed
    2. Cons:
        * Inability to perform range queries between the nodes
        * Adding or removing node requires whole system to re-partition, which will require movement of all data within the nodes
3. **Consistent hashing partitioning** - the same as hash based partitioning but with the solution for easy re-partitioning using ring topology. It uses so called "range" most from from integer numbers from [0, N], sorted in asc order. The range is divided by N where N is the number of nodes, so the requests with specific key calculated by hasfunction will be directed to the specified node. 
    1. Pros:
        * supports node addition/removal dynamically
        * reduced data movement when replicating to nodes
    2. Const:
        * uneven distribution of load/data between the nodes (**virtual nodes** are used to solve this problem)

For the replication following types can be used:
    * 1:N replication - where 1 node replicates to N hosts
    * Chained replication - replication is performed from one to another by chain
    * Mixed replication - mix of both replications
    
## Availability
Availability - is the ability of the system to continue functioning even though some parts were failed. To achieve it **replication** is used.

### Replication
Replication is the technique of storing the same piece of data in multiple nodes, so when one of then is crashed - data is not lost and the requests can be handled in other nodes almost immediately. There are 2 main strategies: pessimistic and optimistic:
* **Pessimistic replication** - provides single view for all replicated data as if they are stored on single node.
* **Optimistic replication** - allows different replicas to diverge, but get converged at a later time.

#### Single-Master replication
Simplest type of replication where only one **leading** or **master** node and multiple **follower** nodes which is also called **replicas**. Here the leader node mostly perform write operations, whereas replicas do read operations, thus making them horizontally scalable.

The updates from master can be replicated:
* **Synchornous mode** - where the client receives acknowledge once master receives akcnowledge from all replicas. This mode has increased durability, but with slower response time.
* **Asynchronous mode** - where the clietn receives acknowledge once master finishes writing data locallly, thus perlicating eventually. This mode decreases durability and consistency but increases response time.
    
Pros:
* simple to understand and implement
* remove the need for complicated protocols
* horizontally scalalble read-replicas. Perfect for read heavy workload operations.

Cons:
* not scalalble for write heavy workfloads
* a lot of read replicas can create a bottleneck for network traffic, since there is only single master node performing replication
* switch to failover node can not performed instantly when master node crashes
    
#### Failover
Failover is process of one node taking over control the when main node crashes. There are 2 approaches:
* Manual - performed manually. Counted as more reliable but it incurs significant downtime
* Automatic - fast but can but risky, since if there are complicated operations it can lead to inconsistent state.

## Resiliency
Resiliency is the ability of the system to withstand certain kind of failures - that is being responsive under failure. Resilience is an attribute, reliability is an outcome.

## Distributed System Design Patterns
### Timeout
Very simple to impleent pattern - used to set timeout to blocking requests. After specified timeout the request is terminated and related exception is thrown.

### Bulkheads
Used to partition services or service resources into pools to mitigate the impact of the failure in one pool to other pools. The term is coming from ship hull, where it has partitioned sections called "bulkheads", so if one of the bulkheads is filled with water, it will not get cascaded to other parts and bring down the whole ship.

### Circuit breaker
Circuit breaker is used when system is unable to serve user requests due to internal or dependent service errors for some time, but it is important to send default response to the client instead of error message/status. In that case system uses fallback method which might return, for example response from static file or use another DB data. The pattern also used alongside with Retry, Timeout patterns, where the only usages of them can degrade system significantly or make unresponsive at all. For example, if the system is using Retry pattern but at the same time it is failing systematically - then circuit-breaker can help to increase throughput and decrease error rate/latency.

It can be in one of following states:
1. **CLOSED** - dependent service(s) are healthy and serving all the requests in normal mode.
2. **OPEN** - dependent service(s) are unhealthy, thus service is returning fallback results (from static/config file, db, cache etc). Requests are not sent to services.
3. **HALF-OPEN** - dependent service(s) are partially operating normally. Only sepecified portion of requests are sent to services.


## Three pillars of observability
1. Metrics
2. Logging
3. Tracing
