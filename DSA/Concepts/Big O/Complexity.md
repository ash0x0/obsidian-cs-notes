Time and space complexity expresses the rate of growth of the time and the space an algorithm takes up depending on the growth of its inputs or factors. This is expressed in  [[Big O]] notation.
# Time complexity

1. Add the runtime complexity if the algorithm takes the form do x then do y
2. Multiple the runtime complexities if the algorithm takes the form do y for each time you do x
### Examples

#### Recursion

For recursion the complexity is O(branches ^ depth) where
	1. Branches is the number of recursive calls in each iteration 
	2. Depth is the total number of iterations
 
This is most often [[O(n)]] if the branches are 2 and the calls don't go all the way to N

#### Different Inputs

In a nested loop with two different inputs, the runtime won't be [[O(n²)]], it'll be `O(a*b)` where a and b are the lengths of the inputs respectively

#### Sorting array of strings

Suppose we had an algorithm that took in an array of strings, sorted each string, and then sorted the full array. What would the runtime be? Many candidates will reason the following: sorting each string is O(N log N) and we have to do this for each string, so that's O(N*N log N). We also have to sort this array, so that's an additional O(N log N) work.Therefore,the total runtime isO(N2 log N + N log N),which isjust0(N2 log N). This is completely incorrect. Did you catch the error? The problem is that we used N in two different ways. In one case, it's the length of the string (which string?). And in another case, it's the length of the array. In your interviews, you can prevent this error by either not using the variable "N" at all, or by only using it when there is no ambiguity as to what N could represent. In fact, I wouldn't even use a and b here, or m and n. It's too easy to forget which is which and mix them up. An O(a2 ) runtime is completely different from an O(a*b) runtime. Let's define new terms-and use names that are logical. Let s be the length of the longest string. Let a be the length of the array. Now we can work through this in parts: • Sorting each string is 0( s log s). • We have to do this for every string (and there are a strings), so that's 0( a* s log s). Now we have to sort all the strings. There are a strings, so you'll may be inclined to say that this takes O ( a log a) time. This is what most candidates would say. You should also take into account that you need to compare the strings. Each string comparison takes O(s) time. There are O(a log a) comparisons, therefore this will take 0( a*s log a) time. If you add up these two parts, you get 0( a* s ( log a + log s)). This is it. There is no way to reduce it further

## Amortized time

In the case of [[Vector]] the array grows in size infrequently. Even though this growth is [[O(n)]], it's so infrequent that it can be amortized over all the insertion operations essentially bringing actual time to [[O(1)]]

# Space complexity

The way this is calculated is a function of the size of the data
1. An array of n size takes up n space, so [[O(n)]]
2. A two-dimensional array of n * n size takes up n^2 space, so [[O(n²)]] 

A stack in recursive function counts for space as well
1. Calls like `return n + sum(n-1` take up [[O(n)]] 
2. Calls like `sum += pairSum(i, i + 1)` take up [[O(1)]] because the running tally is added up with each return, so the stack doesn't grow, one recursive call just takes up the same space the one that just finished took