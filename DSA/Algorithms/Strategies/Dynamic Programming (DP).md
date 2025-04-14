A programming paradigm to explore all possible solutions to a problem. Problems often have these characteristics:

1. It can be broken down into overlapping subproblems - smaller versions of the original problem that are re-used multiple times.
2. The problem has an optimal substructure - an optimal solution can be formed from optimal solutions to the overlapping subproblems.

This is similar to:

1. [[Greedy Algorithms]] in which problems have optimal substructure, but not overlapping ones.
2. [[Divide & Conquer]] algorithms that break a problem into subproblems, but not overlapping ones.
# Identification

There are some problem descriptors that hit at a possible DP solution. These are shorthands that aren't always to be followed

- The problem asks for an optimum value (max or min) of something
	- Minimum cost of x
	- Maximum profit from x
	- What is the longest possible
- The problem asks for the number of ways to do something
	- How many ways are there to do x
	- Is it possible to reach a certain point
- **Future decisions rely on earlier decisions**
	- Deciding to do something at step i affect the ability to do something at a later step
	- Using one element prevents the usage of other elements,
	- This is what makes a greedy algorithm invalid and a DP approach the correct one
	- When trying to determine if this characteristic exists, assume it doesn't, come up with the greedy algorithm then use a counterexample to prove it won't work. This leads to concluding DP is the correct approach
# Implementation

Any DP algorithm can be implemented in one of two ways. Bottom-up with tabulation or top-down with memoization.

To convert a top-down solution into a bottom-up solution, we must first find a logical order in which to iterate over the states.
## Bottom-up (Tabulation)

- Starts at a base case
- Implemented as iteration. Typically nested for loops with [[Arrays]]
- Runtime is usually faster, as iteration does not have the overhead that recursion does.
- Its downside is the ordering matters. We need to go through a logical ordering of solving subproblems.

## Top-down ([[Memoization]])

- Implemented as recursion with memoization (typically using [[Hashmap]])
- Usually easier to write, because with recursion the ordering of subproblems does not matter
- This also means that we don't need to solve every single subproblem to find the answer to a problem

## Top-down to Bottom-up

Start with the completed top-down implementation

1. Initialize an array `dp` that is sized according to the state variables. 
	1. For example, if input is an array `nums` and an integer `k` that represents the maximum number of actions allowed.
	2. Your array `dp` would be 2D with one dimension of length `nums.length` and the other of length `k`. 
	3. The values should be initialized as some default value opposite of what the problem is asking for. 
	4. If the problem is asking for the maximum of something, set the values to negative infinity. 
	5. If it is asking for the minimum of something, set the values to infinity.
2. Set your base cases and initialize them in the array at the proper indices, same as the ones you are using in your top-down function.
3. Write a for-loop(s) that iterates over state variables. 
	1. If you have multiple state variables, you will need nested for-loops.
	2. These loops should **start iterating from the base cases**.
	3. Each iteration of the inner-most loop represents a given state, and is equivalent to a function call to the same state in top-down. 
4. Copy the logic from your function into the for-loop and change the function calls to accessing your array. All `dp(...)` changes into `dp[...]`.
5. Return the answer to the original problem, by changing `return dp(...)` to `return dp[...]`.

### Example

Starting from [[House Robber 1]] Top-down implementation

```cpp
using namespace std;
class Solution {
public:
    unordered_map<int, int> memo;
    vectoc<int> nums;
    
    int dp(int i) {
        // Base cases
        if (i == 0) return nums[0];
        if (i == 1) return max(nums[0], nums[1]);
        if (!memo.contains(i)) {
            memo[i] = max(dp(i - 1), dp(i - 2) + nums[i]);
        }
        return memo[i];
    }
    
    int rob(vector<int> nums) {
        this->nums = nums;
        return dp(nums.size() - 1);
    }
}
```

1. Initialize an array `dp`

```cpp
using namespace std;
class Solution {
public:
    int rob(vector<int> nums) {
	    if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size(), 0);
    }
}
```

2. Set your base cases and initialize them in the array

```cpp
using namespace std;
class Solution {
public:
    int rob(vector<int> nums) {
	    if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size(), 0);

		dp[0] = nums[0];
		dp[1] = max(nums[0], nums[1]);
    }
}
```

3. Write a loop to iterate over state variables

```cpp
using namespace std;
class Solution {
public:
    int rob(vector<int> nums) {
	    if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size(), 0);

		dp[0] = nums[0];
		dp[1] = max(nums[0], nums[1]);
	
		for (int i = 0; i < nums.size(); i++) {
		
		}
    }
}
```

4. Copy the logic from the function into the loop and change from function calls to array acces

```cpp
using namespace std;
class Solution {
public:
    int rob(vector<int> nums) {
	    if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size(), 0);

		dp[0] = nums[0];
		dp[1] = max(nums[0], nums[1]);
	
		for (int i = 0; i < nums.size(); i++) {
			if (!dp.contains(i)) {
	            dp[i] = max(dp[i - 1] dp[i - 2] + nums[i]);
	        }
		}
		return dp[nums.size() -1];
    }
}
```
## Framework

1. Define state - a state is a current step in the process, e.g. standing on step 6 in [[70. Climbing Stairs]] or robbing house 4 in [[House Robber 1]].
2. Define state variables - a set of variables that can describe the scenario and define the current state, e.g. the step we're standing on or the house we're considering robbing, usually this is just the `i` step in the iteration
3. Create a data structure or function that will compute or contain the answer to a problem given a certain state.
	1. If this is a function then it's just a generalized step-wise form of the question we're being asked. For example, if it's "How many ways to climb to the top?" the function solves "How many ways to climb to step `i`?" 
4. Find a recurrence relation to transition from state `i` to `i-1`, `i+1` or any other state
	1. Ask what are the things we can do to get to state `i` or to another state from this state
	2. Ask what are the options we have at state `i`
5. Find a base case. This is most often initialized at the beginning and presents a known solution to a simple or initial form of the problem such as when `i = 1` and `i = 2`
	1. Ask what state(s) can I find the answer to without using dynamic programming?

# Multidimensional DP

The dimension of a DP problem refers to the number of state variables needed to define each state.

Some common things to look out for as state variables:

- An index along some input. 
	- This is usually used if an input is given as an array or string. 
	- It usually represents the answer to the problem if the input was considered only up to that index in one-dimensional problems.
- A second index along some input. 
	- Sometimes, you need two index state variables, say `i` and `j`. 
	- In some problems, these variables represent the answer to the original problem if you considered the input starting at index `i` and ending at index `j`.
- Explicit numerical constraints given in the problem. 
	- For example, "you are only allowed to complete `k` transactions", or "you are allowed to break up to `k` obstacles", etc.
- Variables that describe statuses in a given state. 
	- For example "true if currently holding a key, false if not", "currently holding `k` packages" etc.
- Some sort of data like a tuple or bitmask used to indicate things being "visited" or "used". 
	- For example, "bitmask is a mask where the `i`th bit indicates if the `i`th city has been visited". 
	- Mutable data structures like arrays cannot be used. Typically, only immutable data structures like numbers and strings can be hashed, and therefore memoized.

# Examples

## Fibonacci

If you wanted to find the `n` Fibonacci number $f(n)$, you can break it down into smaller subproblems 
1. Find $f(n-1)$ and $f(n-2)$ 
2. Adding $f(n-1) + f(n-2)$ to get $f(n)$

This means the problem has optimal substructure, since a solution to the original problem can be formed from the solutions to the subproblems. These subproblems are also overlapping - for example, we would need $f(4)$ to calculate both $f(5)$ and $f(6)$.



- Top-down (memoization)
- Bottom-up (tabulation)**.

Common patterns:
- knapsack
- coin change
- longest common subsequence (LCS)
- matrix chain multiplication
 
 