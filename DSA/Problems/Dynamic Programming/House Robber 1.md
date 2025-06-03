#DynamicProgramming 
# Problem

# Solution

## [[Dynamic Programming]] Top-Down ([[Memoization]]) - [[O(n)]] time - [[O(n)]] space

```cpp
#include<vector>

using namespace std;
class Solution {
public:
    unordered_map<int, int> memo;
    vectoc<int> nums;
    
    int dp(int i) {
        // Base cases
        if (i == 0) return nums[0];
        if (i == 1) return max(nums[0], nums[1]);
        if (!memo.contains(i)) {
            memo[i] = max(dp(i - 1), dp(i - 2) + nums[i]);
        }
        return memo[i];
    }
    
    int rob(vector<int> nums) {
        this->nums = nums;
        return dp(nums.size() - 1);
    }
}
```
## [[Dynamic Programming]] Bottom-up - [[O(n)]] time - [[O(n)]] space

```cpp
#include<vector>

using namespace std;
class Solution {
    public int rob(vector<int> nums) {
        if (nums.size() == 1) return nums[0];

        vector<int> dp;
        
        // Base cases
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        
        return dp[nums.size() - 1];
    }
}
```