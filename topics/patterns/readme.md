# Patterns

Patterns are not bets practices that must always be applied. They are also not shortcuts that replace reasoning.

Patterns do not come first, constraints do. Performance requirements, scale expectations, team structure, and operational realities shape
designs. Patterns emerge as responses to these pressures.

# Framework for choosing the right system design pattern


1. Starting with goals and non-goals

Every pattern choice should begin with clarity around goals. 
- What is the system optimizing for?
- What does it explicitly not need to optimize for?
- Stating non-goals helps eliminate unncecessary patterns early

2. Identifying the dominant constraint
Most systems have one dominant constraint at a time. It might be:
- Latency
- Throughput
- Reliability
- Operational simplicity

3. Evaluating tradeoffs before commiting
Before committing to a pattern, strong candidates explain what it improves and what it complicates. This evaluation makes
the decision feel intentional rather than accidental.

# Core architectural patterns in system desing

## Monolithic and microservices architectures
Monolithic and layered architectures group functionality into a single deployable unit or a small number of layers
These patterns emphasize simplicity , ease of deployment and straightfoward debugging


## Event-driven architectures