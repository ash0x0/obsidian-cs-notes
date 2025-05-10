When hashing values in a size-constrained data structures, it may be the case that multiple keys would have the same hash index, therefore needing to be stored at the same location

# Strategies

The easiest way to solve collisions is to add a linked list or similar data structure at the location where the collision takes place. That would mean the following:
1. Accessing any of the keys that collide with one another returns the same index
2. Accessing the index returns a linked list
3. We need to search the list to find the specific key we're looking for
 
## Chaining

Each position in the hash structure becomes the head of a linked list containing the elements stored at that position. Essentially creating a chain under that address.

## Open Addressing

Open addressing makes deletions tricky