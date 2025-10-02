#Amazon #DailyCodingProblem #Easy 
# Problem

Implement a stack that has the following methods:

- push(val), which pushes an element onto the stack
- pop(), which pops off and returns the topmost element of the stack. If there are no elements in the stack, then it should throw an error or return null.
- max(), which returns the maximum value in the stack currently. If there are no elements in the stack, then it should throw an error or return null.

Each method should run in constant time.
# Solution

## Two Stacks Approach - [[O(1)]] for all operations

Use two stacks: one for the actual stack values, and another to track the maximum at each level.

```cpp
class MaxStack {
private:
    stack<int> mainStack;
    stack<int> maxStack;
    
public:
    void push(int val) {
        mainStack.push(val);
        
        if (maxStack.empty() || val >= maxStack.top()) {
            maxStack.push(val);
        } else {
            maxStack.push(maxStack.top());
        }
    }
    
    int pop() {
        if (mainStack.empty()) {
            throw runtime_error("Stack is empty");
        }
        
        int val = mainStack.top();
        mainStack.pop();
        maxStack.pop();
        return val;
    }
    
    int max() {
        if (maxStack.empty()) {
            throw runtime_error("Stack is empty");
        }
        return maxStack.top();
    }
};
```

**Time Complexity**: [[O(1)]] for all operations
**Space Complexity**: [[O(n)]] for storing the max stack

## Alternative: Single Stack with Pairs

Store pairs of (value, currentMax) in the stack.

```cpp
class MaxStack {
private:
    stack<pair<int, int>> stack; // (value, max at this level)
    
public:
    void push(int val) {
        int currentMax = stack.empty() ? val : std::max(val, stack.top().second);
        stack.push({val, currentMax});
    }
    
    int pop() {
        if (stack.empty()) {
            throw runtime_error("Stack is empty");
        }
        
        int val = stack.top().first;
        stack.pop();
        return val;
    }
    
    int max() {
        if (stack.empty()) {
            throw runtime_error("Stack is empty");
        }
        return stack.top().second;
    }
};
```

# Related Problems

- [[Min Stack]]
- [[Stack]]
- LeetCode #155 - Min Stack
