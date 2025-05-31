---
sources:
  - https://www.educative.io/blog/crack-amazon-coding-interview-questions
---
#Amazon 
# Problem

You are given a linked list where the node has two pointers. The first is the regular `next` pointer. The second pointer is called `arbitrary` and it can point to any node in the linked list. Your job is to write code to make a deep copy of the given linked list. Here, deep copy means that any operations on the original list should not affect the copied list.

# Solution

```cpp
struct ListNode{
	ListNode* next;
	ListNode* arbitrary;
	int val;
	ListNode(int x, ListNode* next, ListNode* arbitrary): val(x), next(next), arbitrary(arbitrary) {};
	ListNode(int x): val(x), next(nullptr), arbitrary(nullptr) {}
}
```

## Iterative - O(n) time - O(n) space

```cpp
class Solution{

	ListNode* deepCopyLinkedList(ListNode* root) {
		unordered_set<ListNode*> nodes;
		while(root) {
			ListNode* currentNode = new ListNode(root->val);
			ListNode* nextNode = new ListNode(root->next->val);
			ListNode* arbitraryNode = new ListNode(root->arbitrary->val);
			ListNode* found = nodes.find(arbitraryNode);
			if (found) currectNode->arbitrary = found;
			else currectNode->arbitrary = arbitraryNode;
			currectNode->next = nextNode;
			root = root->next;
		}
	}
}
```