---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-mini-max-sum
---
#TODO 
# Solution

## [[Idiomatic C++]] - [[O(n)]] time - [[O(1)]] space 

```cpp
#include<algorithm>
#include<iostream;

using namespace std;

void miniMaxSum(vector<int> arr) {
	sort(arr.begin(), arr.end());
	
	long long int total = accumulate(arr.begin(), arr.end(), 0LL);
	long long int maxSum = total - arr.front();
	long long int minSum = total - arr.back();
	
	printf("%lli %lli", minSum, maxSum);
}
```