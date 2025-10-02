#Google #DailyCodingProblem #Medium 
# Problem

Given a singly linked list and an integer k, remove the kth last element from the list. k is guaranteed to be smaller than the length of the list.

The list is very long, so making more than one pass is prohibitively expensive.

Do this in constant space and in one pass.
# Solution

## Two Pointer - [[O(n)]] time - [[O(1)]] space

Use fast and slow pointers with k gap between them.

```cpp
ListNode* removeKthFromEnd(ListNode* head, int k) {
    ListNode* dummy = new ListNode(0);
    dummy->next = head;
    
    ListNode* fast = dummy;
    ListNode* slow = dummy;
    
    // Move fast k+1 steps ahead
    for (int i = 0; i <= k; i++) {
        fast = fast->next;
    }
    
    // Move both until fast reaches end
    while (fast != nullptr) {
        fast = fast->next;
        slow = slow->next;
    }
    
    // Remove kth from end
    ListNode* toDelete = slow->next;
    slow->next = slow->next->next;
    delete toDelete;
    
    return dummy->next;
}
```

# Algorithm

1. **Setup**: Create dummy node, place two pointers at start
2. **Gap creation**: Move fast pointer k+1 steps ahead
3. **Move together**: Move both pointers until fast reaches end
4. **Delete**: slow->next is the kth node from end

# Why It Works

When fast reaches end (nullptr):
- fast has moved n steps (n = list length)
- slow has moved n - (k+1) steps
- slow->next is at position n - k = kth from end

# Example: Remove 2nd from end

```
List: 1 → 2 → 3 → 4 → 5, k = 2

Step 1: Place pointers
dummy → 1 → 2 → 3 → 4 → 5 → null
^fast
^slow

Step 2: Move fast k+1=3 steps
dummy → 1 → 2 → 3 → 4 → 5 → null
^slow           ^fast

Step 3: Move both until fast = null
dummy → 1 → 2 → 3 → 4 → 5 → null
                ^slow     ^fast

Step 4: Delete slow->next (4)
dummy → 1 → 2 → 3 → 5 → null

Result: 1 → 2 → 3 → 5
```

# Edge Cases

```cpp
// Remove last node
list: 1 → 2 → 3, k = 1
Result: 1 → 2

// Remove first node
list: 1 → 2 → 3, k = 3
Result: 2 → 3

// Single node
list: 1, k = 1
Result: null
```

# Why Dummy Node?

Handles edge case where head needs to be removed:

```cpp
// Without dummy, special case needed
if (k == length) {
    return head->next;
}

// With dummy, unified logic
```

# Time Complexity

[[O(n)]] - single pass through list

# Space Complexity

[[O(1)]] - only two pointers

# Related Problems

- LeetCode #19 - Remove Nth Node From End of List
- [[Two Pointer]]
- [[Linked List]]
- Fast and slow pointer technique
