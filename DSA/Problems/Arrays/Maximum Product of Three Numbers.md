---
Leetcode: https://leetcode.com/problems/maximum-product-of-three-numbers/description/
---
#DailyCodingProblem #Facebook #Easy 

# Problem
## DailyCodingProblem

Given a list of integers, return the largest product that can be made by multiplying any three integers.

For example, if the list is `[-10, -10, 5, 2]`, we should return `500`, since that's `-10 * -10 * 5`.

You can assume the list has at least three integers.
## LeetCode

Given an integer array `nums`, _find three numbers whose product is maximum and return the maximum product_.

**Example 1:**
**Input:** nums = [1,2,3]
**Output:** 6

**Example 2:**
**Input:** nums = [1,2,3,4]
**Output:** 24

**Example 3:**
**Input:** nums = [-1,-2,-3]
**Output:** -6

**Constraints:**
- `3 <= nums.length <= 104`
- `-1000 <= nums[i] <= 1000`

# Solution

## Brute Force [[O(n^3)]] time - [[O(1)]] space

```cpp
int maxProductBruteForce(vector<int>& nums) {
    int n = nums.size(), maxProd = INT_MIN;
    for (int i = 0; i < n; ++i)
        for (int j = i+1; j < n; ++j)
            for (int k = j+1; k < n; ++k)
                maxProd = max(maxProd, nums[i]*nums[j]*nums[k]);
    return maxProd;
}
```

Test cases
1. `[-10, -10, 5, 2]` → `500`
2. `[1, 2, 3]` → `6`
3. `[-5, -4, -3, -2, -1]` → `-6`
4. `[0, -1, 3, 100, -70, -50]` → `350000`
5. `[1, 10, -5, 1, -100]` → `5000`

We can note the answer from the examples as either one of
1. The three largest integers in the list
2. The two smallest integers (both negative) and the largest one

Knowing this we can solve the problem by finding both answers and returning the maximum of the two.

## [[Sorting]] - [[O(n log n)]] time - [[O(1)]] space

```cpp
int maximumProductSort(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    int n = nums.size();
    return max(nums[n - 1] * nums[n - 2] * nums[n - 3],
               nums[0] * nums[1] * nums[n - 1]);
}
```

## [[Greedy Algorithm]] - [[O(n)]] time - [[O(1)]] space

We need to propagate the values down the line to each other to ensure that we don't overwrite previous maximums sacrificing the correct order. Same thing for minimums.

We need to make it INT_MIN and INT_MAX in case we have an all negatives array or all positives. Can't be 0 as the initial value.

```cpp
#include <climits>
#include <algorithm>
using namespace std;

int maximumProduct(vector<int>& nums) {
    int max1 = INT_MIN, max2 = INT_MIN, max3 = INT_MIN;
    int min1 = INT_MAX, min2 = INT_MAX;

    for (int n : nums) {
        // Update top 3 max values
        if (n > max1) {
            max3 = max2;
            max2 = max1;
            max1 = n;
        } else if (n > max2) {
            max3 = max2;
            max2 = n;
        } else if (n > max3) {
            max3 = n;
        }

        // Update bottom 2 min values
        if (n < min1) {
            min2 = min1;
            min1 = n;
        } else if (n < min2) {
            min2 = n;
        }
    }

    return max(max1 * max2 * max3, 
				max1 * min1 * min2);
}
```