#Google #DailyCodingProblem #Sorting #Hard
# Problem

Given an array of strictly the characters 'R', 'G', and 'B', segregate the values of the array so that all the Rs come first, the Gs come second, and the Bs come last. 

You can only swap elements of the array.

Do this in linear time and in-place.

For example, given the array ['G', 'B', 'R', 'R', 'B', 'R', 'G'], it should become ['R', 'R', 'R', 'G', 'G', 'B', 'B'].
# Solution
## [[Two Pointer]] - [[O(n)]] time - [[O(1)]] space

1. Iterate over the array
	1. Move all the R's to beginning
2. Iterate over the array again, starting after R's
	1. Move all the G's to beginning

Time O(n+n-x) -> O(n)

```cpp
class Solution {
	void rgbSegragation(vector<char>& list) {
		int left = 0, right = 1;
		while(right < list.size()) {
			if (list[right] == 'R') {
				list[right] = list[left];
				list[left] = 'R';
				left++;
			}
			right++;
		}
		right = left + 1;
		while (right < list.size()) {
			if (list[left] == 'B' && list[right] == 'G') {
				list[left] = 'G';
				list[right] = 'B';
				left++;
			}
			right++;
		}
		return;
	}
}
```
## [[Counting Sort]] - [[O(n)]] time - [[O(1)]] space

```cpp
class Solution {
	void rgbSegragation(vector<char>& list) {
		int g = 0, r = 0, b = 0;
		for (char c : list) {
			if (c == 'R') r++;
			else if (c == 'G') g++;
			else if (c == 'B') b++;
		}
		for (int i = 0; i < list.size(); i++) {
			if (r > 0) { list[i] = 'R'; r--; }
			else if (g > 0) { list[i] = 'G'; g--; }
			else if (b > 0) { list[i] = 'B'; b--; }
		}
	}
}
```

More concise version

```cpp
class Solution {
	void rgbSegragation(vector<char>& list) {
		int countR = 0, countG = 0, countB = 0;
		for (char c : a) {
			if (c == 'R') ++countR;
			else if (c == 'G') ++countG;
			else ++countB;
		}
		int i = 0;
		while (countR--) a[i++] = 'R';
		while (countG--) a[i++] = 'G';
		while (countB--) a[i++] = 'B';
	}
}
```
## [[Dutch National Flag]] - [[O(n)]] time - [[O(1)]] space

```cpp
class Solution {
	void rgbSegragation(vector<char>& list) {
		int low = 0, mid = 0, high = (int)a.size() - 1;
	    while (mid <= high) {
	        if (a[mid] == 'R') {
	            std::swap(a[low++], a[mid++]);
	        }
	        else if (a[mid] == 'G') {
	            ++mid;
	        }
	        else {  // a[mid] == 'B'
	            std::swap(a[mid], a[high--]);
	        }
	    }
	}
}
```