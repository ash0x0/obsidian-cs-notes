---
sources: https://www.geeksforgeeks.org/unordered_set-in-cpp-stl/
---
An unordered associative container that stores unique elements. Unlike [[Set]], it stores its elements using hashing. This provides average constant-time **O(1)** search, insert, and delete operations but the elements are not sorted in any particular order.

# Implementations

## C++

In C++, `unordered_set` provides the built-in implementation of [[Hash Table]] data structure. Each element is hashed on the basis of its value. This hash value determines where to store it in hash table. Each key in an unordered set is unique, and if an attempt is made to insert a duplicate element, it will be ignored. 
As it uses hashing, insertion, deletion and search operations take O(1) amortized time.
## Unordered Set vs [[Set]]

- Unordered set stores elements in any order and insertion, deletion, and access operations are ****O(1)**** time due to the use of ****hashing****.
- Set stores elements in a sorted order and operations such as insertions, deletions, and accessing operations are takes logarithmic ****O(log n)**** in time complexity.

### Usage

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    
    // Create an empty unordered_set
    unordered_set<int> us1;
  
    // Initialize an unordered_set using
    // using intializer list
    unordered_set<int> us2 = {1, 2, 3, 4, 5};    
    
    // Insert elments using insert()
    us.insert(3);
    
    // Accessing third element
	auto it = next(us.begin(), 2);
    cout << *it;

	// Finding 4
    auto it = us.find(4);
    if (it != us.end()) cout << *it;

	// Delete element by value
    us.erase(5);    
    // Delete element by position
    us.erase(us.begin());

    for(auto x : us2)
        cout << x << " ";
    return 0;
}
```

### Time Complexity

|****Operation****|****Time Complexity****|
|---|---|
|Insert an element|****O(1) (average)****|
|Delete an element|****O(1) (average)****|
|Access element by position|****O(n)****|
|Find element by value|****O(1) (average)****|
|Traverse the set|****O(n)****|