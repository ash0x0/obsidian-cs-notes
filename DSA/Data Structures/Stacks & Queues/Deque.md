A deque (double-ended queue) is a linear data structure that allows insertion and deletion of elements from both ends (front and rear). It's a generalization of both [[Stack]] and [[Queue]].

# Characteristics

- **Double-ended**: Operations possible at both front and rear
- **Dynamic size**: Can grow or shrink as needed
- **Flexible**: Can function as stack or queue
- **Random access**: Elements can be accessed by index (in some implementations)

# Operations

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| push_front | [[O(1)]] | Insert at front |
| push_back | [[O(1)]] | Insert at rear |
| pop_front | [[O(1)]] | Remove from front |
| pop_back | [[O(1)]] | Remove from rear |
| front | [[O(1)]] | Access front element |
| back | [[O(1)]] | Access rear element |
| at(i) | [[O(1)]] | Random access |

# Implementation in C++

```cpp
#include <deque>

deque<int> dq;

// Insert at front
dq.push_front(1);

// Insert at back
dq.push_back(2);

// Remove from front
dq.pop_front();

// Remove from back
dq.pop_back();

// Access
int front = dq.front();
int back = dq.back();
int elem = dq[0];  // Random access
```

# Use Cases

- Implementing sliding window problems
- Browser history (forward/back navigation)
- Undo/redo functionality
- Palindrome checking
- Task scheduling with priority at both ends

# Advantages

- [[O(1)]] insertion/deletion at both ends
- Random access capability
- Can be used as stack or queue

# Disadvantages

- More complex than simple stack or queue
- Higher memory overhead than array
- Not cache-friendly compared to vector

# Related

- [[Stack]]
- [[Queue]]
- [[Circular Queue]]
