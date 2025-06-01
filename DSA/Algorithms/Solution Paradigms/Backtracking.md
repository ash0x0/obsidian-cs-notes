A form of [[Recursion]] that generates different solutions to a problem and continuously check if the solution satisfies constraints.
If violations are found in the current solution, it **backtracks** the latest step to undo it and try a different path.
This is considered doing [[Depth First Search|DFS]] through a solution tree.
# When to use

1. “Generate all permutations,"
2. “subsets”
3. “combinations”
4. “N-Queens”
5. “word search”
6. “sudoku.
7. n is small enough (n ≤ 20, or constraints permit exponential time).
# Methodology

- Build partial solutions step by step.
- If adding the next choice violates constraints, **backtrack** (undo the change) and try something else.
## Sketch

Generate subsets of `arr[0..n–1]`

```cpp
void backtrack(int idx, vector<int>& current) {
    if (idx == n) {
        output.push_back(current);
        return;
    }
    // Option 1: skip arr[idx]
    backtrack(idx + 1, current);
    // Option 2: include arr[idx]
    current.push_back(arr[idx]);
    backtrack(idx + 1, current);
    current.pop_back();
}
backtrack(0, {});
```

Time: O(2ⁿ) subsets. Space: O(n) recursion plus O(2ⁿ · n) output.