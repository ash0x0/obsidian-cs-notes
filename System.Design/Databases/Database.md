
# Concepts

[[ACID]]
[[SQL]]
[[NOSQL]]
[[Transaction]]
[[Consistency]]

# Types

- [[Relational Database]]
- [[Non-Relational Database]]

# Scaling

- [[Partitioning]]
- [[Sharding]]

# Replication

- A master database generally only supports write operations. 
- A slave database gets copies of the data from the master database and only supports read operations. 
- All the data-modifying commands like insert, delete, or update must be sent to the master database. 
- Most applications require a much higher ratio of reads to writes; thus, the number of slave databases in a system is usually larger than the number of master databases.

## Benefits

- Better performance
	- In the master-slave model, all writes and updates happen in master nodes; whereas, read operations are distributed across slave nodes. 
	- This model improves performance because it allows more queries to be processed in parallel.
- Reliability
	- If one of your database servers is destroyed by a natural disaster, such as a typhoon or an earthquake, data is still preserved. 
- High availability
	- By replicating data across different locations, your website remains in operation even if a database is offline as you can access data stored in another database server.
## Fail-over

- If only one slave database is available and it goes offlin
	- Read operations will be directed to the master database temporarily. 
	- As soon as the issue is found, a new slave database will replace the old one. 
	- In case multiple slave databases are available, read operations are redirected to other healthy slave databases. 
	- A new database server will replace the old one.
- If the master database goes offline
	- A slave database will be promoted to be the new master. 
	- All the database operations will be temporarily executed on the new master database. 
	- A new slave database will replace the old one for data replication immediately. 
	- In production systems, promoting a new master is more complicated as the data in a slave database might not be up to date. 
		- The missing data needs to be updated by running data recovery scripts.
		- Some other replication methods like multi-masters and circular replication could help, those setups are more complicated

[[Clustered Database]]

![[Pasted image 20250305213614.png]]
![[Pasted image 20250305215128.png]]
