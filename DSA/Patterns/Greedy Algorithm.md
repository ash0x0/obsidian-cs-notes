An approach to finding global optimum solutions by finding local optimum solutions.
A `greedy choice property` means that choosing the local optimal choice can't hurt the final solution.
# When to use

1. Need to make a sequence of local choices that lead to a global optimum.
2. Hiring schedule 
3. Interval scheduling / Activity Selection
	1. Choose the lecture that ends earliest
	2. Remove all that overlap
	3. repeat → maximal number of non-overlapping intervals.
4. Minimum coins for change with canonical coin systems
	1. Always pick the largest coin ≤ remaining amount.
5. Jump Game
	1. At each position, greedily pick the farthest reachable index.
# Mechanism

## Sketch

For the activity selection problem

1. Sort lectures by end time.
2. `count = 1`, `lastEnd = end[0]`
3. For each interval `(s[i], e[i])` starting from i=1:
	- If `s[i] ≥ lastEnd`, `count++`, `lastEnd = e[i]`.
4. Return `count`

Time: O(n log n)
Space: O(1).