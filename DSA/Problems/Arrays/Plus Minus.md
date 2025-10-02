---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-plus-minus/
---

# Solution

```cpp
#include<iostream>

using namespace std;

void plusMinus(vector<int> arr) {
    float positives = 0, negatives = 0, zeroes = 0;
    int n = arr.size();

    for (int i = 0; i < n; i++) {
        zeroes += (arr[i] == 0);
        positives += (arr[i] > 0);
        negatives += (arr[i] < 0);
    }
    
    zeroes = zeroes / (double)n;
    positives = positives / (double)n;
    negatives = negatives / (double)n;
    
    printf("%0.06lf\n%0.06lf\n%0.06lf\n", positives, negatives, zeroes);
}
```