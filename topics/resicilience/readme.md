# Circuit breaker pattern

When a service fails (DB, API,) your system can:
- Keep trying
- Saturate resources
- Fail completely

The main idea is: IF something is failing, stop trying temporarly

THere are 3 states:
1. Closed
- Requests go through normally

2. Open
- Fails a lot of times
- Calls are blocked

3. Half-open
- Some requests go through
- If it works then CLOSED
- If fails, then OPEN

# Retry pattern
If it fails, then try again but do smarter

1. Immediate retry
2. Exponential backoff
3. Jitter 