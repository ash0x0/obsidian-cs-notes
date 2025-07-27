---
sources:
  - https://www.jointaro.com/course/crash-course-data-structures-and-algorithms-concepts/intersection-approach/
---
# Problem

> Write a function, _intersection_, that takes in two lists, _a_,_b_, as arguments. The function should return a new list containing elements that are in both of the two lists.
> 
> You may assume that each input list does not contain duplicate elements.

```
intersection([4,2,1,6], [3,6,9,2,10]) # -> [2,6]
```

```
intersection([2,4,6], [4,2]) # -> [2,4]
```

```
intersection([4,2,1], [1,2,4,6]) # -> [1,2,4]
```

# Solution
## [[Hash Set]] - [[O(n)]] time - [[O(n)]] space

```cpp
vector<int> intersection(vecton<int> a, vector<int> b) {
	unordered_set<int> firstSet(a.begin(), a.end());
	vector<int> result;
	
	for (int i = 0; i < b.size(); i++) {
		if (firstSet.count(b[i])) result.push_back(b[i]);
	}
	return result;
}
```