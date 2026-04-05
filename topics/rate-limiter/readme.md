# Rate limiter
A rate limiter is used to control the rate traffic sent by a client or service. In the HTTP world a rate limiter limits the
number of client request allowed to be sent over a specified period. If the api requests count exceeds the threshold defined by
the rate limiter, all the excess calls are blocked.

Benefits:
- PRevent resource starvation caused by Denial Of service attack.
- Reduce cost. Limiting excess requests means fewer servers and allocating more resources to high priority APIs. Its important if you use Third party APIs.

Requirements:
- Accurately limit excessive requests
- Low latency.
- Use as litte memory as possible
- Distributed rate limiting. The rate limiter can be shared across multiple servers or processes
- Exception handling
- High fault tolerance

WHere to put the rate limiter?
Can be put either client or server-side
- Client: Is an unreliable place to enforce rate limiting becuase client can easily 
- Server: Usually in the Api gateway

# Algorithms for rate limiting
Rate limiting can be implemented using different algorithms and each of them has distinct pros and cons
Here is a list:
- Token bucket
- Leaking bucket
- Fixed window counter
- Sliding window log
- Sliding window count

# Token bucket algorithm
A token bucket is a container that has pre-defined capacity. Tokens are put in the bucket at preset rates periodically. Once its full no more tokens
are added. Each request consumes one token. When a request arrives, we check if there are enough tokens in the bucket, each request consumes one token

The token bucket algorithm takes two parameters
- Bucket size
- Refill rate

How many buckets do we need? It is usually necessary to have different buckets for different API endpoints

# Leaking bucket algorithm
The leaking bucket algorithm is similar to the token bucket except that requests are processed at a fixed rate. It is usually implemented with a FIFO queue.

- When a request arrives, the system checks if the queue is full, if it is not full, the request is added to the queue
- Otherwise, the request is dropped
- Request are pulled from the queue and processed at regular intervals

It takes two algoritms
- Bucket size: The queue holds the requests to be processed at fized rate
- Outflow rate: it defines how many requests can be processed at a fixed rate

# Fixed window counter algorithm
The algorithm divides the timeline into fix-sized time windows, and assign a counter for each window. Each request increments the counter by one. ONce the request counter reaches the pre-defined
threshold, new requests are dropped until a new time window starts

A major problem with this algorithm is that a burst of traffic at the edges of time windows could cause mroe requests that allowed quota to go through.

# Sliding window log algorithm
Instead of counters you store exact timestamp of each request. The algorithm keeps track of timestamps. Timestamp data is usually kept in cache.
WHen a new request comes in, remove all the outdated timestamps.
Outdated timestamps means: Those older than the start of the current time window
if the log size is the same or lower than the allowed count, a request is accepted. Otherwise is rejected
Rate limiting implemented by this algorithm is very accurate 

# Sliding window counter algorithm
Hybrid approach that combines the fixed window countera algorithm and sliding window

In case a request is rate limited, API's return a HTTP response code 429 (too many requsts)

# Rate limiter headers
- X-RateLimit-Remaining: The remaining number of allowed request within the window
- X-RateLimit-Limit: It indicates hwo many calls the client can make per time window
- X-RateLimit-Retry-After: The number of seconds to wait until you can make a request again without being 