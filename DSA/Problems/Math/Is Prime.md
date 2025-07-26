---
sources:
  - https://www.geeksforgeeks.org/dsa/check-for-prime-number/
---
# Problem

Given a number `n` return true if the number is a prime number and false otherwise.
# Solution

## Brute Force - [[O(n)]] time - [[O(1)]] space

```cpp
bool isPrime(int n) {
	if (n < 2) return false;
	
    for (int i = 2; i < n; i++) {
        if (n % i == 0) return false;
    }

    return true;
}
```

## Square root - [[O(n)]] time - [[O(1)]] space

When finding the divisors of a number n, they always appear in pairs.  
For example, if 4 is a divisor of 28, then 28 / 4 = 7 is also a divisor — forming the pair (4, 7).  
This means that for every divisor d of n, there is a corresponding divisor n / d.
-> If d ≤ √n, then n / d ≥ √n. So, we only need to check for divisors up to √n.  
-> If n has a divisor greater than √n, its paired divisor would already have been checked below √n.

```cpp
bool isPrime(int n) {
	if (n < 2) return false;

    for (int i = 2; i <= sqrt(n); i++) {
        if (n % i == 0) return false;
    }

    return true;
}
```
## Prime number format - [[O(n)]] time - [[O(1)]] space

Any integer can be written as 6k + i where i is 0 to 5.
Among these, the forms 6k, 6k+2, 6k+3, and 6k+4 are divisible by 2 or 3, so they can't be prime (except numbers 2 and 3).  
Therefore, all prime numbers greater than 3 must be of the form 6k+1 or 6k+5.  
So, while checking for factors up to √n, we can skip numbers that don't match these forms — reducing the number of checks.

```cpp
bool isPrime(int n) {
    if (n <= 1) return false;
    if (n == 2 || n == 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    
    // Check from 5 to square root of n
    // Iterate i by (i+6)
    for (int i = 5; i <= sqrt(n); i = i + 6)
        if (n % i == 0 || n % (i + 2) == 0)
            return false;

    return true;
}
```