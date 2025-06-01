---
sources:
  - https://leetcode.com/discuss/post/494279/comprehensive-data-structure-and-algorit-tdez/
  - https://github.com/khanhnamle1994/technical-interview-prep/blob/master/Technical-Interview-Study-Guide.pdf
  - https://www.youtube.com/playlist?list=PLUl4u3cNGP61Oq3tWYp6V_F-5jb5L2iHb
  - https://www.youtube.com/playlist?list=PLDV1Zeh2NRsDGO4--qE8yH72HFL1Km93P
  - https://www.youtube.com/playlist?list=PLDV1Zeh2NRsB6SWUrDFW2RmDotAfPbeHu
  - https://www.youtube.com/mycodeschool
  - https://www.youtube.com/channel/UCD8yeTczadqdARzQUp29PJw
  - https://www.youtube.com/@abdul_bari
  - https://www.linkedin.com/pulse/20141120061048-6976444-ace-the-coding-interview-every-time/
  - https://steve-yegge.blogspot.com/2008/03/get-that-job-at-google.html
---
# Solution Steps

1. Listen and pay attention to the problem description
	1. Record every piece of unique or potentially useful information. Likely needed for optimal solution
2. Ask questions to understand problem
	1. Repeat the problem as you understand it out loud
	2. Ask questions about the part you think will be hardest. You might be understanding it incorrectly and might be easier than you think or not need to address it
	3. Ask questions about the data: types, what the data presents, what it's meant for, who'll be using it, etc.
3. Create an example that's representative, not too small and not a special case
4. Develop a brute force solution and share it. Don't write code, just talk through it or write it in comments 
5. Design an algorithm
	1. If the answer isn't obvious, look for a simpler variation of the problem. 
		1. Remove one of the constraints or multiple
		2. Think about any subproblems you can solve and start from there
	2. Does your algorithm leverage all available information or did you disregard or overlooking something useful?
	3. What's the space and time complexity?
	4. What happens if there's a lot of data?
	5. What happens if there's no data or wrong data?
	6. Does this design create other issues?
	7. Did you make the right trade-offs? For which scenarios might the trade-offs be less optimal?
6. Walk through the optimal approach in detail before you start coding. Make sure you understand each step
7. Write code at a moderate pace
	1. Write pseudocode or comments first. Make sure to tell the interviewer you'll write real code after
	2. Write the code. Modularize and refactor as you go. Objective is to write good clean code
	3. Use data structures generously. This shows you're comfortable with them and care about good OOP design
	4. When writing on whiteboard or paper, start from upper lefthand corner to not crowd your code
8. Optimize using [[BUD Optimization]]
	1. Look for ununsed info
	2. Solve manually on an example and reverse engineer how the algorithm works
	3. Solve incorrectly and think about why the algorithm fails
	4. Use a fresh and different example if stuck
	5. Make time vs space tradeoffs. [[Hash Structures]] are useful
	6. Pre-process information. Maybe sort the array first, etc.
	7. What can we store or [[Memoization|Memoize]] to save time?
	8. If array related, consider [[Sorting]]
	9. If search related, consider [[Binary Search]]
	10. If parsing [[Tree]] or [[Graphs]] or traversal or reversal consider a [[DSA/Data Structures/Stacks & Queues/Stack]]
	11. If dealing with lots of strings, try putting them in a [[Trie|Prefix Tree]]
	12. Does solving the problem for `n-1` help solve it for `n`? Consider [[Dynamic Programming]]
	13. Does the problem have optimal substructure? optimal solution to sub-problems helps get optimal solution to the problems? Consider [[Greedy Algorithm]]
	14. Do you have to take min/max of a dynamic collection? Consider [[Heap]]
9. Test the code
	1. Conceptually walk through the code like in a detailed code review
	2. Test happy paths
	3. Target unusual or non-standard code like loop indices that may be off by one
	4. Hot spots like arithmetic and null nodes, base cases in recursion, all integer divisions to see if they need rounding or could cause division exceptions, etc.
	5. Small test cases
	6. Special cases and edge cases
	7. Extreme cases: 0, negative, null, maximums, minimums
	8. User error: wrong input, wrong usage
10. When you find a bug, think first then act
	1. Don't willy nilly trying something. You don't want to be seen as a random fixer and it creates more issues down the line

# Solution Guidelines

## Clean Code

1. Correct: operate correctly on expected and unexpected inputs.
2. Efficient: operate as efficiently as possible in terms of time and space. This includes [[Big O]] and real-life efficiency. A constant factor might get dropped when you use [[Big O]] but in real life, it can matter. 
3. Simple: If you can do something in 10 lines instead of 100, you should.
4. Readable: A developer should be able to read your code and understand what it does and how it does it.
5. Maintainable: Adaptable to changes and easy to maintain by others.

It's advisable to sacrifice some degree of efficiency to make code more maintainable, and vice versa. Ask when in doubt.
## Good Code

1. Use Data Structures Generously
	1. Think of all the possible data structures you can use for the solution
	2. Sometimes it's better to create your custom data structure, like a struct or a class or an enum
2. Code Reuse
	1. Try to reuse the same modules or functions for similar or variants of the same problem
3. Modular
	1. Use modules or functions to separate code with different concerns and functions
4. Flexible and Robust
	2. Code should be flexible to different constraints. For example, don't assume board size in a board problem
	3. Use of generics or templates when appropriate
	4. Using constants instead of variables or vice versa when appropriate
5. Error Checking
	1. You can skip validations in an interview
		1. Ask the interviewer if we need to validate the input or can assume it's valid
		2. Tell the interviewer you would normally validate input and do it in x and y ways but for time you won't do it immediately but at the end after the problem is solved
	2. If statements
	3. Assert statements
## Speed and skipping

1. Skip initialization code and obvious boilerplate and replace it with an assumed function call that does the job for you
2. Use todos in place of things that aren't core to the algorithm like error checking, validation, and testing. Come back to them later if the interviewer likes

# Five Problem Approaches
## Exmaplify

1. Write out specific examples of a problem
2. See if you can derive a general rule from the examples
### Example

Given a time, calculate the angle between the hour and minute hands. 
#### Solution

1. Let's start with an example like 3:27.
2. We can draw a picture of a clock by selecting where the 3 hour hand is and where the 27 minute hand is. 
3. For the below solution, we'll assume that h is the hour and m is the minute. 
4. We'll also assume that the hour is specified as an integer between 0 and 23, inclusive.
5. By playing around with these examples, we can develop a rule: 
	1. Angle between the minute hand and 12 o'clock: 360 * m / 60 
	2. Angle between the hour hand and 12 o'clock: 360 * (h % 12) / 12 + 360 (m / 60) * (1 / 12) 
	3. Angle between hour and minute: (hour angle - minute angle) % 360 
	4. By simple arithmetic, this reduces to (30h - 5.5m) % 360.

## Pattern Matching

1. Think about what problems this problem is similar to
2. Write out the solution to the other problem
3. Modify it to develop the solution for this problem
### Example

A sorted array has been rotated so that the elements might appear in the order 3, 4, 5, 6, 7, 1, 2. How would you find the minimum element? 

You may assume that the array has all unique elements. 
#### Solution

There are two problems that jump to mind as similar: 
1. Find the minimum element in an array. 
	1. Finding the minimum element in an array isn't an interesting algorithm (iterate through all the elements), nor does it use the information provided (that the array is sorted). It's unlikely to be useful here. 
2. Find a particular element in a sorted array (i.e., binary search)
	2. Binary search is applicable. You know that the array is sorted, but rotated. So, it must proceed in an increasing order, then reset, and increase again. 
	3. The minimum element is the "reset" point. 
	4. If you compare the middle and last element (6 and 2), you will know the reset point must be between those values, since MID > RIGHT. 
	5. This wouldn't be possible unless the array "reset" between those values. 
	6. If MID < RIGHT, then either the reset point is on the left half, or there is no reset point (the array is really sorted).
	7. We can continue to apply this approach, dividing the array in half like binary search. We will eventually find the minimum element (or the reset point).
## Simplify and Generalize

1. Change a constraint such as the data type or amount of data. This helps simplify the problem. 
2. Solve the new simplified version of the problem. 
3. Once we have an algorithm for the simplified problem, we generalize the problem and try to adapt the solution for the more complex version
### Example

A ransom note can be formed by cutting words out of a magazine to form a new sentence. How would you figure out if a ransom note (represented as a string) can be formed from a given magazine (string)? 
#### Solution

1. Modify it so that we are cutting characters out of a magazine instead of whole words.
2. Solve the simplified problem with characters
	1. Creating an array 
	2. Counting the characters
	3. Each spot in the array corresponds to one letter
	4. Count the number of times each character in the ransom note appears 
	5. Go through the magazine to see if we have all of those characters. 
3. Generalize the algorithm 
	1. Rather than creating an array with character counts, we create a hash table that maps from a word to its frequency

## Base Case and Build

This often lead to natural recursive algorithms.

1. Solve the problem first for a base case (e.g., `n = 1`). This usually means recording the correct result. 
2. Try to solve the problem for `n = 2`, assuming that you have the answer for `n = 1`. 
3. Try to solve it for `n = 3`, assuming that you have the answer for `n = 1` and `n = 2`.
4. Build a solution that can always compute the result for `n` if we know the correct result for `n-1`. 
5. It may not be until `n` equals 3 or 4 that we get an instance interesting enough to try to build the solution based on the previous result.
### Example

Design an algorithm to print all permutations of a string. For simplicity, assume all characters are unique. Consider a test string "abcdefg"
#### Solution

1. Case "a" --> {"a"} 
2. Case "ab" --> {"ba"}
3. Case "abc" --> ? 
	1. This is the first "interesting" case. If we had the answer to P("ab"), how could we generate P("abc")? 
		1. Additional letter is "c," so we can just stick c in at every possible point. 
			1. P("abc") = insert "c" into all locations of all strings in P("ab") 
			2. P("abc") = insert "c" into all locations of all strings in {"ab", "ba"} 
			3. P("abc") = merge({"cab", "acb", "abc"}, {"cba", "bca", bac"}) 
			4. P("abc") = {"cab", "acb", "abc", "cba", "bca", bac"} 
4. Develop a general recursive algorithm. 
	1. Generate all permutations of a string s₁...s by "chopping off" the last character and generating all permutations of s₁...Sn-1
	2. Once we have the list of all permutations of S1..Sn-1 we iterate through this list
	3. For each string in it, we insert s, into every location of the string
## Data Structure Brainstorm

1. Run through a list of data structures and try to apply each one.
### Example

Numbers are randomly generated and stored into an (expanding) array. How would you keep track of the median? 
#### Solution

1. Linked list? Probably not. Linked lists tend not to do very well with accessing and sorting numbers. 
2. Array? Maybe, but you already have an array.
	1. Could you somehow keep the elements sorted? That's probably expensive. Let's hold off on this and return to it if it's needed.
3. Binary tree? This is possible, since binary trees do fairly well with ordering. 
	1. If the binary search tree is perfectly balanced, the top might be the median.
	2. If there's an even number of elements, the median is actually the average of the middle two elements. 
	3. The middle two elements can't both be at the top. 
	4. This is probably a workable algorithm, but let's come back to it. 
4. Heap? A heap is really good at basic ordering and keeping track of max and mins. 
	1. If you had two heaps, you could keep track of the bigger half and the smaller half of the elements. 
	2. The bigger half is kept in a min heap, such that the smallest element in the bigger half is at the root. 
	3. The smaller half is kept in a max heap, such that the biggest element of the smaller half is at the root. 
	4. You have the potential median elements at the roots. 
	5. If the heaps are no longer the same size, you can "rebalance"the heaps by popping an element off the one heap and pushing it onto the other. 
# Sources

## Exercise

- [HackerRank](https://www.hackerrank.com/)
- [LeetCode](https://leetcode.com/)
- [Aonecode](https://aonecode.com/)
- [Daily Coding Problem](https://www.dailycodingproblem.com/)
- [Neetcode](https://neetcode.io/)
- [GeeksForGeeks](https://www.geeksforgeeks.org/)
- [Hackerearth](https://www.hackerearth.com/)
## Answers

- [TutorialCup](https://www.tutorialcup.com/)
## Tutorials

- [Visualgo.net](https://visualgo.net/en)
## Coaching & Preparation

- [AlgoExpert](https://www.algoexpert.io)
- [InterviewGenie](https://interviewgenie.com/)
- [InterviewKickstart](https://www.interviewkickstart.com/)
- [AlgoMonster](https://algo.monster/)
- [Hellointerview](https://www.hellointerview.com/)
- [Interview Cake](https://www.interviewcake.com/)
- [InterviewBit](https://www.interviewbit.com/)
## Social

- [TeamBlind](https://www.teamblind.com/)