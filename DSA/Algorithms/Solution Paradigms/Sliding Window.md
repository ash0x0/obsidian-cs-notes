Perform an operation on a specific window size of a given data structure.
# When to use

1. Input is an array, string, or lined list
2. Find the substring, subarray, or value to satisfy a condition
	1. Longest subarray containing all 1s.
	2. Max sum
	3. Exactly k distinct
	4. Longest without repeating
	5. Minimum size subarray ≥ S.
# Methodology

1. Start from the 1st element
2. Keep shifting right by one element
3. Adjust the length of the window according to the problem
	1. In some cases, the window size remains constant
	2. In other cases the sizes grows or shrinks.
## Idea

- Maintain a window `[i..j]` that satisfies (or almost satisfies) your condition.
- Expand `j` until the condition is violated or met
- Then contract `i` as far as possible while still (barely) satisfying the condition.
## Sketch

1. Initialize `i = 0, currentSum = 0`.
2. For `j` in `0..n–1`:
	- Add `arr[j]` to `currentSum`.
	- While `currentSum ≥ S`:
		- Update `minLength = min(minLength, j – i + 1)`.
		- Subtract `arr[i++]` from `currentSum`.
3. If `minLength` was never updated, no solution; else return `minLength`.

Time: O(n), since each element enters/exits window at most once. 
Space: O(1), or O(k) if you track counts of distinct elements via a hash.

![[Pasted image 20250509223647.png]]
