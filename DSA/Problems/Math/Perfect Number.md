---
Leetcode: https://leetcode.com/problems/perfect-number/description/
---
# Problem

A [**perfect number**](https://en.wikipedia.org/wiki/Perfect_number) is a **positive integer** that is equal to the sum of its **positive divisors**, excluding the number itself. A **divisor** of an integer `x` is an integer that can divide `x` evenly.

Given an integer `n`, return `true` _if_ `n` _is a perfect number, otherwise return_ `false`.

**Example 1:**
**Input:** num = 28
**Output:** true
**Explanation:** 28 = 1 + 2 + 4 + 7 + 14
1, 2, 4, 7, and 14 are all divisors of 28.

**Example 2:**
**Input:** num = 7
**Output:** false

**Constraints:**
- `1 <= num <= 108`

# Solution

## Brute Force - [[O(n)]] time - [[O(1)]] space

```cpp
class Solution {
public:
    bool checkPerfectNumber(int num) {      
        int sum = 0;
        for (int i = 1; i < num; i++) {
            if (num % i == 0) sum += i;
        }
        return sum == num;
    }
};
```

## Square Root - [[O(n)]] time - [[O(1)]] space

Minimize the steps by adding both divisors at once.
Minimize search space by going up to the square root of the number.
This is because the largest factor $a$ in $a*b$ will be $sqrt(n)$ after which $a$ and $b$ cross-over and we start trying duplicate factor pairs.

```cpp
class Solution {
public:
    bool checkPerfectNumber(int num) {      
        if (num <= 1) return false;
        int sum = 1;
        
        for (int i = 2; i <= sqrt(num); i++) {
            if (num % i == 0) {
                sum += i;
                if (i != num / i) sum += num / i;
            }
        }
        return sum == num;
    }
};
```