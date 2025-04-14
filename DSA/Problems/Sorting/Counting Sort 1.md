---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-countingsort1/
---
# Solution

## Iterative - [[O(n)]] time - [[O(n)]] space

```cpp
vector<int> countingSort(vector<int> arr) {
	vector<int> result = vector<int>(100, 0);

	for (int i = 0; i < arr.size(); i++) {
		result[arr[i]]++;
	}

	return result;
}
```