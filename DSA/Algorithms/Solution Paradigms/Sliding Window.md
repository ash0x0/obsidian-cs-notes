Perform an operation on a specific window size of a given data structure.
# Mechanism

1. Start from the 1st element
2. Keep shifting right by one element
3. Adjust the length of the window according to the problem
	1. In some cases, the window size remains constant
	2. In other cases the sizes grows or shrinks.

![[Pasted image 20250509223647.png]]
## When to use

1. The problem input is a linear data structure such as a linked list, array, or string.
2. You’re asked to find the longest/shortest substring, subarray, or a desired value
	1. Finding the longest subarray containing all 1s.