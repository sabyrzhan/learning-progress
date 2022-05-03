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


## Three pillars of observability
1. Metrics
2. Logging
3. Tracing
