# System Design Learning Journal

A public learning repository where I document my journey from **system design fundamentals** to designing scalable, reliable, and maintainable software systems.

The goal of this repository is not to collect textbook definitions. I want to understand each concept well enough to explain it in simple language, connect it to a real-world example, sketch the architecture, and practise it with a small design exercise.

## Current Progress

| Day | Topic | Status |
|---|---|---|
| Day 01 | System Design Fundamentals | Complete |
| Day 02 | Client–Server, Networking & APIs | Planned |
| Day 03 | Databases & Data Modeling | Planned |
| Day 04 | Caching | Planned |
| Day 05 | Load Balancing & Scaling | Planned |
| Day 06 | Message Queues & Asynchronous Processing | Planned |
| Day 07 | Reliability, Availability & Fault Tolerance | Planned |

## Day 01 — System Design Fundamentals

My first day focuses on understanding **why system design exists** and how a simple application can evolve as traffic, data, and system complexity increase.

Topics covered:

- What system design means
- Why systems need to scale
- Distributed systems
- Vertical scaling
- Horizontal scaling
- Preparing for traffic spikes
- Single Points of Failure (SPOF)
- Backups and redundancy
- Load balancing
- Microservices
- Decoupling
- Logging and observability basics
- High-Level Design (HLD)
- Low-Level Design (LLD)
- Scalability, availability, reliability, fault tolerance, performance, complexity, and cost

Read the complete Day 01 note here:

**[docs/day-01-system-design-basics.md](docs/day-01-system-design-basics.md)**

## How I Learn

For every topic, I try to follow the same process:

1. Understand the idea.
2. Explain it in my own words.
3. Connect it to a real-world example.
4. Draw or describe a simple architecture.
5. Identify trade-offs.
6. Practise with a small design problem.
7. Write down what I still need to learn.

My rule is simple:

> If I cannot explain a concept without reading the definition again, I probably have not understood it well enough yet.

## A Simple Example: Scaling an Application

A small application may begin like this:

```text
Users
  |
  v
Application
  |
  v
Database
```

This can work perfectly well at a small scale.

If traffic grows, the application layer may become a bottleneck. One possible evolution is:

```text
              Users
                |
                v
          Load Balancer
          /     |      \
         v      v       v
      App 1   App 2   App 3
          \     |      /
                v
             Database
```

This immediately creates new design questions:

- How should requests be distributed?
- What happens if one application server fails?
- What happens if the database fails?
- Where should logging be added?
- Do we need a database replica?
- Which workloads should be separated?
- Which parts need to scale independently?

These questions are the beginning of system design thinking.

## Beginner Capacity Calculation

System design becomes easier when architecture decisions are connected to numbers.

Suppose an application has:

- 10,000 daily active users
- Each user makes about 20 requests per day

Estimated daily requests:

```text
10,000 users × 20 requests
= 200,000 requests/day
```

Average requests per second:

```text
200,000 / 86,400
≈ 2.3 requests/second
```

Average traffic alone is not enough for production planning. If peak traffic is 10× the average:

```text
2.3 × 10
≈ 23 requests/second at peak
```

This simple calculation gives me a starting point for thinking about server capacity, scaling, caching, database load, and load balancing.

## HLD vs LLD

| High-Level Design | Low-Level Design |
|---|---|
| Overall architecture | Internal component design |
| Services | Classes and interfaces |
| Major data flow | Detailed logic |
| Databases and external systems | Methods and objects |
| How components communicate | How each component works |

A simple memory trick:

**HLD is the city map. LLD is the detailed plan of the buildings and roads.**

## Repository Structure

```text
system-design-learning-journal/
|
|-- README.md
|-- docs/
|   `-- day-01-system-design-basics.md
`-- .gitignore
```

As the learning journey grows, each day will be added as a separate Markdown file.

## Learning Roadmap

The long-term goal is to move from individual concepts to complete system design case studies.

Planned areas include:

- Client–server architecture
- HTTP, HTTPS, DNS, ports, and APIs
- SQL vs NoSQL
- Indexing and database scaling
- Caching strategies
- CDNs
- Load balancers
- Message queues
- Event-driven architecture
- Replication and partitioning
- Consistency and availability
- Rate limiting
- Authentication and authorization
- Monitoring, logging, and alerting
- Fault tolerance and disaster recovery
- Microservices trade-offs
- System design calculations
- End-to-end case studies

## Practice Strategy

For every major concept, I plan to ask:

- What problem does this solve?
- When would I use it?
- What are its limitations?
- What can fail?
- How does it affect performance?
- How does it affect cost?
- What changes when traffic grows?
- Can I explain the design to another beginner?

## Purpose of This Repository

This repository serves as:

- My daily system design learning log
- A revision resource
- A place to practise technical writing
- A record of architecture exercises
- A foundation for future system design case studies

The objective is progress, not pretending to know everything from day one.

---

### Repository Status

**Started:** September 2026  
**Level:** Beginner → Intermediate → Advanced  
**Focus:** Practical system design, architecture reasoning, and clear explanations
