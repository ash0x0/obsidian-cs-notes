#Microsoft #DailyCodingProblem #Easy
# Problem

A number is considered perfect if its digits sum up to exactly 10.
Given a positive integer `n`, return the `n`-th perfect number.
For example, given 1, you should return 19. Given 2, you should return 28.
# Solution

## Brute Force - [[O(1)]] time - [[O(1)]] space

Because this is a two digit number, we know that the digitSum function will only have 2 iterations so it's constant time.
We also know that the perfectNumber function will have at most 9 iterations, so it's also constant time.

O(9 * 2) = O(1)  

```cpp
class Solution {
	int digitSum(int number) {
		int sum = 0;
		while (number != 0) {
			sum += number % 10;
			number /= 10;
		}
		return sum;
	}

	int perfectNumber(int digit) {
		for (int i = 0; i < 10; i++) {
			int number = digit * 10 + i;
			if (digitSum(number) == 10) {
				return number;
			}
		}
	}
}
```