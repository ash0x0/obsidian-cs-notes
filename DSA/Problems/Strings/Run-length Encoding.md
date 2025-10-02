#Amazon #DailyCodingProblem #Easy 
# Problem

Run-length encoding is a fast and simple method of encoding strings. The basic idea is to represent repeated successive characters as a single count and character. For example, the string "AAAABBBCCDAA" would be encoded as "4A3B2C1D2A".

Implement run-length encoding and decoding. You can assume the string to be encoded have no digits and consists solely of alphabetic characters. You can assume the string to be decoded is valid.
# Solution

## Encoding - [[O(n)]] time - [[O(n)]] space

```cpp
string encode(string s) {
    if (s.empty()) return "";
    
    string result = "";
    int count = 1;
    
    for (int i = 1; i < s.length(); i++) {
        if (s[i] == s[i-1]) {
            count++;
        } else {
            result += to_string(count) + s[i-1];
            count = 1;
        }
    }
    
    // Add the last group
    result += to_string(count) + s[s.length()-1];
    
    return result;
}
```

## Decoding - [[O(n)]] time - [[O(n)]] space

```cpp
string decode(string s) {
    string result = "";
    int i = 0;
    
    while (i < s.length()) {
        // Extract the count (may be multiple digits)
        int count = 0;
        while (i < s.length() && isdigit(s[i])) {
            count = count * 10 + (s[i] - '0');
            i++;
        }
        
        // Extract the character
        char ch = s[i];
        i++;
        
        // Append character count times
        result.append(count, ch);
    }
    
    return result;
}
```

# Examples

```
"AAAABBBCCDAA" → "4A3B2C1D2A"
"WWWWWWWWWWWWBWWWWWWWWWWWWBBBWWWWWWWWWWWWWWWWWWWWWWWWB" → "12W1B12W3B24W1B"
"" → ""
"A" → "1A"
```

# Edge Cases

- Empty string
- Single character
- All same characters
- No consecutive duplicates

# Optimization

For very long runs, consider limiting run length and splitting:

```cpp
string encodeWithLimit(string s, int maxRun = 999) {
    string result = "";
    int i = 0;
    
    while (i < s.length()) {
        char current = s[i];
        int count = 0;
        
        while (i < s.length() && s[i] == current && count < maxRun) {
            count++;
            i++;
        }
        
        result += to_string(count) + current;
    }
    
    return result;
}
```

# Related Problems

- String compression
- [[Two Pointer]]
- LeetCode #443 - String Compression
