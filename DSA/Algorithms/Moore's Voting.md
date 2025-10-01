Moore's Voting Algorithm (also known as Boyer-Moore Majority Vote Algorithm) is an efficient algorithm to find the **majority element** in an array. A majority element appears more than ⌊n/2⌋ times in an array of size n.

# Problem Statement

Given an array of elements, find the element that appears more than n/2 times, where n is the size of the array. If no such element exists, return -1 or null.

# Algorithm

The algorithm works in two phases:

## Phase 1: Find Candidate

1. Initialize a `candidate` variable and a `count` variable to 0
2. For each element in the array:
   - If `count` is 0, set `candidate` to the current element
   - If current element equals `candidate`, increment `count`
   - Otherwise, decrement `count`

## Phase 2: Verify Candidate

3. Count the occurrences of the `candidate` in the array
4. If count > n/2, return `candidate`; otherwise, no majority element exists

# Implementation

```cpp
int findMajorityElement(vector<int>& nums) {
    // Phase 1: Find candidate
    int candidate = -1;
    int count = 0;
    
    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        count += (num == candidate) ? 1 : -1;
    }
    
    // Phase 2: Verify candidate
    count = 0;
    for (int num : nums) {
        if (num == candidate) {
            count++;
        }
    }
    
    if (count > nums.size() / 2) {
        return candidate;
    }
    
    return -1; // No majority element
}
```

# Why It Works

The key insight is that if an element appears more than n/2 times, it will survive the cancellation process in Phase 1. When we pair each majority element with a different element, the majority element will still have unpaired instances left.

# Example

```
Array: [3, 3, 4, 2, 4, 4, 2, 4, 4]
Step-by-step:
- candidate = 3, count = 1
- candidate = 3, count = 2
- candidate = 3, count = 1
- candidate = 3, count = 0
- candidate = 4, count = 1
- candidate = 4, count = 2
- candidate = 4, count = 1
- candidate = 4, count = 2
- candidate = 4, count = 3

Verification: 4 appears 5 times > 9/2 = 4.5
Result: 4 is the majority element
```

# Complexity

- **Time Complexity**: [[O(n)]] - two passes through the array
- **Space Complexity**: [[O(1)]] - constant extra space

# Extensions

## Finding Elements Appearing More Than n/3 Times

For finding up to 2 elements that appear more than n/3 times, use two candidates:

```cpp
vector<int> majorityElementII(vector<int>& nums) {
    int candidate1 = 0, candidate2 = 0;
    int count1 = 0, count2 = 0;
    
    // Phase 1: Find candidates
    for (int num : nums) {
        if (num == candidate1) {
            count1++;
        } else if (num == candidate2) {
            count2++;
        } else if (count1 == 0) {
            candidate1 = num;
            count1 = 1;
        } else if (count2 == 0) {
            candidate2 = num;
            count2 = 1;
        } else {
            count1--;
            count2--;
        }
    }
    
    // Phase 2: Verify candidates
    count1 = 0;
    count2 = 0;
    for (int num : nums) {
        if (num == candidate1) count1++;
        else if (num == candidate2) count2++;
    }
    
    vector<int> result;
    int n = nums.size();
    if (count1 > n / 3) result.push_back(candidate1);
    if (count2 > n / 3) result.push_back(candidate2);
    
    return result;
}
```

# Applications

- Finding most frequent element in voting systems
- Stream processing where we need to find dominant elements
- Data analysis and statistics
- Network traffic analysis (finding most frequent IP addresses)

# Related Problems

- [[Majority Element]] - LeetCode #169
- [[Majority Element II]] - LeetCode #229
- Finding k most frequent elements
