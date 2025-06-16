---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-tower-breakers-1
---
# Problem

Two players are playing a game of Tower Breakers! Player  always moves first, and both players always play optimally.The rules of the game are as follows:

- Initially there are  towers.
- Each tower is of height .
- The players move in alternating turns.
- In each turn, a player can choose a tower of height  and reduce its height to , where  and  [evenly divides](https://en.wiktionary.org/wiki/evenly_divisible) .
- If the current player is unable to make a move, they lose the game.

Given the values of  and , determine which player will win. If the first player wins, return . Otherwise, return .

**Example**.   

There are  towers, each  units tall. Player  has a choice of two moves:  
- remove  pieces from a tower to leave  as   
- remove  pieces to leave 

Let Player  remove . Now the towers are  and  units tall.

Player  matches the move. Now the towers are both  units tall.

Now Player  has only one move.

Player  removes  pieces leaving . Towers are  and  units tall.  
Player  matches again. Towers are both  unit tall.

Player  has no move and loses. Return .

**Function Description**

Complete the _towerBreakers_ function in the editor below.

towerBreakers has the following paramter(s):

- _int n:_ the number of towers
- _int m:_ the height of each tower

**Returns**

- _int:_ the winner of the game

**Input Format**

The first line contains a single integer , the number of test cases.  
Each of the next  lines describes a test case in the form of  space-separated integers,  and .

**Constraints**

**Sample Input**

```
STDIN   Function
-----   --------
2       t = 2
2 2     n = 2, m = 2
1 4     n = 1, m = 4
```

**Sample Output**

```
2
1
```

**Explanation**

We'll refer to player  as  and player  as 

In the first test case,  chooses one of the two towers and reduces it to . Then  reduces the remaining tower to a height of . As both towers now have height ,  cannot make a move so  is the winner.

In the second test case, there is only one tower of height .  can reduce it to a height of either  or .  chooses  as both players always choose optimally. Because  has no possible move,  wins.
# Solution

## Iterative - [[O(n)]] time - [[O(1)]] space

```cpp
int towerBreakers(int n, int m) {
    vector<int> towers = vector<int>(n, m);
    int turn = 1;
    for (int i = 0; i < towers.size(); i++) {
        if (towers[i] > 2) {
            while (towers[i] % 2 == 0) {
                towers[i] = towers[i] / 2;
                turn = 3 - turn;
            }
            towers[i] = 1;
            turn = 3 - turn;
        }
    }
    return (3 - turn);
}
```

Optimize by realizing that `turn = 3 - turn` isn't necessary inside the while loop as the final result will always be the same after flipping inside the loop then flipping again after

```cpp
int towerBreakers(int n, int m) {
    vector<int> towers = vector<int>(n, m);
    int turn = 1;
    for (int i = 0; i < towers.size(); i++) {
        if (towers[i] > 2) {
            while (towers[i] % 2 == 0) {
                towers[i] = towers[i] / 2;
            }
            towers[i] = 1;
            turn = 3 - turn;
        }
    }
    return (3 - turn);
}
```

Optimize further by realizing since the only thing we care about is the turn and not the tower sizes, we can remove all non-important internal factors, like the `while` loop since it makes no difference to the turns

```cpp
int towerBreakers(int n, int m) {
    vector<int> towers = vector<int>(n, m);
    int turn = 1;
    for (int i = 0; i < towers.size(); i++) {
        if (towers[i] > 2) {
            towers[i] = 1;
            turn = 3 - turn;
        }
    }
    return (3 - turn);
}
```

## One-liner

Since the testcases don't accurately reflect the problem description this is actually solvable according to the testcases in a way such that
1. If `n` is odd player 1 always wins
2. If `n` is even player 2 always wins
3. Player 2 wins if `m` is `1`

```cpp
int towerBreakers(int n, int m) {
    return (n % 2 == 0 || m == 1) ? 2 : 1;
}
```