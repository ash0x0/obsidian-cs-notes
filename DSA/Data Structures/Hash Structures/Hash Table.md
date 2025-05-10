---
sources:
  - https://www.geeksforgeeks.org/unordered_map-in-cpp-stl/
  - https://en.wikipedia.org/wiki/Hash_table
aliases:
  - Hashtable
  - Hash Table
  - Hash Map
  - Dictionary
  - Hashmap
---
A structure that implements an [[Associative Array]] that maps keys to values and uses a hash function to compute an _index_, also called a _hash code_, into an array of _buckets_ or _slots_, from which the desired value can be found.

During lookup, the key is hashed and the resulting hash indicates where the corresponding value is stored. A map implemented by a hash table is called a **hash map**.

Most hash table designs employ an imperfect [[Hash Function]]. [[Hash Collision]], where the hash function generates the same index for more than one key, therefore typically must be accommodated in some way.

In many situations, hash tables turn out to be on average more efficient than [[Search Tree]] or any other lookup structure. For this reason, they are widely used in many kinds of applications, particularly for associative arrays, database indexing, caches, and sets.

# Implementations

## C++

In C++, `unordered_map` is an unordered associative container that stores data in the form of unique key-value pairs. But unlike [[Map]], unordered map stores its elements using hashing. This provides average constant-time complexity [[O(1)]] for search, insert, and delete operations but the elements are not sorted in any particular order.

### Usage

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    // Create an empty unordered_map
    unordered_map<int, string> um1;
    // Creating an unordered_map with integer
    // keys and string values
    unordered_map<int, string> um = {{1, "Geeks"}, {2, "For"}, {3, "C++"}};

	// Insert elements using insert() method
    um.insert({2, "For"});
    um.insert({3, "C++"});

	// Access value associated with key 2
    // using [] operator
    cout << um[2] << endl;
    // Access value associated with key 1
    // using at() function
    cout << um.at(1);

	// Updating value associated with key 2
    // using [] operator
    um[2] = "By";
    // Updating value associated with key 1
    //using at() function
    um.at(1) = "Tips";

	// Finding element with key 2
    auto it = um.find(2);
    if (it != um.end())
        cout << it->first << ": " << it->second;

	// Delete element which have key 2
    um.erase(2);
    // Delete first element
    um.erase(um.begin());

	// Traversing using iterators with loop
    for(auto it = um.begin(); it != um.end(); it++)
        cout << it->first << ": " << it->second
        << endl;

    for (auto i : um) 
        cout << i.first << ": " << i.second
        << endl;
    return 0;
}
```

### Time Complexity

|****Operation****|****Time Complexity****|
|---|---|
|Insert an element|****O(1)**** (average)|
|Delete an element by key|****O(1)**** (average)|
|Access element by key|****O(1)**** (average)|
|Find element by key|****O(1)**** (average)|
|Update element by key|****O(1)**** (average)|
|Traverse the map|****O(n)****|