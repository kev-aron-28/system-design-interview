# Caching
A cache is fasta layer of memory (RAM) that avoids to hit the database

# Why is it important?
- Helps reduce latency
- Scalability
- Increases throughput as it can handle more requests per second
- Reduces the load to the DB

# Patterns

1. Cache Aside (Lazy loading)
Here the backend controls the cache

    1. Request come through
    2. You try to find data in cache
        HIT: Return the data
        MISS: Go to db
    3. Save on cache
    4. Return data

This is pretty simple but you can get cache inconsistency and the first requests is slow

2. Write through
Here you write to DB and then to the cache at the same time.
So the cache is always updated and the reads are fast but you get slower writes and you can fill the cache with 
unnecesary data

3. Write back (Write behind)
First you write to cache and then to DB

    1. App writes to cache
    2. Returns OK response
    3. Worker writes to DB 

Fast writes and high throughput but you get risk on loosing data

# TTL
Each key in the cache can expire with a TTL, this way you can avoid stale data but there is a hard problem

## Cache invalidation
The question is when to delete/update the cache?
There are different strategies

1. Time-based (TTL)

2. Write based invalidation: Each time you update the DB

3. Event based

This depends on the consistency requirements

## Cache stampede
What happens when a bunch of requests arrive when the cache just expired?

1. Locking
Only 1 request goes to DB and then sets the cache

2. Early expiration (refresh ahead)
You refresh before it expires

3. Request coalescing

4. Random TTL

# Stale data
Invalid data on the cache

1. Stale-while-revalidate
here you return the stale data and then refresh on the background

2. Versioning

3. Soft TTL + Hard TTL