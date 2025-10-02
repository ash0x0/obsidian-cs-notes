#Amazon #DailyCodingProblem #Hard 
# Problem

Given a string, find the longest palindromic contiguous substring. If there are more than one with the maximum length, return any one.

For example, the longest palindromic substring of "aabcdcb" is "bcdcb". The longest palindromic substring of "bananas" is "anana".
# Solution
# Solution

## Expand Around Center - [[O(n²)]] time - [[O(1)]] space

Try each position as center and expand outward.

```cpp
string longestPalindrome(string s) {
    if (s.empty()) return "";
    
    int start = 0, maxLen = 0;
    
    for (int i = 0; i < s.length(); i++) {
        // Odd length palindromes (single center)
        int len1 = expandAroundCenter(s, i, i);
        // Even length palindromes (two centers)
        int len2 = expandAroundCenter(s, i, i + 1);
        
        int len = max(len1, len2);
        
        if (len > maxLen) {
            maxLen = len;
            start = i - (len - 1) / 2;
        }
    }
    
    return s.substr(start, maxLen);
}

int expandAroundCenter(string s, int left, int right) {
    while (left >= 0 && right < s.length() && s[left] == s[right]) {
        left--;
        right++;
    }
    return right - left - 1;
}
```

# Algorithm

For each possible center:
1. Expand outward while characters match
2. Track the longest palindrome found

**Two cases**:
- Odd length: single center (e.g., "aba")
- Even length: two centers (e.g., "abba")

# Example: "babad"

```
Center at 'b' (index 0):
  Odd: "b" → length 1
  Even: "" → length 0

Center at 'a' (index 1):
  Odd: "bab" → length 3 ✓
  Even: "" → length 0

Center at 'b' (index 2):
  Odd: "aba" → length 3 ✓
  Even: "" → length 0

Center at 'a' (index 3):
  Odd: "bad" → "a" → length 1
  Even: "" → length 0

Center at 'd' (index 4):
  Odd: "d" → length 1
  Even: "" → length 0

Longest: "bab" or "aba" (both length 3)
```

# Dynamic Programming Approach - [[O(n²)]] time - [[O(n²)]] space

```cpp
string longestPalindrome(string s) {
    int n = s.length();
    if (n < 2) return s;
    
    vector<vector<bool>> dp(n, vector<bool>(n, false));
    int start = 0, maxLen = 1;
    
    // All single characters are palindromes
    for (int i = 0; i < n; i++) {
        dp[i][i] = true;
    }
    
    // Check for length 2
    for (int i = 0; i < n - 1; i++) {
        if (s[i] == s[i + 1]) {
            dp[i][i + 1] = true;
            start = i;
            maxLen = 2;
        }
    }
    
    // Check for lengths 3 to n
    for (int len = 3; len <= n; len++) {
        for (int i = 0; i < n - len + 1; i++) {
            int j = i + len - 1;
            
            if (s[i] == s[j] && dp[i + 1][j - 1]) {
                dp[i][j] = true;
                start = i;
                maxLen = len;
            }
        }
    }
    
    return s.substr(start, maxLen);
}
```

# Manacher's Algorithm - [[O(n)]] time - [[O(n)]] space

Most efficient but complex:

```cpp
string longestPalindrome(string s) {
    if (s.empty()) return "";
    
    // Transform string: "abc" → "#a#b#c#"
    string t = "#";
    for (char c : s) {
        t += c;
        t += '#';
    }
    
    int n = t.length();
    vector<int> p(n, 0);  // p[i] = radius of palindrome at i
    int center = 0, right = 0;
    int maxLen = 0, maxCenter = 0;
    
    for (int i = 0; i < n; i++) {
        // Mirror of i with respect to center
        int mirror = 2 * center - i;
        
        if (i < right) {
            p[i] = min(right - i, p[mirror]);
        }
        
        // Try to expand palindrome at i
        while (i + p[i] + 1 < n && i - p[i] - 1 >= 0 &&
               t[i + p[i] + 1] == t[i - p[i] - 1]) {
            p[i]++;
        }
        
        // Update center and right boundary
        if (i + p[i] > right) {
            center = i;
            right = i + p[i];
        }
        
        // Track maximum
        if (p[i] > maxLen) {
            maxLen = p[i];
            maxCenter = i;
        }
    }
    
    // Extract palindrome
    int start = (maxCenter - maxLen) / 2;
    return s.substr(start, maxLen);
}
```

# Comparison

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Expand Around Center | [[O(n²)]] | [[O(1)]] | Simple, space efficient |
| Dynamic Programming | [[O(n²)]] | [[O(n²)]] | Clear logic |
| Manacher's | [[O(n)]] | [[O(n)]] | Most efficient, complex |

# Related Problems

- LeetCode #5 - Longest Palindromic Substring
- [[Palindrome]]
- [[Two Pointer]]
- [[Dynamic Programming]]
