There are several ways to swap two elements in a data structure or two variables in general.

# Using a Temp

```cpp
char temp = s[i];
s[i] = s[j];
s[j] = temp;
```

# Using [[Bitwise XOR]] Swap

This is mostly academic and not used for 

```cpp
// Let's assume a and b are int, and a != b, and &a != &b
a = a ^ b;
b = a ^ b;  // effectively b = (a ^ b) ^ b = a
a = a ^ b;  // effectively a = (a ^ b) ^ a = b

// If i != j
s[i] = s[i] ^ s[j];
s[j] = s[i] ^ s[j];
s[i] = s[i] ^ s[j];
```

# C++ STL
## Using `std::swap`

**`std::swap`** is usually the most straightforward and idiomatic in modern C++.

```cpp
#include <string>
#include <utility>
using namespace std;

int a = 10, b = 20;
swap(a, b);  // Now a = 20, b = 10

string s = "Hello";
swap(s[0], s[4]); // Swap 'H' and 'o', s becomes "oellH"
```
## Using `std::exchange`

**`std::exchange`** is sometimes handy for one-liners or if you need the “old value” while assigning a “new value” in the same expression.

```cpp
#include <utility>
using namespace std;

int a = 10, b = 20;
a = exchange(b, a);

string s = "Hello";
s[0] = exchange(s[4], s[0]);
```
## Using References in a Function

Sometimes you want a function that swaps two elements in an array, or two characters in a string, by passing them **by reference**:

```cpp
void swapTwo(int& x, int& y) {
    int temp = x;
    x = y;
    y = temp;
}

// Usage:
int arr[] = {1, 2, 3, 4};
swapTwo(arr[0], arr[1]);  // arr is now {2, 1, 3, 4}
```