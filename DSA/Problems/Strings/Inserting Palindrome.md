#Quora #DailyCodingProblem #Medium 
# Problem

Given a string, find the palindrome that can be made by inserting the fewest number of characters as possible anywhere in the word. 
If there is more than one palindrome of minimum length that can be made, return the lexicographically earliest one (the first one alphabetically).

For example, given the string "race", you should return "ecarace", since we can add three letters to it (which is the smallest amount to make a palindrome). 
There are seven other palindromes that can be made from "race" by adding three letters, but "ecarace" comes first alphabetically.

As another example, given the string "google", you should return "elgoogle".
# Solution
# Solution

## Dynamic Programming - [[O(n²)]] time - [[O(n²)]] space

Find minimum insertions needed to make palindrome, then construct it.

```cpp
string shortestPalindrome(string s) {
    int n = s.length();
    
    // dp[i][j] = min insertions to make s[i..j] palindrome
    vector<vector<int>> dp(n, vector<int>(n, 0));
    
    // Fill DP table
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            
            if (s[i] == s[j]) {
                dp[i][j] = dp[i + 1][j - 1];
            } else {
                dp[i][j] = 1 + min(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    // Reconstruct palindrome
    return constructPalindrome(s, 0, n - 1, dp);
}

string constructPalindrome(string& s, int i, int j, vector<vector<int>>& dp) {
    if (i > j) return "";
    if (i == j) return string(1, s[i]);
    
    if (s[i] == s[j]) {
        return s[i] + constructPalindrome(s, i + 1, j - 1, dp) + s[j];
    }
    
    if (dp[i + 1][j] < dp[i][j - 1]) {
        return s[i] + constructPalindrome(s, i + 1, j, dp) + s[i];
    } else {
        return s[j] + constructPalindrome(s, i, j - 1, dp) + s[j];
    }
}
```

# Algorithm

1. **Calculate minimum insertions** needed using DP:
   - If characters match: no insertion needed
   - If not: insert one character (min of two options)

2. **Reconstruct palindrome** by backtracking through DP table:
   - If characters match: include both
   - Otherwise: insert the character that gives minimum insertions

# Example: "race"

```
DP table for "race":

    r  a  c  e
r   0  1  2  3
a      0  1  2
c         0  1
e            0

dp[0][3] = 3 (need 3 insertions)

Reconstruction:
- s[0]='r', s[3]='e' → not equal
- min(dp[1][3], dp[0][2]) → min(2, 2)
- Choose left: insert 'e' at start

Continue recursively...

Result: "ecarace"
```

# Example: "google"

```
Minimum insertions = 2

Process:
"google"
- 'g' != 'e' → need to insert
- Choose to insert 'e' at start → "egoogle"
- Continue: need 'l' at start → "elgoogle"

Result: "elgoogle"
```

# Alternative: Reverse and Find LCS

```cpp
string shortestPalindrome(string s) {
    string rev = s;
    reverse(rev.begin(), rev.end());
    
    // Find longest common subsequence
    int lcs = longestCommonSubsequence(s, rev);
    
    // Insertions needed = n - lcs
    int insertions = s.length() - lcs;
    
    // Build palindrome
    return buildPalindrome(s, rev, lcs);
}
```

# For Lexicographically Earliest

When multiple palindromes have same min length, choose lexicographically smallest by:
1. At each decision point, try both options
2. Choose the one that gives smaller result alphabetically

```cpp
string makePalindrome(string s) {
    int n = s.length();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    
    // Calculate DP table (same as before)
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s[i] == s[j]) {
                dp[i][j] = dp[i + 1][j - 1];
            } else {
                dp[i][j] = 1 + min(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    // Reconstruct with lexicographic ordering
    return constructLexSmallest(s, 0, n - 1, dp);
}

string constructLexSmallest(string& s, int i, int j, vector<vector<int>>& dp) {
    if (i > j) return "";
    if (i == j) return string(1, s[i]);
    
    if (s[i] == s[j]) {
        return s[i] + constructLexSmallest(s, i + 1, j - 1, dp) + s[j];
    }
    
    // Try both options and choose lexicographically smallest
    if (dp[i + 1][j] <= dp[i][j - 1]) {
        string option1 = s[i] + constructLexSmallest(s, i + 1, j, dp) + s[i];
        
        if (dp[i + 1][j] == dp[i][j - 1]) {
            string option2 = s[j] + constructLexSmallest(s, i, j - 1, dp) + s[j];
            return min(option1, option2);
        }
        return option1;
    }
    return s[j] + constructLexSmallest(s, i, j - 1, dp) + s[j];
}
```

# Time Complexity

[[O(n²)]] for DP table + reconstruction

# Space Complexity

[[O(n²)]] for DP table

# Related Problems

- [[Dynamic Programming]]
- [[Palindrome]]
- Shortest palindrome (LeetCode #214)
- Palindrome partitioning
