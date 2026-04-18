# Non functiona requirements

1. Latency.

Time it takes to respond

## Techniques
1. Caching

Pros:
- Massive latency reduction
- Reduces DB load

Cons:
- Cache invalidation
- Stale data

2. Scalability
System handles more traffic without breaking

## Technique
. Horizontal scaling

Pros:
- Easy to scale
- Fault tolerant

Cons:
- Requires stateles design

. Load balancing
. Sharding
Pros:
- Handles huge datasets
Cons:
- Complex queries
- Rebalancing pain

. Asynchronous processing

Pros:
- Smooth spikes

Cons:
- Eventual consistency


3. Availability
System works even when parts fail

## Techniques
. Replication
Multiples copies of data

Pros:
- Fault tolerance

Cons:
- Consistency issues

. Failover
Automatic switch to backup

Pros:
- Minimal downtime
Cons:
- Complexity
