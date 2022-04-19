# Software Engineering
## SOLID principle
1. S - Single-Responsibility Principle
    1. Each class/function should have single respinsibility or reason to change
    2. Strive for high cohesion and loose coupling
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

## SoC - Seperation of Concerns
SRP and SoC are closely relative to each other. In this principle function responsibiliy should also do one thing very well and be seperated from other layer responsibilties.
