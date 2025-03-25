---
aliases:
  - Vector
  - Vectors
  - vector
  - vectors
  - arraylist
  - ArrayList
  - Array List
source 1: https://www.geeksforgeeks.org/vector-in-cpp-stl/
---
An ArrayList/Vector is a dynamically resizing array. It resizes itself when needed while still providing [[O(1)]] time for access.

# Mechanism

1. When the array is full, create a new array with size like size * 2
2. Deep copy all elements from old array to new array
3. Return or change the old array reference to point to the new array

This has O(n) time but is done so infrequent that it's still essentially O(1) access if we amortize the O(n) resizing time over the number of access operations.

# Implementations

## C++

In C++, **vector** is a dynamic array that stores collection of elements same type in contiguous memory. It has the ability to resize itself automatically when an element is inserted or deleted.

### Usage

```cpp
#include<vector>
#include<iostream>
using namespace std;  

int main() {      
	// Creating an empty vector     
	vector<int> v1;
	// Creating a vector of 5 elements from 
	// initializer list 
	vector<int> v1 = {1, 4, 2, 3, 5}; 
	// Creating a vector of 5 elements with 
	// default value 
	vector<int> v2(5, 9);
	
	vector<char> v = {'a', 'f', 'd'};
	
	// Inserting 'z' at the back 
	v.push_back('z'); 
	// Inserting 'c' at index 1 
	v.insert(v.begin() + 1, 'c');

	// Accessing and printing values 
	cout << v[3] << endl;
	cout << v.at(2);

	// Updating values using indexes 3 and 2
	v[3] = 'D';
	v.at(2) = 'F';

	// Finding size 
	cout << v.size();

	// Deleting last element 'z' 
	v.pop_back(); 
	// Deleting element 'f' 
	v.erase(find(v.begin(), v.end(), 'f'));

	// Traversing vector using range based for loop 
	for (int i = 0; i < v.size(); i++) cout << v[i] << " ";
	
	return 0; 
}
```

### Time Complexity

|****Operation****|****Time Complexity****|
|---|---|
|Insert an element at the end|****O(1)**** (amortized)|
|Insert an element somewhere in the middle|****O(n)****|
|Delete an element from the end|****O(1)****|
|Delete an element from somewhere in the middle|****O(n)****|
|Access an element by index|****O(1)****|
|Traverse the vector|****O(n)****|
|Find an element by value|****O(n)****|
