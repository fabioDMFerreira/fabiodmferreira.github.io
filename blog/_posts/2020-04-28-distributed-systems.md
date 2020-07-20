---
title: Distributed Systems
image: /assets/img/blog/sea-star-featured.jpg
description: >
  A distributed system in its most simplest definition is a group of computers working together as to appear as a single computer to the end-user.
---

# Distributed Systems

```
- What is?
  - Collection of autonomous computing systems
  - Single coherent system
  - Uses Middleware to interconnect systems
    - Layer that connects distributed systems
    - Functions
      - Communication
        - RPC
          - Drawback: caller and callee have to be available
        - Messaging
          - Message-oriented middleware (MOM)
          - Pub/Sub Systems
            - Streaming
      - Transactions
        - ACID
          - Atomic (transaction is indivisible)
          - Consistent (does not violate the system)
          - Isolated (concurrent transactions don't interfere with each other)
          - Durable (once committed the change is permanent)
      - Service Composition
      - Reliability
        - message sent by a process is received by all or no process
    - 4 ways to integrate distributed systems
      - File Transfer
      - Shared DB
      - RPC
      - Messaging
    - Organization
      - wrapper/adapter
        - offers interface to client
        - Broker
          - centralized component
          - handles all the accesses between different applications
          - system based on messages
      - interceptor
        - break the usual flow of control and allow other code to be executed

- Design Goals
  - Support resource sharing
  - Making Distribution Transparent
  - Being open
    - offers components that are easy to use
    - Interoperability, composability and extensibility
  - Being scalable
    - Partitioning and distribution
    - Replication
      - Caching
      - Leads to consistency problems
  - Pitfalls (false assumptions)
    * The network is reliable
    * The network is secure
    * The network is homogeneous
    * The topology does not change
    * Latency is zero
    * Bandwidth is infinite
    * Transport cost is zero
    * There is one administrator

- Types
  - High Performance Distributed Computing
  - Distributed information systems
    - EAI - Enterprise Application Integration
  - Pervasive Systems (mobile and embedded systems)

- Characteristics
  - way components are connected
  - how data is exchanged (format)
  - how elements are jointly configured in the system

- Data communication
  - Communication message protocols
    - REST API
    - gRPC
    - AMQP (asynchronous communication)
  - procedure calls like REST and RPC
  - message passing
  - streaming data

- Controller (or connector)
  - mediates communication
  - coordinate
  - cooperate

- Design Patterns
```

## Systems Architectures

- Different from software architectures
- Project where the components are placed across multiple machines
- Centralized
  - Client Server Model
  - Multitiered architectures
- Decentralized - P2P
  - Architectures styles
    - Structured P2P
      - nodes organised in a deterministic topology
        - ring
        - binary tree
        - grid
      - each node is responsible for storing pairs
      - system implements a distributed hash table (DHT)
      - reduced lookups once every node knows where data is stored
      - scalability issues on DHT
    - Unstructured P2P
      - each node maintains a list of neighbours
      - Communication Patterns
        - Flooding
          - node passes requests to all neighbour nodes
          - request has TTL (time to live)
        - Random walk
          - randomly request information to a neighbour node
    - Hierarchically Organised P2P
      - peers have different roles
      - super peers
        - special nodes that index data items
- Hybrid Architectures

## System Characteristics

* Scalability
```
    Ability to handle a growing amount of work
    Metrics
      Performance/Latency/Response Time
        Amount of useful work compared to the time and resources used
        Response time for a given piece of work
        High Throughput
        Low utilization of computing resources
      Availability/Fault Tolerance


    Limitations
      Number of processing nodes
      Distance between nodes

    How do we scale?

        Increase Resources
          more nodes
          more space
          more speed
          more data
          same latency
        Expand geographic availability
        Partitioning
          slice dataset into smaller independent sets
          reduces impact of dataset growth
          pros
            limit the amount of data to be examined
          cons
            partition may becoming unavailable, slow or unresponsive
        Replication
          copies of the same data in multiple machines
          pros
            more servers to take part on computation
            improves availability
          cons
            all replicas must be consistent
```
* Availability
```
    time a system is in function
    system with high availability is fault-tolerant
    clients can always read and write
    Performance
      short response time
      high rate of processing
      low usage of resources
```

* Consistency
```
    Consensus
    each client always has the same view of data
    every replica sees every update in the same order (high consistency)
```
* Latency
* Throughput
* Redundancy
* Maintainability
* Reliability

### The CAP Theorem

System just can have two properties of three at once
  Consistency
    all nodes see the same data at the same time
    What you read and write sequentially is what is expected
  Availability
    node failures do not prevent survivors from continuing to operate
    the whole system does not die — every non-failing node always returns a response.
    Distributed systems which put availability forward
      BASE (properties of database transactions)
        <b>B</b>asically <b>A</b>vailable — The system always returns a response
        <b>S</b>oft state — The system could change over time, even during times of no input (due to eventual consistency)
        <b>E</b>ventual consistency — In the absence of input, the data will spread to every node sooner or later — thus becoming consistent
  Partition Tolerance
    the system continues to operate despite message loss due network and/or node failure
    the system continues to function and uphold its consistency/availability guarantees in spite of network partitions
    you cannot have consistency and availability without partition tolerance.

  CA system
    stops accepting writes if the majority of nodes fail
    Full strict quorum protocols (Two/multi phase commit)
    RDBMS
  AP system
    Conflict resolution protocols (Gossip, Dynamo)
    Cassandra, Riak, Voldemort
  CP system
    majority quorum protocols (paxos/raft)
    Zookeeper, Couchbase, Redis and HBase

### ACID
Properties of database transactions
  Atomicity
  Consistency
  Isolation
  Durability

### Key Properties
* System Model (async/sync)
* Failure Model (crash-fail, partitions, byzantine)
* Consistency Model (strong, eventual)


## Replication

* Leader election
* Failure detection
* Mutual exclusion
* Consensus
* Global Snapshots
* Atomic Broadcast

### Consistency models (Preventing divergence)

* Communication models
  - 1n messages (async primary/backup) update | master/slave replication
  - 2n messages (sync primary/backup) update and aknowledgment
  - 4n messages (2 phase commit, multi-paxos)
  - 6n messages (3 phase commit, paxos with repeated leader election)
NOTE: Where *n* is the number of system nodes


#### CA model

2 phase commit
```
1. Voting
  - Coordinator sends updates to every node
  - each process votes whether to commit or abort
  - commits are stored in temporaty table
2. Decision
  - the coordinator decides the outcome based on nodes votes
  - communicate decision to every node
```

Characteristics
```
No node can fail.
  If a node fails there are no writes.
    Nodes are writing for all participants to be available
Latency
  system is as fast to write as the slowest node
```

#### CP model

Raft/Paxos/ZAB (Zookeeper Atomic Broadcast)

Characteristics
```
Majority Decisions
  If a partition network fails
    the partition with more nodes will continue active and writing updates
Node Roles
  Leader
  Other Nodes
    forward messages to the leader
Epoch/Term
  During one epoch only one node is designated leader
```      

### Availability Models (accepting divergence)

Consistency in these models exist but it's designated `weak consistency`.

Distributed Programming is about dealing with:
```
* information that travels the speed of light
* independent things failing independently
```

* Types of systems that provide eventual consistency
  - with probablistic guarantees
    - conflicts are solved by overwrite the newer value (ex. Amazon Dynamo)
  - with strong guarantees
    - results converge to a common value
    - Disorderly Programming
      - CRDTs (convergent replicated data types)
      - CALM (consistency as logical monotonicity)
        - monotonicity
          - something that can be executed safely without coordination

```
Dynamo
consistent hashing - keys are mapped to nodes
partial quorums - R + W > N
conflict detection and read repair - detect conflicts at read time
Replica Synchronization
  Gossip - method to reach nodes in the system
  Merkle Trees - technique to identify which nodes need synchronization
```

## Computing

* Threads
* Vitualization

### Types
* Services
Online Systems
Response time
Availability

* Batch Processing
Offline systems
Throughput
  time it takes to crunch through an input database

* Stream Processing
Operate some moments after an event happen

You split your huge task into many smaller ones, have them execute on many machines in parallel, aggregate the data appropriately and you have solved your initial problem.
Algorithm: MapReduce (by Google)
  Apache Hadoop is based in this algorithm
New techniques
  Lambda architecture and Kappa architecture
  Kafka Streams, Apache Spark, Apache Storm, Apache Samza


## Communication

1. RPC
2. Message-oriented communication
3. Multicast Communication
  Application-level - tree-based - multicasting
  Flooding - based multicasting
  Gossip - based data dissemination

Messaging systems provide a central place for storage and propagation of messages/events inside your overall system. They allow you to decouple your application logic from directly talking with your other systems.
RabbitMQ
Kafka

## Storage

### Consistency and Replication

Enhance reliability and performance
Creates inconsistency
  called divergence
    happens when there are multiple different copies in the system
  Solution is relax consistency
    Numerical Deviation between replicas
    staleness Deviation
    Deviations in the ordering of operations
Managing Replicas
  Placement of replicas
  How content is distributed
How replicas are kept consistent?
  Caching Protocols

### Scaling Database

    Master-Slave replication
      Add slave databases only used to read
      Slave machines sync up with the master database
      Only master database is used to write data
      Pitfall: Consistency
        Not consistent because data isn't the same in the multiple machines before the syncing up
    Multi Master replication
      Replicate machines used to write
      Pitfall: may create conflicts like creating two records with same id
    Sharding (partitioning)
      server is spit in multiple small servers (shards)
      each shard hold different records
      rules are created to define to which shards the records go
        should be chosen carefully
          if one shard receives many more records than other shards it receives more requests than others
      Pitfall
        should be avoid until really needed
        A query may need to request records to every shard what makes the query slow

## Naming

1. Flat-naming systems
systems referred by an identifier
need special mechanisms to trace the location

2. Domain Name System
readable names
structured naming
  DNS
  Graph Approach

3. Describe entities by means of various characteristics
Attribute-based naming

## Coordination

Synchronization is about doing the right thing in the right time.

How processes can synchronize and coordinate their actions?
  Coordinate who can access a shared resource
  Cooperate to find when the events happened

Process Synchronization
Data Synchronization

Important that a group of processes can appoint one process as the coordinator
  Leader Election
    used when a coordinator crashes
    used to select super peers

Mutual Exclusion for a shared resource

Gossip based coordination problems
  Aggregation
  Peer Sampling
  Overlay Construction

## Fault Tolerance

Partial failure is a characteristic that distinguishes Distributed Systems.
They are able to continue working even if part of the system fails.
Achieved through redundancy and resilience through process groups.

In a process group there's the necessity to elect a leader to coordinate operations.
The agreement about a leader is done through consensus algorithms.

### Consensus
Paxos Vs Raft

```
atomic multicasting - atomicity
distributed commit protocols
2 phase commit protocol
checkpoints
```

### Circuir Breaker
  The Circuit Breaker can prevent an application from repeatedly trying to execute an operation that’s likely to fail. Allowing it to continue without waiting for the fault to be fixed or wasting CPU cycles while it determines that the fault is long lasting. The Circuit Breaker pattern also enables an application to detect whether the fault has been resolved. If the problem appears to have been fixed, the application can try to invoke the operation.

## Security

Communication
  a secure channel relies on
    Authentication
    Integrity
    Confidentiality
Authorization

## Components

- Load Balancer
- Proxy and Reverse Proxy
- Caching
- Leader election
- Rate Limit
- Hashing
- Relational Databases
- Key-Value stores
- Replication and Partitioning/Sharding
- P2P network
- Polling and streaming and batching
- Publish/subscribe pattern
- Logging Monitoring

## Resources
- Articles
  * [A thorough introduction to distributed systems](https://www.freecodecamp.org/news/a-thorough-introduction-to-distributed-systems-3b91562c9b3c/)
  * [Distributed Systems Basics](http://book.mixu.net/distsys/single-page.html)

- Videos
  * [Distributed Systems in one lesson](https://www.youtube.com/watch?v=Y6Ev8GIlbxc)
  * [Four Distributed Systems Architectural Patterns](https://www.youtube.com/watch?v=tpspO9K28PM)

- Books
  * Design Data-Intensive Applications
    * Foundations
      1. Reliable, Scalable, and Maintainable applications
      2. Data models and Query languages
      3. Storage and Retrieval
      4. Encoding and Evolution
    * Distributed Data
      5. Replication
      6. Partitioning
      7. Transactions
      8. Challenges
      9. Consistency and Consensus
    * Derived Data
      10. Batch Processing
      11. Streaming Processing
      12. Future of data systems

  * Distributed Systems Principles - Andrew Tanenbaum
    1. Introduction
      1. Types
    2. Architectures
      1. Architectural styles
      2. Middleware organization
      3. System architecture
    3. Processes
      1. Threads
      2. Virtualization
      3. Clients and servers
    4. Communication
      1. RPC
      2. Message-oriented
      3. Multicast Communication
    5. Naming
    6. Coordination
      1. Clock
      2. Mutual Exclusion
      3. Election algorithms
    7. Consistency and Replication
    8. Fault tolerance
    9. Security


    ```
        * [Distributed Systems Principles - Andrew Tanenbaum](https://www.amazon.com/Distributed-Systems-Principles-Andrew-Tanenbaum/dp/0130888931)
        * [Design Data-Intensive Applications](https://dataintensive.net/)
    ```

- Courses
  * [Algoexpert - System Design Fundamentals](https://www.algoexpert.io/systems/fundamentals)

- Other Subjects
  - Distributed Programming
  - Security/Cryptography
