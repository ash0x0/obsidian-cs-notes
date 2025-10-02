#Facebook #Medium #DailyCodingProblem 
# Problem

Given a function that generates perfectly random numbers between 1 and k (inclusive), where k is an input, write a function that shuffles a deck of cards represented as an array using only swaps.

It should run in O(N) time.

Hint: Make sure each one of the 52! permutations of the deck is equally likely.
# Solution

## Fisher-Yates (Knuth) Shuffle - [[O(n)]] time - [[O(1)]] space

Randomly shuffle an array in-place using only swaps.

```cpp
#include <random>

void shuffle(vector<int>& deck) {
    random_device rd;
    mt19937 gen(rd());
    
    int n = deck.size();
    
    // Start from the end and swap with random earlier position
    for (int i = n - 1; i > 0; i--) {
        // Generate random index from 0 to i (inclusive)
        uniform_int_distribution<> dis(0, i);
        int j = dis(gen);
        
        // Swap deck[i] with deck[j]
        swap(deck[i], deck[j]);
    }
}
```

# Algorithm

1. Start from last element (index n-1)
2. Pick random index j from 0 to i (inclusive)
3. Swap element at i with element at j
4. Move to i-1 and repeat
5. Continue until i = 0

# Why It Works

- Each position has equal probability of containing any card
- Total permutations: n!
- Each swap selects from remaining unshuffled cards
- Guarantees uniform distribution over all n! permutations

# Proof of Uniformity

For position i, probability of any specific card being there:
- P(card at position i) = (1/n) × (n/(n-1)) × ((n-1)/(n-2)) × ... = 1/n

This holds for all positions, ensuring uniform distribution.

# Common Mistakes

**Wrong**: Swap with random position from 0 to n-1
```cpp
// INCORRECT - biased shuffle
for (int i = 0; i < n; i++) {
    int j = rand() % n;  // Wrong!
    swap(deck[i], deck[j]);
}
```

**Correct**: Swap with random position from 0 to i
```cpp
// CORRECT - unbiased shuffle
for (int i = n - 1; i > 0; i--) {
    int j = rand() % (i + 1);  // Correct!
    swap(deck[i], deck[j]);
}
```

# Using Given Random Function

If you have `randK(k)` that returns 1 to k:

```cpp
void shuffle(vector<int>& deck, function<int(int)> randK) {
    int n = deck.size();
    
    for (int i = n - 1; i > 0; i--) {
        // randK returns 1 to k, so subtract 1 for 0-based index
        int j = randK(i + 1) - 1;
        swap(deck[i], deck[j]);
    }
}
```

# Example Walkthrough

```
Initial: [A, B, C, D]

i=3: Pick j from [0,3], say j=1
     Swap deck[3] with deck[1]
     Result: [A, D, C, B]

i=2: Pick j from [0,2], say j=0
     Swap deck[2] with deck[0]
     Result: [C, D, A, B]

i=1: Pick j from [0,1], say j=1
     Swap deck[1] with deck[1] (no change)
     Result: [C, D, A, B]

Final: [C, D, A, B]
```

# Time Complexity

[[O(n)]] - exactly n-1 swaps

# Space Complexity

[[O(1)]] - in-place shuffle

# Verification

To verify uniformity, run shuffle 10000 times and check distribution:

```cpp
void testShuffle() {
    map<vector<int>, int> frequency;
    vector<int> deck = {1, 2, 3, 4};
    
    for (int i = 0; i < 100000; i++) {
        vector<int> copy = deck;
        shuffle(copy);
        frequency[copy]++;
    }
    
    // Should have ~4167 occurrences for each of 24 permutations
    for (auto& [perm, count] : frequency) {
        cout << "Permutation: ";
        for (int card : perm) cout << card << " ";
        cout << "- Count: " << count << endl;
    }
}
```

# Related Problems

- LeetCode #384 - Shuffle an Array
- [[Random Sampling]]
- [[Reservoir Sampling]]
- Array permutations
