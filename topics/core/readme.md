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