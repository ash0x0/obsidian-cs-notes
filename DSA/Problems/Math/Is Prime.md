---
sources:
  - https://www.geeksforgeeks.org/dsa/check-for-prime-number/
  - https://www.jointaro.com/course/crash-course-data-structures-and-algorithms-concepts/is-prime-approach/
---
# Problem

## Basic

Given a number `n` return true if the number is a prime number and false otherwise.
## Structy

> Write a function, _is_prime_, that takes in a number as an argument. The function should return a boolean indicating whether or not the given number is prime.
> 
> A prime number is a number that is only divisible by two distinct numbers: 1 and itself.
> 
> For example, 7 is a prime because it is only divisible by 1 and 7. For example, 6 is not a prime because it is divisible by 1, 2, 3, and 6.
> 
> You can assume that the input number is a positive integer.

```
is_prime(2) # -> True
```

```
is_prime(3) # -> True
```

```
is_prime(4) # -> False
```

```
is_prime(5) # -> True
```

```
is_prime(6) # -> False
```

```
is_prime(7) # -> True
```

```
is_prime(8) # -> False
```

```
is_prime(2017) # -> True
```

```
is_prime(2048) # -> False
```
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
## Prime number formula - [[O(n)]] time - [[O(1)]] space

Any integer can be written as 6k + i where i is 0 to 5.
Among these, the forms 6k, 6k+2, 6k+3, and 6k+4 are divisible by 2 or 3, so they can't be prime (except numbers 2 and 3).  
Therefore, all prime numbers greater than 3 must be of the form 6k+1 or 6k+5. i.e. any prime number can be `6k +/- 1`
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