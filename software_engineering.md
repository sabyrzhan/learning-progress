# Software Engineering
## Architecture vs Design
* Architecture - highest level of abstraction that shows from which components/modules the system consists and desribe their interaction.
* Design - is applied to component/module and describes its functionality (functions, relations, dependencies etc)

From this definition I can say that architecture itself consists from designs of each component. In reality, when new component's design is complete it is merged into system architecture, thus the original design spec becomes obsolete.

## SOLID principle
1. S - Single-Responsibility Principle
    1. Each class/function should have single respinsibility or reason to change
    2. Strive for high cohesion (putting related methods in one single place) and loose coupling (seperating different functionalities into multiple classes)
    3. Keep classes/functions small and testable
2. O - Open-Closed Principle
    1. Meaning: open for extension, closed for modification
    2. Anyway, before applying principle any problem should be solved using simple and concrete code first
3. L - Liskov Principle
    1. Subtypes must be substitutable for their base types
    2. Actually subset of polymorphism with addition of IS-SUBSTITUTABLE alogside of IS-A relation, which means the subtype actually must not change the behavior of the parent class but extends its functionality, thus preserving the compatibility.
    3. Key violations to look for:
        1. Type checking
        2. Null checking
        3. `NotImplementedException` like exceptions
    4. Use "Tell, but don't Ask" principle
4. I - Interface segregation principle
    1. Prefer small, cohesive interfaces to large expensive ones. Clients should not be forced to implements the methods they dont use.
    2. For large interfaces following can be applied:
        1. Interface inheritance
        2. The adapter pattern
        3. The facade pattern
5. D - Dependency Inversion Principle
    1. Most classes should depend on abstractions but not on implementation details. That is highler level modules should not depend on lower level modules.
    2. Abstractions focus on WHAT, implementation focus on HOW - which means details depend on interfaces not vice versa
    3. Classes should be explicit on their dependencies
    4. Clients should inject dependencies when they create other classes

## DRY - Dont Repeat Yourself
Each pience of logic must be unique and should not be repeated in other places. Following signs show that the logic is repeated elsewhere if used:
* magical hardcoded literals
* repeated functions
* repeated logics with if-else or switch statements

## KISS - Keep It Simple, Stupid
Principle dictates that simplicity is the key in architecting and writing quality application. Overengineered design can be hard to understand and maintain in long term perspective.

## YAGNI - You Aint' Gona Need It
I see this principle as some kind of extension of KISS. Because it dictates - dont create the functionality you are not going to use. Most of the times developers or architects create additional modules or functions thinking they can be used and be useful in future.

## SoC - Seperation of Concerns
SRP and SoC are closely relative to each other. In this principle function responsibiliy should also do one thing very well and be seperated from other layer responsibilties. For example, UI and Business layers should do actions only in scope of their own responsibility.

## CQS - Command-Query Principle
This principle dictates that one function should either query data or operate on data but not both. For example, if have the method `isLoggedIn(username)` responsible for checking loggedin user, then it should not be responsible for logging (perform action) in the user, but only to verify (query data) that user is logged in.

## LOD - Law Of Demeter
Dictates that any class:
* should call its own method
* should call methods of its properties/fields
* methods of its parameters
* methods of objects created within this class

In other words the principle is used to weaken loose coupling and make encapsulation stronger between the classes.

## POLA - Principle of Least Astonishment
Each class behavior has to perform actions in compliance to expectation of the user. Call of its method should not bring unexpected exceptions or perform different behavior. It is used in places where:
* fault-tolerance is required. For example in APIs
* immuatbility state
* CQS actions
* contract based API

## RAP - Reused Abstraction Principle
There should be minimum three extending classes of interface or abstract class to remain seperately. Otherwise extreme amount of abstraction will increase the capability of easy maintenance and add additional complexity to understand. Base steps to acheive the principle:
1. begin with concrete implementation of behavior
2. search for similar behaviors in other places
3. apply "rule of three" principle

