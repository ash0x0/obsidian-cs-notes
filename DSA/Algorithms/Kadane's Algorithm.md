Kadane's Algorithm is used to find the **maximum sum contiguous subarray** within a one-dimensional array of numbers. It's an optimal solution with [[O(n)]] time complexity.

# Problem Statement

Given an array of integers (which may include negative numbers), find the contiguous subarray with the largest sum.

# Algorithm

The key insight is to maintain a running sum and reset it to 0 when it becomes negative, as a negative sum would only decrease future sums.

1. Initialize `maxSum` to the first element (or negative infinity)
2. Initialize `currentSum` to 0
3. For each element in the array:
   - Add the element to `currentSum`
   - Update `maxSum` if `currentSum` is greater
   - If `currentSum` becomes negative, reset it to 0

# Implementation

```cpp
int maxSubArraySum(vector<int>& nums) {
    int maxSum = INT_MIN;
    int currentSum = 0;
    
    for (int num : nums) {
        currentSum += num;
        maxSum = max(maxSum, currentSum);
        
        if (currentSum < 0) {
            currentSum = 0;
        }
    }
    
    return maxSum;
}
```

# Variant: Track Indices

To also return the starting and ending indices of the maximum subarray:

```cpp
struct SubarrayResult {
    int maxSum;
    int start;
    int end;
};

SubarrayResult maxSubArrayWithIndices(vector<int>& nums) {
    int maxSum = INT_MIN;
    int currentSum = 0;
    int start = 0, end = 0, tempStart = 0;
    
    for (int i = 0; i < nums.size(); i++) {
        currentSum += nums[i];
        
        if (currentSum > maxSum) {
            maxSum = currentSum;
            start = tempStart;
            end = i;
        }
        
        if (currentSum < 0) {
            currentSum = 0;
            tempStart = i + 1;
        }
    }
    
    return {maxSum, start, end};
}
```

# Example

```
Array: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Maximum subarray: [4, -1, 2, 1]
Maximum sum: 6
```

# Complexity

- **Time Complexity**: [[O(n)]] - single pass through the array
- **Space Complexity**: [[O(1)]] - constant extra space

# Applications

- Finding the best time to buy and sell stocks
- Image processing (finding the brightest region)
- Genomics (finding interesting segments in DNA sequences)
- Data analysis (identifying trends or patterns)

# Related Problems

- [[Maximum Product Subarray]]
- Maximum sum circular subarray
- Maximum sum of non-adjacent elements
