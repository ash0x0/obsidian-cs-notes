---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-grid-challenge
---
# Solution

## Sort then compare - [[O(n²)]] time - [[O(1)]] space

```cpp
  

string gridChallenge(vector<string> grid) {
	for (int i = 0; i < grid.size(); i++) {
		sort(grid[i].begin(), grid[i].end());
	}
	
	for (int i = 0; i < grid.size(); i++) {
		for (int j = 1; j < grid[i].length(); j++) {
			if ((int) grid[j-1][i] > (int) grid[j][i])
				return "NO";
			}
		}
	return "YES";
}
```