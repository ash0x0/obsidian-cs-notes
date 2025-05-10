- `x ^ 0 = x`
- `x ^ 1 = ~x` ([[Bitwise Flip]])
- `x ^ x = 0`

## Swapping

Bitwise XOR is often used to swap two values without a temp variable
1. `a = a ^ b`
2. `b = a ^ b`
3. `a = a ^ b`

## Finding Lonely Integer

If we have a sequence where all the numbers in the sequence are repeated twice except one number, we can use XOR to find the lonely number by performing XOR on all the numbers in the sequence