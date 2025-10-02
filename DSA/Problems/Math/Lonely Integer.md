---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-lonely-integer/
---
#Google #DailyCodingProblem #Hard 
# Problem

## DailyCodingProblem

Given an array of integers where every integer occurs three times except for one integer, which only occurs once, find and return the non-duplicated integer.

For example, given [6, 1, 3, 3, 3, 6, 6], return 1. Given [13, 19, 13, 13], return 19.

Do this in O(N) time and O(1) space.
# Solution

## [[Bitwise XOR]] - [[O(n)]] time - [[O(1)]] space

```cpp
int lonelyinteger(vector<int> a) {
	int xorSum = 0;
	for (int i = 0; i < a.size(); i++) {
		xorSum ^= a[i];
	}
	return xorSum;
}
```
