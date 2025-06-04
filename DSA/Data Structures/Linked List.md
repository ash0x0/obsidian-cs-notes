---
sources:
  - https://www.geeksforgeeks.org/detect-loop-in-a-linked-list/
  - https://www.geeksforgeeks.org/write-a-c-function-to-print-the-middle-of-the-linked-list/
---
Linked lists offer easy modification for any part of the list but there's no random or direct access, we have to traverse the entire list to get to a location.

When we're working with a linked list we always need to check for the `nullptr` before doing any operations.

When working with linked lists, [[Recursion]] can often help.
# Operations

## Insertion
## Deletion

Deleting a node `curr` just requires setting the reference to skip it.
```cpp
// Delete current node
prev.next = curr.next;
```
We could also delete without a previous pointer
```cpp
// Delete current node
curr.data = curr.next.data;
curr.next = curr.next.next;
```
## [[21. Merge Two Sorted Lists]]
## [[Floyd's Cycle Detection]]

### [[141. Linked List Cycle]]
## [[206. Reverse Linked List]]
