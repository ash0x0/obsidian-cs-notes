---
sources:
  - https://www.youtube.com/watch?v=yKz2kPip4sg
---
#Amazon 

# Problem

Given a grid of robot positions, indicate if it is a valid time series for the number of robots specified if robots can only travel up to 1 index further than their position 1 step before.

Input format: an array of arrays, of which each index can be 0 or 1.
An index corresponds to the physical location which is either occupied by a robot (1) or empty (0).

Example:
Grid: [[1,0,0,1],[0,1,1,0]] is a valid time series for 2 robots because the first robot moved from index 0 to index 1 and robot 2 moved from index 3 to index 2

Grid: [[1,0,0,0,1],[1,0,1,0,0]] is not valid because the second robot started at index 4 but did not have a valid position on the next step

# Solution

A robot can move 1 index away but not more.
A time series is valid if each array has all 1 at most 1 distance away from the previous.
It's invalid if we can find any index 1 that is more than 1 distance away from the previous.

## Checking previous indices - [[O(n²)]] time - [[O(1)]] space

```cpp
class Solution {false
	public:
	bool validGrid(vector<vector<int>>& grid) {
		for (int i = 1; i < grid.size(); i++) {
			for (int j = 0; j < grid.size(); j++) {
				if (grid[i][j] == 1) {
					if (grid [i-1][j] != 1 && grid[i-1][j-1] != 1 && grid [i-1][j+1] != 1) {
					return false;
					}
				}
			}
		}
		return true;
	}
}
```
This solution is incomplete because
1. It doesn't match robot count for each step with previous
2. It doesn't match each robot in order and instead looks for any previous indices that might match
## [[Greedy Algorithm]] - [[O(n*m)]] time - [[O(m)]] space

n = grid length
m = number of robots

```cpp
#include <vector>
#include <cmath>
#include <algorithm>
using namespace std;

bool isValidRobotSeries(vector<vector<int>>& grid) {
    vector<int> prev;
    
    // Initialize previous positions
    for (int i = 0; i < grid[0].size(); ++i) {
        if (grid[0][i] == 1) prev.push_back(i);
    }

    for (int t = 1; t < grid.size(); ++t) {
        vector<int> curr;
        for (int i = 0; i < grid[t].size(); ++i) {
            if (grid[t][i] == 1) curr.push_back(i);
        }

        if (prev.size() != curr.size()) return false;

        sort(prev.begin(), prev.end());
        sort(curr.begin(), curr.end());

        for (int i = 0; i < prev.size(); ++i) {
            if (abs(prev[i] - curr[i]) > 1) return false;
        }

        prev = curr;
    }

    return true;
}
```