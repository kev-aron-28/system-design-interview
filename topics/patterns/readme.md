# Patterns

Patterns are not bets practices that must always be applied. They are also not shortcuts that replace reasoning.

Patterns do not come first, constraints do. Performance requirements, scale expectations, team structure, and operational realities shape
designs. Patterns emerge as responses to these pressures.

# Framework for choosing the right system design pattern


1. Starting with goals and non-goals

Every pattern choice should begin with clarity around goals. 
- What is the system optimizing for?
- What does it explicitly not need to optimize for?
- Stating non-goals helps eliminate unncecessary patterns early

2. Identifying the dominant constraint
Most systems have one dominant constraint at a time. It might be:
- Latency
- Throughput
- Reliability
- Operational simplicity

3. Evaluating tradeoffs before commiting
Before committing to a pattern, strong candidates explain what it improves and what it complicates. This evaluation makes
the decision feel intentional rather than accidental.

# Core architectural patterns in system desing

## Monolithic and microservices architectures
Monolithic and layered architectures group functionality into a single deployable unit or a small number of layers
These patterns emphasize simplicity , ease of deployment and straightfoward debugging

## Event-driven architectures
Event-driven patterns decouple producers from consumers by communicating through events. These patterns improve scalability and flexibility but
complicate debugginb and reasoning about system state

Synchronous = simple, predictable, tight control
- Caller waits for everything
- Strong control flow
- Easy to reason about

Event-driven = Scalable, decoupled, flexible but complex
- Producer does not wait
- Work happens later
- Multiple consumers can react

When is event-driven justified?
- You need decoupling or multiple reactions
Example: User signs up

```
Signup Service:
  → create user
  → send email
  → create billing profile
  → analytics
```

So instead of this bunch you create an event and each service reacts

```
UserCreated event →
  Email Service
  Billing Service
  Analytics Service
```

. You dont want signup tightly coupled to everything
. You can add / revmove consumers without touching core flow

- You need scalability (fan-out)
- You can tolerate eventual consistency
- You need resilience to spikes
- Long-running or unreliable tasks
- Independent evolution of services

When synchronous is better?
- You need immediate consistency

Example:
Payment and then send the order confirmation
or a bank transfer

You must know did it succeed or not?

Here event driven:
. ambiguitity
. race conditions
. reconciliations

- The flow is simpler and linear
- Debugging matters a lot
- Low scale or early stage systems

# Scalability patterns for growing systems

## Horizontal scaling and stateless services
Horizontal scaling relies on adding more instances rather than increasing the size of single machine. Stateless service patterns make 
this possible by ensuring any instance can handle any request.

## Sharding and partitioning patterns

What is sharding? splitting one logical dataset into multiple physical database (shards)
Each shard:
- Handles a subset of data
- Reduces load per node
- Scales horizontally

Why sharding?
You shard when a single DB cant handle:
- Write throughput (too many inserts / updates)
- Storage limits
- Read contention / locks

You have
- Range-based sharding
- Hash-based sharding
- Directory based
- Geo/functional sharding

What changes after sharding?
It changes your entire system behavior?

- Cross-shard queries become hard
- Transactions become distributed
- Rebalancing is painful
- Faiulres mode change


When to shard?
- DB is maxed out on writes
- Dataset is too large for one node
- Clear partition key exists 


## Replication

What is replication?
You keep multiple copies of the same data across different nodes

Typical setup (leader-follower)
- The leader handles writes
- Followers replicate data and serve reads

The main goal: Scale reads without overloading the primary database

Without replication:
ALl reads + writes to one db so it gets bottlenecked

With replication:
- Writes to the leader
- Reads to the follower

# How replication works

1. Synchrous replication
When you write to the leader then you write to the followers immedialty so you get strong consistency
but there will be:
- High latency
- Lower availability

2. Asynchrounous replication
You write to the leader and then replicate to followers later so writing is fast and high availability
but there will be replication lag

## Replication lag
- Read-after-write inconsistency
- stale reads

How to handle this?
1. Read from leader (for critical reads)
2. Read-your-writes routing: route that users reads to leader for a short time
3. Accept eventual consistency

What replication does NOT solve?
- Does not scale writes
- Does not eliminate single point of failure

## Failover
When the leader crashes what happends?

1. Elect new leader from followers
2. Redirect traffic

Problems:
- Some data may be lost
- Split brain risk
- downtime during election

Failure modes introduced by replication:

- Stale data bugs
- Replica inconsistency
- Failover data loss

## When to use it?
- Read-heavy systems
- SOcial media feeds
- Content platforms
- Analytics dashboards

## Replication + Sharding
At scale you combine both

# Performance and latency optimization patterns
- Caching:
This avoids hitting DB
Treadeoffs:
- Stale data
- Cache invalidation
- Consistency issues

Add it when:
- Read-heavy
- Latency sensistive

- Load balacing
Distributes traffic across servers so this helps
- Reduce latency under load
- Enables horizontal scalling

but having sticky sessions is complex

- Asynchrounous processing
Move slow operations out of the request path

- Sharding
Split data across machines, reducing DB latency under heavy load