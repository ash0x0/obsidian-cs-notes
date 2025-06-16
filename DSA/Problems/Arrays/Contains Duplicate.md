---
Leetcode: https://leetcode.com/problems/contains-duplicate
---
# Problem

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**
**Input:** nums = [1,2,3,1]
**Output:** true
**Explanation:**
The element 1 occurs at the indices 0 and 3.

**Example 2:**
**Input:** nums = [1,2,3,4]
**Output:** false
**Explanation:**
All elements are distinct.

**Example 3:**
**Input:** nums = [1,1,1,3,3,4,3,2,4,2]
**Output:** true

**Constraints:**
- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

# Solution
## Sort then check - [[O(n log n)]] time - [[O(1)]] space

```cpp
using namespace std;

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
		sort(begin(nums), end(nums));
		for (int i = 1; i < nums.size(); i++) {
		    if (nums[i] == nums[i-1]) return true;
        }
        return false;
    }
};
```

## [[Hash Set]] - [[O(n)]] time - [[O(n)]] space

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> seen;
        for (int num : nums) {
            if (seen.count(num) > 0)
                return true;
            seen.insert(num);
        }
        return false;
    }
};
```
## [[Hash Table|Hashmap]] -  [[O(n)]] time - [[O(n)]] space

```cpp
using namespace std;

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
		unordered_map<int, int> counter;
        for (int i = 0; i < nums.size(); i++) {
            if(counter[nums[i]] != 1) counter[nums[i]] = 1;
            else return true;
        }
        return false;
    }
};
```

Improved version

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_map<int, int> counter;
        for (int num : nums) {
            if (counter[num] >= 1) return true;
            counter[num]++;
        }
        return false;
    }
};
```