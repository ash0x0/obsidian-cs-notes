#Microsoft #DailyCodingProblem #Easy
# Problem

A number is considered perfect if its digits sum up to exactly 10.
Given a positive integer `n`, return the `n`-th perfect number.
For example, given 1, you should return 19. Given 2, you should return 28.
# Solution

## Brute Force - [[O(n)]] time - [[O(1)]] space

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
		int count = 0;
		int number = 19;
		while(true) {
			if (digitSum(number) == 10) {
				count++;	
			}
			if (count == digit) return number;
			number++;
		}
	}
}
```