A map (also known as dictionary or associative array) is a data structure that stores **key-value pairs**. Each key is unique and maps to exactly one value.

# Characteristics

- **Key-Value Pairs**: Each entry consists of a unique key and its associated value
- **Unique Keys**: No duplicate keys allowed
- **Fast Lookup**: Efficient retrieval of values by key
- **Dynamic Size**: Can grow or shrink as needed

# Types of Maps

## Unordered Map (Hash Map)

Uses hash table implementation. Keys are not stored in any particular order.

```cpp
#include <unordered_map>

unordered_map<string, int> hashMap;
hashMap["apple"] = 5;

// Access
int value = hashMap["apple"];

// Check if key exists
if (hashMap.find("apple") != hashMap.end()) {
    cout << "Found";
}
```

**Time Complexity**: Average [[O(1)]] for insert, search, delete

## Ordered Map

Uses balanced binary search tree. Keys stored in sorted order.

```cpp
#include <map>

map<string, int> treeMap;
treeMap["apple"] = 5;
```

**Time Complexity**: [[O(log n)]] for insert, search, delete

# Common Operations

```cpp
// Insertion
map["key"] = value;
map.insert({"key", value});

// Access
int val = map["key"];
int val = map.at("key");  // throws if not found

// Deletion
map.erase("key");

// Check existence
if (map.find("key") != map.end()) {}
if (map.count("key") > 0) {}
```

# Applications

- Caching and memoization
- Counting frequencies
- Database indexing
- Symbol tables
- Graph adjacency lists

# Related

- [[Hash Table]]
- [[Set]]
- [[Binary Search Tree]]
