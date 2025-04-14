---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-lonely-integer/
---

# Solution

## [[Bitwise XOR]] - [[O(n)]] time - [[O(1)]] space

```cpp
int lonelyinteger(vector<int> a) {
	int xorSum = 0;
	for (int i = 0; i < a.size(); i++) {
		xorSum ^= a[i];
	}
	return xorSum;
}
```
