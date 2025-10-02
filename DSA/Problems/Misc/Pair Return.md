#DailyCodingProblem #Medium #JaneStreet 
# Problem

`cons(a, b)` constructs a pair, and `car(pair)` and `cdr(pair)` returns the first and last element of that pair. For example, `car(cons(3, 4))` returns `3`, and `cdr(cons(3, 4))` returns `4`.

Given this implementation of cons:

```
def cons(a, b):
    def pair(f):
        return f(a, b)
    return pair
```

Implement `car` and `cdr`.
# Solution

## Understanding cons, car, and cdr

These are Lisp-inspired functions for working with pairs:
- `cons(a, b)`: Creates a pair
- `car(pair)`: Returns first element
- `cdr(pair)`: Returns second element

# Implementation

```python
def cons(a, b):
    def pair(f):
        return f(a, b)
    return pair

def car(pair):
    def get_first(a, b):
        return a
    return pair(get_first)

def cdr(pair):
    def get_second(a, b):
        return b
    return pair(get_second)

# Test
p = cons(3, 4)
print(car(p))  # 3
print(cdr(p))  # 4
```

# How It Works

1. **cons(a, b)** returns a function that:
   - Takes another function `f` as parameter
   - Calls `f(a, b)` with the stored values

2. **car(pair)** returns first element by:
   - Passing a function that returns the first parameter
   - `pair` calls this function with stored `(a, b)`

3. **cdr(pair)** returns second element similarly

# Visualization

```
cons(3, 4) returns:
    λf. f(3, 4)

car(pair) does:
    pair(λ(a,b). a)
    = (λf. f(3, 4))(λ(a,b). a)
    = (λ(a,b). a)(3, 4)
    = 3

cdr(pair) does:
    pair(λ(a,b). b)
    = (λf. f(3, 4))(λ(a,b). b)
    = (λ(a,b). b)(3, 4)
    = 4
```

# C++ Implementation

```cpp
#include <functional>

template<typename A, typename B>
auto cons(A a, B b) {
    return [a, b](auto f) { return f(a, b); };
}

template<typename Pair>
auto car(Pair pair) {
    return pair([](auto a, auto b) { return a; });
}

template<typename Pair>
auto cdr(Pair pair) {
    return pair([](auto a, auto b) { return b; });
}

// Usage
int main() {
    auto p = cons(3, 4);
    cout << car(p) << endl;  // 3
    cout << cdr(p) << endl;  // 4
    
    return 0;
}
```

# Key Concept: Data as Functions

This demonstrates **Church encoding** - representing data using only functions. The pair is stored in the closure of the returned function.

# Alternative: Using Classes

More conventional approach:

```python
class Pair:
    def __init__(self, first, second):
        self.first = first
        self.second = second

def cons(a, b):
    return Pair(a, b)

def car(pair):
    return pair.first

def cdr(pair):
    return pair.second
```

# Related Concepts

- [[Closures]]
- [[Higher-Order Functions]]
- [[Lambda Calculus]]
- Church encoding
- Functional programming
