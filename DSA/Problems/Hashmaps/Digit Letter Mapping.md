#DailyCodingProblem #Easy #Yelp 
# Problem

Given a mapping of digits to letters (as in a phone number), and a digit string, return all possible letters the number could represent. You can assume each valid number in the mapping is a single digit.

For example if {“2”: [“a”, “b”, “c”], 3: [“d”, “e”, “f”], …} then “23” should return [“ad”, “ae”, “af”, “bd”, “be”, “bf”, “cd”, “ce”, “cf"].
# Solution

## Backtracking - [[O(4ⁿ)]] time where n = number of digits

Generate all combinations using backtracking.

```cpp
vector<string> letterCombinations(string digits) {
    if (digits.empty()) return {};
    
    vector<string> result;
    string current = "";
    
    // Phone keyboard mapping
    unordered_map<char, string> phone = {
        {'2', "abc"}, {'3', "def"}, {'4', "ghi"},
        {'5', "jkl"}, {'6', "mno"}, {'7', "pqrs"},
        {'8', "tuv"}, {'9', "wxyz"}
    };
    
    backtrack(digits, 0, current, phone, result);
    return result;
}

void backtrack(string& digits, int index, string& current,
               unordered_map<char, string>& phone,
               vector<string>& result) {
    if (index == digits.length()) {
        result.push_back(current);
        return;
    }
    
    string letters = phone[digits[index]];
    for (char letter : letters) {
        current.push_back(letter);
        backtrack(digits, index + 1, current, phone, result);
        current.pop_back();
    }
}
```

# Algorithm

1. For each digit, get its corresponding letters
2. Try each letter and recursively process remaining digits
3. When all digits processed, add combination to result
4. Backtrack to try other combinations

# Example: "23"

```
Digit '2' → letters "abc"
  Try 'a':
    Digit '3' → letters "def"
      Try 'd': current = "ad" ✓
      Try 'e': current = "ae" ✓
      Try 'f': current = "af" ✓
  Try 'b':
    Digit '3' → letters "def"
      Try 'd': current = "bd" ✓
      Try 'e': current = "be" ✓
      Try 'f': current = "bf" ✓
  Try 'c':
    Digit '3' → letters "def"
      Try 'd': current = "cd" ✓
      Try 'e': current = "ce" ✓
      Try 'f': current = "cf" ✓

Result: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

# Iterative Approach

```cpp
vector<string> letterCombinations(string digits) {
    if (digits.empty()) return {};
    
    unordered_map<char, string> phone = {
        {'2', "abc"}, {'3', "def"}, {'4', "ghi"},
        {'5', "jkl"}, {'6', "mno"}, {'7', "pqrs"},
        {'8', "tuv"}, {'9', "wxyz"}
    };
    
    vector<string> result = {""};
    
    for (char digit : digits) {
        vector<string> temp;
        string letters = phone[digit];
        
        for (string& combination : result) {
            for (char letter : letters) {
                temp.push_back(combination + letter);
            }
        }
        
        result = temp;
    }
    
    return result;
}
```

# Time Complexity

[[O(4ⁿ × n)]] where n = length of digits
- Most digits map to 3 letters, some to 4
- Worst case: all map to 4 letters
- Each combination has length n

# Space Complexity

[[O(4ⁿ × n)]] for storing all combinations

# Related Problems

- LeetCode #17 - Letter Combinations of a Phone Number
- [[Backtracking]]
- [[Recursion]]
- Permutations and combinations
