---
sources:
  - https://www.dailycodingproblem.com/
---
#Google #LinkedLists #DailyCodingProblem
# Problem

Given two singly linked lists that intersect at some point, find the intersecting node. The lists are non-cyclical.

For example, given A = 3 -> 7 -> 8 -> 10 and B = 99 -> 1 -> 8 -> 10, return the node with value 8.

In this example, assume nodes with the same value are the exact same node objects.

Do this in O(M + N) time (where M and N are the lengths of the lists) and constant space.

# Solution

```cpp
// Definition for singly-linked list node.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```
## Examplify

1. Two empty lists
	A = nullptr
	B = nullptr
	ans -> nullptr
2. One empty list
	A = nullptr
	B = 8 -> 10
	ans -> nullptr
3. Two non-empty lists, no intersection
	A = 10 -> 20 -> 30
	B = 7 -> 6 -> 14
	ans -> nullptr
4. One node in both lists (identical lists)
	A = 8
	B = 8
	ans -> 8
5. Two lists same length / intersecting at tail
	A = 8 -> 10
	B = 8 -> 10
	ans -> 8
6. Two lists different lengths, answer beginning of one tree middle of another
	A = 3 -> 7 -> 8 -> 10
	B = 8 -> 10
	ans -> 8.
## Brute Force - O(n\*m) time - O(1)

1. Iterate through first list
2. For each node, iterate through second list and compare

```cpp
ListNode* BruteForceFindIntersection(ListNode* headA, ListNode* headB) {
    ListNode* currA = headA;
    while (currA != nullptr) {
        ListNode* currB = headB;
        while (currB != nullptr) {
            if (currA == currB) {
                return currA;
            }
            currB = currB->next;
        }
        currA = currA->next;
    }
    return nullptr; 
}
```

## [[Hash Set]] - O(n+m) time - O(n) space

```cpp
class Solution {
	struct ListNode {
		ListNode* next;
		int val;
	};
	
	ListNode* findIntersectingNodes(ListNode* root1, ListNode* root2) {
		unordered_set<ListNode*> nodes;
		while(root1 != nullptr) {
			nodes.insert(root1);
			root1 = root1->next;
		}
		while(root2 != nullptr) {
			int location = nodes.find(root2);
			if (location) return location;
			nodes.insert(root2);
			root2 = root2->next; 
		}
		return new ListNode();
	}
}
```


## Optimized [[Hash Set]] - O(n+m) time - O(n) space

This stores less items in the hash set and loops for fewer iterations. It's the same complexity but actual runtime and space overhead is less.

```cpp
ListNode* HashSetFindIntersection(ListNode* headA, ListNode* headB) {
    // First decide which list is shorter, so we store fewer nodes.
    ListNode* ptrA = headA;
    ListNode* ptrB = headB;
    int lenA = 0, lenB = 0;
    while (ptrA) { ++lenA; ptrA = ptrA->next; }
    while (ptrB) { ++lenB; ptrB = ptrB->next; }

    // We'll insert nodes of the shorter list into the hash set.
    ListNode* shorter = (lenA < lenB ? headA : headB);
    ListNode* longer  = (lenA < lenB ? headB : headA);

    unordered_set<ListNode*> visited;
    ListNode* curr = shorter;
    while (curr) {
        visited.insert(curr);
        curr = curr->next;
    }

    curr = longer;
    while (curr) {
        if (visited.count(curr)) {
            return curr;  // first common node
        }
        curr = curr->next;
    }
    return nullptr;
}
```

## [[Two Pointer]] - O(n+m) time - O(1) space

1. Observe in the examples and problem that if two linked lists intersect then all the items after the intersections are the same, i.e. the two lists intersect in a node and its succeeding nodes, i.e. full tail.
2. If we know the difference in lengths, we could advance a pointer in the longer list by that difference and start comparisons from there so we're not comparing the earlier nodes in the longer list needlessly.
```cpp
ListNode* OptimalFindIntersection(ListNode* headA, ListNode* headB) {
    // 1. Compute lengths of both lists
    int lenA = 0, lenB = 0;
    ListNode* ptrA = headA;
    ListNode* ptrB = headB;
    while (ptrA) { ++lenA; ptrA = ptrA->next; }
    while (ptrB) { ++lenB; ptrB = ptrB->next; }

    // 2. Reset pointers to heads
    ptrA = headA;
    ptrB = headB;

    // 3. Advance the pointer of the longer list by |lenA - lenB|
    if (lenA > lenB) {
        int diff = lenA - lenB;
        while (diff-- > 0 && ptrA) {
            ptrA = ptrA->next;
        }
    } else {
        int diff = lenB - lenA;
        while (diff-- > 0 && ptrB) {
            ptrB = ptrB->next;
        }
    }

    // 4. Move both pointers until they collide or both reach nullptr
    while (ptrA && ptrB) {
        if (ptrA == ptrB) return ptrA;
        ptrA = ptrA->next;
        ptrB = ptrB->next;
    }

    // If we exited the loop, there's no intersection
    return nullptr;
}
```