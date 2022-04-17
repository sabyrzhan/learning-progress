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
    2. Actually subset of polymorphism with addition of IS-SUBSTITUTABLE alogside of IS-A principle
    3. Key violations to look for:
        1. Type checking
        2. Null checking
        3. `NotImplementedException` like exceptions
4. I - Interface segregation principle
    1. Prefer small, cohesive interfaces to large expensive ones
    2. For large interfaces following can be applied:
        1. Interface inheritance
        2. The adapter apttern
5. D - Dependency Inversion Principle
    1. Most classes should depend on abstractions but not on implementation details
    2. Abstractions focus on WHAT, implementation focus on HOW - which means details depend on interfaces not vice versa
    3. Classes should be explicit on their dependencies
    4. Clients should inject dependencies when they create other classes