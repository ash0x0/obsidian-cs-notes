---
sources:
  - https://www.dailycodingproblem.com/
---
#DynamicProgramming

# Problem

A builder is looking to build a row of `n` houses that can be of `k` different colors. He has a goal of minimizing cost while ensuring that no two neighboring houses are of the same color.

Given an `n` by `k` matrix where the `n`th row and `k`th column represents the cost to build the `n`th house with `k`th color, return the minimum cost which achieves this goal.

# Solution

Examplify the problem to see a pattern

1. **Empty input**
	- `n = 0, k = any` → cost = 0 (no houses to paint)

2. **Single house**
	- `n = 1, k = 3`, e.g. `[[5,2,7]]` 
	- answer = 2 (pick the cheapest color)

3. **Multiple houses, one color**
	- `n = 3, k = 1`, e.g. `[[4],[3],[5]]` 
	- Impossible to avoid same‐color neighbors 
	- Invalid set

4. **Small example**

```
n = 3, k = 3
costs = [
  [1, 5, 3],
  [2, 9, 4],
  [3, 6, 1]
]
```

- house0 -> color0 (1)
- house1 -> color2 (4)
- house2 -> color0 or2 ? best is color2 (1)
- total = 1+4+1 = 6

5. **Larger random example** 
	- Verify correctness on random small arrays by brute force.
## Brute-Force (Exhaustive Search) - [[O(k^n)]] time - [[O(n)]] space

**Idea.** Try every possible sequence of colors (kⁿ possibilities), skip sequences where adjacent colors match, track minimum cost.

**Time Complexity:** O(kⁿ · n)  
**Space Complexity:** O(n) recursion depth

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, k;
vector<vector<int>> cost;
int best = INT_MAX;

// Recursive helper: paint house `i`, with last color `lastColor`
void dfs(int i, int lastColor, int accCost) {
    if (i == n) {
        best = min(best, accCost);
        return;
    }
    for (int c = 0; c < k; ++c) {
        if (c == lastColor) continue;
        int newCost = accCost + cost[i][c];
        if (newCost >= best) continue;  // prune
        dfs(i+1, c, newCost);
    }
}

int minCostBrute() {
    if (n == 0) return 0;
    best = INT_MAX;
    dfs(0, -1, 0);
    return best;
}

int main() {
    // Example usage
    cost = {{1,5,3},{2,9,4},{3,6,1}};
    n = cost.size(); k = cost[0].size();
    cout << minCostBrute() << "\n";  // expect 6
    return 0;
}
```

---

## 2. Classic DP (O(n·k²))

**Idea.** Let `dp[i][c]` = minimum cost to paint up to house i with color c at i.  
Transition:

```
dp[i][c] = cost[i][c] + min_{c'≠c}( dp[i-1][c'] )
```

Compute row by row in O(k) per color by scanning previous row.

**Time Complexity:** O(n·k²)  
**Space Complexity:** O(n·k), can reduce to O(k) rolling array

```cpp
#include <bits/stdc++.h>
using namespace std;

int minCostDP(const vector<vector<int>>& cost) {
    int n = cost.size();
    if (n == 0) return 0;
    int k = cost[0].size();
    if (k == 0) return 0;
    
    vector<int> prev(cost[0]);  // dp for house 0
    vector<int> curr(k);
    
    for (int i = 1; i < n; ++i) {
        for (int c = 0; c < k; ++c) {
            // find min over all other colors
            int best = INT_MAX;
            for (int pc = 0; pc < k; ++pc) {
                if (pc == c) continue;
                best = min(best, prev[pc]);
            }
            curr[c] = cost[i][c] + best;
        }
        swap(prev, curr);
    }
    return *min_element(prev.begin(), prev.end());
}

int main() {
    vector<vector<int>> cost = {{1,5,3},{2,9,4},{3,6,1}};
    cout << minCostDP(cost) << "\n";  // 6
    return 0;
}
```

---

## 3. Optimized DP with Two Minimums (O(n·k))

**Idea.** For each row `i−1`, track:

- `min1`, the smallest `dp[i−1][c]`, and its color `idx1`.
    
- `min2`, the second smallest value.
    

Then for each color `c` at house `i`:

- If `c != idx1`, use `min1`; else use `min2`.
    
- `dp[i][c] = cost[i][c] + (c!=idx1 ? min1 : min2)`.
    

This avoids the inner k-scan, yielding O(k) per row.

**Time Complexity:** O(n·k)  
**Space Complexity:** O(k)

```cpp
#include <bits/stdc++.h>
using namespace std;

int minCostOptimized(const vector<vector<int>>& cost) {
    int n = cost.size();
    if (n == 0) return 0;
    int k = cost[0].size();
    if (k == 0) return 0;
    
    // dp for previous row
    vector<int> dp(cost[0]);
    
    for (int i = 1; i < n; ++i) {
        // find smallest and second smallest in dp
        int min1 = INT_MAX, min2 = INT_MAX, idx1 = -1;
        for (int c = 0; c < k; ++c) {
            if (dp[c] < min1) {
                min2 = min1;
                min1 = dp[c];
                idx1 = c;
            } else if (dp[c] < min2) {
                min2 = dp[c];
            }
        }
        vector<int> newDp(k);
        for (int c = 0; c < k; ++c) {
            int bestPrev = (c == idx1 ? min2 : min1);
            newDp[c] = cost[i][c] + bestPrev;
        }
        dp.swap(newDp);
    }
    return *min_element(dp.begin(), dp.end());
}

int main() {
    vector<vector<int>> cost = {{1,5,3},{2,9,4},{3,6,1}};
    cout << minCostOptimized(cost) << "\n";  // 6
    return 0;
}
```

---

### Summary of Complexities

|Approach|Time|Space|
|---|---|---|
|Brute Force|O(kⁿ · n)|O(n)|
|DP with O(k²) Transition|O(n·k²)|O(k) or O(n·k)|
|Optimized DP using two minimums|O(n·k)|O(k)|

The **optimized DP** is the best choice for large n and k, running in **O(n·k)** time and **O(k)** extra space.
