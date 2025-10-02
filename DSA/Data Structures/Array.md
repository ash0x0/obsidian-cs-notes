An array is a fundamental data structure that stores a collection of elements in contiguous memory locations. Arrays offer easy and random access but hard modification; having to move elements around like shifting to the front or back of the array.

# Characteristics

- **Fixed size**: Arrays have a predetermined size that cannot be changed after creation (in static arrays)
- **Homogeneous**: All elements are of the same data type
- **Contiguous memory**: Elements are stored in consecutive memory locations
- **Index-based access**: Elements can be accessed using zero-based indices
- **Random access**: [[O(1)]] time complexity for accessing any element

# Operations & Time Complexity

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| Access | [[O(1)]] | Direct access via index |
| Search | [[O(n)]] | Linear search through elements |
| Insert (end) | [[O(1)]] | Add to end (if space available) |
| Insert (middle) | [[O(n)]] | Requires shifting elements |
| Delete (end) | [[O(1)]] | Remove from end |
| Delete (middle) | [[O(n)]] | Requires shifting elements |

# Declaration

## C++

```cpp
// Static array
int arr[5] = {1, 2, 3, 4, 5};

// Dynamic array using vector
vector<int> vec = {1, 2, 3, 4, 5};
vector<int> vec(10);  // Size 10, default initialized

// Multi-dimensional array
int matrix[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
vector<vector<int>> mat(3, vector<int>(3));
```

# Common Operations

## Traversal

```cpp
for (int i = 0; i < arr.size(); i++) {
    cout << arr[i] << " ";
}

// Range-based loop
for (int element : arr) {
    cout << element << " ";
}
```

## Insertion

```cpp
// Insert at end
vec.push_back(6);

// Insert at position
vec.insert(vec.begin() + 2, 10);  // Insert 10 at index 2
```

## Deletion

```cpp
// Remove from end
vec.pop_back();

// Remove from position
vec.erase(vec.begin() + 2);  // Remove element at index 2
```

## Searching

```cpp
// Linear search
int search(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}

// Binary search (for sorted arrays)
int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

# Advantages

- Fast access to elements ([[O(1)]])
- Memory efficient (no extra pointers needed)
- Cache-friendly due to contiguous memory
- Simple and easy to use

# Disadvantages

- Fixed size (for static arrays)
- Expensive insertions and deletions ([[O(n)]])
- Wastes memory if not fully utilized
- Difficult to resize

# Applications

- Storing and accessing sequential data
- Implementing other data structures ([[Stack]], [[Queue]], [[Heap]])
- Lookup tables and hash tables
- Matrix operations
- Buffers for I/O operations
- Image processing (pixel data)

# Related Data Structures

- [[Vector]] - Dynamic array
- [[Linked List]] - Alternative for frequent insertions/deletions
- [[Hash Table]] - For key-value storage
- [[Matrix]] - Multi-dimensional array
