# Monolithic architecture

A monolith is a single deployable unit qhere all funciontality lives together:
- UI
- Business logic
- Database access
- Background jobs

Everything is in onde codebase + one deployment

## Advantages
1. Simplicity
    - Easy to understand at the beginning
    - No distributed system complexity

2. Easy deployment
    - Just deploy one thing

3. Strong consistency

## Problems
1. Scaling limitations
You can only scale:
- vertically
- or clone whole app

2. Tight coupling
- Changing one parte cna break others
- Hard to isolate features

3. Slow development over time
- Codebase becomes huge
- Teams step on each other

4. Deployment risk
A small change you have to redeploy the whole system

When to use a monolith:
- Startup
- MVP
- SMall team

# Microservices
A system split into multiple independent services, each responsible for a specific business capability,
communicating over the network


Services talk via:
- HTTP
- gRPC
- Message queues

Advantages:

1. Scaling: Scale only what you need

2. Team ownership: Teams own services

3. Deployment risk: Deploy small parts

Problems:
1. Network latency

2. Failure handling
- retries
- timeouts
- circuit breakers

3. Data consistency 
Distributed transactions
- Eventual consistency
- Sagas
- Events

4. Debugging hell