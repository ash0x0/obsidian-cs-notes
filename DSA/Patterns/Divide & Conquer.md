Divide and Conquer is a fundamental algorithm design paradigm that breaks down a problem into smaller subproblems, solves them independently, and combines their solutions to solve the original problem.

# Strategy

The divide and conquer approach consists of three steps:

1. **Divide**: Break the problem into smaller subproblems of the same type
2. **Conquer**: Recursively solve the subproblems (or solve directly if small enough)
3. **Combine**: Merge the solutions of subproblems to create the solution to the original problem

# Key Characteristics

- Recursion is typically used to implement divide and conquer
- Subproblems should be independent of each other
- Often results in [[O(n log n)]] time complexity
- Base case needed to stop recursion

# Common Examples

## 1. Binary Search

```cpp
int binarySearch(vector<int>& arr, int target, int left, int right) {
    if (left > right) return -1;
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) return mid;
    if (arr[mid] > target) return binarySearch(arr, target, left, mid - 1);
    return binarySearch(arr, target, mid + 1, right);
}
```

## 2. Merge Sort

```cpp
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) temp[k++] = arr[i++];
        else temp[k++] = arr[j++];
    }
    
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    
    for (int i = 0; i < k; i++) {
        arr[left + i] = temp[i];
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;
    
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}
```

## 3. Quick Sort

```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

# Time Complexity Analysis

The time complexity can often be expressed using recurrence relations:

- **Binary Search**: T(n) = T(n/2) + O(1) = [[O(log n)]]
- **Merge Sort**: T(n) = 2T(n/2) + O(n) = [[O(n log n)]]
- **Quick Sort**: T(n) = 2T(n/2) + O(n) = [[O(n log n)]] average case

Use the **Master Theorem** to analyze recurrence relations.

# Advantages

- Often leads to efficient algorithms
- Naturally parallelizable (subproblems are independent)
- Easier to analyze time complexity
- Reduces problem complexity

# Disadvantages

- May use more memory due to recursion stack
- Overhead of recursive calls
- Not always the most optimal approach
- Base case must be handled carefully

# Applications

- Sorting algorithms ([[Merge Sort]], [[Quick Sort]])
- Searching algorithms ([[Binary Search]])
- Matrix operations (Strassen's algorithm)
- Closest pair of points problem
- Fast Fourier Transform (FFT)
- Karatsuba multiplication
- Tower of Hanoi

# Related Concepts

- [[Dynamic Programming]] - Similar but subproblems overlap
- [[Recursion]] - Implementation technique
- [[Greedy Algorithms]] - Alternative paradigm
- [[Backtracking]] - Related approach for constraint problems
