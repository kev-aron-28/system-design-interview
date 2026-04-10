# Unique ID

The first thought might be to use a primary key with the auto_increment attribute database. However auto_increment does not
work in a distributed environment because a single databse server is not large enough and generating unique IDS across muitple
database is challenging

- IDs must be unique and sortable
- The increments by time 
- Only numerical values
- Should fit into 64-bit
- Generate 10,000 ids per second

There are some options:
- Multi-master replication
- UUID
- Ticket server
- Twitter snowflake approach

# Multi master replication

This uses the auto_increment feature. Instead of increasing the next ID by 1, we increase it by k where K
is the number of database servers in use

This has some major drawbacks:
- Hard to scale wit multiple data centers
- ID do not go up with time across mutiple servers
- It does not scale well when a server is added or removed


# UUID 
Another easy way to obtain unique IDs, is a 128-bit number used to identify information in computer system. UUIDs can be generated independently without
coordination between servers

This is easy to scale because each web server is responsible for generating IDS they consume

# Ticket server
Flicker developed ticket servers to generate distributed primary keys.
They idea is to use a centralized auto_increment in a single databse server, this is of course a single point of failure as all services
depend on this to get the IDs

# Twitter snowflake approach
This is twitters made.

Divide and conquer is our friend, we divide the ID into different sections

1 bit = sign biy
41 bit = timestamp
5 bits = datacenter ID
5 bits = machine ID
12 bits = sequence number

Datacenter IDs and machine IDs are chosen at the startup time, generally fixed once the system is up an running