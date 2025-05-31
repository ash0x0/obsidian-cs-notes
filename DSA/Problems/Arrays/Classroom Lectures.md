# Problem

Given an array of time intervals (start, end) for classroom lectures (possibly overlapping), find the minimum number of rooms required.

For example, given [(30, 75), (0, 50), (60, 150)], you should return 2.

# Solution

## Examplify

1. No lectures
	[]
	ans -> 0
2. One lecture
	[(30, 75)]
	ans -> 1
3. Two non-overlapping lectures, far apart
	[(30, 75), (200, 250)]
	ans -> 1
4. Two consecutive lectures
	[(30, 75), (75, 130)]
	ans -> 1
5. Two overlapping lectures
	[(30, 75), (50, 100)]
	ans -> 2
6. Many pair-overlapping lectures (no more than two lectures overlap at same time)
	[(30, 75), (50, 100), (80, 130), (110, 160)]
	ans -> 2
7. Many overlapping lectures (more than two lectures overlap at same time)
	[(30, 75), (0, 60), (50, 100), (50, 110), (80, 130), (120, 180), (110, 160)]
	ans -> 4
# Timeline - O(n) time - O(1) space

```cpp
class Solution {
	int minimumClassrooms(vector<vector<int>> times) {
		unordered_map<int, int> timesMap;
		unordered_set
		for (int i = 0; i < times.size(); i++) {
			unordered_map.insert({times[i][0], times[i][1]});
		}
	}
}
```