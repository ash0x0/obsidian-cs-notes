#Facebook  #DailyCodingProblem #Hard 
# Problem

Implement regular expression matching with the following special characters:

- `.` (period) which matches any single character
- `*` (asterisk) which matches zero or more of the preceding element

That is, implement a function that takes in a string and a valid regular expression and returns whether or not the string matches the regular expression.

For example, given the regular expression "ra." and the string "ray", your function should return true. The same regular expression on the string "raymond" should return false.

Given the regular expression ".*at" and the string "chat", your function should return true. The same regular expression on the string "chats" should return false.
# Solution

## Dynamic Programming - [[O(m*n)]] time - [[O(m*n)]] space

```cpp
bool isMatch(string s, string p) {
    int m = s.length();
    int n = p.length();
    
    // dp[i][j] = true if s[0..i-1] matches p[0..j-1]
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    
    // Empty string matches empty pattern
    dp[0][0] = true;
    
    // Handle patterns like a*, a*b*, a*b*c* that can match empty string
    for (int j = 2; j <= n; j++) {
        if (p[j-1] == '*') {
            dp[0][j] = dp[0][j-2];
        }
    }
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (p[j-1] == '.' || p[j-1] == s[i-1]) {
                // Characters match
                dp[i][j] = dp[i-1][j-1];
            } else if (p[j-1] == '*') {
                // * can match zero or more of preceding element
                // Case 1: * matches zero occurrences
                dp[i][j] = dp[i][j-2];
                
                // Case 2: * matches one or more occurrences
                if (p[j-2] == '.' || p[j-2] == s[i-1]) {
                    dp[i][j] = dp[i][j] || dp[i-1][j];
                }
            }
        }
    }
    
    return dp[m][n];
}
```

# Algorithm

The key insight is that `*` modifies the preceding character:
- `a*` can match "", "a", "aa", "aaa", ...
- `.*` can match any sequence

**Cases**:
1. **Current chars match** (or `.`): `dp[i][j] = dp[i-1][j-1]`
2. **Pattern has `*`**:
   - Match zero: `dp[i][j] = dp[i][j-2]` (skip `x*`)
   - Match one or more: if chars match, `dp[i][j] = dp[i-1][j]`

# Examples

```
s = "aa", p = "a"  → false
s = "aa", p = "a*" → true (a* matches "aa")
s = "ab", p = ".*" → true (.* matches anything)
s = "aab", p = "c*a*b" → true (c* matches "", a* matches "aa", b matches "b")
```

# Time Complexity

- **Time**: [[O(m*n)]] where m = length of s, n = length of p
- **Space**: [[O(m*n)]] for the DP table

# Space Optimization

Can reduce to [[O(n)]] by using rolling array:

```cpp
bool isMatch(string s, string p) {
    int m = s.length();
    int n = p.length();
    
    vector<bool> prev(n + 1, false);
    vector<bool> curr(n + 1, false);
    
    prev[0] = true;
    
    for (int j = 2; j <= n; j++) {
        if (p[j-1] == '*') {
            prev[j] = prev[j-2];
        }
    }
    
    for (int i = 1; i <= m; i++) {
        curr[0] = false;
        for (int j = 1; j <= n; j++) {
            if (p[j-1] == '.' || p[j-1] == s[i-1]) {
                curr[j] = prev[j-1];
            } else if (p[j-1] == '*') {
                curr[j] = curr[j-2];
                if (p[j-2] == '.' || p[j-2] == s[i-1]) {
                    curr[j] = curr[j] || prev[j];
                }
            } else {
                curr[j] = false;
            }
        }
        swap(prev, curr);
    }
    
    return prev[n];
}
```

# Related Problems

- LeetCode #10 - Regular Expression Matching
- [[Dynamic Programming]]
- Wildcard Matching (LeetCode #44)
