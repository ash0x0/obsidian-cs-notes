Linked lists offer easy modification for any part of the list but there's no random or direct access, we have to traverse the entire list to get to a location.
When we're working with a linked list we always need to check for the `nullptr` before doing any operations.
When working with linked lists, [[Recursion]] can often help.
# Types

- singly
- doubly 
- circular

# Operations

## Insertion
## Deletion

Deleting a node `n` just requires setting the reference to skip it.
```cpp
prev.next = n.next;
```
We could also delete without a previous pointer
```cpp
curr.data = curr.next.data;
curr.next = curr.next.next;
```
## Merging
## [[Cycle Detection]]

## Reversing

### In-place Reversal
#### Mechanism

1. Create a pointer `current` to point to the head of the list
2. Create a pointer `previous` to point to the previous node processed
3. Reverse `current` node by pointing it to `previous`
4. Move `current` to the next node
5. Update `previous` to the node we just processed

![[Pasted image 20250509233141.png]]

#### When to use

1. Reverse a linked list without using extra memory
## Finding Middle Node

A quick way to do this is to use a [[Two Pointer]] technique, fast and slow. 
1. We increment the fast pointer by 2 for each step
2. We increment the slow pointer by 1 for each step
3. When the fast pointer hits the end of the list, the slow pointer is at the middle

- [[Detecting Loops]]