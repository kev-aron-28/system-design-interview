# Consisten hashing

To achieve horizontal scaling, it is important to distribute requests/data efficiently and evenly across servers

## The rehashin problem
If you have N servers, a common way to balance the load is to use the following hash method:

```
serverIndex = hash(key) % N
```

Where N is the size of the server pool

This approach works well when the size of the server pool is fixed, and data distribution is even. However, problems arise when new servers are added

Consisten hashing is a special kind of hashing such that when a hash table is re-sized and consisten hashing is used, only k/n keys need to be remapped on average
where k is the number of keys and n is the number of slots

In contrast, in most traditional hash tables, a change in the number of array slost causes nearly all keys to be remapped

## Hash space and hash ring

Lets assume SHA-1 is used as the hash function f and the output range is : 0x, x1, x2, x3, x4, ..., xn
SHA-1 hash space goes from: 0, to 2^160 so x0 = 0 and xn = 2^160 - 1 


x0 -> ---------------------------------------------------------------------------------------- <- xn

If you connect both ends, we get a hash ring

## Hash servers
Using the same hash function f, we map servers based on server IP or name onto the ring

## Hash keys
One thing worth mentioning is that hash function used here is different from the one in "In the rehashing problem" and there is no modular operation.

## Server lookup

To determine which server a key is stored on, we go clockwise from the key position on the ring until a server is found

## Add a server / remove server
Adding a new server will only require redistribution of a fraction of keys

# Issues with this approach
Two problems are identified with this approach

1. It is impossible to keep the same size of partitions on the ring for all servers considering a server can be added or removed
    A partition is the hash space between adjacent servers

2. It is impossible to have a non-uniform key distributino on the ring

A technique called virtual nodes or replicas is used to solve these problems

## Virtual nodes
A virtual node refers to the real node, and each serveris represented by multiple virutal nodes on the ring.

With virtual nodes, each server is responsible for multiple partitions, as the number of virtual nodes increases, the distribution of keys becomes more balanced
This is because the standard deviation gets smaller with mroe virutal nodes

# Find affected keys
WHen a server is added or removd, a fraction of data needs to be redistributed. how can we find the affected range to redistribute the keys

The affected range starts from the newly added node and moves anticlockwise around the ring until a serveris found. Thus, the keys located between the server behind the newly added server
need to be redistributed to the new server