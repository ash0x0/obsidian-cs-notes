# Types

- singly
- doubly 
- circular

# Operations

## Insertion
## Deletion
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

# Problems

- [[Reversing]]
- Finding the middle node
- [[Detecting Loops]]
- Merging sorted lists