---
HackerRank:
  - https://www.hackerrank.com/challenges/one-week-preparation-kit-recursive-digit-sum/problem
---
# Problem

We define super digit of an integer  using the following rules:

Given an integer, we need to find the _super digit_ of the integer.

- If  has only  digit, then its super digit is .
- Otherwise, the super digit of  is equal to the super digit of the sum of the digits of .

For example, the super digit of  will be calculated as:

	super_digit(9875)   	9+8+7+5 = 29 
	super_digit(29) 	2 + 9 = 11
	super_digit(11)		1 + 1 = 2
	super_digit(2)		= 2  

**Example** 

The number  is created by concatenating the string   times so the initial .

    superDigit(p) = superDigit(9875987598759875)
                  9+8+7+5+9+8+7+5+9+8+7+5+9+8+7+5 = 116
    superDigit(p) = superDigit(116)
                  1+1+6 = 8
    superDigit(p) = superDigit(8)

All of the digits of  sum to . The digits of  sum to .  is only one digit, so it is the super digit.

**Function Description**

Complete the function _superDigit_ in the editor below. It must return the calculated super digit as an integer.

superDigit has the following parameter(s):

- _string n:_ a string representation of an integer
- _int k:_ the times to concatenate  to make 

**Returns**

- _int:_ the super digit of  repeated  times

**Input Format**

The first line contains two space separated integers,  and .

**Constraints**

$1 < n < 10^100000$
$1 <= K <= 10^5$
# Solution

## [[Recursion]] - [[O(n)]] time - [[O(1)]] space

```cpp
int superDigit(string n, int k) {}
	if (n.length() == 1) return n[0] - '0';
	unsigned long long result = 0;
	for (unsigned long long i = 0; i < n.length(); i++) {
		result += (n[i] - '0');
	}
	result *= k;
	return superDigit(to_string(result), 1);
}
```

> **Note:** To pass all the test cases the `result` and `i` both need to be large enough numeric types. The problem mentions that the return must be `int` but the return happens in the tail with the first line, always int. In the intermediate steps in the recursion tree and especially the first step very large numbers are expected.