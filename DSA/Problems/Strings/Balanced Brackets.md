---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-balanced-brackets/problem
---
# Problem

A bracket is considered to be any one of the following characters: `(`, `)`, `{`, `}`, `[`, or `]`.

Two brackets are considered to be a _matched pair_ if the an opening bracket (i.e., `(`, `[`, or `{`) occurs to the left of a closing bracket (i.e., `)`, `]`, or `}`) _of the exact same type_. There are three types of matched pairs of brackets: `[]`, `{}`, and `()`.

A matching pair of brackets is _not balanced_ if the set of brackets it encloses are not matched. For example, `{[(])}` is not balanced because the contents in between `{` and `}` are not balanced. The pair of square brackets encloses a single, unbalanced opening bracket, `(`, and the pair of parentheses encloses a single, unbalanced closing square bracket, `]`.

By this logic, we say a sequence of brackets is _balanced_ if the following conditions are met:

- It contains no unmatched brackets.
- The subset of brackets enclosed within the confines of a matched pair of brackets is also a matched pair of brackets.

Given  strings of brackets, determine whether each sequence of brackets is balanced. If a string is balanced, return `YES`. Otherwise, return `NO`.

**Function Description**

Complete the function _isBalanced_ in the editor below.

isBalanced has the following parameter(s):

- _string s_: a string of brackets

**Returns**

- _string:_ either `YES` or `NO`

**Input Format**

The first line contains a single integer , the number of strings.  
Each of the next  lines contains a single string , a sequence of brackets.

**Constraints**

- , where  is the length of the sequence.
- All chracters in the sequences ∈ { **{**, **}**, **(**, **)**, **[**, **]** }.

**Output Format**

For each string, return `YES` or `NO`.

**Sample Input**

STDIN Function ----- -------- 3 n = 3 {[()]} first s = '{[()]}' {[(])} second s = '{[(])}' {{[[(())]]}} third s ='{{[[(())]]}}'

**Sample Output**

```
YES
NO
YES
```

**Explanation**

1. The string `{[()]}` meets both criteria for being a balanced string.
2. The string `{[(])}` is not balanced because the brackets enclosed by the matched pair `{` and `}` are not balanced: `[(])`.
3. The string `{{[[(())]]}}` meets both criteria for being a balanced string.
# Solution
## [[DSA/Data Structures/Stacks & Queues/Stack]] - [[O(n)]] time - [[O(n)]] space

Technically `O(s)` time and `O(s)` space because `s` is the variable that refers to string length, while `n` in the problem refers to the number of strings

```cpp
string isBalanced(string s) {
	stack<char> brackets;
	for (char c : s) {
		if (c == '{' || c == '(' || c == '[') {
			brackets.push(c);
			continue;
		}
		if (brackets.empty()) return "NO";
		char last = brackets.top();
		if ((last == '{' && c == '}')
			|| (last == '(' && c == ')')		
			|| (last == '[' && c == ']')) {
			brackets.pop();
		} else return "NO";
	}
	return brackets.empty() ? "YES" : "NO";
}
```

Could also use a dictionary for matching for better readability

```cpp
string isBalanced(string s) {
	stack<char> brackets;
	unordered_map<char, char> match = {{')', '('}, {']', '['}, {'}', '{'}};
	for (char c : s) {
		if (c == '{' || c == '(' || c == '[') {
			brackets.push(c);
		}
		else if (brackets.empty() || match[c] != brackets.top()) {
			return "NO";
		} else brackets.pop();
	}
	return brackets.empty() ? "YES" : "NO";
}
```
