---
aliases:
  - Atomic Consistency
  - Strong Consistency
  - Immediate Consistency
  - External Consistency
---

A [[Consistency]] model that makes a system appear as if there is only one copy of the data, and all operations on it are atomic. 
It is also known as atomic consistency, strong consistency, immediate consistency, or external consistency.

# Key Aspects

## Single Copy Illusion

The  system gives the appearance that there is only one copy of the data, even though there may be multiple replicas in reality. The application does not need to worry about replication.

## Recency Guarantee

Linearizability guarantees that the value read is the most recent, up-to-date value, and doesn’t come from a stale cache or replica. 
As soon as one client successfully completes a write, all clients reading from the database must be able to see the value just written. Thus, linearizability is a recency guarantee.

## Total order of operations

In a linearizable system, operations are executed in a well-defined order. For any two operations, it can always be determined which one happened first.

- **Contrast with serializability** Linearizability is a recency guarantee on reads and writes of a register (an individual object). Serializability is an isolation property of transactions, where every transaction may read and write multiple objects; it guarantees that transactions behave the same as if they had executed in some serial order. A database may provide both serializability and linearizability, a combination known as strict serializability or strong one-copy serializability. Serializable snapshot isolation is not linearizable.

Linearizability is useful in situations such as:

- **Locking and leader election** Systems using single-leader replication need to ensure there is only one leader to avoid a split brain scenario. Coordination services like Apache ZooKeeper and etcd are often used to implement distributed locks and leader election, relying on consensus algorithms to implement linearizable operations in a fault-tolerant way.
- **Uniqueness constraints** A hard uniqueness constraint typically found in relational databases requires linearizability.
- **Cross-channel timing dependencies** Race conditions between different communication channels can be avoided with the recency guarantee of linearizability.

However, linearizability has trade-offs:

- **Performance and availability** Making a system linearizable can harm its performance and availability, especially with significant network delays.
- **CAP theorem** Applications that don’t require linearizability can be more tolerant of network problems. The CAP theorem characterizes the trade-offs between consistency (in the sense of linearizability) and availability in the presence of network partitions.

Achieving linearizability typically involves replication:

- **Single-leader replication** If reads are made from the leader or synchronously updated followers, there is potential to be linearizable. However, not every single-leader database is actually linearizable.
- **Leaderless replication** It is safest to assume that a leaderless system with Dynamo-style replication does not provide linearizability. While it may seem that strict quorum reads and writes should be linearizable, race conditions are possible with variable network delays.