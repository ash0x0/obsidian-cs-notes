---
sources:
  - https://www.geeksforgeeks.org/max_element-in-cpp/
  - https://www.geeksforgeeks.org/accumulate-and-partial_sum-in-c-stl-numeric-header/
  - https://www.geeksforgeeks.org/stdfind_if-stdfind_if_not-in-c/
---
# Data Structures

## `std::accumulate()`

## `std::partial_sum()`

## `std::min_element()`

## `std::max_element()`

## `std::find_if()`

## `std::find_if_not()`

Implements [[Linear Search]] to find the largest element in the range. It compares each element of the range one by one using the iterator/pointer provided to it as arguments.

Not specialized for sorted containers such as std::set, std::map, etc. and still will compare all the elements of these containers to find the maximum element.

It has [[O(n)]] time complexity.

```cpp
#include <algorithm>
using namespace std;

// Get Element Having Maximum Remainder After Division with 5
bool comp(int a, int b) {
  return a % 5 < b % 5;
}
 
struct St {
    string name;
    int sno;
};

int main() {
    vector<int> v = {2, 1, 17, 10};
	int arr[4] = {33, 87, 1, 71};
	int n = sizeof(arr)/sizeof(arr[0]);
	
	// Max element in vector
    cout << *max_element(v.begin(), v.end()) << endl;
	// Max element in array
    cout << *max_element(arr, arr + n);
 
    // Finding the maximum element with custom comparator
    cout << *max_element(v.begin(), v.end(), comp);

	// Create a vector of structure
    vector<St> v = {{"Ashok", 11}, {"Deepak", 15}, 
                    {"Anmol", 23}, {"Vikas", 19}};

	// Find Maximum Element in Vector of User Defined Data Type
    // Find the maximum element in the vector of structure
    // based on the sno field
    St max = *max_element(v.begin(), v.end(), [](const St &i, const St &j) { 
                        return i.sno < j.sno;
                    });
    cout << max.name << " " << max.sno;

    return 0;
}
```