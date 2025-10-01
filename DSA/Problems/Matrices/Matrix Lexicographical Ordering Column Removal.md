---
Leetcode: https://leetcode.com/problems/delete-columns-to-make-sorted/description/
---
#DailyCodingProblem #Medium #Google 
# Problem

You are given an N by M 2D matrix of lowercase letters. Determine the minimum number of columns that can be removed to ensure that each row is ordered from top to bottom lexicographically.
That is, the letter at each column is lexicographically later as you go down each row. It does not matter whether each row itself is ordered lexicographically.

For example, given the following table:
```
cba
daf
ghi
```
This is not ordered because of the `a` in the center. We can remove the second column to make it ordered:
```
ca
df
gi
```
So your function should return 1, since we only needed to remove 1 column.

As another example, given the following table:
```
abcdef
```
Your function should return 0, since the rows are already ordered (there's only one row).

As another example, given the following table:
```
zyx
wvu
tsr
```
Your function should return 3, since we would need to remove all the columns to order it.
# Solution
## Brute Force 

1. Iterate over rows
	1. Iterate over columns
		1. if the char at `[i][j]` is < char at `[i-1][j]` then we remove this column
		2. continue with next row

```cpp
class Solution {
public:
	int minDeletionSize(vector<string>& strs) {	
		unordered_set<int> removals;
		
		for (int i = 1; i < strs.size(); i++) {
			for (int j = 0; j < strs[i].size(); j++) {
				if (removals.count(j)) continue;
				if (strs[i-1][j] > strs[i][j]) {
					removals.insert(j);
				}
			}
		}
		return removals.size();
	}
};
```

## Row-wise Traversal

```cpp
class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int deleteCount = 0;

        for (int col = 0; col < strs[0].size(); ++col) {
            for (int row = 0; row < strs.size() - 1; ++row) {
                if (strs[row][col] > strs[row + 1][col]) {
                    deleteCount++;
                    break;
                }
            }
        }
        return deleteCount;
    }
};
```