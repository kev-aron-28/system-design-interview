# Framework

Many think that system design interview is all about a persons technical design skills
, its much more than that, this gives strong signals about a persons ability to collaborate, work under pressure

# A 4-steps process for effective system design interview

1. Understand the problem and establish design scope

When you ask a question the interviewer either answers your question directly or asks you to make your assumptions

- What specific features arewe going to build?
- How manyu users does the product have?
- How fast does the company anticipate to scale up?


2. Propose a high-level design and get buy-in 
Here aim to develop a high-level design and reach an agreement.
- Come up with an initial blueprint
- Draw box diagrams with key componets on the whiteboard or paper
- Do BOE calculations to evaluate if your blueprint fits the sacle constraints 

3. Design deep dive
Here you should work with the interviewer to identify and prioritize components in the architecture

4. Wrap up

---

0. Restate + scope
The goal here is: align and constrain

- What are the core user actions?
- Any out-of-the-scope features i can ignore?

The output should be:
- 2-3 functinal requirements
- 2-3 non-functional priorities (latency, availability, consistency)

1. Quantify (Back of the envelope)
Turn words into numers

ask:
- DAU? % active per feature
- Read/write ratio?
- avg payload size
- latency target

Compute:
- QPS (reads/writes)
- Storage/day, storage/year
- Peak vs average (x2, x5)


2. Core data model
Goal: What are we storing and how do we access it?

ask:
- What is the primary lookup key?
- What queries must be fast?

Produce a simple table

3. High level architecture
Goal: draw boxes and flows

Components:
- Client
- Load balancer
- Stateles API servers
- Database
- Cache

4. Critical flows (write+read)
Write flow:
- ask where does ID come from?
- any contention point?
Read flow:
- ask how to i make this fast

5. Deep dive on the core problem
Every system has a hard part

1. List options
2. Compare tradeoffs
3. Pick one

6. Scaling strategy
A. Read scaling
ask where are most requests?
- Cache
- CDN


B. Write scaling
Will DB become bottleneck?
- vertical scaling
- sharding

C. DB scaling
Single node enough?
- read replicas
- partitioning

7. Bottlenecks and failure modes 
What breaks first?
- DB overload
- Cache miss storms
- Hot keys

Mitigations:
- Caching
- Rate limiting
- Replication

8. Tradeoffs
- Consistency vs availability
- Simplicity vs scalability
- Cost vs performance