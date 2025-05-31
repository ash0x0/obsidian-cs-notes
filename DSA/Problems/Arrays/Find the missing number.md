---
sources:
  - https://www.educative.io/blog/crack-amazon-coding-interview-questions
---
#Amazon
# Problem

You are given an array of positive numbers from `1` to `n`, such that all numbers from `1` to `n` are present except one number `x`. You have to find `x`. The input array is not sorted.

[3,7,1,2,8,4,5]
n = 8 
missing number = 6

# Solution

Any solution will be at least O(n) because we need to examine all numbers to find the missing one.

## Sort then iterate - O(n) time - O(1) space

```cpp
class Solution {
	int findMissing(vector<int> numbers) {
		sort(numbers.begin(), numbers.end());
		for (int i = 0; i < numbers.size(); i++) {
			if (numbers[i] != i+1) return i+1;
		}
		return -1;
	}
}
```

## Missing Sum - O(n) time - O(1) space

```cpp
class Solution {
	int findMissing(vector<int> numbers) {
		int correctSum = 0;
		int currentSum;
		for (int i = 1; i <= numbers.size(); i++) correctSum++;
		for (int i = 0; i < numbers.size(); i++) currentSum += numbers[i];
		int answer = correctSum - currentSum;
		return answer >= 0 ? answer : -1;
	}
}
```

## [[Cyclic Sort]]