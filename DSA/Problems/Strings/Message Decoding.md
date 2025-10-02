#DailyCodingProblem #Facebook #Medium 
# Problem

Given the mapping a = 1, b = 2, ... z = 26, and an encoded message, count the number of ways it can be decoded.

For example, the message '111' would give 3, since it could be decoded as 'aaa', 'ka', and 'ak'.

You can assume that the messages are decodable. For example, '001' is not allowed.
# Solution

## Dynamic Programming - [[O(n)]] time - [[O(n)]] space

Count ways to decode by considering single digit and two-digit decodings.

```cpp
int numDecodings(string s) {
    if (s.empty() || s[0] == '0') return 0;
    
    int n = s.length();
    vector<int> dp(n + 1, 0);
    
    // Base cases
    dp[0] = 1;  // Empty string has 1 way
    dp[1] = 1;  // First character (already checked it's not '0')
    
    for (int i = 2; i <= n; i++) {
        // Single digit decode
        int oneDigit = stoi(s.substr(i-1, 1));
        if (oneDigit >= 1 && oneDigit <= 9) {
            dp[i] += dp[i-1];
        }
        
        // Two digit decode
        int twoDigits = stoi(s.substr(i-2, 2));
        if (twoDigits >= 10 && twoDigits <= 26) {
            dp[i] += dp[i-2];
        }
    }
    
    return dp[n];
}
```

# Algorithm

`dp[i]` = number of ways to decode string up to index i

For each position:
1. Try decoding single digit (1-9)
2. Try decoding two digits (10-26)
3. Sum the ways from both options

# Example: "111"

```
Index:  0   1   2   3
String:     1   1   1
dp:     1   1   ?   ?

Position 2 (second '1'):
- Single: "1" → valid (1-9) → add dp[1] = 1
- Double: "11" → valid (10-26) → add dp[0] = 1
- dp[2] = 1 + 1 = 2

Position 3 (third '1'):
- Single: "1" → valid → add dp[2] = 2
- Double: "11" → valid → add dp[1] = 1
- dp[3] = 2 + 1 = 3

Decodings:
1. "1" "1" "1" → aaa
2. "11" "1" → ka
3. "1" "11" → ak

Result: 3 ways
```

# Example: "226"

```
dp[0] = 1
dp[1] = 1 (single '2')

dp[2]:
- Single '2' → valid → dp[1] = 1
- Double '22' → valid → dp[0] = 1
- dp[2] = 2

dp[3]:
- Single '6' → valid → dp[2] = 2
- Double '26' → valid → dp[1] = 1
- dp[3] = 3

Decodings:
1. "2" "2" "6" → bbf
2. "22" "6" → vf
3. "2" "26" → bz

Result: 3 ways
```

# Space Optimization

Only need last two values:

```cpp
int numDecodings(string s) {
    if (s.empty() || s[0] == '0') return 0;
    
    int n = s.length();
    int prev2 = 1;  // dp[i-2]
    int prev1 = 1;  // dp[i-1]
    
    for (int i = 2; i <= n; i++) {
        int current = 0;
        
        int oneDigit = stoi(s.substr(i-1, 1));
        if (oneDigit >= 1 && oneDigit <= 9) {
            current += prev1;
        }
        
        int twoDigits = stoi(s.substr(i-2, 2));
        if (twoDigits >= 10 && twoDigits <= 26) {
            current += prev2;
        }
        
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}
```

**Space**: [[O(1)]]

# Edge Cases

```cpp
"0" → 0 (can't decode)
"10" → 1 (only "j")
"01" → 0 (leading zero invalid)
"27" → 1 (can't decode 27, only "2" "7")
"12" → 2 ("ab" or "l")
```

# Time Complexity

[[O(n)]] - single pass through string

# Space Complexity

[[O(n)]] or [[O(1)]] with optimization

# Related Problems

- LeetCode #91 - Decode Ways
- [[Dynamic Programming]]
- Climbing stairs
- Fibonacci sequence
