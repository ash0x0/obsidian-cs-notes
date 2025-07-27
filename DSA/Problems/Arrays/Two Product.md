---
sources:
  - https://www.jointaro.com/course/crash-course-data-structures-and-algorithms-concepts/pair-product-approach/
---
# Problem

> Write a function, _pair_product_, that takes in a list and a target product as arguments. The function should return a tuple containing a pair of indices whose elements multiply to the given target. The indices returned must be unique.
> 
> Be sure to return the indices, not the elements themselves.
> 
> There is guaranteed to be one such pair whose product is the target.

```
pair_product([3, 2, 5, 4, 1], 8) # -> (1, 3)
```

```
pair_product([3, 2, 5, 4, 1], 10) # -> (1, 2)
```

```
pair_product([4, 7, 9, 2, 5, 1], 5) # -> (4, 5)
```
# Solution

The same as [[1. Two Sum]] but for product instead of sum

```cpp
vector<int> pair_product(vector<int>& nums, int target) {
	unordered_map<int, int> complements;
	for (int i = 0; i < nums.size(); i++) {
		if (complements.count(target / nums[i])) {
			return {i, complements[nums[i]]};
		}
		complements[nums[i]] = i;
	}
	return {};
}
```