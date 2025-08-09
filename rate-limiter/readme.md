# Design a rate limiter

in a network system, a rate limiter is used to control the rate of traffic
sent by a client or a service, In http world, a rate limter limits the 
number of client request allowed to be sent over a specified period.

If the api request count execeeds the threshold defined by the rate limiter, all
the exess calls are blocked.


## Benefits
- Prevent resource starvation caused by Denial of Service attack.
- Reduce cost, by limitng excess request means fewer serves and allocating 
more resources to high priority.

# Step 1 - Understand the problem and stablish scope

What kind of rate limiter client-side or server-side?

Does the rate limiter throttle API request based on IP, user id or other properties?

What is the scale of the system startup company or small?

Will it work on distributed enviroment?

Is it a separate services or should it be implemented in the code?

Do we inform users that are throttled?

## Requirements
- Accurately limit excess requests
- Low latency. 
