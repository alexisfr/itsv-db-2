# Redis
Redis is an open source, advanced key-value store and an apt solution for building highperformance, scalable web applications.


- Redis holds its database entirely in the memory, using the disk only for persistence.

- Redis has a relatively rich set of data types when compared to many key-value data stores.

- Redis can replicate data to any number of slaves.

## Redis Advantages
Following are certain advantages of Redis.

- Exceptionally fast − Redis is very fast and can perform about 110000 SETs per second, about 81000 GETs per second.

- Supports rich data types − Redis natively supports most of the datatypes that developers already know such as list, set, sorted set, and hashes. This makes it easy to solve a variety of problems as we know which problem can be handled better by which data type.

- Operations are atomic − All Redis operations are atomic, which ensures that if two clients concurrently access, Redis server will receive the updated value.

- Multi-utility tool − Redis is a multi-utility tool and can be used in a number of use cases such as caching, messaging-queues (Redis natively supports Publish/Subscribe), any short-lived data in your application, such as web application sessions, web page hit counts, etc.

### Installing Redis
Run:
```
docker run -d --name redis -p 6379:6379 redis
```
To run commands
```
docker exec -it redis /bin/sh
redis-cli
```
### Configuration
https://www.tutorialspoint.com/redis/redis_configuration.htm

### Strings
Please do the tutorials:

https://www.tutorialspoint.com/redis/redis_keys.htm
https://www.tutorialspoint.com/redis/redis_strings.htm

### Hashes
https://www.tutorialspoint.com/redis/redis_hashes.htm

### Lists
https://www.tutorialspoint.com/redis/redis_lists.htm

### Sets
https://www.tutorialspoint.com/redis/redis_sets.htm

### Channels
https://www.tutorialspoint.com/redis/redis_pub_sub.htm

### Backup
https://www.tutorialspoint.com/redis/redis_backup.htm


## Exercises

1 - Install Redis with docker.    
*  Add port mapping.
*  Set up a bindmount volume.
    
2 - Connect to Redis and run basic commands
*    Write the command to connect using the cli
*    do a ping
*    get config values
*    etc
    
3 - Write examples with string

4 - Write examples with hashes

5 - Write examples with Lists

6 - Write examples with Sets

7 - Write examples with Sorted Sets

8 - Write examples using Publish Subscribe

9 - Write examples using Transactions

10 - Investigate backups

11 - Investigate Benchmarks - Run some

12 - Write a driver application (client) in Python, do some operations with it.
