A cache is a temporary storage area that stores the result of expensive responses or frequently accessed data in memory so that subsequent requests are served more quickly. 
- It is a temporary data store layer, much faster than the database. 
- The benefits of having a separate cache tier include better system performance, ability to reduce database workloads, and the ability to scale the cache tier independently. 

# Caching Strategies

## Read-through Cache

1. A server reads a piece of data from the cache
2. If cache hit, it retrieves the data
3. If cache miss, it retrieves the data from the datastore then it caches it in the cache

![[Pasted image 20250305213533.png]]


# Decision Structure

- Consider using cache when data is read frequently but modified infrequently. 
- A cache server is not ideal for persisting data as it stores in volatile memory. If a cache server restarts, all the data in memory is lost. 
- Important data should be saved in persistent data stores. 

# Considerations

## Expiration

- Once cached data is expired, it is removed from the cache. 
- When there is no expiration policy, cached data will be stored in the memory permanently. Data can also become stale as it's not updated from datastore. 
- If expiration date is too short this will cause the system to reload data from the database too frequently. 

## Consistency

- This involves keeping the data store and the cache in sync. 
- Inconsistency can happen because data-modifying operations on the data store and cache are not in a single transaction. 
- When scaling across multiple regions, maintaining consistency between the data store and cache is challenging. 
## Failure Mitigation 

- A single cache server represents a  single point of failure (SPOF). 
- Multiple cache servers across different data centers are recommended. 
- Also could overprovision the required memory by certain percentages. This provides a buffer as the memory usage increases.

## Cache Eviction 

- Once the cache is full, any requests to add items to the cache might cause existing items to be removed. This is called eviction.
- Least-recently-used (LRU) is the most popular cache eviction policy. 
- Other eviction policies, such as the Least Frequently Used (LFU) or First in First Out (FIFO), can be adopted to satisfy different use cases.