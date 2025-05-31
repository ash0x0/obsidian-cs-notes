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

## [[Hash Table|Hashmap]] - O(n\*m) time - O(n) space

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

## Examplify

1. One node in both trees
	A = 8
	B = 8
	ans -> 8.
2. Two trees same length
	A = 3 -> 7 -> 8 -> 10
	B = 99 -> 1 -> 8 -> 10
	ans -> 8.
3. Two trees different lengths
	A = 3 -> 7 -> 8 -> 10
	B = 8 -> 10
	ans -> 8.