#DailyCodingProblem #Google #Hard 
# Problem

An XOR linked list is a more memory efficient doubly linked list. Instead of each node holding `next` and `prev` fields, it holds a field named `both`, which is an XOR of the next node and the previous node. Implement an XOR linked list; it has an `add(element)` which adds the element to the end, and a `get(index)` which returns the node at index.

If using a language that has no pointers (such as Python), you can assume you have access to `get_pointer` and `dereference_pointer` functions that converts between nodes and memory addresses.
# Solution

## XOR Linked List Implementation

**Key Idea**: Store XOR of previous and next node addresses instead of storing both separately.

```cpp
struct Node {
    int data;
    Node* both;  // XOR of prev and next
    
    Node(int val) : data(val), both(nullptr) {}
};

class XORLinkedList {
private:
    Node* head;
    Node* tail;
    
    // Helper: XOR two pointers
    Node* XOR(Node* a, Node* b) {
        return (Node*)((uintptr_t)(a) ^ (uintptr_t)(b));
    }
    
public:
    XORLinkedList() : head(nullptr), tail(nullptr) {}
    
    void add(int element) {
        Node* newNode = new Node(element);
        
        if (head == nullptr) {
            // First node
            head = tail = newNode;
            newNode->both = nullptr;
        } else {
            // Add at end
            newNode->both = XOR(tail, nullptr);
            tail->both = XOR(XOR(tail->both, nullptr), newNode);
            tail = newNode;
        }
    }
    
    int get(int index) {
        Node* curr = head;
        Node* prev = nullptr;
        Node* next;
        
        for (int i = 0; i < index; i++) {
            if (curr == nullptr) {
                throw out_of_range("Index out of bounds");
            }
            
            // Get next: next = curr->both XOR prev
            next = XOR(prev, curr->both);
            
            // Move forward
            prev = curr;
            curr = next;
        }
        
        if (curr == nullptr) {
            throw out_of_range("Index out of bounds");
        }
        
        return curr->data;
    }
    
    void traverse() {
        Node* curr = head;
        Node* prev = nullptr;
        Node* next;
        
        while (curr != nullptr) {
            cout << curr->data << " ";
            
            next = XOR(prev, curr->both);
            prev = curr;
            curr = next;
        }
        cout << endl;
    }
    
    void traverseReverse() {
        Node* curr = tail;
        Node* next = nullptr;
        Node* prev;
        
        while (curr != nullptr) {
            cout << curr->data << " ";
            
            prev = XOR(next, curr->both);
            next = curr;
            curr = prev;
        }
        cout << endl;
    }
};
```

# How XOR Works

**Property**: `A XOR B XOR B = A`

For a node:
```
both = prev XOR next

To get next from both:
next = both XOR prev
     = (prev XOR next) XOR prev
     = prev XOR prev XOR next
     = 0 XOR next
     = next

To get prev from both:
prev = both XOR next
     = (prev XOR next) XOR next
     = prev XOR next XOR next
     = prev XOR 0
     = prev
```

# Memory Comparison

**Traditional Doubly Linked List**:
```cpp
struct Node {
    int data;
    Node* prev;  // 8 bytes
    Node* next;  // 8 bytes
};
// Total: 16 bytes for pointers
```

**XOR Linked List**:
```cpp
struct Node {
    int data;
    Node* both;  // 8 bytes
};
// Total: 8 bytes for pointer
// 50% memory savings!
```

# Example

```
List: 1 ↔ 2 ↔ 3

Node 1:
  both = NULL XOR addr(2) = addr(2)

Node 2:
  both = addr(1) XOR addr(3)

Node 3:
  both = addr(2) XOR NULL = addr(2)

Traversal from head:
  curr = node1, prev = NULL
  next = both(1) XOR NULL = addr(2)
  
  curr = node2, prev = node1
  next = both(2) XOR addr(1) = addr(3)
  
  curr = node3, prev = node2
  next = both(3) XOR addr(2) = NULL
```

# Python Implementation (with simulated pointers)

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.both = 0  # XOR of prev and next

class XORLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.nodes = []  # Simulate memory
        
    def get_pointer(self, node):
        if node is None:
            return 0
        return id(node)
    
    def dereference_pointer(self, ptr):
        if ptr == 0:
            return None
        for node in self.nodes:
            if id(node) == ptr:
                return node
        return None
    
    def add(self, element):
        new_node = Node(element)
        self.nodes.append(new_node)
        
        if self.head is None:
            self.head = self.tail = new_node
        else:
            new_node.both = self.get_pointer(self.tail)
            self.tail.both ^= self.get_pointer(new_node)
            self.tail = new_node
    
    def get(self, index):
        curr = self.head
        prev_ptr = 0
        
        for _ in range(index):
            if curr is None:
                raise IndexError()
            
            next_ptr = prev_ptr ^ curr.both
            prev_ptr = self.get_pointer(curr)
            curr = self.dereference_pointer(next_ptr)
        
        if curr is None:
            raise IndexError()
        
        return curr.data
```

# Advantages

1. **Memory efficient**: 50% less pointer storage
2. **Can traverse both directions**: Like doubly linked list

# Disadvantages

1. **Complex**: More difficult to implement and debug
2. **Pointer arithmetic**: Not available in all languages
3. **Garbage collection**: Harder with XOR pointers
4. **Less readable**: Code is more obscure

# Time Complexity

- **Add**: [[O(1)]]
- **Get**: [[O(n)]]
- **Traverse**: [[O(n)]]

# Space Complexity

[[O(1)]] per node for pointer (vs [[O(2)]] for traditional doubly linked list)

# Related Topics

- [[Linked List]]
- [[Bitwise Operations]]
- [[XOR]]
- Memory optimization
