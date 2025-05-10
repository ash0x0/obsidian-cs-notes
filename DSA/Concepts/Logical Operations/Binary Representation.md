- Integers are represented as a string of bits (binary digits)
# Endianity

## Big Endian 

- Most significant place value at left
- Three-hundred twenty-one as `321`

## Little Endian 

- Most significant place value at right
- Three-hundred twenty-one or `123`. 

# Unsigned Integers

- Number of bits used (width) determines the numbers that can be encoded 2<sup>n</sup>.
# Signed Integers

There are four methods for signed integer representation

## Two's Complement

To negate a number:
- Invert all the bits in a number
- Add 1

This approach only has one representation of `0`

The range for this is from -2<sup>n</sup> to 2<sup>n-1</sup> - 1

# Arithmetic

## Multiplication

Multiplying a number by any factor of `2` (any even number) is done with [[Bitwise Shift Left]]. 
- $3 * 4 = 3 << 2$
This can work for odd numbers as well by factorizing and grouping then adding `1`
- $3 * 5 = 3 * (4 + 1) = 3 << 2 + 1$ 
## Division

Dividing is done with [[Bitwise Shift Right]]. It's integer division (no float representation) so it's rounded down (approximated).
