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

