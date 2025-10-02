C++ provides various built-in data types for storing different kinds of data.

# Fundamental Data Types

## Integer Types

```cpp
// Signed integers (can be negative)
char c = 'A';              // 1 byte, -128 to 127
short s = 32000;           // 2 bytes, -32,768 to 32,767
int i = 2147483647;        // 4 bytes, -2³¹ to 2³¹-1
long l = 2147483647L;      // 4 or 8 bytes (platform dependent)
long long ll = 9223372036854775807LL;  // 8 bytes, -2⁶³ to 2⁶³-1

// Unsigned integers (non-negative only)
unsigned char uc = 255;              // 1 byte, 0 to 255
unsigned short us = 65535;           // 2 bytes, 0 to 65,535
unsigned int ui = 4294967295U;       // 4 bytes, 0 to 2³²-1
unsigned long ul = 4294967295UL;     // 4 or 8 bytes
unsigned long long ull = 18446744073709551615ULL;  // 8 bytes, 0 to 2⁶⁴-1

// Fixed-width integers (C++11)
#include <cstdint>
int8_t i8 = -128;      // Exactly 8 bits
int16_t i16;           // Exactly 16 bits
int32_t i32;           // Exactly 32 bits
int64_t i64;           // Exactly 64 bits
uint8_t u8;            // Unsigned 8 bits
uint16_t u16;          // Unsigned 16 bits
uint32_t u32;          // Unsigned 32 bits
uint64_t u64;          // Unsigned 64 bits
```

## Floating-Point Types

```cpp
float f = 3.14f;           // 4 bytes, ~7 decimal digits precision
double d = 3.14159;        // 8 bytes, ~15 decimal digits precision
long double ld = 3.14159L; // 8, 12, or 16 bytes (platform dependent)

// Scientific notation
double large = 1.23e10;    // 1.23 × 10¹⁰
double small = 1.23e-10;   // 1.23 × 10⁻¹⁰
```

## Boolean Type

```cpp
bool flag = true;
bool isValid = false;

// Implicit conversions
bool fromInt = 42;      // true (non-zero)
bool fromZero = 0;      // false
int toInt = true;       // 1
int toInt2 = false;     // 0
```

## Character Types

```cpp
char c = 'A';              // 1 byte, ASCII character
wchar_t wc = L'अ';         // Wide character (2 or 4 bytes)
char16_t c16 = u'Ω';       // 16-bit Unicode character
char32_t c32 = U'🔥';       // 32-bit Unicode character

// C++17
char8_t c8 = u8'a';        // UTF-8 character
```

## Void Type

```cpp
void function();           // Function returns nothing
void* ptr;                // Generic pointer (can point to any type)
```

# Size and Range

```cpp
#include <climits>
#include <cfloat>

// Integer limits
std::cout << "int: " << sizeof(int) << " bytes" << std::endl;
std::cout << "int min: " << INT_MIN << std::endl;
std::cout << "int max: " << INT_MAX << std::endl;

// Float limits
std::cout << "float: " << sizeof(float) << " bytes" << std::endl;
std::cout << "float min: " << FLT_MIN << std::endl;
std::cout << "float max: " << FLT_MAX << std::endl;

// Using sizeof
std::cout << "char: " << sizeof(char) << " bytes" << std::endl;
std::cout << "short: " << sizeof(short) << " bytes" << std::endl;
std::cout << "int: " << sizeof(int) << " bytes" << std::endl;
std::cout << "long: " << sizeof(long) << " bytes" << std::endl;
std::cout << "long long: " << sizeof(long long) << " bytes" << std::endl;
std::cout << "float: " << sizeof(float) << " bytes" << std::endl;
std::cout << "double: " << sizeof(double) << " bytes" << std::endl;
```

# Type Modifiers

```cpp
// Signed/unsigned
signed int si = -10;      // Can be negative
unsigned int ui = 10;     // Non-negative only

// Short/long
short int si = 100;       // Shorter integer
long int li = 100000;     // Longer integer
long long int lli = 1000000000LL;

// const
const int MAX = 100;      // Cannot be modified
const double PI = 3.14159;

// constexpr (C++11) - compile-time constant
constexpr int SIZE = 10;
constexpr double E = 2.71828;
```

# Compound Data Types

## Arrays

```cpp
int arr[5];                      // Array of 5 ints
int arr[5] = {1, 2, 3, 4, 5};   // Initialized array
int arr[] = {1, 2, 3};          // Size inferred: 3

// Multi-dimensional
int matrix[3][4];                // 3x4 matrix
int cube[2][3][4];               // 3D array
```

## Pointers

```cpp
int x = 10;
int* ptr = &x;         // Pointer to int
*ptr = 20;             // Dereference and modify

double* dptr = nullptr; // Null pointer (C++11)

// Pointer arithmetic
int arr[] = {1, 2, 3, 4, 5};
int* p = arr;
p++;                   // Points to arr[1]
p += 2;                // Points to arr[3]
```

## References

```cpp
int x = 10;
int& ref = x;          // Reference to x
ref = 20;              // Modifies x

// Must be initialized
// int& ref;           // Error!

// Cannot be reassigned
int y = 30;
ref = y;               // Assigns y's value to x, doesn't rebind reference
```

## Structures

```cpp
struct Point {
    int x;
    int y;
};

Point p1 = {10, 20};
Point p2;
p2.x = 30;
p2.y = 40;
```

## Enumerations

```cpp
// Traditional enum
enum Color {
    RED,      // 0
    GREEN,    // 1
    BLUE      // 2
};

Color c = RED;

// Enum class (C++11) - type-safe
enum class Status {
    Success,
    Error,
    Pending
};

Status s = Status::Success;
```

# Type Conversions

## Implicit Conversion

```cpp
int i = 10;
double d = i;          // int → double (safe)

double d2 = 3.14;
int i2 = d2;           // double → int (truncation warning)
```

## Explicit Conversion (Casting)

```cpp
double d = 3.14;
int i = (int)d;                    // C-style cast
int i2 = static_cast<int>(d);      // C++ style (preferred)
```

# Auto Type Deduction (C++11)

```cpp
auto x = 10;           // x is int
auto y = 3.14;         // y is double
auto z = "hello";      // z is const char*

auto add = [](int a, int b) { return a + b; };  // Lambda
```

# Type Aliases

```cpp
// Using typedef
typedef unsigned long ulong;
typedef int* IntPtr;

// Using using (C++11, preferred)
using ulong = unsigned long;
using IntPtr = int*;
using Vec2D = vector<vector<int>>;
```

# Best Practices

1. **Use fixed-width types** (`int32_t`, `uint64_t`) for portability
2. **Prefer `double`** over `float` for floating-point calculations
3. **Use `auto`** when type is obvious and improves readability
4. **Initialize variables** at declaration
5. **Use `const`** and `constexpr` for constants
6. **Avoid C-style casts**, use C++ casts (`static_cast`, etc.)
7. **Prefer references** over pointers when possible
8. **Use `nullptr`** instead of `NULL` or `0` for null pointers

# Common Issues

```cpp
// Integer overflow
int max = INT_MAX;
max++;                 // Undefined behavior!

// Floating-point precision
double a = 0.1;
double b = 0.2;
if (a + b == 0.3) {}  // May be false due to precision!

// Use epsilon for comparison
const double EPSILON = 1e-9;
if (abs(a + b - 0.3) < EPSILON) {}  // Better

// Signed/unsigned mismatch
unsigned int u = 10;
int i = -5;
if (i < u) {}         // i converted to unsigned, becomes large positive!
```

# Related Topics

- [[Casting]]
- [[Memory Management]]
- [[Type Traits]]
- [[Templates]]
