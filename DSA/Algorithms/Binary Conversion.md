Binary conversion is the process of converting numbers between decimal (base-10) and binary (base-2) representation. This is fundamental in computer science as computers internally work with binary numbers.

# Decimal to Binary Conversion

To convert a decimal number to binary, repeatedly divide the number by 2 and record the remainders in reverse order.

## Algorithm

1. Divide the decimal number by 2
2. Record the remainder (0 or 1)
3. Update the number to the quotient
4. Repeat steps 1-3 until the number becomes 0
5. The binary representation is the remainders read in reverse order

## Implementation

```cpp
string decimalToBinary(int n) {
    if (n == 0) return "0";
    
    string binary = "";
    while (n > 0) {
        binary = to_string(n % 2) + binary;
        n /= 2;
    }
    return binary;
}
```

# Binary to Decimal Conversion

To convert a binary number to decimal, multiply each digit by powers of 2 and sum the results.

## Algorithm

1. Start from the rightmost digit (LSB - Least Significant Bit)
2. Multiply each bit by 2^position (where position starts at 0)
3. Sum all the products

## Implementation

```cpp
int binaryToDecimal(string binary) {
    int decimal = 0;
    int power = 0;
    
    for (int i = binary.length() - 1; i >= 0; i--) {
        if (binary[i] == '1') {
            decimal += pow(2, power);
        }
        power++;
    }
    return decimal;
}
```

# Using Built-in Functions

## C++

```cpp
// Decimal to Binary using bitset
#include <bitset>
string decToBin = bitset<32>(num).to_string();

// Binary to Decimal using stoi
int binToDec = stoi(binaryString, nullptr, 2);
```

# Time Complexity

- Decimal to Binary: [[O(log n)]] where n is the decimal number
- Binary to Decimal: [[O(n)]] where n is the length of the binary string

# Space Complexity

[[O(1)]] for the conversion itself (excluding output storage)
