# Behavioral

 1. We all kind of have things about ourselves we'd like to improve on at work. Can you give me an example of something that you have worked on to improve your overall work effectiveness?
	 1. What exactly were the skills and things you worked on?
	 2. How did you identify the resources to build those snapshots/tree?
	 3. What was the overall impact of this shift in mindset?
 2. Could you talk about a time that you maybe worked on a project or a task where you had minimal information or minimal direction?
# Coding
## Problem

You are given a list of words. Some of these are compound words, where all parts are also in the list.

Example input:
["rockstar", "rock", "star", "rocks", "tar", "rockstars", "super", "superhighway", "highway", "high", "way", "superhighway"]

Identify all combinations of words in the list which form a compound word also in the list.
Return the set of unique combinations.

Example Compound: "highway" = ("high + "way")
## Discussion

find unique combinations
    valid combination: two or more words that make up a word also in the list
    combination is two words only

ex1
["rockstar", "rock", "star", "rocks", "tar", "rockstars"]
["rock", "star", "rocks", "tar"]
[["rock", "star"], ["rocks", "tar"]]

ex2
["super", "superhighway", "highway", "high", "way", "superhighway"]
[["super", "highway"]]

ex3
["rockstar", "rock", "star", "rocks", "tar", "rockstars", "super", "superhighway", "highway", "high", "way", "superhighway"]
[["rock", "star"], ["rocks", "tar"], ["super", "highway"], ["high", "way"]]

1 word can have more than 1 combination
it could have no combinations
ignore duplicate words
ignore duplicate combinations

edge case:
1. empty word list
2. no combinations
3. all words are combinations for one word

### Interviewer Points

watch out for duplicate back to front and front to back
input: ["toptop", "top", "top"]
## Code

```cpp
unordered_set<pair<string, string>> findUniqueCombications(vector<string>& words) {
    if (words.size() < 1) return {};
    
    unordered_set<string> wordSet(words.begin(), words.end());
    unordered_set<pair<string, string>> result;
    
    for (int i = 0; i < words.size() - 1; i++) {
        // we need to update this to search the entire list to pair with the word we select
        // nested loop
        // outer loop selects a word
        // inner loop select every other word in the list
        // we check the combination every time
        // O(n^2)
        
        // another approach
        // prefix search
        // select one word
        // find if there's another word in the list that has this word as prefix
            // if so, find if the suffix exists as well
            // if exists, then valid result
        string forwardCombination = words[i] + words[i+1];
        string backwardCombination = words[i] + words[i+1];
        
        if (wordSet.count(forwardCombination)) result.insert(pair<string, string>(words[i], words[i+1]));
        if (wordSet.count(backwardCombination)) result.insert(pair<string, string>(words[i+1], words[i]));
    }
    
    return result;
}
```
## Follow-up

What happens in the case of ["super", "superhighway", "highway"]

