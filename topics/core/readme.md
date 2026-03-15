# What is a server?
A server is nothing but a physical machine where your app code is running.
When you search abc.com in the browser:
1. abc goes to the DNS resolver to find the IP address of the server which corresponds
to this domain. Every server has an IP address
2. Your browser got the IP address of the server. With the help of an IP address your 
browser requests the server
3. Now the server got the response, the server finds the corret application with the help of Port. Then it returns the response

# Latency and Throughput

## Latency
Is the time taken fora single request to travel from the client to the server and back. Also can be seen as a single unit
of work to complete. Usually measured in milliseconds. In simple terms, if a website loads faster, then it takes less time, so it has
low latency. If it loads slower, then it takes more time, so it has high latency.

**Round Trip Time (RTT)**: A replacement word for latency

## Throughput
Is the numberof requests or units of work the system can handle per second. It is tipically measured in requests per second (RPS) or
transactions per second (TPS).

Every server has a limit, which means it can handle X number of requests per section. Giving further load cna chocke it or may go down.
- High: The system can process many requests at the same time
- Low: The system struggles to process many requests concurrently

We want:
1. High throughput
2. Low latency

In short,

- Latency measures the time to process a single request.
- Throughput measures how many requests can be handled concurrently.

# Scaling and its types
Scaling means we need to increase the specs of our machine or add more machines to handle the load

It is of 2 types:
- Vertical Scaling
- Horizontal Scaling

## Vertical (Scale Up/Down)
If we increase the specs of the same machine to handle more load, then its called vertical scaling

## Horizontal Scaling (Scale Out/In)
Adding more machines to handle the traffic distributely. Usually you will have a load balancer to distribute load
across the multiple servers

# Back of the envelope estimation
|Power of 2| Approximate value| Power of 10 | Full name   |  Short name |
|----------|------------------|-------------|-------------|-------------|
|  10      |  1 Thousand      |   3         |  1 Kilobyte |   1 KB      |
|  20      |  1 Million       |   6         |  1 Megabyte |   1 MB      |
|  30      |  1 Billion       |   9         |  1 Gigabyte |   1 GB      |
|  40      |  1 Trillion      |   12        |  1 Terabyte |   1 TB      |
|  50      |  1 Quadrillion   |   15        |  1 Petabyte |   1 PB      |

Will learn to :
- Load Estimation
- Storage Estimation
- Resource Estimation

## Load estimation
1. Ask for DAU
    1. Calculate No of writes
    2. Calculate No. of reads

Example:
If twitter has 100 million daily active users, and one user posts 10 tweets per day
Number of tweets in one day: 100 million * 10 tweets = 1 billion/day
Suppose 1 user reads 1000 tweets per day
Number of reads in one day: 100 million * 1000 tweets = 1000 billions reads per day

2. Storage estimation:
Example:
Tweets are of two types:
- Text
- Text with photo

Suppose only 10% of the tweets contain photos.
Let one tweet comprise 200 chars. 
- 1 char = 2 bytes
- 1 photo = 2MB

- Size of normal tweet: 2 bytes * 200 chars ~= 500 bytes
- Total no. of tweets with photo: 10% of 1 billion = 100 million

So the total storage for each day:
- (500 bytes) * (1 billion) + (2Mb) * (100 million) = 1 TB + 1 PB
Each day we require 1PB of storage


3. Resource estimation:
Here calculate the total number of CPUs and servers required:

Assuming we get 10 thousand requests per second and each request takes 10ms for the CPU to process

- Total CPU time to process: (10,000 r/s) * (10ms) = 100,000 ms per second
- assumming each core of the CPU can handle 1000ms of processing per second. total of cores: 100,000/1000 = 100 cores
- if each server has 4 cores of CPU, the total no of servers required = 100/4 = 25 servers

# CAP theorem
This theorem states a very important tradeoff while designing any system.
CAP:
- C: Consistency
- A: Availability
- P: Partition tolerance

Everything is in terms of distributed systems. Distributed system means data is stored in multiple servers instead of one, as in
horizontal scaling

One individual server that is part of the overall distributed system is called Node, in this system we replicate the same data across different
servers

- Consistency: Every read requests returns the same result irrespective of whichever node we are reading from. This means all the nodes have the
same date at the same time.
- Availability: The system is available and always able to respond to request, even if some nodes fail. This means even if some nodes fail,
the system should continue serving requests with other healthy nodes
- Partition tolerance: The system continues to operate even if there is a communication breakdown or network partition between different
nodes

CAP theorem states that in a distributed system, you can only guarantee two of these properties simultaneously. Its imposible to achieve all three
simultaneously
- CA
- AP
- CP
- CAP impossible

In a distributed system, network partition is bound to happen, so the system should be Partition tolerant. This means for a distributed system, “P” will always be there. We will make a tradeoff between CP and AP.

What to choose, CP or AP?
- For secure apps like banking, payments, stocks, etc, go with consistency. You can’t afford to show inconsistent data.
- For social media etc, go with availability. If likeCount on a post is inconsistent for different users, we are fine.

# Scaling of database
When you reach a certain scale, this database server starts giving slow responses, or may go down because of its
limitations. In that situation, how to scale a database.

Suppose you have a database server and there is a user table. And there are a lot of read requests fired from
the application server to get the user with specific ID. You can do the next to make the read request faster:

## Indexing
If you dont index, the database checks each row in the table to find your data, a full table scan.

With indexing the database uses the index to directly jump to the rows you need, making it much faster.
You make the "id" column indexed then the database will make a copy of that id column in a data structure(B-trees). Now searching
is faster because they are stored in a sorted way such that you can apply binary search kind of thing getting O(log(n))

## Partitioning
Partitioning means breaking the big table into multiple small tables so instead of having users you have now:
- user_table_1
- user_table_2
- user_table_3

WHen your index file becomes very big, then it also starts showing some performance issues when searching on that
large index file. Now after partitioning, each table has its own index, so searching on smaller tables is faster

## Master slave architecture
Use this when you hit a bottleneck, like even after doing indexing, partitioning and vertical scaling, your queires are slow
or your databse cant handle further requests on the single server.
in this way, you replicate the data into multiple servers instead of one.

When you do any read request, your read request will be redirected to the least busy server, in this way you distribute your load

But all the write requests, will only by processed by one server,  this server is called the Master Node and the ones that take the reads
are called Slave nodes

## Multi-master setup
When write queries become slow or one master node cannot handle all the writes requests.

In this, instead of a single master, use multiple master database to handle writes.

in this setup the most challenging part is how you would handle conflicts. If for the same ID, there are two different data present in both
masters, then you have to write the logic in code like, do you want to accept both, override the previous with the latest one, concatenate it,
It totally depends on the business use case.

## Database sharding
Sharding is similar to partitioning, as we saw above, but instead of putting the different tables in the same server, we put it into a different server.
These servers are usually called shards. 

The sharding key should distribute data evenly across shards to avoid overloading a single shard.

### Why sharding is difficult?
In partitioning, (keeping chuncks of the table in the same database server) you dont have to worry about which table to query from.
POstgres handles that for you. But in sharding you have to handle this at the application level.

#### Sharding strategies
1. Range based sharding: Data is divided into shards based on ranges of values in the same sharding key.
Pros: Simple to implement
Cons: Uneven distributino if data is skewed
2. Hash-based sharding: A hash-function is applied to the sharding key, and the result determines the shard
Pros: Ensures even distribution of data
Cons: Rebalancing is difficult when adding new shards, as hash result change
3. Geographic/entity based sharding: Data is divided based on logical grouping
4. Directory based sharding: A mapping directory keeps track of which shard contains specific data.

Disadvantages of sharding:
- Difficult to implement because you have to write the logic yourself to know which shard to query from and in which shard to write data
- Partitions are kept in different servers. So when you perform joins then you have to pull data out from different shards to do the join. It is expensive
- You lose consistency

# SQL vs NoSQL databases
