#Microsoft #DailyCodingProblem 
# Problem

Compute the running median of a sequence of numbers. That is, given a stream of numbers, print out the median of the list so far on each new element.

Recall that the median of an even-numbered list is the average of the two middle numbers.

For example, given the sequence [2, 1, 5, 7, 2, 0, 5], your algorithm should print out:
```
2
1.5
2
3.5
2
2
2
```
# Solution
## Brute Force

1. create a vector
2. for each new number, push to vector
3. keep vector sorted
4. get the median of the vector

## [[Two Heaps]] - [[O(n)]] time - [[O(n)]] space

```cpp
#include<queue>

class Solution {
	priority_queue<int> maxHeap;
	priority_queue<int, <greater>> minHeap;
	
	int streamMedian(int number) {
		if (maxHeap.empty() || number <= maxHeap.top()) maxHeap.push(number);
		else minHeap.push(number);

		if (maxHeap.size() > minHeap.size() + 1) {
			minHeap.push
		}
	}
}
```