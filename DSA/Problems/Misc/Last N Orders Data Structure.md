#DailyCodingProblem #Twitter #Easy 
# Problem

You run an e-commerce website and want to record the last `N` `order` ids in a log. Implement a data structure to accomplish this, with the following API:

- record(order_id): adds the order_id to the log
- get_last(i): gets the ith last element from the log. i is guaranteed to be smaller than or equal to N.

You should be as efficient with time and space as possible.
# Solution

## Circular Buffer - [[O(1)]] operations

Use a circular buffer (ring buffer) with fixed size N.

```cpp
class OrderLog {
private:
    vector<int> buffer;
    int capacity;
    int index;  // Current write position
    int size;   // Current number of elements
    
public:
    OrderLog(int N) : capacity(N), index(0), size(0) {
        buffer.resize(N);
    }
    
    void record(int order_id) {
        buffer[index] = order_id;
        index = (index + 1) % capacity;
        if (size < capacity) {
            size++;
        }
    }
    
    int get_last(int i) {
        if (i <= 0 || i > size) {
            throw out_of_range("Invalid index");
        }
        
        // Calculate position: go back i positions from current index
        int pos = (index - i + capacity) % capacity;
        return buffer[pos];
    }
};

// Usage
int main() {
    OrderLog log(3);
    
    log.record(1);  // buffer = [1, _, _], index = 1
    log.record(2);  // buffer = [1, 2, _], index = 2
    log.record(3);  // buffer = [1, 2, 3], index = 0
    log.record(4);  // buffer = [4, 2, 3], index = 1 (overwrites oldest)
    
    cout << log.get_last(1) << endl;  // 4 (most recent)
    cout << log.get_last(2) << endl;  // 3
    cout << log.get_last(3) << endl;  // 2
    
    return 0;
}
```

# Time Complexity

- **record**: [[O(1)]]
- **get_last**: [[O(1)]]

# Space Complexity

[[O(N)]] - fixed size buffer

# Alternative: Deque Implementation

```cpp
class OrderLog {
private:
    deque<int> orders;
    int capacity;
    
public:
    OrderLog(int N) : capacity(N) {}
    
    void record(int order_id) {
        if (orders.size() >= capacity) {
            orders.pop_front();
        }
        orders.push_back(order_id);
    }
    
    int get_last(int i) {
        if (i <= 0 || i > orders.size()) {
            throw out_of_range("Invalid index");
        }
        return orders[orders.size() - i];
    }
};
```

# Why Circular Buffer?

1. **Space efficient**: Fixed memory, no dynamic allocation
2. **Cache friendly**: Contiguous memory
3. **Fast**: [[O(1)]] operations with simple modulo arithmetic
4. **No memory leaks**: No dynamic allocation/deallocation

# Related Topics

- [[Ring Buffer]]
- [[Deque]]
- [[Queue]]
- Sliding window with fixed size
