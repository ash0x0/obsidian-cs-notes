Recursion is an approach to solving problems using a function that calls itself as a subroutine. Each time a recursive function calls itself, it reduces the given problem into subproblems. The recursion call continues until it reaches a point where the subproblem can be solved without further recursion.

A recursive function should have the following properties so that it does not result in an infinite loop:

1. A simple `base case` (or cases) — a terminating scenario that does not use recursion to produce an answer.
2. A set of rules, also known as `recurrence relation` that reduces all other cases towards the base case.

There could be multiple places where the function may call itself.

Recursion basics: **base case, recursive case**.

Common problems:
- factorial
- Fibonacci 
- permutations 
- subsets
