# Redis Cheatsheet

*A comprehensive reference for Redis's most commonly used features*

📚 **Official Documentation:** <a href="https://redis.io/docs/" target="_blank">Redis Documentation</a>

## Table of Contents

### 🚀 Getting Started
- [Installation & Setup](#installation--setup)
- [Basic Commands](#basic-commands)
- [Data Types](#data-types)

### 🎯 Core Data Structures
- [Strings](#strings)
- [Lists](#lists)
- [Sets](#sets)
- [Sorted Sets](#sorted-sets)
- [Hashes](#hashes)

### 🔧 Advanced Features
- [Pub/Sub](#pubsub)
- [Transactions](#transactions)
- [Pipelining](#pipelining)

### 🛠️ Administration
- [Persistence](#persistence)
- [Memory Management](#memory-management)
- [Replication](#replication)

### ⚙️ Configuration & Monitoring
- [Configuration](#configuration)
- [Monitoring](#monitoring)

### 📚 Resources
- [Quick Reference Links](#quick-reference-links)

---

## Installation & Setup

📖 <a href="https://redis.io/docs/getting-started/" target="_blank">Getting Started Documentation</a>

```bash
# Install Redis (Ubuntu/Debian)
sudo apt update
sudo apt install redis-server

# Install Redis (macOS with Homebrew)
brew install redis

# Start Redis server
redis-server

# Start Redis server with config file
redis-server /path/to/redis.conf

# Connect to Redis CLI
redis-cli

# Connect to remote Redis
redis-cli -h hostname -p port -a password

# Test connection
redis-cli ping
# Output: PONG
```

## Basic Commands

📖 <a href="https://redis.io/commands/" target="_blank">Commands Documentation</a>

```bash
# Server information
INFO                    # Server info
PING                    # Test connection
SELECT 0                # Select database (0-15)
FLUSHDB                 # Clear current database
FLUSHALL                # Clear all databases
DBSIZE                  # Number of keys in current DB

# Key operations
KEYS *                  # List all keys (avoid in production)
SCAN 0                  # Iterate over keys (production-safe)
EXISTS key              # Check if key exists
TYPE key                # Get key type
TTL key                 # Time to live in seconds
PTTL key                # Time to live in milliseconds
EXPIRE key seconds      # Set expiration
PEXPIRE key milliseconds # Set expiration in ms
PERSIST key             # Remove expiration
DEL key1 key2           # Delete keys
RENAME oldkey newkey    # Rename key
```

## Data Types

📖 <a href="https://redis.io/docs/data-types/" target="_blank">Data Types Documentation</a>

Redis supports several data types:
- **String**: Simple key-value pairs
- **List**: Ordered collections of strings
- **Set**: Unordered collections of unique strings
- **Sorted Set**: Ordered sets with scores
- **Hash**: Field-value pairs (like objects)
- **Stream**: Log-like data structure
- **Bitmap**: String treated as bit array
- **HyperLogLog**: Probabilistic data structure

## Strings

📖 <a href="https://redis.io/docs/data-types/strings/" target="_blank">Strings Documentation</a>

```bash
# Basic string operations
SET key value           # Set string value
GET key                 # Get string value
MSET k1 v1 k2 v2       # Set multiple keys
MGET k1 k2             # Get multiple keys
SETNX key value        # Set if not exists
SETEX key seconds value # Set with expiration

# String manipulation
APPEND key value       # Append to string
STRLEN key             # Get string length
GETRANGE key start end # Get substring
SETRANGE key offset value # Set substring

# Numeric operations
INCR key               # Increment by 1
DECR key               # Decrement by 1
INCRBY key increment   # Increment by amount
DECRBY key decrement   # Decrement by amount
INCRBYFLOAT key increment # Increment by float

# Examples
SET user:1000:name "John Doe"
SET user:1000:age 30
INCR user:1000:visits
GET user:1000:name     # "John Doe"
```

## Lists

📖 <a href="https://redis.io/docs/data-types/lists/" target="_blank">Lists Documentation</a>

```bash
# Adding elements
LPUSH key value1 value2    # Push to left (head)
RPUSH key value1 value2    # Push to right (tail)
LPUSHX key value           # Push to left if list exists
RPUSHX key value           # Push to right if list exists

# Removing elements
LPOP key                   # Pop from left
RPOP key                   # Pop from right
BLPOP key timeout          # Blocking pop from left
BRPOP key timeout          # Blocking pop from right
LREM key count value       # Remove count occurrences of value

# Accessing elements
LINDEX key index           # Get element at index
LRANGE key start stop      # Get range of elements
LLEN key                   # Get list length
LSET key index value       # Set element at index

# List operations
LTRIM key start stop       # Trim list to range
RPOPLPUSH src dest         # Pop from src, push to dest

# Examples
RPUSH tasks "task1" "task2" "task3"
LPOP tasks                 # "task1"
LRANGE tasks 0 -1          # All remaining tasks
```

## Sets

📖 <a href="https://redis.io/docs/data-types/sets/" target="_blank">Sets Documentation</a>

```bash
# Adding/removing elements
SADD key member1 member2   # Add members to set
SREM key member1 member2   # Remove members from set
SPOP key count             # Remove and return random members
SRANDMEMBER key count      # Return random members (no removal)

# Set information
SISMEMBER key member       # Check if member exists
SCARD key                  # Get set size
SMEMBERS key               # Get all members
SSCAN key cursor           # Iterate over members

# Set operations
SUNION key1 key2           # Union of sets
SINTER key1 key2           # Intersection of sets
SDIFF key1 key2            # Difference of sets
SUNIONSTORE dest k1 k2     # Store union result
SINTERSTORE dest k1 k2     # Store intersection result
SDIFFSTORE dest k1 k2      # Store difference result

# Examples
SADD users:online "user1" "user2" "user3"
SISMEMBER users:online "user1"  # 1 (true)
SCARD users:online              # 3
```

## Sorted Sets

📖 <a href="https://redis.io/docs/data-types/sorted-sets/" target="_blank">Sorted Sets Documentation</a>

```bash
# Adding/updating elements
ZADD key score1 member1 score2 member2  # Add with scores
ZINCRBY key increment member             # Increment member score

# Removing elements
ZREM key member1 member2        # Remove members
ZREMRANGEBYRANK key start stop  # Remove by rank range
ZREMRANGEBYSCORE key min max    # Remove by score range

# Querying by rank
ZRANGE key start stop WITHSCORES    # Get by rank (low to high)
ZREVRANGE key start stop WITHSCORES # Get by rank (high to low)
ZRANK key member                    # Get member rank (0-based)
ZREVRANK key member                 # Get reverse rank

# Querying by score
ZRANGEBYSCORE key min max WITHSCORES    # Get by score range
ZREVRANGEBYSCORE key max min WITHSCORES # Get by score (reverse)
ZCOUNT key min max                      # Count in score range
ZSCORE key member                       # Get member score

# Set operations
ZUNIONSTORE dest numkeys key1 key2 # Union with score sum
ZINTERSTORE dest numkeys key1 key2  # Intersection with score sum

# Examples
ZADD leaderboard 100 "player1" 85 "player2" 92 "player3"
ZREVRANGE leaderboard 0 2 WITHSCORES  # Top 3 players
ZRANK leaderboard "player2"            # Rank of player2
```

## Hashes

📖 <a href="https://redis.io/docs/data-types/hashes/" target="_blank">Hashes Documentation</a>

```bash
# Setting fields
HSET key field1 value1 field2 value2  # Set fields
HSETNX key field value                # Set if field doesn't exist
HMSET key field1 value1 field2 value2 # Set multiple fields (deprecated)

# Getting fields
HGET key field              # Get field value
HMGET key field1 field2     # Get multiple fields
HGETALL key                 # Get all fields and values
HKEYS key                   # Get all field names
HVALS key                   # Get all values
HLEN key                    # Get number of fields

# Field operations
HEXISTS key field           # Check if field exists
HDEL key field1 field2      # Delete fields
HINCRBY key field increment # Increment field by integer
HINCRBYFLOAT key field inc  # Increment field by float

# Iteration
HSCAN key cursor            # Iterate over fields

# Examples
HSET user:1000 name "John" email "john@example.com" age 30
HGET user:1000 name         # "John"
HINCRBY user:1000 age 1     # Increment age
HGETALL user:1000           # All user data
```

## Pub/Sub

📖 <a href="https://redis.io/docs/interact/pubsub/" target="_blank">Pub/Sub Documentation</a>

```bash
# Publishing
PUBLISH channel message     # Publish message to channel

# Subscribing
SUBSCRIBE channel1 channel2 # Subscribe to channels
PSUBSCRIBE pattern         # Subscribe to pattern
UNSUBSCRIBE channel1       # Unsubscribe from channels
PUNSUBSCRIBE pattern       # Unsubscribe from pattern

# Information
PUBSUB CHANNELS pattern    # List active channels
PUBSUB NUMSUB channel1     # Number of subscribers
PUBSUB NUMPAT              # Number of pattern subscriptions

# Examples
# Terminal 1 (Subscriber)
SUBSCRIBE news sports

# Terminal 2 (Publisher)
PUBLISH news "Breaking news!"
PUBLISH sports "Game result"
```

## Transactions

📖 <a href="https://redis.io/docs/interact/transactions/" target="_blank">Transactions Documentation</a>

```bash
# Transaction commands
MULTI                  # Start transaction
EXEC                   # Execute transaction
DISCARD               # Discard transaction
WATCH key1 key2       # Watch keys for changes
UNWATCH               # Unwatch all keys

# Example transaction
MULTI
SET key1 value1
INCR counter
LPUSH list item
EXEC

# Conditional transaction with WATCH
WATCH mykey
val = GET mykey
val = val + 1
MULTI
SET mykey $val
EXEC
```

## Pipelining

📖 <a href="https://redis.io/docs/manual/pipelining/" target="_blank">Pipelining Documentation</a>

Pipelining allows sending multiple commands without waiting for replies, improving performance:

```bash
# Using redis-cli with pipelining
echo -e "SET key1 value1\nSET key2 value2\nGET key1\nGET key2" | redis-cli --pipe

# In application code (conceptual)
pipeline = redis.pipeline()
pipeline.set('key1', 'value1')
pipeline.set('key2', 'value2')
pipeline.get('key1')
results = pipeline.execute()
```

## Persistence

📖 <a href="https://redis.io/docs/management/persistence/" target="_blank">Persistence Documentation</a>

```bash
# RDB (Redis Database) snapshots
SAVE                    # Synchronous save (blocks)
BGSAVE                  # Background save
LASTSAVE                # Last successful save time

# AOF (Append Only File)
BGREWRITEAOF           # Rewrite AOF file

# Configuration in redis.conf
# RDB settings
save 900 1             # Save if 1 key changed in 900 seconds
save 300 10            # Save if 10 keys changed in 300 seconds
save 60 10000          # Save if 10000 keys changed in 60 seconds

# AOF settings
appendonly yes         # Enable AOF
appendfsync everysec   # Sync every second
```

## Memory Management

📖 <a href="https://redis.io/docs/management/optimization/" target="_blank">Memory Optimization Documentation</a>

```bash
# Memory information
INFO memory            # Memory usage statistics
MEMORY USAGE key       # Memory used by key
MEMORY STATS           # Detailed memory stats

# Memory optimization
CONFIG SET maxmemory 100mb              # Set memory limit
CONFIG SET maxmemory-policy allkeys-lru # Eviction policy

# Eviction policies
# noeviction - return errors when memory limit reached
# allkeys-lru - evict least recently used keys
# volatile-lru - evict LRU among keys with expire set
# allkeys-random - evict random keys
# volatile-random - evict random among keys with expire
# volatile-ttl - evict keys with shortest TTL
```

## Replication

📖 <a href="https://redis.io/docs/management/replication/" target="_blank">Replication Documentation</a>

```bash
# Slave/Replica configuration
REPLICAOF host port    # Make this instance a replica
REPLICAOF NO ONE       # Stop replication (promote to master)

# Replication information
INFO replication       # Replication status
ROLE                   # Get role (master/slave)

# Master configuration in redis.conf
# bind 192.168.1.100
# port 6379

# Replica configuration in redis.conf
# replicaof 192.168.1.100 6379
# replica-read-only yes
```

## Configuration

📖 <a href="https://redis.io/docs/management/config/" target="_blank">Configuration Documentation</a>

```bash
# Runtime configuration
CONFIG GET parameter   # Get configuration parameter
CONFIG SET parameter value # Set configuration parameter
CONFIG RESETSTAT       # Reset statistics
CONFIG REWRITE         # Rewrite config file

# Important configuration parameters
CONFIG GET maxmemory
CONFIG GET timeout
CONFIG GET databases
CONFIG GET save

# Common redis.conf settings
port 6379
bind 127.0.0.1
timeout 300
tcp-keepalive 60
databases 16
maxclients 10000
```

## Monitoring

📖 <a href="https://redis.io/docs/management/admin/" target="_blank">Administration Documentation</a>

```bash
# Real-time monitoring
MONITOR                # Watch all commands in real-time
INFO                   # Server information
INFO stats             # Statistics
INFO clients           # Client connections
INFO memory            # Memory usage
INFO replication       # Replication info

# Slow query log
CONFIG SET slowlog-log-slower-than 10000  # Log queries > 10ms
SLOWLOG GET 10         # Get last 10 slow queries
SLOWLOG LEN            # Number of slow queries
SLOWLOG RESET          # Clear slow query log

# Client information
CLIENT LIST            # List connected clients
CLIENT KILL ip:port    # Kill client connection
CLIENT SETNAME name    # Set client name
CLIENT GETNAME         # Get client name
```

---

## Quick Reference Links

### Essential Resources
- 📚 <a href="https://redis.io/docs/" target="_blank">Redis Documentation</a> - Complete documentation
- 🎮 <a href="https://try.redis.io/" target="_blank">Try Redis</a> - Interactive tutorial
- 📝 <a href="https://redis.io/commands/" target="_blank">Command Reference</a> - All Redis commands
- 🔧 <a href="https://redis.io/docs/getting-started/" target="_blank">Getting Started Guide</a> - Installation and basics

### Learning Resources
- 🎓 <a href="https://university.redis.com/" target="_blank">Redis University</a> - Free courses
- 📖 <a href="https://redis.io/docs/manual/" target="_blank">Redis Manual</a> - In-depth guides
- 🧪 <a href="https://redis.io/docs/data-types/tutorial/" target="_blank">Data Types Tutorial</a> - Hands-on examples

### Tools & Clients
- 🔧 <a href="https://github.com/RedisInsight/RedisInsight" target="_blank">RedisInsight</a> - GUI management tool
- 🎨 <a href="https://redis.io/clients/" target="_blank">Redis Clients</a> - Client libraries for all languages
- 💻 <a href="https://redis.io/docs/stack/" target="_blank">Redis Stack</a> - Extended Redis with modules

### Performance & Operations
- 📊 <a href="https://redis.io/docs/management/optimization/" target="_blank">Performance Optimization</a> - Best practices
- 🔒 <a href="https://redis.io/docs/management/security/" target="_blank">Security Guidelines</a> - Securing Redis
- 🚀 <a href="https://redis.io/docs/management/scaling/" target="_blank">Scaling Redis</a> - Clustering and sharding

---

*This cheatsheet covers Redis 7.0+ features. For the latest updates, always refer to the <a href="https://redis.io/docs/" target="_blank">official documentation</a>.*