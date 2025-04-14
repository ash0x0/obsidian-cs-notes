It is impossible for a distributed system to provide more than two of the following three guarantees simultaneously:

- **[[Consistency]]**: All clients see the same data at the same time, regardless of the node they connect to.
- **[[Availability]]**: Any client requesting data receives a response, even if some nodes are down.
- **[[Partition Tolerance]]**: The system continues to operate despite [[network partitions]], which are communication breaks between two nodes.

[[Key-Value Store]] are often classified based on the two CAP characteristics they support:

- **CP (Consistency and Partition Tolerance)**: Support consistency and partition tolerance while sacrificing availability.
- **CA (Consistency and Availability)**: Support consistency and availability while sacrificing partition tolerance. CA cannot exist in real-world because network failure is unavoidable.
- **AP (Availability and Partition Tolerance)**: Support availability and partition tolerance while sacrificing consistency.
# Implications

- In a distributed system, data is usually replicated multiple times.
- Ideally, data written to one node is automatically replicated to others, achieving both consistency and availability. 
- In the real world, network partitions can occur, forcing a choice between consistency and availability.
- The theorem encouraged database engineers to explore distributed shared-nothing systems, which were more suitable for implementing large-scale web services and led to the explosion of new database technologies.

# Limitations

- Sometimes presented as "Consistency, Availability, Partition tolerance: pick 2 out of 3", which is misleading because network partitions are a type of fault and will inevitably happen.
- A system can provide both consistency ([[Linearizability]]) and total availability when the network is working correctly. When a network fault occurs, a choice must be made between linearizability and total availability.
- There are contradictory definitions of availability in discussions of CAP, and the formalization of the theorem does not match its usual meaning. 
- Many "highly available" systems do not meet CAP's definition of availability.
- It only considers one consistency model (linearizability) and one kind of fault ([[network partitions]]). It doesn’t address network delays, [[dead nodes]], or other trade-offs.
- CAP has little practical value for designing systems and is mostly of historical interest today. 
- It has been superseded by more precise results, and there are more interesting impossibility results in distributed systems.