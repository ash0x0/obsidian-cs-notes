An efficient [[Breadth First Search]] approach to handle permutations and combinations problems
# Mechanism

Given a set of [1, 5, 3]

1. Start with an empty set `[[]]`
2. Add the first number `1` to all the existing subsets to create new subsets `[[], [1]]`
3. Add the second number `5` to all the existing subsets `[[], [1], [5], [1,5]]`
4. Add the third number `3` to all the existing subsets `[[], [1], [5], [1,5], [3], [1,3], [5,3], [1,5,3]]`

![[Pasted image 20250510000851.png]]

# When to Use

1. Problems where you need to find the combinations or permutations of a given set

- Subsets With Duplicates (easy)
- String Permutations by changing case (medium)