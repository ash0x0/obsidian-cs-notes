C++ provides several casting operators to convert between types safely and explicitly.

# C-Style Cast

Legacy casting from C, not type-safe.

```cpp
double d = 3.14;
int i = (int)d;  // C-style cast

// Can do dangerous casts
int* ptr = (int*)&d;  // Dangerous!
```

**Use**: Avoid in modern C++

# static_cast

Performs compile-time type conversion between related types.

```cpp
// Numeric conversions
double d = 3.14;
int i = static_cast<int>(d);  // 3

// Pointer conversions in inheritance hierarchy (safe direction)
class Base {};
class Derived : public Base {};

Derived* derived = new Derived();
Base* base = static_cast<Base*>(derived);  // Upcast - safe

// Downcast - compiler allows but dangerous without runtime check
Base* base = new Base();
Derived* derived = static_cast<Derived*>(base);  // Compiles but unsafe!

// Explicit conversions
float f = 3.5f;
int i = static_cast<int>(f);

// Enum conversions
enum Color { RED = 0, GREEN = 1, BLUE = 2 };
int colorValue = static_cast<int>(RED);  // 0
Color color = static_cast<Color>(1);  // GREEN
```

**When to use**:
- Numeric conversions
- Upcasting in inheritance
- Explicit type conversions
- Converting to/from void*

**Pros**: Fast (compile-time), clear intent
**Cons**: No runtime safety for downcasts

# dynamic_cast

Performs runtime type checking for polymorphic types.

```cpp
class Base {
    virtual ~Base() {}  // Must have virtual function
};

class Derived : public Base {};
class Other : public Base {};

Base* base = new Derived();

// Safe downcast with runtime check
Derived* derived = dynamic_cast<Derived*>(base);
if (derived != nullptr) {
    // Success - base was actually Derived
}

// Failed downcast returns nullptr
Other* other = dynamic_cast<Other*>(base);
// other == nullptr

// With references, throws std::bad_cast on failure
try {
    Derived& ref = dynamic_cast<Derived&>(*base);
} catch (std::bad_cast& e) {
    // Cast failed
}
```

**When to use**:
- Safe downcasting in inheritance hierarchies
- Runtime type identification
- Working with polymorphic types

**Pros**: Runtime safety
**Cons**: Requires RTTI, slower than static_cast, requires virtual functions

# const_cast

Adds or removes const/volatile qualifiers.

```cpp
void processData(const int* data) {
    // Remove const to modify (dangerous!)
    int* mutableData = const_cast<int*>(data);
    *mutableData = 42;  // Undefined behavior if data was originally const
}

const int value = 10;
// int* ptr = &value;  // Error: cannot convert const to non-const

// Can compile but undefined behavior
int* ptr = const_cast<int*>(&value);
*ptr = 20;  // Undefined behavior!

// Legitimate use: calling legacy API
void legacyFunction(int* data);  // Old API doesn't accept const

void modernFunction(const int* data) {
    // We know legacyFunction won't modify data
    legacyFunction(const_cast<int*>(data));
}
```

**When to use**:
- Interfacing with legacy code
- Overloaded member functions (const and non-const versions)

**Avoid**: Removing const from actually const data (undefined behavior)

# reinterpret_cast

Low-level reinterpretation of bit patterns. Most dangerous cast.

```cpp
// Pointer to integer
int* ptr = new int(42);
uintptr_t address = reinterpret_cast<uintptr_t>(ptr);
std::cout << "Address: " << address << std::endl;

// Integer to pointer (dangerous!)
int* reconstructed = reinterpret_cast<int*>(address);

// Unrelated pointer types
double d = 3.14;
int* intPtr = reinterpret_cast<int*>(&d);  // Treats double as int - dangerous!

// Function pointers
void (*funcPtr)() = reinterpret_cast<void(*)()>(someAddress);
```

**When to use**:
- Low-level programming
- Hardware interfacing
- Serialization
- Converting pointers to integers and back

**Danger**: No type safety, can easily cause undefined behavior

# Comparison

| Cast Type | Safety | Speed | Use Case |
|-----------|--------|-------|----------|
| static_cast | Compile-time | Fast | Related types, numeric conversions |
| dynamic_cast | Runtime | Slow | Safe polymorphic downcasts |
| const_cast | None | Fast | Add/remove const qualifier |
| reinterpret_cast | None | Fast | Low-level bit manipulation |
| C-style | None | Fast | **Avoid** in modern C++ |

# Best Practices

1. **Prefer static_cast** for most conversions
2. **Use dynamic_cast** when you need runtime safety
3. **Avoid const_cast** unless interfacing with legacy code
4. **Avoid reinterpret_cast** unless doing low-level work
5. **Never use C-style casts** in modern C++
6. **Prefer implicit conversions** when safe and clear

# Examples

```cpp
// Good: Clear numeric conversion
double price = 19.99;
int dollars = static_cast<int>(price);

// Good: Safe polymorphic downcast
if (Derived* d = dynamic_cast<Derived*>(basePtr)) {
    d->derivedMethod();
}

// Acceptable: Interfacing with C API
void c_function(char* str);
void cpp_function(const std::string& str) {
    c_function(const_cast<char*>(str.c_str()));
}

// Bad: Using C-style cast
double d = 3.14;
int i = (int)d;  // Use static_cast<int>(d) instead

// Dangerous: reinterpret_cast without careful thought
int x = 42;
double* d = reinterpret_cast<double*>(&x);  // Undefined behavior!
```

# Related Topics

- [[Type Conversion]]
- [[Polymorphism]]
- [[RTTI]]
- [[const Correctness]]
