# Problem

Given a circular array of non-zero integers, determine if there is a cycle in the array. A cycle exists if:
1. Starting from an index and following the values as movements, you return to the starting index
2. The cycle's length is greater than 1
3. The cycle moves in only one direction (all forward or all backward)

For example, in `[2, -1, 1, 2, 2]`:
- Starting at index 0: move forward 2 steps → index 2, move forward 1 step → index 3, move forward 2 steps → index 0 (cycle found!)

# Solution
## [[Two Pointer]]

Use Floyd's Cycle Detection ([[Fast & Slow Pointer]]) with modifications for circular arrays.

```cpp
int getNextIndex(vector<int>& arr, int currentIndex) {
    int n = arr.size();
    int nextIndex = (currentIndex + arr[currentIndex]) % n;
    // Handle negative indices
    if (nextIndex < 0) {
        nextIndex += n;
    }
    return nextIndex;
}

bool isNotOneCycle(vector<int>& arr, int prevDirection, int currentDirection) {
    // Direction changed or same element cycle
    return prevDirection * currentDirection < 0;
}

bool circularArrayLoop(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n; i++) {
        int slow = i;
        int fast = i;
        bool forward = arr[i] > 0;
        
        while (true) {
            // Move slow pointer one step
            slow = getNextIndex(arr, slow);
            if (isNotOneCycle(arr, forward ? 1 : -1, arr[slow])) {
                break;
            }
            
            // Move fast pointer two steps
            fast = getNextIndex(arr, fast);
            if (isNotOneCycle(arr, forward ? 1 : -1, arr[fast])) {
                break;
            }
            fast = getNextIndex(arr, fast);
            if (isNotOneCycle(arr, forward ? 1 : -1, arr[fast])) {
                break;
            }
            
            // Cycle detected
            if (slow == fast) {
                // Check if cycle length > 1
                if (slow == getNextIndex(arr, slow)) {
                    break;  // Cycle length is 1
                }
                return true;
            }
        }
    }
    
    return false;
}
```

**Time Complexity**: [[O(n²)]] - checking from each index
**Space Complexity**: [[O(1)]]

# Optimization

Mark visited indices to avoid rechecking:

```cpp
bool circularArrayLoop(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n; i++) {
        if (arr[i] == 0) continue;
        
        int slow = i, fast = i;
        bool forward = arr[i] > 0;
        
        while (arr[fast] != 0 && arr[getNextIndex(arr, fast)] != 0) {
            slow = getNextIndex(arr, slow);
            fast = getNextIndex(arr, getNextIndex(arr, fast));
            
            if (slow == fast) {
                if (slow == getNextIndex(arr, slow)) break;
                return true;
            }
        }
        
        // Mark as visited
        int idx = i;
        int val = arr[i];
        while (arr[idx] * val > 0) {
            int next = getNextIndex(arr, idx);
            arr[idx] = 0;
            idx = next;
        }
    }
    
    return false;
}
```

# Related Problems

- [[Linked List Cycle]]
- [[Happy Number]]
- [[Floyd's Cycle Detection]]