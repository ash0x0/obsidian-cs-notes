---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-new-year-chaos/problem?
---
# Problem

It is New Year's Day and people are in line for the Wonderland rollercoaster ride. Each person wears a sticker indicating their _initial_ position in the queue from  to . Any person can bribe the person _directly in front_ of them to swap positions, but they still wear their original sticker. One person can bribe _at most two others_.

Determine the minimum number of bribes that took place to get to a given queue order. Print the number of bribes, or, if anyone has bribed more than two people, print `Too chaotic`.

**Example**

If person  bribes person , the queue will look like this: . Only  bribe is required. Print `1`.

Person  had to bribe  people to get to the current position. Print `Too chaotic`.

**Function Description**

Complete the function _minimumBribes_ in the editor below.

minimumBribes has the following parameter(s):

- _int q[n]_: the positions of the people after all bribes

**Returns**

- No value is returned. Print the minimum number of bribes necessary or `Too chaotic` if someone has bribed more than  people.

**Input Format**

The first line contains an integer , the number of test cases.

Each of the next  pairs of lines are as follows:  
- The first line contains an integer , the number of people in the queue  
- The second line has  space-separated integers describing the final state of the queue.

**Constraints**

**Subtasks**

For  score   
For  score 

**Sample Input**

```
STDIN       Function
-----       --------
2           t = 2
5           n = 5
2 1 5 3 4   q = [2, 1, 5, 3, 4]
5           n = 5
2 5 1 3 4   q = [2, 5, 1, 3, 4]
```

**Sample Output**

```
3
Too chaotic
```

**Explanation**

**Test Case 1**

The initial state:

![](https://s3.amazonaws.com/hr-challenge-images/494/1451665589-31d436ba19-pic11.png "pic1(1).png")

After person  moves one position ahead by bribing person :

![](https://s3.amazonaws.com/hr-challenge-images/494/1451665679-6504422ed9-pic2.png "pic2.png")

Now person  moves another position ahead by bribing person :

![](https://s3.amazonaws.com/hr-challenge-images/494/1451665818-27bd62bb0d-pic3.png "pic3.png")

And person  moves one position ahead by bribing person :

![](https://s3.amazonaws.com/hr-challenge-images/494/1451666025-02a2395a00-pic5.png "pic5.png")

So the final state is  after three bribing operations.

**Test Case 2**

No person can bribe more than two people, yet it appears person  has done so. It is not possible to achieve the input state.
# Solution
## Iterative - [[O(n²)]] worst-case time - [[O(1)]] space

Instead of thinking about how many people this person has bribed, think about the people who bribed this person

If someone is back from their original position, then they were bribed

To know how many people bribed them we just need to count the number of people who were originally supposed to be behind them (larger number) between their original position and current position in the queue

```cpp
void minimumBribes(vector<int> q) {
	int minimumBribes = 0;

	for (int i = 0; i < q.size(); i++) {
		if (q[i] > i + 1 + 2) {
			cout << "Too chaotic" << endl;
			return;
		}
		for (int j = q[i] - 2; j < i; j++) {
			if (q[j] > q[i]) minimumBribes++;
		}
	}
	cout << minimumBribes << endl;
	return;
}
```

The time complexity of this can be [[O(n²)]] if the person is bribed by all others behind them and they bribe no one. It gets up to `n` if that person was supposed to start at the beginning of the queue.