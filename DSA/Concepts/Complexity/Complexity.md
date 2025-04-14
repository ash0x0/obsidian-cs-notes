Time and space complexity expresses the rate of growth of the time and the space an algorithm takes up depending on the growth of its inputs or factors. This is expressed in  [[Big O]] notation.
# Time complexity

1. Add the runtime complexity if the algorithm takes the form do x then do y
2. Multiple the runtime complexities if the algorithm takes the form do y for each time you do x
## Examples

### Recursion

For recursion the complexity is O(branches ^ depth) where
	1. Branches is the number of recursive calls in each iteration 
	2. Depth is the total number of iterations
 
This is most often [[O(n)]] if the branches are 2 and the calls don't go all the way to N
### Different Inputs

In a nested loop with two different inputs, the runtime won't be [[O(n²)]], it'll be `O(a*b)` where a and b are the lengths of the inputs respectively
### Sorting array of strings

Suppose we had an algorithm that took in an array of strings, sorted each string, and then sorted the full array. What would the runtime be? 

Initial intuition would be:
1. Sorting each string is `O(n log n)` and we have to do this for each string, so that's O`(n * n log n)`. 
2. We also have to sort this array, so that's an additional `O(n log n)` work.
3. Therefore, the total runtime is `O(n^2 log n + n log n)`, which is `0(n^2 log n)`. 

This is incorrect. The problem is that we used `n` in two different ways. In one case, it's the length of the string. And in another case, it's the length of the array. 

In an interview, you can prevent this error by not using `n`, or by only using it when there is no ambiguity as to what `n` could represent.  Let's define new terms-and use names that are logical. 

1. Let s be the length of the longest string.
2. Let a be the length of the array.
3. Sorting each string is `0( s log s)`.
4. We have to do this for every string (and there are `a` strings), so that's `0( a*s log s)`.
5. Now we have to sort all the strings. There are `a` strings. You should also take into account that you need to compare the strings.
6. Each string comparison takes `O(s)` time.
7. There are `O(a log a)` comparisons.
8. Therefore this will take `0( a*s log a)` time.
9. Add up the two parts, you get `0( a* s ( log a + log s))`.

## Amortized time

In the case of [[Vector]] growth for example, the data structure grows in size infrequently. Even though this growth is [[O(n)]], it's so infrequent that it can be amortized over all the access and insertion operations, essentially bringing actual time to [[O(1)]]

# Space complexity

The way this is calculated is a function of the size of the data
1. An array of n size takes up n space, so [[O(n)]]
2. A two-dimensional array of n * n size takes up n^2 space, so [[O(n²)]] 

A stack in recursive function counts for space as well
1. Calls like `return n + sum(n-1)` take up [[O(n)]] 
2. Calls like `sum += pairSum(i, i + 1)` take up [[O(1)]] because the running tally is added up with each return, so the stack doesn't grow, one recursive call just takes up the same space the one that just finished took