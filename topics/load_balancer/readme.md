# Loadbalancer 
A load balancer is a component in charge of:
- Receiving request
- Distributing those requests to multiple servers
- It helps improve:
    - Disponibility
    - Scalability
    - Latency
    - Failure torelarance

The client never knows how many servers are behind 

# Algorithms

1. Round robin
It distributes the load in a circular way:
A - B - C - A - B - C

Pros:
- Simple
- Works good if each server is equal

Cons:
- It does not take into account real load
- so a slow server still takes traffic

2. Least connections
It sends the request to the server with least active connetions

Pros:
- Better for variable load
- Best for APis or long requests

Cons: 
- complex
- It does not reflect CPU/memory in realtime


3. Weighted versions
You can combine both of the previous algorithms with weights
So now the algorithm takes into account the weight 


# Health checks
A loadbalancer not only distributes but also detects servers that are not working, there are two types of Health checks
1. Active Health Checks: Here the LB makes requests periodically and if it fails then removes the server from the pool
2. Passive Health checks: Detects fails in traffic, if it finds a lot of 500 then its an unhealty server

# Types of loadbalancers:

1. Layer 4:
It works with IP , TCP/UDP
It does not understand HTTP

Pros:
- Very fast
- Low latency

2. Layer 7
It works in application layer so it understands: HTTP, headers, cookies, paths

Pros:
- Smarter
- Advanced routing

# Common tools
- Nginx
    - Web server
    - Reverse proxy
    - Load balancer
    - SSL termination
    - Caching
- HAProxy
    - 100% loadbalancing
    - Efficient
    - L4 and L7
- AWS ELB
    - Managed by AWS
    - ALB (Application load balancer) on layer 7
    - NLB (Network load balancer) on layer 4
    - CLB (Classic)

