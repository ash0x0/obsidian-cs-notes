#Google #DailyCodingProblem #Medium 
# Problem

The area of a circle is defined as πr^2. Estimate π to 3 decimal places using a Monte Carlo method.

Hint: The basic equation of a circle is x2 + y2 = r2.
# Solution

## Monte Carlo Simulation - [[O(n)]] time where n = number of samples

Use random sampling to estimate π by comparing points inside vs outside a circle.

```cpp
#include <random>
#include <cmath>

double estimatePi(int numSamples) {
    random_device rd;
    mt19937 gen(rd());
    uniform_real_distribution<> dis(-1.0, 1.0);
    
    int insideCircle = 0;
    
    for (int i = 0; i < numSamples; i++) {
        double x = dis(gen);
        double y = dis(gen);
        
        // Check if point is inside unit circle (x^2 + y^2 <= 1)
        if (x * x + y * y <= 1.0) {
            insideCircle++;
        }
    }
    
    // Ratio of areas: circle/square = π/4
    // So π ≈ 4 * (insideCircle / totalSamples)
    return 4.0 * insideCircle / numSamples;
}

int main() {
    cout << fixed << setprecision(3);
    
    // More samples = better accuracy
    cout << "10,000 samples: " << estimatePi(10000) << endl;
    cout << "100,000 samples: " << estimatePi(100000) << endl;
    cout << "1,000,000 samples: " << estimatePi(1000000) << endl;
    
    return 0;
}
```

# Algorithm Explanation

1. **Setup**: Consider a square with side 2 containing a circle of radius 1
2. **Random Sampling**: Generate random points (x, y) where -1 ≤ x, y ≤ 1
3. **Test**: Check if point is inside circle: x² + y² ≤ 1
4. **Calculate**:
   - Area of circle = πr² = π (for radius 1)
   - Area of square = 4
   - Ratio = π/4
   - Therefore: π ≈ 4 × (points inside circle / total points)

# Visual Representation

```
    +---+---+
    | * | * |  * = outside circle
    +---O---+  O = inside circle
    | O | O |
    +---+---+

Ratio of O to total ≈ π/4
```

# Python Implementation

```python
import random
import math

def estimate_pi(num_samples):
    inside_circle = 0
    
    for _ in range(num_samples):
        x = random.uniform(-1, 1)
        y = random.uniform(-1, 1)
        
        if x*x + y*y <= 1:
            inside_circle += 1
    
    return 4.0 * inside_circle / num_samples

# Test
print(f"Estimate: {estimate_pi(1000000):.3f}")
print(f"Actual π: {math.pi:.3f}")
```

# Accuracy Analysis

- Error decreases as O(1/√n)
- For 3 decimal places (~0.001 error), need ~1,000,000 samples
- Not the most efficient method, but demonstrates Monte Carlo principle

# Time Complexity

[[O(n)]] where n is the number of samples

# Space Complexity

[[O(1)]]

# Related Concepts

- [[Monte Carlo Methods]]
- [[Random Number Generation]]
- Buffon's Needle Problem
- Approximation algorithms
