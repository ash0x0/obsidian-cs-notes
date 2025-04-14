---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-time-conversion
---
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