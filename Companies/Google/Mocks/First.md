A conveyor belt has packages that must be shipped from one port to another within `days` days.

The `ith` package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

# Examples

**Example 1:**
**Input:** weights = [1,2,3,4,5,6,7,8,9,10], days = 5
**Output:** 15
**Explanation:** A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.

**Example 2:**
**Input:** weights = [3,2,2,4,1,4], days = 3
**Output:** 6
**Explanation:** A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4

**Example 3:**
**Input:** weights = [1,2,3,1,1], days = 4
**Output:** 3
**Explanation:**
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1

**Constraints:**
- `1 <= days <= weights.length <= 5 * 104`
- `1 <= weights[i] <= 500`

# Info
- the goal is to make sure the `weights` array would be shipped within `days`
- load the ship in the same order in weights array
- ship load < max capacity

# Requirement
- determine ship capacity such that all packages in `weights` are shipped within `days`

# Algorithm
**Input:** weights = [1,2,3,4,5,6,7,8,9,10], days = 5
**Output:** 15

**Input:** weights = [3,2,2,4,1,4], days = 3
**Output:** 6
**Input:** weights = [1,2,3,1,1], days = 4
**Output:** 3

max(ceil(sumOfWeights / days), max(weights)) = 11;

1. assume capacities from 1 to 500 - 
	1. iterate over the array - for loop - O(n)
		1. if subarraySum + weights[i] >= capacity
			1. create a new chunk
			2. subarraySum = 0
		2. if we reach the end of weights and # chunks <= days then we're successful
		3. break if the weight[i] > capacity assumption
		4. break if # chunks > days
		5. subarraySum += weight[i]

```cpp
#include<cmath>

using namespace std;

class Solution {
public:
	int getMinCapacity(vector<int>& weights, int days) {
		int sumOfWeights = 0;
		for (int x : weights) sumOfWeights += x; 
		int maxOfWeights = max_element(weights.begin(), weights.end());
		int left = maxOfWeights, right = sumOfWeights;
		
		while (capacity <= sumOfWeights) {
			capacity = (left + right) / 2;
			int subarraySum = 0;
			int loads = 0;
			for (int i = 0; i < weights.length(); i++) {
				if (weights[i] > capacity) break;
				if (loads > days) break;
				if (subarraySum + weights[i] >= capacity) {
					loads++;
					subarraySum = 0;
				}
				subarraySum += weights[i];
			}
			if (loads < days) left = capacity;
			if (loads > days) right = capacity;
			if (loads = days) return capacity;
		}
		return capacity;
	}
}
```

O(sumOfWeights * n) = O(n)

maxOfWeights ..............................................capacity .............................................. sumOfWeight

1. modularize the code into functions
2. 
