#Sorting

This is a single-pass, in-place partitioning of an array into three groups, like the Dutch flag has three horizontal bands of colors (red, white, blue).

It’s a special case of multi-way partitioning, and forms the basis of efficient three-way [[Quick Sort]] and other algorithms where you need to group items by one of three categories.
# Problem

You have an array `A[0…n−1]` whose elements each belong to one of three categories (often represented as 0, 1, 2 or ‘R’, ‘W’, ‘B’). Rearrange the array **in-place** so that all 0s come first, then all 1s, then all 2s. You may only swap elements; you want a single linear-time pass.
# Solution

1. Maintain three regions in the array:
	- `[0 … low−1]` = “all 0s”
	- `[low … mid−1]` = “all 1s”
	- `[mid … high]` = “unknown”
	- `[high+1 … n−1]` = “all 2s”
2. initialize `low = 0, mid = 0, high = n−1`
3. Loop while `mid ≤ high` and inspect `A[mid]`:
	- `if (A[mid] == 0)`
		- `swap(A[low], A[mid]);`
		- `low++; mid++;`
	- `else if A[mid] == 1`
		- `mid++;`
	- `else (A[mid] == 2)`
		- `swap(A[mid], A[high]);`
		- `high––;`
		- do _not_ increment mid here, since the swapped-in element at mid is unexamined

Before each iteration
- Everything left of `low` is 0
- Between `low` and `mid−1` is 1
- Between `mid` and `high` is unknown
- Right of `high` is 2

Because each swap either grows the 0-region or the 2-region (or skips over a 1), and `mid` only moves forward, the loop terminates in O(n) time.

- **Time:** O(n) — each element is examined at most once; swaps are O(1).
- **Space:** O(1) — in-place, only three extra indices.

```cpp
#include <vector>
#include <algorithm>
using namespace std;

// 0,1,2 partition in one pass: Dutch National Flag
void dutchFlagPartition(vector<int>& A) {
    int low = 0, mid = 0, high = (int)A.size() - 1;
    while (mid <= high) {
        if (A[mid] == 0) {
            swap(A[low], A[mid]);
            ++low; ++mid;
        }
        else if (A[mid] == 1) {
            ++mid;
        }
        else { // A[mid] == 2
            swap(A[mid], A[high]);
            --high;
        }
    }
}
```