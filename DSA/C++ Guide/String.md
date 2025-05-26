---
sources: https://www.geeksforgeeks.org/strings-in-cpp/
---
# Basic Usage

```cpp
string str_name;
string str = "Some Text here"; 

// Accessing 3rd character 
cout << str[2]; 
// Accessing first character 
cout << str[0];

// Updating string
str = "Singh";
// Updating second character
str[1] = 'o';
// Get the last chracter
str.back() == "-";
// Remove the last character
str.pop_back();
// Add character to end
str.push_back('c');
```
# Erase

## Remove all occurrences of a character

```cpp
#include <algorithm>
using namespace std;
// Remove all occurrences of .
str.erase(remove(str.begin(), str.end(), '.'), str.end());
```
# Reverse

```cpp
using namespace std;

string s = "Hello World";
// Using reverse() function to reverse s
reverse(s.begin(), s.end());
// Using reverse iterators
s = string(s.rbegin(), s.rend());
```
## [[Two Pointer]]

```cpp
int reverseTwoPointer() {
    string s = "Hello World";

    // Initialize two pointers: left at start
    //  and right at the end of the string
    int l = 0;
    int r = s.length() - 1;

    // Loop until the two pointers meet in the middle
    while (l < r) {
      
        // Swap characters at position left and right
        swap(s[l], s[r]);

        // Move the left pointer to right
        l++;

        // Move the right pointer to left
        r--;
    }

    cout << s;
    return 0;
}
```
## [[Recursion]]

```cpp
// Function to reverse the string using recursion
void reverseRecursion(string &s, int l, int r) {
  
    // Base case: When left and right pointer meet
    if (l >= r)
        return;

    // Swap characters at left and right
    swap(s[l], s[r]);

    // Recursive call to reverse remaining substring
    reverseRecursion(s, l + 1, r - 1);
}

// Call the recursive function to reverse string
revStr(s, 0, s.length() - 1);
```

## [[DSA/Data Structures/Stacks & Queues/Stack]]

```cpp
int reverseWithStack() {
    string s = "Hello World";
    stack<char> st;

    // Push each character of string into stack
    for (char c : s)
        st.push(c);

    // Clear the string
    s.clear();

    // Pop characters from stack and add them to
    // reversed string
    while (!st.empty()) {
        s.push_back(st.top());
        st.pop();
    }

    cout << s;
    return 0;
}
```
# Compare

```cpp
#include <iostream>

using namespace std;

void compareFunction(string s1, string s2)
{
	// comparing both using inbuilt function
	int x = s1.compare(s2);

	if (x != 0) {
		cout << s1 
			<< " is not equal to "
			<< s2 << endl;
		if (x > 0)
			cout << s1 
				<< " is greater than "
				<< s2 << endl;
		else
			cout << s2 
				<< " is greater than "
				<< s1 << endl;
	}
	else
		cout << s1 << " is equal to " << s2 << endl;
}

// Driver Code
int main()
{
	string s1("Geeks");
	string s2("forGeeks");
	compareFunction(s1, s2);
	string s3("Geeks");
	string s4("Geeks");
	compareFunction(s3, s4);
	return 0;
}
/*
Geeks is not equal to forGeeks
forGeeks is greater than Geeks
Geeks is equal to Geeks
*/

```

# Convert

## String to number

```cpp
#include <string>
int stoi(string __str__, size_t __position = 0__, int __base = 10__);

string str1 = "45";
string str2 = "3.14159";
char str3[] = "31337 geek";

int myint1 = stoi(str1);
int myint2 = stoi(str2);
int myint3 = stoi(str3);

/* 
stoi("45") is 45
stoi("3.14159") is 3
stoi("31337 geek") is 31337
*/
```

****Parameters:****

- ****str:**** string to be converted. (compulsory)
- ****position:**** starting position. (optional with default value = 0)
- ****base:**** base of the number system. (optional with default value = 10)

```cpp
#include <stdlib.h>
char* str1 = "141";
char* str2 = "3.14";

int res1 = atoi(str1);
int res2 = atoi(str2);

/* 
atoi(141) is 141 
atoi(3.14) is 3
*/
```

```cpp
// C++ Program to convert String
// into number using for loop
#include <bits/stdc++.h>

using namespace std;
int main()
{
    string number = "13";
    int i = 0;

    // Traversing string
    for (char c : number) {
        // Checking if the element is number
        if (c >= '0' && c <= '9') {
            i = i * 10 + (c - '0');
        }
        // Otherwise print bad output
        else {
            cout << "Bad Input";
            return 1;
        }
    }

    cout << i;
}
```
## Number to string

```cpp
int i = 10;
string str = to_string(i);
```

# [Search](https://www.geeksforgeeks.org/string-find-in-cpp/)

## Usage

```cpp
s.find(sub, pos); // For substring`
s.find(c, pos); // For character`
```
### Parameters:

- s: String which is to be searched.
- sub: Substring to search. Can be C++ or C style string.
- pos: Position from where the string search is to begin. By default, it is 0.
- n: Number of characters to match.
### Return Value:

- Returns the index of the first occurrence of the sub-string.
- If the sub-string is not found, it returns [string::npos.](https://www.geeksforgeeks.org/stringnpos-in-c-with-examples/)
## Check if a substring exists

```cpp
string s = "Welcome to GfG!";
string sub = "hello";

// Checking if sub is present in s
int res = s.find(sub);
if (res != string::npos) cout << res;
```

## Find multiple occurrences of a substring

```cpp
string s = "welcome to geeksforgeeks";
char sub[] = "geeks";

// Loop that runs till string::find()
// returns string::npos
int res = -1;
while ((res = s.find(sub, res + 1)) != string::npos)
	cout << res << " ";
```

## Find all occurrences of a character

```cpp
string s = "welcome to GfG";
char c = 'e';

// Loop that runs till string::find()
// returns string::npos
int res = 0;
while ((res = s.find(c, res + 1)) != string::npos)
	cout << res << " ";
```

## Find occurrences of a partial substring

```cpp
string s = "welcome to GfG!";
char sub[] = "come to my house";

// Only searching for "come"
int res = s.find(sub, 0, 4);
if (res != string::npos) cout << res;
```
# [Substring](https://www.geeksforgeeks.org/substring-in-cpp/)

## Usage

```cpp
#include <string>
string substr (size_t pos, size_t len) const;

// Take any string
string s1 = "Geeks";
// Copy two characters of s1 (starting from index 3)
string r = s1.substr(3, 2);
/*
ks
*/
```

### Parameters:
- **pos**: Index of the first character to be copied.
- **len**: Length of the sub-string.
- **size_t**: It is an unsigned integral type.
## Get a Sub-String after a character

```cpp
// Take any string
string s = "dog:cat";
// Copy substring after pos
string sub = s.substr(s.find(":") + 1);
```
## Get a Sub-String before a character

```cpp
// Take any string
string s = "dog:cat";
string sub = s.substr(0, s.find(":"));
```
## Print all Sub-Strings of a given String

```cpp
// Function to print all sub strings
// Pick starting point in outer loop
// and lengths of different strings for
// a given starting point
s = "abcd"
n = s.length();
for (int i = 0; i < n; i++)
	for (int len = 1; len <= n - i; len++)
		cout << s.substr(i, len) << endl;
```
## Sum of all Substrings of a string representing a number

```cpp
// C++ program to print sum of all possible substring of
// a number represented as a string
#include <bits/stdc++.h>
using namespace std;

// Utility method to convert character digit to
// integer digit
int toDigit(char ch) { return (ch - '0'); }

// Returns sum of all substring of num
int sumOfSubstrings(string s)
{
    vector<int> v;
    int n = s.length();
    for (int i = 0; i < n; i++) {
        for (int len = 1; len <= n - i; len++) {
            string sub = (s.substr(i, len));
            int x = stoi(sub);
            v.push_back(x);
        }
    }
    int res = accumulate(v.begin(), v.end(), 0);

    return res;
}

// Driver code to test above methods
int main()
{
    string num = "1234";
    cout << sumOfSubstrings(num) << endl;
    return 0;
}
```
## Print the maximum value of all substrings of a string representing a number

```cpp
// C++ program to demonstrate max. of all possible
// substrings of a given string
#include <bits/stdc++.h>
using namespace std;

void subString(string s, int n)
{
    vector<int> v;
    for (int i = 0; i < n; i++) {
        for (int len = 1; len <= n - i; len++) {
            string sub = (s.substr(i, len));
            int x = stoi(sub);
            v.push_back(x);
        }
    }
    cout << *max_element(v.begin(), v.end()) << endl;
}

// Driver program to test above function
int main()
{
    string s = "823";
    subString(s, s.length());
    return 0;
}
```
## Print the minimum value of all substrings of a string representing a number

```cpp
// C++ program to demonstrate minimum of all possible
// substrings of a given string
#include <bits/stdc++.h>
using namespace std;

void subString(string s, int n)
{
    vector<int> v;
    for (int i = 0; i < n; i++) {
        for (int len = 1; len <= n - i; len++) {
            string sub = (s.substr(i, len));
            int x = stoi(sub);
            v.push_back(x);
        }
    }
    cout << *min_element(v.begin(), v.end()) << endl;
}

// Driver program to test above function
int main()
{
    string s = "4572";
    subString(s, s.length());
    return 0;
}
```
# Casing
## Convert string to uppercase

```cpp
for (auto& x : s) {
	x = toupper(x);
}
```
## Convert string to lowercase

```cpp
for (auto& x : s) {
	x = tolower(x);
}
```

# Substring

`s.substr(begin, length)`

```cpp

// get all but last two characters
string first = s.substr(0, s.size() - 2);
// get the last two characters
string last = s.substr(s.size() - 2);
```

# Repeating

If we want to create a string with repeating characters, similar to python's `*` string operator, we can use the overloaded string constructor

```cpp
string spaces = string(5, ' ');
```

The second argument has to be a character, not a string.