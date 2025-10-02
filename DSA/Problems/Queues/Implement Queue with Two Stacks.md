#Apple #DailyCodingProblem #Medium 
# Problem

Implement a queue using two stacks. Recall that a queue is a FIFO (first-in, first-out) data structure with the
following methods: enqueue, which inserts an element into the queue, and dequeue, which removes it.
# Solution

## Two Stacks Approach - Amortized [[O(1)]]

Use two stacks: one for enqueue operations and one for dequeue operations.

```cpp
class QueueWithStacks {
private:
    stack<int> inStack;  // For enqueue
    stack<int> outStack; // For dequeue
    
    void transferStacks() {
        if (outStack.empty()) {
            while (!inStack.empty()) {
                outStack.push(inStack.top());
                inStack.pop();
            }
        }
    }
    
public:
    void enqueue(int val) {
        inStack.push(val);
    }
    
    int dequeue() {
        transferStacks();
        
        if (outStack.empty()) {
            throw runtime_error("Queue is empty");
        }
        
        int val = outStack.top();
        outStack.pop();
        return val;
    }
    
    int peek() {
        transferStacks();
        
        if (outStack.empty()) {
            throw runtime_error("Queue is empty");
        }
        
        return outStack.top();
    }
    
    bool empty() {
        return inStack.empty() && outStack.empty();
    }
};
```

# Algorithm

**Enqueue**: Simply push to inStack - [[O(1)]]

**Dequeue**:
1. If outStack is empty, transfer all elements from inStack to outStack
2. Pop from outStack
3. Amortized [[O(1)]] - each element is moved at most once

# Example Walkthrough

```
enqueue(1): inStack = [1], outStack = []
enqueue(2): inStack = [1,2], outStack = []
enqueue(3): inStack = [1,2,3], outStack = []

dequeue(): Transfer to outStack
           inStack = [], outStack = [3,2,1]
           Return 1
           inStack = [], outStack = [3,2]

enqueue(4): inStack = [4], outStack = [3,2]

dequeue(): Return 2 (from outStack)
           inStack = [4], outStack = [3]
```

# Time Complexity

- **Enqueue**: [[O(1)]]
- **Dequeue**: Amortized [[O(1)]]
  - Worst case [[O(n)]] when transferring
  - But each element transferred only once
  - Average: [[O(1)]]

# Space Complexity

[[O(n)]] where n is the number of elements

# Related Problems

- LeetCode #232 - Implement Queue using Stacks
- [[Stack]]
- [[Queue]]
- Implement Stack using Queues
