You are selected to play a game with n balloons. Each balloon is painted with a number on it representing the score you can get by popping the balloon. But there is a rule that you can not pop adjacent balloons. Given an integer array nums representing the scores of each balloon, return the maximum score you can get without violating the rule.

# Distillation

Given an array nums of balloons
What’s the max sum of balloons you can pop
Given that each balloon has a score n in nums
You can not pop two adjacent balloons
Scores can be any length
Count can be any number
# Example 

[3,2,5,1,9]
[x,2,x,1,x] => max = 17
[2,2,2,2]
[2,1,1,5]
# Requirement

- Max score I can get
# Solution

## Brute Force

1. Iterate through    
2. Pop every other balloon
3. Get the max of this run
4. Repeat, this time popping the ones we didn’t pop in 2 and skipping the ones we popped there

[2,1,1,5] => max = 7
[2, x, 1, x] => maxSum1 = 6
[x, 1, x, 1] => maxSum2 = 3
[x,1,1,x] 
[x,2,x,1,x]
[x,2,x,1,x]
[2,3,5,1,9]
[2,3,x,1,x]

Overlapping pairs
1. Select overlapping pairs
2. Determine which candidate to pop
3. Keep going until you can’t replace the current candidate, then pop it
4. Go over array again to find ones you can pop

[2,3,5,1,9]
[X]
[X,Y]
max(i, i+1)
[X,Y,Z]
sol(i,sol([0, i-2]))
sol(i-1, 

1. At every index i
2. Pop and considering up to i - 2 or i + 2
3. Skip and considering up to i - 1 or i + 1
4. Solution = solution(i) + solution(solution (n-1))
5. Solution[i] = max(i, solutions(i-2), solutions(i-1))

[2,3,5,1,9]
I stops before 6
[2,3,0,0,0,0,0]
I = 2
[2,3,7,0,0,0,0]
I = 3
[2,3,7,7,0,0,0]
I = 4
[2,3,7,7,16,0,0]

```cpp
using namespace std;

class Solution {
public:
	int findMaxBaloons(vector<int>& nums) 
		vector<int> solutions = vector(nums.size(), 0);
		solutions[0] = nums[0];
		solutions[1] = max(solusions[0], nums[1]);

		for (int i = 2; i < nums.size(); i++) {		
			solutions[i] = max(nums[i] + solutions[i-2], solutions[i-1]);
		}
		return solutions[solusions.size() - 1];
	}
}
```
