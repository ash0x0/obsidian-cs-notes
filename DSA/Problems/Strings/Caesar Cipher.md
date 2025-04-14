---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-caesar-cipher-1
---
# Solution

## Iterative - [[O(n)]] time - [[O(1)]] space

Notes:
1. We do `k = k % 26` first because any conversions over the number of alphas would just turn things around needlessly so this short-circuits the if conditions if `k > 26`
2. We use `(int)'A'` and others instead of ascii characters for readability, and because nobody remembers ascii codes.
3. We add 1 to first line because we want inclusive count and not just difference, which doesn't count 'A' in the range
4. We remove 1 from the ascii modding because we actually want to add whatever the value prior to `A` or `a` is so that we can include the first letter of the alphabet

```cpp
string caesarCipher(string s, int k) {
	k = k % ((int)'Z' - (int)'A' + 1);
	for (int i = 0; i < s.length(); i++) {
		if (!isalpha(s[i])) continue;
		int ascii = (int)s[i] + k;
		if (isupper(s[i]) && ascii > (int)'Z') 
			ascii = ascii % (int)'Z' + (int)'A' - 1;
		if (islower(s[i]) && ascii > (int)'z') 
			ascii = ascii % (int)'z' + (int)'a' - 1;
		s[i] = (char)ascii;
	}
	return s;
}
```