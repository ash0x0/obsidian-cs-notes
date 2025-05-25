---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-time-conversion
---
# Problem

Given a time in [-hour AM/PM format](https://en.wikipedia.org/wiki/12-hour_clock), convert it to military (24-hour) time.

Note: - 12:00:00AM on a 12-hour clock is 00:00:00 on a 24-hour clock.  
- 12:00:00PM on a 12-hour clock is 12:00:00 on a 24-hour clock.

**Example**

- Return '12:01:00'.
    
- Return '00:01:00'.
    

**Function Description**

Complete the  function with the following parameter(s):

- : a time in  hour format

**Returns**

- : the time in  hour format

**Input Format**

A single string  that represents a time in -hour clock format (i.e.:  or ).

**Constraints**

- All input times are valid

**Sample Input 0**

07:05:45PM

**Sample Output 0**

19:05:45
# Solution

## Substrings

```cpp
string timeConversion(string s) {
	string marker = s.substr(s.size() - 2);
	string hours = s.substr(0, 2);
	string minutes = s.substr(3, 2);
	string seconds = s.substr(6, 2);

	if (marker == "PM") {
		if (hours != "12") hours = to_string(stoi(hours) + 12);
	} else {
		if (hours == "12") hours = "00";
	}
	
	return hours + ":" + minutes + ":" + seconds;
}
```

```cpp
string timeConversion(string s) {
	string am_pm = s.substr(s.length() - 2);
	s.pop_back();
	s.pop_back();
	if (am_pm == "PM" && s.substr(0, s.find(":")) != "12") {
		s[0] = s[0] + 1;
		s[1] = s[1] + 2;
	}
	if (am_pm == "AM" && s.substr(0, s.find(":")) == "12") {
		s[0] = '0';
		s[1] = '0';
	}
	return s;
}
```