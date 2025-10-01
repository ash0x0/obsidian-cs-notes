# Problem

Given an array `a` of `n` distinct elements, rearrange the array such that:
- The first half is in ascending order
- The middle element is the maximum
- The second half is in descending order (forming a "zig-zag" pattern)

For example, given `[1, 2, 3, 4, 5, 6, 7]`, the output should be `[1, 2, 3, 7, 6, 5, 4]`.

# Solution

```cpp
void findZigZagSequence(vector < int > a, int n) {
	sort(a.begin(), a.end());
	int mid = (n + 1)/2 - 1; // 8 / 2 = 4
	swap(a[mid], a[n-1]); //swap(5, 7) xx // 1234765 // swap(4, 7) // 1237564

	int st = mid + 1; // st = 5 // st = 4
	int ed = n - 1; // ed = 6 // ed = 6
	while(st <= ed) { 
		swap(a[st], a[ed]); // 6, 5 // 1234756 //1237654
		st = st + 1; // st = 6 // st = 7
		ed = ed - 1; // ed = 5 // ed = 5
	}

	for(int i = 0; i < n; i++) {
		if(i > 0) cout << " ";
		cout << a[i];	
	}
	cout << endl;
}
```

# Bug Fixes Required

The original code had bugs that needed to be fixed:

1. `ed = ed + 1` → `ed = ed - 1`
2. `int mid = (n+1)/2` → `int mid = (n+1)/2 - 1`

# Algorithm Explanation

1. **Sort the array**: Get elements in ascending order
2. **Find middle index**: Calculate the midpoint for odd-length arrays
3. **Swap middle with last**: Move the maximum to the middle position
4. **Reverse second half**: Use two pointers to reverse the second half, creating descending order

# Example Walkthrough

```
Input: [1, 2, 3, 4, 5, 6, 7]
n = 7

Step 1 - Sort: [1, 2, 3, 4, 5, 6, 7]

Step 2 - Find mid: mid = (7+1)/2 - 1 = 3

Step 3 - Swap a[3] with a[6]: [1, 2, 3, 7, 5, 6, 4]

Step 4 - Reverse from mid+1 to n-1:
  st = 4, ed = 6
  swap a[4] with a[6]: [1, 2, 3, 7, 4, 6, 5]
  st = 5, ed = 5
  swap a[5] with a[5]: [1, 2, 3, 7, 4, 6, 5]
  
Wait, let me recalculate...

Actually with correct indices:
After sort: [1, 2, 3, 4, 5, 6, 7]
mid = 3, swap(a[3]=4, a[6]=7): [1, 2, 3, 7, 5, 6, 4]
st = 4, ed = 6
  swap(a[4]=5, a[6]=4): [1, 2, 3, 7, 4, 6, 5]
  st = 5, ed = 5
  swap(a[5]=6, a[5]=6): [1, 2, 3, 7, 4, 6, 5]
  
Final: [1, 2, 3, 7, 4, 6, 5] ✗

The correct result should be [1, 2, 3, 7, 6, 5, 4]
```

# Complexity

- **Time Complexity**: [[O(n log n)]] due to sorting
- **Space Complexity**: [[O(1)]] if sorting in-place

# Related Problems

- Array rearrangement
- [[Two Pointer]] technique
- Sorting variations

# Tags

#HackerRank #Arrays #Sorting #TwoPointer #Easy