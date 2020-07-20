---
title: Software Architecture
image: /assets/img/blog/sea-star-featured.jpg
description: >
  Software architecture refers to the fundamental structures of a software system and the discipline of creating such structures and systems.
---

# Software Architecture

[Software Architecture Patterns](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/)

The way the highest level components are wired together.

Questions the architect has:

  Which are the applications boundaries?
  Design Driven Design (DDD) has a set of tools that help answer this question

## Fowler Architecture Topics
  - Application architecture
    * Application Boundary
    * Microservices
    * Serverless
    * Micro Frontends
    * GUI Architectures
      - MVC
      - MVP
    * Presentation Domain Data Layering
  - Enterprise architecture

## Architecture Styles
- CQRS (Command Query Responsibility Separation)
  - separates reading and writing into two different models
  - each method is a command or a query
- Event Source
- Microservices



### Microkernel

### Resource Centered

Uses REST Apis to exchange data.

### Object Oriented

SOA and Microservices related

Encapsulates data  (object state)
Provide Operations (methods)
Client binds a distributed object through a proxy loaded in client machine
Marshal/Serialize and unmarshal/deserialize

### Layered architecture
  * Related with Monolithic
    - software application is designed to work as a single, self-contained unit
    - Advantages
      - Better performance
      - Easier to debug, test and deploy
      - Works well for small applications
    - Caveats
      - codebase growth
      - hard to maintain
      - commitment to a particular programming language
  * built around database
  * Examples of structures that use layered architecture
    - MVC (Model-View-Controller)
      * Model
        - Business Logic
        - Information about types stored in Database
      * View
        - HTML/CSS/Javascript
      * Controller
        - Transform data between view and model
        - Provides access to users
    - MVP
    - MVVM
  * Advantages
    - Separation of concerns
  * Caveats
  * Best for
    - new applications
    - need to mirror traditional IT departments and processes

### EDA (Event-Driven Architecture)
  * central unit (broker) accepts events and delegates them to the modules interested
  * Advantages
    - Adaptable to chaotic environments
    - Scale easily
    - Extendable
  * Caveats
    -
  * Best for
- Microkernel architecture
  * Plug-in architecture
  * Philosophy
    - Push basic tasks to the microkernel.
    - Different business units write plug-ins for the different features.
    - Plug-ins call basic functions in the kernel.
  * Caveats
    - Kernel is difficult to change once there are modules depending on it.
    - Difficult to decide the main features of Kernel.
    - Plug-ins need to handshake with kernel before starting working.
  * Best for
    - Product-based applications
      * Operating Systems
      * Desktop Applications
    - Applications with clear division of features

### Microservice architecture
  * SOA (Service Oriented Architecture) done right!
  * software applications using small, autonomous, independently versioned, self-contained services.
    - small, focused services
    - well-defined service interfaces
    - Autonomous and independently deployable services
    - Independent data storage
    - Better fault isolation
  * Similar to event-driven and microkernel architectures
  * Used when tasks are easily separated
  * Concepts
    - API gateway
      - where incoming requests are commonly handled (entry point to the system)
  * Advantages
    - Easy scaling
  * Caveats
    - Communication costs may be significant
      * API requests
      * Web pages
  * Best for
    - websites with small components
    - corporate data centers with well-defined boundaries

### Space-based/Cloud architectures
  * split up processing and storing
  * supports unpredictable spikes by eliminating database as the bottleneck
  * Caveats
    - difficult transactional support
    - difficult to test
    - cache data without corrupting multiple copies is difficult
  * Best for
    - high volume data
    - low value data that can be lost occasionally
    - social networks

## Infrastructure Patterns
- Monolithic
- Microservices
- Serverless
  - BaaS
  - FaaS
  - IaaS
  - method of providing backend services without servers
  - users are charged based on usage
