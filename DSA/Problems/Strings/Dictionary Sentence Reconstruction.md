#Microsoft #DailyCodingProblem #Medium 
# Problem

Given a dictionary of words and a string made up of those words (no spaces), return the original sentence in a list. If there is more than one possible reconstruction, return any of them. If there is no possible reconstruction, then return null.

For example, given the set of words 'quick', 'brown', 'the', 'fox', and the string "thequickbrownfox", you should return ['the', 'quick', 'brown', 'fox'].

Given the set of words 'bed', 'bath', 'bedbath', 'and', 'beyond', and the string "bedbathandbeyond", return either ['bed', 'bath', 'and', 'beyond] or ['bedbath', 'and', 'beyond'].
# Solution

## [[Sliding Window]] [[Greedy Algorithm]] - O($n^2$) time - O(n) space

2. Iterate over the string with sliding window, starting with size 1
3. For each iteration, check if the current word in the window is in the dictionary
	1. If so, add it to word list, then reset the window to start again at the next letter
	2. If not, grow the window by 1. Fix the left side and increment right

```cpp
class Solution {
	vector<string> sentenceReconstruction(vector<string> dictionary, string sentence) {
		if (dictionary.size() == 0) return {};
		if (sentence.length() == 0) return {};
		int start = 0;
		vector<string> result;
		for (int i = 1; i <= sentence.length(); i++) {
			string currentWord = sentence.substr(start, i - start);
			if (find(dictionary.begin(), dictionary.end(), currentWord)) {
				result.push_back(currentWord);
				start = i;
			}
		}
		return result;
	}
}
```

This solution is incomplete because it doesn't backtrack over the word to check other possible constructions. The greedy logic can fail on certain dictionaries where an earlier (shorter) match prevents you from finding a segmentation of the remainder.
That's why a correct solution needs to be DP, because earlier matches affect subsequent matches.
## 
