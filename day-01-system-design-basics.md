# Day 01 — System Design Basics

**Date:** 22 September 2026  
**Level:** Foundation / Beginner  
**Goal:** Understand how software systems are structured, scaled, separated, and made more reliable.

## 1. What is System Design?

System design is the process of deciding how the major parts of an application should be organized and how they should work together.

For a small application, one program and one database may be enough. As the application grows, we need to think about:

- Traffic
- CPU and memory
- Database load
- Failures
- Backups
- Response time
- Security
- Monitoring
- Capacity

A simple way to think about it:

```text
User -> Application -> Services -> Database / External Services
```

System design is not only about drawing boxes. It is about explaining **why each box exists and what problem it solves**.

## 2. Why Do We Need System Design?

Growth introduces new problems:

- More users send requests at the same time.
- A single server may not have enough resources.
- A database can become overloaded.
- Servers and services can fail.
- Some operations are slower than others.
- Different teams may need to own different parts of a product.
- Data needs to be recoverable.

The purpose of system design is to think about these problems before they become production incidents.

## 3. Distributed Systems

A distributed system has different parts of the application running on different computers, processes, or services.

```text
              Application
           /       |       \
          v        v        v
     Service A  Service B  Service C
        |          |          |
       Data       Data    External API
```

Example: an online shopping platform may separate orders, payments, and delivery.

### Benefit

Different parts can sometimes be deployed or scaled independently.

### Trade-off

The system now has:

- Network communication
- More failure points
- More monitoring
- More operational complexity

## 4. Scaling

Scaling means increasing a system's ability to handle more work.

The work may be:

- Users
- API requests
- Transactions
- Files
- Data processing

### 4.1 Vertical Scaling

Vertical scaling means making one machine stronger.

```text
4 CPU cores + 8 GB RAM
          |
          v
16 CPU cores + 32 GB RAM
```

**Simple memory trick:** Vertical = make the machine bigger.

### 4.2 Horizontal Scaling

Horizontal scaling means adding more machines or application instances.

```text
              Users
                |
                v
          Load Balancer
          /     |      \
         v      v       v
      Server 1 Server 2 Server 3
```

**Simple memory trick:** Horizontal = increase the number of machines.

## 5. Vertical vs Horizontal Scaling

| Question | Vertical | Horizontal |
|---|---|---|
| What changes? | Machine resources | Number of machines |
| Example | More CPU/RAM | More application servers |
| Easy to begin? | Usually yes | Requires traffic distribution |
| Main limitation | One machine has a limit | Architecture becomes more distributed |

## 6. Preparing for Heavy Traffic

If traffic patterns are predictable, capacity can be prepared before the spike arrives.

Possible techniques include:

- Cache frequently requested data.
- Pre-process work that does not need to happen during the request.
- Add capacity before a predictable traffic spike.
- Distribute requests across multiple instances.
- Move slow background work away from the immediate request when appropriate.

## 7. Single Point of Failure

A Single Point of Failure (SPOF) is a component whose failure can cause a major part of the system to stop working.

```text
Application
    |
    v
One Database
    X
```

If the application depends on only one database, a database failure can affect the entire application.

### Backup vs Redundancy

- **Backup:** mainly helps with recovery.
- **Redundancy:** can help the system continue operating after a component fails.

They are related, but they solve different problems.

## 8. Load Balancer

When several application servers exist, incoming traffic needs to be distributed.

```text
Clients
   |
   v
+---------------+
| Load Balancer |
+---------------+
   |    |    |
   v    v    v
 App1 App2 App3
```

A load balancer can also use health checks so unhealthy instances are not used for normal traffic.

## 9. Microservices

Microservices divide a larger application into smaller services, usually based on business responsibility.

```text
             Food Platform
          /       |       \
         v        v        v
      Orders   Payments  Delivery
```

The goal is not simply to create many services.

The important idea is to separate responsibilities when independent development, deployment, ownership, or scaling is useful.

### Trade-offs

Microservices can introduce:

- Network calls
- Distributed debugging
- Service discovery
- Deployment complexity
- Monitoring requirements
- Data consistency challenges

## 10. Decoupling

Decoupling reduces unnecessary direct dependency between components.

Tightly coupled:

```text
Service A -> Service B -> Service C
```

A more loosely coupled asynchronous flow may use a queue:

```text
Service A -> Message Queue -> Service B
                         \-> Service C
```

Decoupling does not mean components never communicate. It means dependencies are designed carefully.

## 11. Logging

Large applications need records of important events.

Example:

```text
2026-09-22 16:30:01 | user=101 | order=5001 | status=CREATED
```

Logs are useful for:

- Debugging
- Monitoring
- Failure investigation
- Tracing user problems
- Understanding application behaviour

## 12. High-Level Design

High-Level Design (HLD) shows the big picture.

```text
User
 |
 v
Load Balancer
 |
 +---- App Server 1
 +---- App Server 2
 |
 v
Database
```

At the HLD level, I ask:

- What are the major components?
- Where does data live?
- How does the client reach the backend?
- How is traffic distributed?
- What external systems are involved?
- What happens when a major component fails?

## 13. Low-Level Design

Low-Level Design (LLD) goes inside a component.

Example:

```text
User
|-- id
|-- name
|-- email
`-- login()
```

LLD includes things such as:

- Classes
- Interfaces
- Methods
- Objects
- Database tables
- Detailed API behaviour

**Memory trick:** HLD is the city map; LLD is the detailed plan for the buildings and roads.

## 14. Quality Questions

A good design should consider:

- **Scalability:** What happens when usage grows?
- **Availability:** Can users still access the system if something fails?
- **Fault tolerance:** Can the system continue or recover after failures?
- **Reliability:** Does the system behave correctly and consistently?
- **Performance:** How quickly does it respond?
- **Complexity:** Will the architecture be difficult to operate?
- **Cost:** What infrastructure and operational resources are required?

## 15. Workload Separation Example

Imagine three chefs:

```text
Chef 1 -> Pizza
Chef 2 -> Bread
Chef 3 -> Pizza
```

If pizza orders increase, more pizza capacity may be needed. Bread capacity does not automatically need to increase.

The same idea applies to software. Different workloads can have different scaling needs.

## 16. Beginner Capacity Calculation

Suppose:

- 10,000 daily active users
- 20 requests per user per day

Daily request count:

```text
10,000 x 20 = 200,000 requests/day
```

Average requests per second:

```text
200,000 / 86,400 ≈ 2.3 requests/second
```

If peak traffic is roughly 10 times the average:

```text
2.3 x 10 ≈ 23 requests/second
```

This is not a complete production capacity plan. It is a beginner calculation that connects traffic estimates to architecture decisions.

## 17. What I Learned

A small application can start with one server.

As usage grows:

1. More capacity may be needed.
2. Scaling becomes important.
3. Multiple servers may require load balancing.
4. Failures require redundancy and recovery planning.
5. Large applications may separate responsibilities.
6. Distributed components need good communication and observability.

The useful part is not memorizing isolated definitions. It is understanding how one design problem leads to the next design decision.

## 18. Self-Test

- What problem is system design trying to solve?
- When should vertical scaling be considered?
- What changes in horizontal scaling?
- Why is a load balancer useful?
- What is a Single Point of Failure?
- What is the difference between backup and redundancy?
- Why might an application use microservices?
- What problems can microservices introduce?
- What does decoupling mean?
- Why do production systems need logging?
- What belongs in HLD?
- What belongs in LLD?

## 19. Hands-On Exercise

Design a small food-ordering application twice.

### Version 1

```text
Users -> Application -> Database
```

### Version 2

```text
Users -> Load Balancer -> App 1 / App 2 / App 3 -> Database
```

Then answer:

- Where is the bottleneck?
- What happens if App 2 fails?
- What happens if the database fails?
- Where could redundancy be added?
- Which workload could become a separate service?
- Where should logging be added?

## 20. Reflection Template

**What I understood clearly:**  
_Write here._

**What I found confusing:**  
_Write here._

**What example helped me:**  
_Write here._

**What I need to practise:**  
_Write here._

**What I want to learn next:**  
_Write here._
