Two pointers iterate through the data structure in tandem until one or both of the pointers hit a certain condition.

Two pointers are needed because with just pointer, you would have to continually loop back through the array to find the answer, giving [[O(n²)]] time.

Usually this approach is of [[O(n)]] time complexity.

# When to use

1. Whenever you want to compare two elements within a data structure
2. Whenever you're thinking of a nested loop solution
3. Whenever you want to move an element from one position to another in an array
4. Searching pairs in a sorted array or linked list
5. Optimizing comparisons in a data structure to get better than [[O(n²)]] time
6.  When dealing with sorted arrays (or Linked Lists) and need to find a set of elements that fulfill certain constraints
7. The set of elements in the array is a pair, a triplet, or a subarray

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

![[Pasted image 20250509225504.png]]
# Forms

## Slow & Fast (Hare & Tortoise)

1. One pointer starts from the first element of the array, this is the slow pointer
2. The other pointer starts from the second element, this is the fast pointer
3. The fast pointer is incremented to look ahead in the array to find the next element we want use to either
	1. Compare with the element at the slow pointer
	2. Replace the element at the slow pointer
### Examples

1. Dealing with a loop in a linked list or array
2. Need to know the position of a certain element or the overall length of a linked list.
3. Moving elements ahead in an array
4. Trying to determine if a linked list is a palindrome
5. Dealing with cyclic linked lists or arrays. The fast pointer should catch the slow pointer once both the pointers are in a cyclic loop.

![[Pasted image 20250509225824.png]]

## Front & Back

1. One pointer starts from the front of the array
2. The other pointer starts from the back of the array
3. Usually stopping the loop before both pointers equal one another
	1. `while(left < right)`
	2. `while(right > left)`
### Examples

1. Palindromes, like [[9. Palindrome Number]]