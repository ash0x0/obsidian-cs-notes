#Google #DailyCodingProblem #Hard 
# Problem

Implement an LRU (Least Recently Used) cache. It should be able to be initialized with a cache size `n`, and contain the following methods:

- `set(key, value)`: sets `key` to `value`. If there are already `n` items in the cache and we are adding a new item, then it should also remove the least recently used item.
- `get(key)`: gets the value at `key`. If no such key exists, return null.

Each operation should run in O(1) time.
# Solution

## Hash Map + Doubly Linked List - [[O(1)]] for all operations

Use a hash map for [[O(1)]] lookup and a doubly linked list to maintain access order.

```cpp
class Node {
public:
    int key;
    int value;
    Node* prev;
    Node* next;
    
    Node(int k, int v) : key(k), value(v), prev(nullptr), next(nullptr) {}
};

class LRUCache {
private:
    int capacity;
    unordered_map<int, Node*> cache;
    Node* head; // Most recently used
    Node* tail; // Least recently used
    
    void addToHead(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }
    
    void removeNode(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }
    
    void moveToHead(Node* node) {
        removeNode(node);
        addToHead(node);
    }
    
    Node* removeTail() {
        Node* node = tail->prev;
        removeNode(node);
        return node;
    }
    
public:
    LRUCache(int cap) : capacity(cap) {
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head->next = tail;
        tail->prev = head;
    }
    
    int get(int key) {
        if (cache.find(key) == cache.end()) {
            return -1; // or null
        }
        
        Node* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void set(int key, int value) {
        if (cache.find(key) != cache.end()) {
            Node* node = cache[key];
            node->value = value;
            moveToHead(node);
        } else {
            Node* newNode = new Node(key, value);
            cache[key] = newNode;
            addToHead(newNode);
            
            if (cache.size() > capacity) {
                Node* removed = removeTail();
                cache.erase(removed->key);
                delete removed;
            }
        }
    }
};
```

**Time Complexity**: [[O(1)]] for both get and set
**Space Complexity**: [[O(capacity)]]

# Algorithm

1. **Get Operation**:
   - Check if key exists in hash map
   - If found, move node to head (mark as recently used)
   - Return value

2. **Set Operation**:
   - If key exists: update value and move to head
   - If key doesn't exist:
     - Create new node and add to head
     - Add to hash map
     - If size exceeds capacity, remove tail node (LRU)

# Why This Works

- Hash map provides [[O(1)]] lookup by key
- Doubly linked list maintains access order
- Head = most recently used
- Tail = least recently used
- Moving nodes and removing tail are [[O(1)]] with doubly linked list

# Related Problems

- LeetCode #146 - LRU Cache
- [[Hash Table]]
- [[Linked List]]
- LFU Cache
