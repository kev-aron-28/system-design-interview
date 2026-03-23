# Database replication
Database replication is a pattern where data from one database (primary) is copied to multiple database (replicas)
with goals of:
- Improve read scalability
- Ensure high availability
- Provide fault tolerance

# Key rule

Primary (Leader): handles writes
Replicas (Followers): handles reads

# Why use replication?
A single DB cannot handle massive read traffic but replicas distribute read load
and of course you get high availability, if the primary fails then promote a replica and it becomes the 
new primary and another advantages is disaster recovery

# Types of replication

A. Synchronous replication
Primary waits for replicas before responding
You get:
- Strong consistency
- No data loss

B. Asynchronous Replication
Here you do not wait for replicas so this is faster but there is eventual consistency and there is
replication lag

# REplication lag
This is the delay between the data written to primary and the data visible in replicas
and its called "Read-after-write inconsistency"

# Handling replication lag
1. Read from primary: after a write read from primary temporarily
2. Sticky sessions
3. Cache (Write-through)
4. Quorum reads

# Replication mechanisms

- Statement based: Replicas execute same SQL
- Row based: replicate row changes
- Log-based: Replicas read logs

# Common problems
- Replication lag
- Split brain
- Failover delay: downtime during promotion
- Write bottleneck

# Replication vs Sharding?
| Feature | Replication | Sharding       |
| ------- | ----------- | -------------- |
| Goal    | Scale reads | Scale writes   |
| Data    | Same data   | Partitioned    |
| Writes  | One node    | Multiple nodes |

Replication scales reads, sharding scales writes.

# When to use 
- Ready-heavy workloads
- Need high availability
- Can tolerate eventual consistency