---
title: Software Development
image: /assets/img/blog/sea-star-featured.jpg
description: >
  Software development is the process of conceiving, specifying, designing, programming, documenting, testing, and bug fixing involved in creating and maintaining applications, frameworks, or other software components.
---

# Software Development


## Programming Languages

## Data Structures

## Algorithms

### Sorting
### Searching

## Recursion

## Programming Paradigms
- Object Oriented Programming
- Functional Programming
  - is the process of building software by composing pure functions, avoiding shared state, mutable data, and it’s side-effects.
  - declarative and not imperative
  - Concepts
    * uses pure functions
      - function which gives the same output for the same input arguments
      - don't depend on state
    * avoid side effects
      - global variables may be side effects of an impure function
    * shared state
      - state shared by multiple functions
    * immutable data
    * function composition
  - uses pure functions
    - do not change application state
    - avoid side effects
- Reactive Programming
   - concerned with asynchronous data streams
   - Stream
      - sequence of ongoing events ordered in time
      - may emit
        - values
        - error
        - completed signal
   - Concepts
    * Observable
      - function that produces a stream of values
      - Cold
        * start produce data when at least one observer subscribes
      - Hot
        * always producing data
        * sends data when some observer subscribes
    * Observer
      - reads values sent by observable
      - lazy, they execute functions when observable emit events
    * Subscription
      - happen when observer starts listening observable
- Declarative Programming
  * expresses the logic of a computation without describing its control flow
- Imperative Programming
  * uses statements that change the application state

## Practices

### Reducing Complexity

* Improves
  - Understandability
  - Maintainability
  - Reusability
  - Testability
  - Flexibility
* Can be achieved through
  - Loose Coupling
    Contrary to tight/strong dependency.
    Modules have weak dependencies that can easily be replaced.
  - High Cohesion
    The degree to which the elements inside a module belong together.
* Results
  - Reduces bugs
  - Better productivity
  - Find easily defects on software

### Techniques

* SOLID
  - Single Responsibility Principle (SRP)
  - Open/Closed Principle (OCP)
  - Liskov Substitution Principle (LSP)
  - Interface Segregation Principle (ISP)
  - Dependency Inversion Principle (DIP)  

* DRY (Don't repeat yourself)
* YAGNI (You Aren't Gonna Need It)
* Separation of concerns (SoC)
* Dependency Injection
  Pattern used to easily replace dependencies
  Promotes
    Flexibility
    Testability
* Testing
  - TDD
  - BDD
* Design Patterns (GOF)
  - Structural Patterns
  - Behavioral Patterns
  - Creational Patterns
