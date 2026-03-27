
# Message queue vs Message stream
Queue:
- Someone must process this job
- Only one then its deleted from the queue

Stream:
- This already happened, anyone can react
- Many can react to this message
- Consumers track their own position

# Message queue pattern


A massage queue decouples producers from consumers by introducing an intermediate buffer (the queue)

The core idea is:
- Producer: Sends messages (does not process)
- Queue (broker): stores messages
- Consumer (worker): processes messages asynchronously

# Why use it?
1. Async processing
2. Scalability
3. Fault tolerance
4. Decoupling

# Challenges
- Retries
IF a job fails you could retry immediatly or retry witha delay
- Dead letter queue (DLQ)
If a message keeps failing


# Idempotency
An operation is idempotent if executing it multiple times produces the same result as executing it once

# Strategies

1. Idempotency Key
Every message has a unique ID

``` json
{
  "id": "txn_123",
  "type": "charge",
  "amount": 10
}
```
and then in the implementatation

``` java
if (db.exists(message.id)) {
    return // already processed
}

process()

db.insert(message.id)
```

Of course this must be atomic

2. Upserts (idempotent writes)
So instead of doing a common insert you prefer to user the insert that takes into account conflicts like

``` sql
INSERT ON CONFLIT DO NOTHING
```

so you can prevent duplicates at db level

3. State-based operations
Instead of do action you have state describing it


# Retries

If you try native retries, which mean, retry inmmediatly after a failure you will end up with:
- System overload
- Cascading failures

To handle this you must use a proper retry strategy 

- Exponential backoff
delay = base * 2^n + random()
- Retry 1: 1s
- Retry 2: 5s
- Retry 3: 30s
- Retry 4: 5min

## Retry classification
1. Transient errors (must retry)
    - Network issues
    - Timeouts
2. Permanents errors (no retry)
    - Invalid input
    - Validation errors


# Ordering
Queues dont always guarantee order
But you can get same operations with different order, then you get a different result

A queue seems to promise order in the way it process the tasks

But if you queue has: M1, M2 and then
- Worker 1 (M1) slow process
- Worker 2 (M2) fast process

so worker 2 finishes M2 first and then worker 2 finishes M1

Queues preserve delivery order, not processing completion order

When we say order we dont mean: The entire system must be ordered but 
the operations for the same thing must be ordered

FOr instance in a bank system:
- We dont care about the operations USer A vs User b order
- We care that the user A operations are ordered

To do this, we split the queues logically
Instead of having a single queue for all users:

User A belongs to queue A
User b belongs to queue B

Of course systems does not create thousands of queues manually
You do Partitioning

Ordering is not:
EVerything happens in order 
Ordering is:
Things that affect the same entity happen in order