# Database sharding pattern

It means splitting a large database into multiple smaller databases (shards) each holding a subset of data

# Why sharding?
- A single database cant handle load (CPU, memory, disk)
- High latency with large datasets
- Write bottlenecks
- Lock contention

Benefits:
- Horizontal scalability
- Lower latency per shard
- partial fault isolation

# Shard key
This is a core decision
This decides in which shard to store the data

When you have a bad shard:
- System failure
- Hotspots
- Uneven distribution
- Expensive relabalancing

# Strategies

A. Range sharding
Split data by ranges:

Shard1: user_id 1-1000
Shard2: user_id 1001-2000
Shard3: user_id 2001-3000

B. Hash sharding
Here you use a hash to scatter the data
- Even distribution
- Avoids hotspots
- Great for high write throughput

C. Directory based sharding
 uses a dedicated lookup table to map specific shard keys directly to the physical shard where the data resides

# Rebalancing

What happens when you add a new shard?
ALl data must move now

A. Consistent hashing
map data to a hash ring, so only a portion of data moves

B. Virtual nodes
Each physical shard has multiple positions and improves distribution

C. Manual rebalancing
Move data gradually

# Cross shard queries

Lets say you sharded you database and then try this:

``` java
SELECT * FROM orders from amount > 1000;
```

A. Scatter-gather
1. Query all shards
2. Merge results

B. Denormalization
Duplicate data to avoid joins

C. Global secondary index

If your query does not include the shard key, you are in trouble