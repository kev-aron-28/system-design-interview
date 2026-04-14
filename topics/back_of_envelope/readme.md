# Back of the envelope

Sometimes you are asked to estimate system capacity or performance requirements using a back-of-the-envelope estimation


## CPU and memry (nanoseconds, microseconds)
L1 cache reference → ~0.5 ns
L2 cache reference → ~7 ns
Main memory (RAM) → ~100 ns

Main memory is already ~200x slower than L1

## Storage (microseconds, milliseconds)
- SSD random read: ~100us (0.1ms)
- Disk seek (HDD): ~10ms

Disk is ~100,000x slower than RAM thats why you:
- Cache aggressively
- Batch writes
- Avoid random disk access

## Network
- Same datacenter: ~0.5 ms
- Cross datacenter(regional): ~ 1-5ms
- Cross continent: ~70-150ms

## Mental shortcuts
Nanoseconds → CPU/cache
Microseconds → SSD
Milliseconds → network / DB
Seconds+ → user-perceived delays

# Power of two

1 KB = 2^10 = 1024 bytes = ~10^3  = Thousand
1 MB = 2^20 = 1048576    = ~10^6  = Milliion
1 GB = 2^30 =            = ~10^9  = Billion
1 TB = 2^40 =            = ~10^12 = Trillion
1 PB = 2^50 =            = ~10^15 = Quadrillion

# Availability numbers

High availability is the ability of a system to be continuosuly operational for a desirably long period of time. Its measured as pecentage
with 100% means a service that has 0 downtime. 

# Examples

## Estimate Twitter QPS and storage requirements                                                            
Assumptions:
- 300 million montly active users
- 50% of users use Twitter daily
- USers posts 2 tweets er day on average
- 10% of tweets contain media
- Data is store for 5 years

# Framework

1. Clarify the system
- What feature
- What scale
- What matters the most
 
2. Pick a base number
You need one strong assumption:
Users:
- Small: 1M
- Medium: 10M
- Large: 100M - 1B

Requests per user:
- Low 1/day
- Medium 10/day
- High 100+/day

3. Derive traffic

Convert everything per second

QPS = (users x actions per day) / 86400

Example:
- 10M users
- 10 actions/day 

Seconds in a day = 86400 ~ 10^5

Actions per day = 10^7 * 10 = 10^8 actions/per day
QPS = 10^8 / 10^5 = 10^3 ~ 1000 QPS

4. Estimate data size

Total data = number of events x size per event

Example:
100M events / day
Each = 1KB

10^8 x 10^3 = 10^11 bytes
~ 100 GB/day
