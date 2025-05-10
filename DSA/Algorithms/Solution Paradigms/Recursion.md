---
sources:
  - https://www.youtube.com/playlist?list=PL2_aWCzGMAwLz3g66WrxFGSXvSsvyfzCO
---

Recursion is an approach to solving problems using a function that calls itself as a subroutine. Each time a recursive function calls itself, it reduces the given problem into subproblems. The recursion call continues until it reaches a point where the subproblem can be solved without further recursion.

# Implementation

A recursive function should have the following properties so that it does not result in an infinite loop:

1. Base Case (or cases) - A terminating scenario that does not use recursion to produce an answer and we can compute an answer directly. Also called bottom case(s).
2. Recurrence Relation - the relationship between the result of a problem and the result of its sub-problems. Can also be described as a set of rules that reduces all other cases towards the base case.

There could be multiple places where the function may call itself.

## Methodology

1. Define the problem as a function $f(x)$ where `x` is the input
2. Break the problem into smaller scopes / iterations 0, 1, 2, ... ,n
3. Call the function $f(x)$ recursively to solve subproblems of `x`
4. Process the result from each recursive call to solve the problem for the input iteration `x`
## Guidelines

1. When in doubt whether you can apply recursion, write down the **recurrence relationship** in some mathematical formula.
2. Whenever possible, apply **memoization**. Usually when you trace the calls and find there's duplicate computation in some steps.
3. When stack overflows, **tail recursion** might help.

Recursive calls also often result in re-calculations as for subsequent recursions. The key way to mitigate this is using [[Memoization]].

# Complexity

## Time

This is is typically the product of number of recursion invocations and the time complexity of calculation for each recursive call

It is rarely the case that the number of recursion calls happens to be linear to the size of input. To calculate the correct number of calls we use an execution tree.
### Execution Tree

A tree that is used to denote the execution flow of a recursive function. Each node represents an invocation of the recursive function. The total number of nodes is the number of recursion calls.

It usually forms an [[N-ary Tree]] with `n` as the number of times recursion appears in the recurrence relation. 

For instance, the execution of the Fibonacci function would form a [[Binary Tree]] since there are two function calls in the recurrence relationship `f(n) = f(n-1) + f(n-2)`. In a binary tree with `n` levels there would be $2^n -1$ nodes so the complexity would be [[O(2^N)]].

[[Memoization]] reduces the number of recursive calls. For Fibonacci recurrent relationship for example we wouldn't need to compute `f(n-2)` for each call so the number of calls becomes `n-1` and the complexity becomes [[O(n)]].
## Space

If there is no space take up in the algorithm then the space complexity is just the recursion related space.
### Recursion Overhead

Memory cost incurred by recursive function calls. i.e. call stack space. Since recurrsion only resolves at the end of the callchain when we reach the base case, this space taken up for each of the recursive steps for the entire chain. 

Due to this space consumption a stack overflow might happen. When designing a recursive algorithm, we should check if there is a possibility of stack overflow as the input scales.

One way to alleviate this is using [[Tail Recursion]].
### Non-recursion related

This is the space take up for other variables and data structures, usually global ones accessed by the recursive function. This applies to [[Memoization]].

# Conversion to Iteration

We could convert recursion into iteration using a [[Stack]]. Similar to the behaviors of the function call stack, a stack follows the pattern of FILO (First-In-Last-Out), _i.e._ the last element that is added to a stack would come out first. 

Common problems:
- factorial
- Fibonacci 
- permutations 
- subsets
