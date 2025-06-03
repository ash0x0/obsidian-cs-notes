Stands for **Bottlenecks, Unnecessary , Duplicated** work. These are the three main things to look for when optimizing code.

1. Look for ununsed info in the problem
2. Solve manually on an example and reverse engineer how the algorithm works
3. Solve incorrectly and think about why the algorithm fails
4. Use a fresh and different example
5. What can we store or [[Memoization|Memoize]] to save time?
6. Make time vs space tradeoffs. [[Hash Structures]] are useful
7. Pre-process information. Maybe sort the array first, etc.
8. If array related, consider [[Sorting]]
9. If search related, consider [[Binary Search]]
10. If parsing [[Tree]] or [[Graphs]] or traversal or reversal consider a [[DSA/Data Structures/Stacks & Queues/Stack]]
11. If dealing with lots of strings, try putting them in a [[Trie|Prefix Tree]]
12. Does the problem have optimal substructure? optimal solution to sub-problems helps get optimal solution to the problems? Consider [[Greedy Algorithm]]
13. Does solving the problem for `n-1` help solve it for `n`? Consider [[Dynamic Programming]]
14. Do you have to take min/max of a dynamic collection? Consider [[Heap]]