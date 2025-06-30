---
sources:
  - https://www.geeksforgeeks.org/linear-search
  - https://www.geeksforgeeks.org/improving-linear-search-technique/
aliases:
  - Sequential Search
---
1. Iterate over all the elements of the array and check if it the current element is equal to the target element. 
2. If we find an element equal to the target element, return the index of the element.
3. Otherwise, return -1.
# Advantages

- Can be used irrespective of whether the array is sorted or not. It can be used on arrays of any data type.
- Does not require additional memory.
- Well-suited for small datasets.
# Usage

- Unsorted list
- Small data set
- Searching linked list. Each node is checked sequentially until the desired element is found.
- When you are searching for a dataset stored in contiguous memory.