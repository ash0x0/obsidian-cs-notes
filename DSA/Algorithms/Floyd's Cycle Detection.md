---
sources:
  - https://www.geeksforgeeks.org/floyds-cycle-finding-algorithm/
  - https://www.geeksforgeeks.org/find-first-node-of-loop-in-a-linked-list/
  - https://www.geeksforgeeks.org/write-a-c-function-to-print-the-middle-of-the-linked-list/
aliases:
  - Hare Tortoise
---
#LinkedLists 

A [[Two Pointer]] algorithm, moving through a list sequence at different speeds. 
It uses two pointers one moving twice as fast as the other one. 
The faster one is called the fast pointer and the other one is called the slow pointer.
# When to use

1. Find a loop in a linked list. 
2. Find middle node in a linked list.
3. Find starting node of a loop in a linked list.

# Mechanism

## Finding Middle Node

1. Traverse the list
	1. Move slow pointer one step at a time
	2. Move fast pointer two steps at a time
2. When the fast pointer hits the end of the list, the slow pointer is at the middle

## Finding Cycles

While traversing the linked list one of these things will occur
- The Fast pointer may reach the end `null` which shows that there is no loop in the linked list.
- The Fast pointer again catches the slow pointer at some time therefore a loop exists in the linked list.

1. Traverse the list
	1. Move slow pointer one step at a time
	2. Move fast pointer moves two steps at a time.
	3. If there's a cycle, the fast pointer will eventually catch up with the slow pointer within the cycle because it's moving faster.
	4. If there's no cycle, the fast pointer will reach the end of the list (i.e., it will become null).
2. When the slow and fast pointers meet, a cycle or loop exists.
## Finding First Node of a Cycle

1. Same as normal cycle detection
2. After detecting that the loop is present
	1. Reset slow pointer to head 
	2. Keep fast pointer at its position
	3. Both slow and fast pointers move one step at a time
	4. When fast equals slow, slow will be the starting node of loop.