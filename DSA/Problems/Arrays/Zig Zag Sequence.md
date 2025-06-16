#TODO 
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

1. `ed = ed + 1` > `ed = ed - 1`
2. `int mid = (n+1)/2` > `int mid = (n+1)/2 - 1`