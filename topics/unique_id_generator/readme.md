# Unique ID

The first thought might be to use a primary key with the auto_increment attribute database. However auto_increment does not
work in a distributed environment because a single databse server is not large enough and generating unique IDS across muitple
database is challenging

- IDs must be unique and sortable
- The increments by time 
- Only numerical values
- Should fit into 64-bit
- 