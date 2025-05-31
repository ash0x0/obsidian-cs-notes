---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-merge-two-sorted-linked-lists/problem
---
#Amazon 
# Problem

Given pointers to the heads of two sorted linked lists, merge them into a single, sorted linked list. Either head pointer may be null meaning that the corresponding list is empty.

**Example**  
 refers to   
 refers to 

The new list is 

**Function Description**

Complete the _mergeLists_ function in the editor below.

_mergeLists_ has the following parameters:

- _SinglyLinkedListNode pointer headA:_ a reference to the head of a list
- _SinglyLinkedListNode pointer headB:_ a reference to the head of a list

**Returns**

- _SinglyLinkedListNode pointer:_ a reference to the head of the merged list

**Input Format**

The first line contains an integer , the number of test cases.

The format for each test case is as follows:

The first line contains an integer , the length of the first linked list.  
The next  lines contain an integer each, the elements of the linked list.  
The next line contains an integer , the length of the second linked list.  
The next  lines contain an integer each, the elements of the second linked list.

**Constraints**

- , where  is the  element of the list.

**Sample Input**

```
1
3
1
2
3
2
3
4
```

**Sample Output**

```
1 2 3 3 4 
```

**Explanation**

The first linked list is: 

The second linked list is: 

Hence, the merged linked list is:
# Solution
## Iterative - O(m+n) time - [[O(1)]] space

```cpp
SinglyLinkedListNode* mergeLists(SinglyLinkedListNode* head1, SinglyLinkedListNode* head2) {
	
	if (head1 == nullptr) return head2;
	if (head2 == nullptr) return head1;
	
	SinglyLinkedListNode head(-1);
	SinglyLinkedListNode* result = &head;
	
	while (head2 != nullptr && head1 != nullptr) {
		if (head1->data < head2->data) {
			result->next = head1;
			head1 = head1->next;
		} else {
			result->next = head2;
			head2 = head2->next;
		}
		result = result-> next;
	}
	
	while (head1 != nullptr) {
		result->next = head1;
		head1 = head1->next;
		result = result-> next;
	}
	
	while (head2 != nullptr) {
		result->next = head2;
		head2 = head2->next;
		result = result-> next;
	}

	result = nullptr;
	return head.next;
}
```