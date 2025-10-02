#DailyCodingProblem 
# Problem

Using a function `rand5()` that returns an integer from 1 to 5 (inclusive) with uniform probability, implement a function `rand7()` that returns an integer from 1 to 7 (inclusive).
# Solution
# Solution

## Using Rejection Sampling - Expected [[O(1)]]

Given `rand5()` that generates 1-5, implement `rand7()` that generates 1-7 uniformly.

```cpp
// Given function
int rand5() {
    // Returns 1, 2, 3, 4, or 5 with equal probability
}

int rand7() {
    int num;
    
    do {
        // Generate number from 1 to 25
        // Row: (rand5() - 1) * 5 gives 0, 5, 10, 15, 20
        // Col: rand5() gives 1, 2, 3, 4, 5
        num = (rand5() - 1) * 5 + rand5();
        
        // Keep only 1-21 (divisible by 7)
        // Reject 22-25
    } while (num > 21);
    
    // Map 1-21 to 1-7
    return (num - 1) % 7 + 1;
}
```

# Algorithm Explanation

1. **Generate larger range**: 
   - `(rand5() - 1) * 5 + rand5()` generates 1-25 uniformly
   - Creates 5×5 grid of possibilities

2. **Rejection sampling**:
   - Keep only 1-21 (3 complete sets of 7)
   - Reject 22-25 to maintain uniformity

3. **Map to target range**:
   - `(num - 1) % 7 + 1` maps 1-21 → 1-7

# Visualization

```
rand5() × rand5() generates:
 1  2  3  4  5
 6  7  8  9 10
11 12 13 14 15
16 17 18 19 20
21 22 23 24 25

Keep: 1-21 (3 complete cycles of 7)
Reject: 22-25

Map:
1→1, 2→2, 3→3, 4→4, 5→5, 6→6, 7→7
8→1, 9→2, 10→3, 11→4, 12→5, 13→6, 14→7
15→1, 16→2, 17→3, 18→4, 19→5, 20→6, 21→7
```

# Expected Calls to rand5()

- Probability of accepting = 21/25 = 0.84
- Expected iterations = 1/0.84 ≈ 1.19
- Each iteration uses 2 calls to rand5()
- Expected total calls ≈ 2.38

# Time Complexity

Expected [[O(1)]], worst case unbounded (but probability decreases exponentially)

# General Pattern: Implement randN() using randM()

```cpp
int randN(int N, int M, function<int()> randM) {
    // Generate enough range: M^k >= N
    int range = M;
    int multiplier = M;
    while (range < N) {
        range *= M;
        multiplier *= M;
    }
    
    int num;
    do {
        num = 0;
        int temp_mult = 1;
        for (int i = 0; i < log(range)/log(M); i++) {
            num += (randM() - 1) * temp_mult;
            temp_mult *= M;
        }
        num++; // Convert to 1-based
    } while (num > (range / N) * N);
    
    return (num - 1) % N + 1;
}
```

# Related Problems

- LeetCode #470 - Implement Rand10() Using Rand7()
- [[Rejection Sampling]]
- [[Probability]]
- Random number generation
