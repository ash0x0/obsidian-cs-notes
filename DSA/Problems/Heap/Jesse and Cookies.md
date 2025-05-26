---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-jesse-and-cookies/problem
---

# Problem

Jesse loves cookies and wants the sweetness of some cookies to be greater than value . To do this, two cookies with the least sweetness are repeatedly mixed. This creates a special combined cookie with:

_sweetness_  _Least sweet cookie_   _2nd least sweet cookie_).

This occurs until all the cookies have a sweetness .

Given the sweetness of a number of cookies, determine the minimum number of operations required. If it is not possible, return .

**Example**  

The smallest values are .  
Remove them then return  to the array. Now .  
Remove  and return  to the array. Now .  
Remove , return  and .  
Finally, remove  and return  to . Now .  
All values are  so the process stops after  iterations. Return .

**Function Description**  
Complete the _cookies_ function in the editor below.

_cookies_ has the following parameters:

- _int k:_ the threshold value
- _int A[n]:_ an array of sweetness values

**Returns**

- _int:_ the number of iterations required or 

**Input Format**

The first line has two space-separated integers,  and , the size of  and the minimum required sweetness respectively.

The next line contains  space-separated integers, .

**Constraints**

  
  

**Sample Input**

STDIN               Function
-----               --------
6 7                 A[] size n = 6, k = 7
1 2 3 9 10 12       A = [1, 2, 3, 9, 10, 12]  

**Sample Output**

```
2
```

**Explanation**

Combine the first two cookies to create a cookie with _sweetness_  =   
After this operation, the cookies are .  
Then, combine the cookies with sweetness  and sweetness , to create a cookie with resulting _sweetness_  =   
Now, the cookies are .  
All the cookies have a sweetness .  
  
Thus,  operations are required to increase the sweetness.
# Solution
## [[Min Heap]] - [[O(log n)]] time - [[O(n)]] space

1. Create a min heap to include the vector numbers
2. As long as the heap has more than 2 elements and the top element has sweetness less than `k`
	1. Get the two least sweet elements
	2. Perform the operation
	3. Push back the result to the heap
3. Return number of operations if the top of the heap is at least `k` sweetness, otherwise return `-1`
```cpp
int cookies(int k, vector<int> A) {
	priority_queue<int, vector<int>, greater<int>> minHeap(A.begin(), A.end());
	int operations = 0;
	while (minHeap.size() >= 2 && minHeap.top() < k) {
		int first = minHeap.top();
		minHeap.pop();
		int second = minHeap.top();
		minHeap.pop();
		minHeap.push(first + (2 * second));
		operations++;
	}
	return minHeap.top() >= k ? operations : -1;
}
```
## [[DSA/Data Structures/Vector|Vector]] in-place - Doesn't Work

1. Sort the vector in descending order
2. Get the last two elements as the least sweet
3. Perform the operation and push back the result
4. Sort the vector in descending order again
5. Keep iterating until the last element is larger than `k`

The issue with this approach is complexity. It won't work for the input size as it grows. This is [[O(n²)]] time and [[O(1)]] space. This is an inherent problem with this approach as we need to keep the vector sorter with each operation to guarantee the smallest numbers are at the last two indices.

```cpp
int cookies(int k, vector<int> A) {
	sort(A.rbegin(), A.rend());
	int operations = 0;
	while (A.back() < k) {
		int first = A.rbegin()[0], second = A.rbegin()[1];
		A.pop_back();
		A.pop_back();
		A.push_back(first + (2 * second));
		operations++;
		sort(A.rbegin(), A.rend());
	}
	return operations;
}
```

