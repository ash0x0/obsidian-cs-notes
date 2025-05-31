---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-diagonal-difference
---
# Solution

## Iterative Sum - [[O(n*m)]] time - [[O(1)]] space

```cpp
int diagonalDifference(vector<vector<int>> arr) {
	int primarySum = 0, secondarySum = 0;
	for (int i = 0; i < arr.size(); i++) {
		for (int j = 0; j < arr[i].size(); j++) {
			if (i == j) {
				primarySum += arr[i][j];
			}
			if (j + i == arr[i].size() - 1) {
				secondarySum += arr[i][j];
			}
		}
	}
	return abs(primarySum - secondarySum);
}
```

## Improved Iterative Sum - [[O(n)]] time - [[O(1)]] space

1. Since we only use `arr[i][j]` when `i = j` we can just use `i` for both indices, giving us `arr[i][i]`
2. Since the equation for the secondary sum `arr[i][j]` such that `j + i = arr[i].size() - 1` we can get it in terms of `j` as `j = arr[i].size() - 1 - i` so we can use an equation in terms of `i` in place of `j`
3. This means we don't need `j` for either of the sums

```cpp
int diagonalDifference(vector<vector<int>> arr) {
	int primarySum = 0, secondarySum = 0;
	for (int i = 0; i < arr.size(); i++) {
		primarySum += arr[i][i];
		secondarySum += arr[i][arr[i].size() - 1 - i];
	}
	return abs(primarySum - secondarySum);
}
```