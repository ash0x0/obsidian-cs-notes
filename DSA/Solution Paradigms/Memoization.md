---
aliases:
  - Memoize
---

A strategy to store intermediate results or results of expensive calls to use them later without re-calculation when the same inputs occur again. Think of it as a computation cache. 

This is commonly used to improve the time complexity in [[Recursion]].
# Implementation

- Storing results of function calls (recursive or not) in an [[Array]], [[Associative Array]] like a [[Hash Table]].
- Instead of calling a function we check the store to see if we have a computed result. 
- If result is memoized, we use it
- If not, we call the function and memoize the result

The memoization data structure needs to be outside the function to save and share state, i.e. it needs to be a class member.

One way to implement memoization in a non-intrusive general way without changing the original function or module is using [[Decorator Design Pattern]]