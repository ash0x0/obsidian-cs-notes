A type of associative containers in which each element has to be unique, because the value of the element identifies it. 
The value of the element cannot be modified once it is added to the set, though it is possible to remove and add the modified value of that element.

# Implementations
## [C++](https://www.geeksforgeeks.org/set-in-cpp-stl/)

In C++, set is an associative container that provides the implementation of [[Red Black Tree]]. This data structure makes sure that elements are always stored in a sorted order. It also makes sure that insertion, deletion, and access operations take logarithmic time.
### Usage

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    // Creating an empty set
    set<int> s1;
    // Creating a set from an initializer list
    set<int> s = {1, 4, 2};
    
    // Printing elements
    for (auto x: s) cout << x << " ";
    
    // Insert elements into set
    s.insert(5);
    s.emplace(3);
    
    // Accessing first element
    auto it1 = s.begin();
    // Accessing third element
    auto it2 = next(it1, 2);
	
	// Finding 3
    auto it = s.find(3);
    if (it != s.end()) cout << *it;
    
    // Traversing using range based for loop
    for(auto it = s.begin(); it != s.end(); it++)
        cout << *it << " ";
    
    // Deleting elements by value
    s.erase(5);
    // Deleting first element by iterator
    s.erase(s.begin());
    
    return 0;
}
```

### Time Complexity

| **Operation**            | **Time Complexity** |
| ------------------------ | ------------------- |
| Insert an element        | **O(log n)**        |
| Delete an element        | ****O(log n)****    |
| Find the largest element | ****O(1)****        |
| Find smallest element    | ****O(1)****        |
| Find element by value    | ****O(log n)****    |
| Traverse the set         | ****O(n)****        |
