---
HackerRank: https://www.hackerrank.com/challenges/one-week-preparation-kit-tower-breakers-1
---
# Problem

This is a badly written and codified problem and the testcases don't reflect the description
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