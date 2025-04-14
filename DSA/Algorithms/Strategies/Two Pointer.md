A method commonly used with arrays or data structures to get better than [[O(n²)]] time complexity. Usually this approach is of [[O(n)]] time complexity.

# When to use

1. Whenever you want to compare two elements within a data structure
2. Whenever you're thinking of a nested loop solution
3. Whenever you want to move an element from one position to another in an array

# Methodology

1. A single loop with two pointers within the loop

```cpp
int j = 0;
for (int i = 1; i < nums.size(); i++) {
	if (nums[i] != nums[j]) {
		...
	}
}
```

# Forms

## Slow & Fast

1. One pointer starts from the first element of the array, this is the slow pointer
2. The other pointer starts from the second element, this is the fast pointer
3. The fast pointer looks ahead in the array to find the next element we want to place in the place of the slow pointer or compare with the element at the slow pointer
### Example Usage

1. Moving elements ahead in an array, like [[26. Remove Duplicates from Sorted Array|26. Remove Duplicates from Sorted Array]]

## Front & Back

1. One pointer starts from the front of the array
2. The other pointer starts from the back of the array
3. Usually stopping the loop before both pointers equal one another
	1. `while(left < right)`
	2. `while(right > left)`
### Example Usage

1. Palindromes, like [[9. Palindrome Number]]