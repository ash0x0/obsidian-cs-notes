In many problems, we are given a set of elements such that we can divide them into two parts. 
To solve the problem, we are interested in knowing the smallest element in one part and the biggest element in the other part.

This pattern uses two heaps
1. [[Min Heap]] to find the smallest element 
2. [[Max Heap]] to find the biggest element

This pattern works by storing the first half of numbers in a Max Heap, because you want to find the largest number in the first half. 

Store the second half of numbers in a Min Heap, as you want to find the smallest number in the second half. 

At any time, the median of the current list of numbers can be calculated from the top element of the two heaps.

# When to Use

- [[Priority Queue]]
- [[Scheduling]]
- If the problem states that you need to find the smallest, largest, or median elements of a set
- Some problems featuring a [[Binary Tree]]

- Find the Median of a Number Stream (medium)