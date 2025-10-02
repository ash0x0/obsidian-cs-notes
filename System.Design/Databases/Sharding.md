Sharding (also called horizontal partitioning) is a database architecture pattern that divides data across multiple database instances (shards) to improve scalability and performance.

# Overview

Sharding splits a large database into smaller, more manageable pieces called shards. Each shard contains a subset of the total data and operates as an independent database.

```
Single Database:
[All Users] → One Database

Sharded:
[Users A-M] → Shard 1
[Users N-Z] → Shard 2
```

# Why Shard?

## Problems with Single Database

1. **Storage limits**: Single server has finite disk space
2. **Memory limits**: Can't fit all data in RAM
3. **CPU limits**: Single server can't handle all queries
4. **I/O bottleneck**: Disk and network throughput limits
5. **Geographic latency**: Far from some users

## Benefits of Sharding

1. **Horizontal scalability**: Add more shards as data grows
2. **Improved performance**: Queries hit smaller datasets
3. **Geographic distribution**: Place data closer to users
4. **Isolation**: Problems in one shard don't affect others
5. **Cost-effective**: Use commodity hardware instead of expensive vertical scaling

# Sharding Strategies

## 1. Range-Based Sharding

Divide data based on key ranges.

```
Shard 1: user_id 1-1000000
Shard 2: user_id 1000001-2000000
Shard 3: user_id 2000001-3000000
```

**Pros**:
- Simple to implement
- Easy to add new ranges

**Cons**:
- Risk of hotspots (uneven distribution)
- Sequential IDs can create hot shards

## 2. Hash-Based Sharding

Use hash function to determine shard.

```python
def get_shard(user_id, num_shards):
    return hash(user_id) % num_shards

# user_id 12345 → hash → shard 2
# user_id 67890 → hash → shard 1
```

**Pros**:
- Even distribution
- No hotspots

**Cons**:
- Adding/removing shards requires rebalancing
- Range queries are difficult

## 3. Geographic Sharding

Shard by location.

```
US Users → US Shard
EU Users → EU Shard
Asia Users → Asia Shard
```

**Pros**:
- Low latency for users
- Regulatory compliance (data residency)

**Cons**:
- Uneven data distribution
- Cross-region queries are expensive

## 4. Directory-Based Sharding

Use lookup table to map keys to shards.

```
Directory Service:
user_id → shard mapping

user_123 → shard_1
user_456 → shard_2
user_789 → shard_1
```

**Pros**:
- Flexible
- Easy to rebalance
- Can use complex logic

**Cons**:
- Directory service is single point of failure
- Extra lookup latency
- More complex

## 5. Consistent Hashing

Distribute data using consistent hashing algorithm.

```
Hash Ring:
0 ──── Shard1 ──── Shard2 ──── Shard3 ──── 0

Key K → hash(K) → find next shard on ring
```

**Pros**:
- Adding/removing shards affects minimal data
- Good for distributed systems

**Cons**:
- More complex to implement
- May still have hotspots

# Shard Key Selection

Critical decision that affects performance and complexity.

## Good Shard Keys

1. **High Cardinality**: Many unique values
2. **Even Distribution**: Prevents hotspots
3. **Query Patterns**: Aligns with common queries
4. **Immutable**: Doesn't change over time

## Bad Shard Keys

1. **Low Cardinality**: Few unique values (e.g., country)
2. **Sequential**: Creates hot shards (e.g., timestamp)
3. **Monotonically Increasing**: All new data to one shard

## Examples

**Good**: user_id, email hash, composite keys
**Bad**: status (active/inactive), boolean flags, timestamps

# Challenges

## 1. Cross-Shard Queries

Queries spanning multiple shards are expensive.

```sql
-- Single shard (fast)
SELECT * FROM users WHERE user_id = 123

-- Cross-shard (slow)
SELECT * FROM users WHERE city = 'New York'
```

**Solutions**:
- Denormalize data
- Use application-level joins
- Cache aggregated results
- Avoid when possible

## 2. Distributed Transactions

Maintaining ACID across shards is difficult.

**Solutions**:
- Eventual consistency
- Saga pattern
- Two-phase commit (slow)
- Avoid when possible

## 3. Rebalancing

Moving data when adding/removing shards.

```
Old: 3 shards
New: 4 shards
Need to move ~25% of data
```

**Solutions**:
- Consistent hashing (minimal movement)
- Virtual shards/slots
- Gradual migration
- Scheduled maintenance windows

## 4. Backups and Restoration

Must coordinate across all shards.

**Solutions**:
- Shard-level backups
- Coordinated snapshots
- Point-in-time recovery tools

## 5. Schema Changes

Apply changes to all shards consistently.

**Solutions**:
- Rolling deployments
- Backward-compatible changes
- Version migrations
- Blue-green deployments

# Implementation Example

## Application Layer

```python
class ShardRouter:
    def __init__(self, shards):
        self.shards = shards
        self.num_shards = len(shards)
    
    def get_shard(self, user_id):
        shard_id = hash(user_id) % self.num_shards
        return self.shards[shard_id]
    
    def query(self, user_id, sql):
        shard = self.get_shard(user_id)
        return shard.execute(sql)
    
    def query_all_shards(self, sql):
        results = []
        for shard in self.shards:
            results.extend(shard.execute(sql))
        return results

# Usage
router = ShardRouter([shard1, shard2, shard3])
user_data = router.query(user_id=123, "SELECT * FROM users WHERE id=123")
```

## Database Configuration

```yaml
shards:
  - id: 1
    host: db1.example.com
    port: 5432
    range: 0-333333
    
  - id: 2
    host: db2.example.com
    port: 5432
    range: 333334-666666
    
  - id: 3
    host: db3.example.com
    port: 5432
    range: 666667-999999
```

# Monitoring

Key metrics to track:

- Data distribution across shards
- Query latency per shard
- Cross-shard query frequency
- Rebalancing progress
- Shard health and availability

# When to Shard

## Consider Sharding When:

- Single database reaches limits (storage, CPU, memory)
- Query performance degrades despite optimization
- Need geographic distribution
- Vertical scaling becomes too expensive

## Alternatives to Consider First:

1. **Vertical Scaling**: Bigger servers
2. **Read Replicas**: Distribute read load
3. **Caching**: Reduce database load
4. **Query Optimization**: Indexes, query tuning
5. **Archiving**: Move old data

# Best Practices

1. **Plan Shard Key Carefully**: Hard to change later
2. **Start with More Shards**: Easier to merge than split
3. **Monitor Distribution**: Detect hotspots early
4. **Document Strategy**: Clear documentation for team
5. **Automate Operations**: Backup, monitoring, failover
6. **Test Failure Scenarios**: Shard failure, rebalancing
7. **Use Connection Pooling**: Manage connections efficiently

# Technologies Supporting Sharding

## Native Sharding Support

- **MongoDB**: Built-in sharding
- **Cassandra**: Partitioning by design
- **Vitess**: MySQL sharding solution
- **Citus**: PostgreSQL extension for sharding

## Middleware Solutions

- **ProxySQL**: MySQL proxy with sharding
- **ShardingSphere**: Database sharding middleware
- **Spanner**: Google's globally distributed database

# Related Topics

- [[Partitioning]]
- [[Replication]]
- [[Consistent Hashing]]
- [[Distributed Databases]]
- [[Scalability]]
