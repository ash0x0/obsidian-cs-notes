Consistency in distributed systems refers to the guarantee that all nodes see the same data at the same time, or the degree to which this guarantee is relaxed for performance and availability.

# Types of Consistency

## 1. Strong Consistency

All reads receive the most recent write. All nodes see same data simultaneously.

```
Client writes X=5 → All nodes immediately have X=5
Any read of X returns 5
```

**Use Cases**: Banking, financial transactions, inventory management

**Pros**:
- Simplest to reason about
- No stale data

**Cons**:
- Higher latency
- Lower availability (CAP theorem)
- Performance overhead

**Implementation**: Two-phase commit, Paxos, Raft

## 2. Eventual Consistency

System will eventually become consistent, but replicas may temporarily differ.

```
Client writes X=5 → Node1
Node1 has X=5
Node2 still has X=3 (old value)
...eventually...
Node2 has X=5
```

**Use Cases**: Social media feeds, DNS, caching, shopping cart

**Pros**:
- High availability
- Better performance
- Partition tolerance

**Cons**:
- May read stale data
- More complex application logic
- Conflict resolution needed

**Implementation**: Gossip protocols, vector clocks, CRDTs

## 3. Causal Consistency

Operations that are causally related are seen in the same order by all nodes.

```
Post A → Comment on Post A
Everyone sees Post A before its comment
(But might see Post B from another user in different order)
```

**Use Cases**: Social networks, collaborative editing

## 4. Read-Your-Writes Consistency

A user always sees their own writes.

```
User1 writes X=5
User1 reads X → always gets 5
User2 might still see old value
```

**Implementation**: Route user's reads to same server, or use version numbers

## 5. Monotonic Read Consistency

Once a user reads a value, they never see an older value.

```
User reads X=5 at time T1
User reads X at time T2 → gets 5 or newer, never 3
```

## 6. Session Consistency

Within a session, reads reflect writes made in that session.

```
Session A:
- Write X=5
- Read X → gets 5

Session B:
- Read X → might get old value
```

# CAP Theorem

In a distributed system experiencing network partition, must choose 2 of 3:

- **C**onsistency: All nodes see same data
- **A**vailability: Every request gets response
- **P**artition tolerance: System works despite network failures

```
        Consistency
           /  \
          /    \
         /      \
    CA         CP
     |          |
     |    Partition
     |          |
    AP         
     \         /
      \       /
   Availability
```

**Real-world trade-offs**:
- **CP Systems**: Consistent but may be unavailable (MongoDB with majority writes, HBase)
- **AP Systems**: Available but may return stale data (Cassandra, DynamoDB)
- **CA Systems**: Consistent and available, but can't handle partitions (traditional RDBMS in single datacenter)

# Consistency Models Comparison

| Model | Guarantees | Latency | Use Case |
|-------|-----------|---------|----------|
| Strong | Latest value always | High | Banking |
| Eventual | Eventually consistent | Low | Social media |
| Causal | Causal order preserved | Medium | Collaborative editing |
| Read-your-writes | See own writes | Medium | User profiles |

# Implementation Techniques

## Quorum Reads/Writes

```
N = number of replicas
W = write quorum (nodes that must acknowledge write)
R = read quorum (nodes queried for read)

Strong consistency: W + R > N
Example: N=3, W=2, R=2 → 2+2 > 3 ✓
```

```python
def quorum_write(key, value, W, replicas):
    writes_completed = 0
    for replica in replicas:
        if replica.write(key, value):
            writes_completed += 1
        if writes_completed >= W:
            return True  # Success
    return False  # Failed to reach quorum

def quorum_read(key, R, replicas):
    values = []
    for replica in replicas:
        value = replica.read(key)
        values.append(value)
        if len(values) >= R:
            return resolve_conflicts(values)
    return None  # Failed to reach quorum
```

## Version Vectors/Vector Clocks

Track causality and detect conflicts.

```python
class VectorClock:
    def __init__(self):
        self.clock = {}
    
    def increment(self, node_id):
        self.clock[node_id] = self.clock.get(node_id, 0) + 1
    
    def update(self, other):
        for node_id, timestamp in other.clock.items():
            self.clock[node_id] = max(
                self.clock.get(node_id, 0),
                timestamp
            )
    
    def compare(self, other):
        # Returns: -1 (before), 0 (concurrent), 1 (after)
        pass

# Usage
node1_clock = VectorClock()
node1_clock.increment("node1")  # {node1: 1}

node2_clock = VectorClock()
node2_clock.increment("node2")  # {node2: 1}

# These are concurrent (conflict!)
```

## Conflict Resolution

### Last-Write-Wins (LWW)

Use timestamp to resolve conflicts.

```python
def resolve_lww(values):
    return max(values, key=lambda v: v.timestamp)
```

**Pros**: Simple
**Cons**: Can lose data

### Application-Level Resolution

Let application decide.

```python
def resolve_shopping_cart(carts):
    # Merge all items from conflicting carts
    merged_cart = {}
    for cart in carts:
        for item, quantity in cart.items():
            merged_cart[item] = merged_cart.get(item, 0) + quantity
    return merged_cart
```

### CRDTs (Conflict-Free Replicated Data Types)

Data structures that automatically resolve conflicts.

```python
class GCounter:
    # Grow-only counter
    def __init__(self, node_id):
        self.node_id = node_id
        self.counts = {}
    
    def increment(self):
        self.counts[self.node_id] = self.counts.get(self.node_id, 0) + 1
    
    def value(self):
        return sum(self.counts.values())
    
    def merge(self, other):
        for node, count in other.counts.items():
            self.counts[node] = max(self.counts.get(node, 0), count)
```

# Consistency in SQL vs NoSQL

## SQL (Relational Databases)

- Strong consistency by default
- ACID transactions
- Slower in distributed scenarios
- Examples: PostgreSQL, MySQL

## NoSQL Databases

### Strong Consistency
- MongoDB (with majority write concern)
- HBase
- Google Spanner

### Eventual Consistency
- Cassandra (tunable)
- DynamoDB (tunable)
- Riak

### Tunable Consistency
Many NoSQL databases allow choosing consistency level per operation:

```python
# Cassandra example
session.execute(query, consistency_level=ConsistencyLevel.QUORUM)
session.execute(query, consistency_level=ConsistencyLevel.ONE)
```

# Best Practices

1. **Choose Based on Requirements**
   - Financial data: Strong consistency
   - Social media: Eventual consistency
   - User profiles: Session consistency

2. **Design for Eventual Consistency**
   - Even if using strong consistency, design to handle delays
   - Graceful degradation

3. **Use Appropriate Tools**
   - Transactions for critical operations
   - Caching for read-heavy workloads
   - Event sourcing for audit trails

4. **Monitor Consistency**
   - Replication lag
   - Conflict frequency
   - Version divergence

5. **Test Edge Cases**
   - Network partitions
   - Concurrent updates
   - Failure scenarios

# Example: Social Media Post

```python
class Post:
    def __init__(self, post_id, content, version):
        self.post_id = post_id
        self.content = content
        self.version = version
        self.likes = 0

# Eventual consistency for likes
def add_like(post_id):
    # Write to local node
    local_db.increment_likes(post_id)
    # Async replicate to other nodes
    async_replicate(post_id, "increment_likes")
    # User sees immediate feedback (read-your-writes)
    return "Like added"

# Strong consistency for critical operations
def delete_post(post_id, user_id):
    # Need strong consistency - can't partially delete
    with distributed_transaction():
        verify_ownership(post_id, user_id)
        delete_from_all_nodes(post_id)
        invalidate_caches(post_id)
    return "Post deleted"
```

# Related Topics

- [[CAP Theorem]]
- [[ACID vs BASE]]
- [[Replication]]
- [[Distributed Transactions]]
- [[Eventual Consistency]]
