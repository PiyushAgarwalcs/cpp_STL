# 🚀 C++ & STL — Complete Syntax Reference 

> **Assumption:** No OOP. No Threads.
---

## 📋 TABLE OF CONTENTS

1. [Compilation Process](#1-compilation-process)
2. [Data Types & Limits](#2-data-types--limits)
3. [Operators](#3-operators)
4. [Conditions & Switch](#4-conditions--switch)
5. [Loops](#5-loops)
6. [Functions — Pass by Value vs Reference](#6-functions--pass-by-value-vs-reference)
7. [Pointers](#7-pointers)
8. [char Array & C-style Strings](#8-char-array--c-style-strings)
9. [std::string & Library Functions](#9-stdstring--library-functions)
10. [STL Overview](#10-stl-overview)
11. [Vector](#11-vector)
12. [Pair & Tuple](#12-pair--tuple)
13. [Stack](#13-stack)
14. [Queue](#14-queue)
15. [Deque](#15-deque)
16. [Priority Queue (Heap)](#16-priority-queue-heap)
17. [Set & Multiset](#17-set--multiset)
18. [Unordered Set & Unordered Multiset](#18-unordered-set--unordered-multiset)
19. [Map & Multimap](#19-map--multimap)
20. [Unordered Map & Unordered Multimap](#20-unordered-map--unordered-multimap)
21. [List (Doubly Linked List)](#21-list-doubly-linked-list)
22. [Forward List (Singly Linked List)](#22-forward-list-singly-linked-list)
23. [Array (std::array)](#23-array-stdarray)
24. [Iterators](#24-iterators)
25. [STL Algorithms](#25-stl-algorithms)
26. [Numeric Algorithms](#26-numeric-algorithms)
27. [Lambda Functions](#27-lambda-functions)
28. [Auto & Range-based For](#28-auto--range-based-for)
29. [Useful Macros & Tricks](#29-useful-macros--tricks)
30. [Interview Q&A — Core C++ Concepts](#30-interview-qa--core-c-concepts)

---

## 1. Compilation Process

```
Source Code (.cpp)
      │
      ▼
 ┌──────────────┐
 │ Preprocessor │  → Expands #include, #define, removes comments
 └──────────────┘    Output: .i file
      │
      ▼
 ┌──────────────┐
 │   Compiler   │  → Syntax/semantic checks, converts to Assembly
 └──────────────┘    Output: .s file
      │
      ▼
 ┌──────────────┐
 │  Assembler   │  → Converts assembly → machine code (binary)
 └──────────────┘    Output: .o / .obj file (Object file)
      │
      ▼
 ┌──────────────┐
 │    Linker    │  → Links .o files + libraries → final executable
 └──────────────┘    Output: a.out / program.exe
```

### Stage Details

| Stage | Input | Output | What Happens |
|---|---|---|---|
| **Preprocessor** | `.cpp` | `.i` | `#include` copy-pasted, macros expanded, comments removed |
| **Compiler** | `.i` | `.s` | Syntax & semantic checks, produces assembly code |
| **Assembler** | `.s` | `.o` | Assembly → binary machine code (not yet runnable) |
| **Linker** | `.o` + libs | `a.out` | Resolves external symbols, merges object files |

### g++ Commands

```bash
g++ -E main.cpp -o main.i      # Only preprocess
g++ -S main.cpp -o main.s      # Compile to assembly
g++ -c main.cpp -o main.o      # Compile + assemble (no linking)
g++ main.cpp -o main           # Full compilation
g++ -O2 main.cpp -o main       # With optimization
g++ -std=c++17 main.cpp -o main # Use C++17 standard
```

### Static vs Dynamic Linking

- **Static linking** — library code is embedded into executable at compile time (`.a` files). Larger binary, no runtime dependency.
- **Dynamic linking** — library is loaded at runtime (`.so` / `.dll`). Smaller binary, needs library installed.

---

## 2. Data Types & Limits
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/aecd2561-f559-446b-bc3f-2f5826b493e3" />

```cpp
#include <climits>     // INT_MAX, INT_MIN, etc.
#include <cfloat>      // FLT_MAX, DBL_MAX
#include <cstdint>     // int32_t, int64_t, etc.
```

### Integer Types

| Type | Size | Range | Use Case |
|---|---|---|---|
| `bool` | 1 byte | `0` / `1` | true/false flags |
| `char` | 1 byte | `-128` to `127` | single character |
| `unsigned char` | 1 byte | `0` to `255` | byte values |
| `short` | 2 bytes | `-32768` to `32767` | rarely used |
| `int` | 4 bytes | `-2,147,483,648` to `2,147,483,647` (~2×10⁹) | **default integer** |
| `unsigned int` | 4 bytes | `0` to `4,294,967,295` (~4×10⁹) | when always positive |
| `long` | 4 or 8 bytes | platform-dependent | |
| `long long` | 8 bytes | `-9.2×10¹⁸` to `9.2×10¹⁸` | **large numbers** |
| `unsigned long long` | 8 bytes | `0` to `1.8×10¹⁹` | |

### Float Types

| Type | Size | Precision | Use Case |
|---|---|---|---|
| `float` | 4 bytes | ~7 digits | avoid in CP |
| `double` | 8 bytes | ~15 digits | **default float** |
| `long double` | 16 bytes | ~18-19 digits | high precision |

### Key Constants

```cpp
INT_MAX        // 2147483647      (~2×10⁹)
INT_MIN        // -2147483648
LLONG_MAX      // 9223372036854775807   (~9.2×10¹⁸)
LLONG_MIN      // -9223372036854775808
DBL_MAX        // ~1.8×10³⁰⁸
CHAR_MAX       // 127
UINT_MAX       // 4294967295

// Practical aliases for Leetcode
#define INF INT_MAX
#define NEGINF INT_MIN
const long long INF_LL = 1e18;
```

### Type Casting

```cpp
int a = 5, b = 2;
double res = (double)a / b;        // C-style cast → 2.5
double res2 = static_cast<double>(a) / b;  // C++ style (preferred)

int x = (int)3.9;     // x = 3  (truncates, does NOT round)
int y = (int)-3.9;    // y = -3 (truncates toward zero)

// Common pitfall: overflow before cast
long long ans = (long long)a * b;  // Cast BEFORE multiply!
```

### Variable Declaration & Initialization

```cpp
int x;           // uninitialized (garbage value)
int x = 5;       // copy-init
int x(5);        // direct-init
int x{5};        // uniform-init (C++11) — prevents narrowing
auto x = 5;      // type deduced as int
auto x = 5LL;    // type deduced as long long
auto x = 5.0;    // type deduced as double
```

---

## 3. Operators

### Arithmetic Operators

```cpp
int a = 10, b = 3;

a + b   // 13  — addition
a - b   // 7   — subtraction
a * b   // 30  — multiplication
a / b   // 3   — integer division (truncates toward zero)
a % b   // 1   — modulo (remainder)

// IMPORTANT modulo rules:
-7 % 3  // -1  (sign follows dividend in C++)
7 % -3  // 1   (sign follows dividend)

// To always get positive mod:
int mod = ((a % b) + b) % b;

// Power (no operator, use function)
#include <cmath>
pow(2, 10);        // 1024.0 (returns double)
(int)pow(2, 10);   // 1024
```

### Relational Operators

```cpp
a == b   // equal
a != b   // not equal
a <  b   // less than
a >  b   // greater than
a <= b   // less than or equal
a >= b   // greater than or equal
// All return bool (true/false)
```

### Logical Operators

```cpp
&&   // AND — both true
||   // OR  — at least one true
!    // NOT — negation

// Short-circuit evaluation:
// false && expr  → expr NOT evaluated
// true  || expr  → expr NOT evaluated
```

### Bitwise Operators

```cpp
int a = 5;  // 0101
int b = 3;  // 0011

a & b    // 0001 = 1  — AND  (both bits 1)
a | b    // 0111 = 7  — OR   (at least one bit 1)
a ^ b    // 0110 = 6  — XOR  (exactly one bit 1)
~a       // 1010 = -6 — NOT  (flip all bits; ~n = -(n+1))
a << 1   // 1010 = 10 — Left shift  (multiply by 2)
a >> 1   // 0010 = 2  — Right shift (divide by 2, floor)
```

### Common Bit Tricks for LeetCode

```cpp
// Check if bit i is set
(n >> i) & 1

// Set bit i
n | (1 << i)

// Clear bit i
n & ~(1 << i)

// Toggle bit i
n ^ (1 << i)

// Check if n is power of 2
n > 0 && (n & (n-1)) == 0

// Count set bits (C++20 has popcount)
__builtin_popcount(n)      // for int
__builtin_popcountll(n)    // for long long

// Get lowest set bit
n & (-n)

// Remove lowest set bit
n & (n-1)

// Check even/odd
n & 1   // 0 = even, 1 = odd

// Swap without temp
a ^= b; b ^= a; a ^= b;

// Absolute value via bit
// (don't rely on this; use abs())

// Left shift by k = multiply by 2^k
n << k   // n * 2^k
n >> k   // n / 2^k  (floor division for non-negative)
```

### Assignment & Increment

```cpp
x += 5;   x -= 5;   x *= 5;   x /= 5;   x %= 5;
x &= b;   x |= b;   x ^= b;   x <<= 1;  x >>= 1;

int i = 5;
int a = i++;   // a = 5 (post-increment: return THEN increment)
int b = ++i;   // b = 7 (pre-increment: increment THEN return)
// After both: i = 7, a = 5, b = 7
```

### Ternary Operator

```cpp
int max_val = (a > b) ? a : b;
string s = (n % 2 == 0) ? "even" : "odd";

// Nested ternary (avoid for readability)
int sign = (n > 0) ? 1 : (n < 0) ? -1 : 0;
```

### sizeof Operator

```cpp
sizeof(int)         // 4
sizeof(long long)   // 8
sizeof(char)        // 1
int arr[5];
sizeof(arr)         // 20  (5 * 4)
sizeof(arr)/sizeof(arr[0])  // 5 — array length trick
```

---

## 4. Conditions & Switch

### if / else if / else

```cpp
if (x > 0) {
    // positive
} else if (x < 0) {
    // negative
} else {
    // zero
}

// Single-line (braces optional but DANGEROUS to omit)
if (x > 0) return true;
```

### Switch Statement — ALL CONDITIONS

```cpp
switch (expression) {
    case value1:
        // code
        break;          // MUST have break or falls through
    case value2:
        // code
        break;
    case value3:
    case value4:        // fall-through: both hit same block
        // code for value3 OR value4
        break;
    default:            // optional, like else
        // code
        break;
}
```

### ⚠️ Switch Rules & Gotchas

| Rule | Detail |
|---|---|
| Expression type | Must be **integer** type: `int`, `char`, `enum`, `bool`, `short`, `long`. NOT `float`, `double`, `string`. |
| Case values | Must be **compile-time constants** (literals or `const` / `constexpr`). NOT variables. |
| Fall-through | Without `break`, execution falls into the **next case** automatically. |
| `default` | Can appear anywhere (not just last), but conventionally at end. |
| Duplicate cases | Compilation **error** — no two cases can have same value. |
| No `break` in `default` | Legal but usually unnecessary since it's last. |
| Variables in switch | You **cannot** declare initialized variables in a `case` without wrapping in `{}`. |

```cpp
// WRONG — cannot declare initialized variable in case
switch(x) {
    case 1:
        int y = 5;   // ❌ Error (jumps over initialization)
        break;
}

// CORRECT — wrap in braces
switch(x) {
    case 1: {
        int y = 5;   // ✅ OK
        break;
    }
}

// char switch — valid
char c = 'A';
switch(c) {
    case 'A': cout << "Alpha"; break;
    case 'B': cout << "Beta";  break;
}

// enum switch
enum Color { RED, GREEN, BLUE };
Color col = GREEN;
switch(col) {
    case RED:   break;
    case GREEN: break;
    case BLUE:  break;
}
```

---

## 5. Loops

```cpp
// for loop
for (int i = 0; i < n; i++) { }
for (int i = n-1; i >= 0; i--) { }       // reverse
for (int i = 0; i < n; i += 2) { }       // step 2

// while loop
while (condition) { }

// do-while (executes at least once)
do { } while (condition);

// Range-based for (C++11)
for (auto x : arr) { }           // copy
for (auto& x : arr) { }          // reference (modify original)
for (const auto& x : arr) { }    // const reference (read-only)

// Nested loops — labeled break trick
// C++ has no labeled break; use goto or flag
bool found = false;
for (int i = 0; i < n && !found; i++)
    for (int j = 0; j < m && !found; j++)
        if (arr[i][j] == target) found = true;

// break, continue
break;      // exit innermost loop immediately
continue;   // skip rest of body, go to next iteration
```

---

## 6. Functions — Pass by Value vs Reference

### Pass by Value

```cpp
void increment(int x) {
    x++;           // modifies LOCAL copy only
}                  // original unchanged

int a = 5;
increment(a);
// a is still 5
```

### Pass by Reference

```cpp
void increment(int& x) {    // & means "reference to"
    x++;           // modifies the ORIGINAL variable
}

int a = 5;
increment(a);
// a is now 6
```

### Pass by Pointer

```cpp
void increment(int* x) {
    (*x)++;        // dereference pointer then modify
}

int a = 5;
increment(&a);     // pass ADDRESS of a
// a is now 6
```

### Comparison Table

| Feature | By Value | By Reference | By Pointer |
|---|---|---|---|
| Modifies original? | ❌ No | ✅ Yes | ✅ Yes |
| Null possible? | N/A | ❌ No | ✅ Yes |
| Syntax at call site | `f(a)` | `f(a)` | `f(&a)` |
| Syntax in function | `int x` | `int& x` | `int* x` |
| Performance | Copy made | No copy | No copy |
| Use for | Small types, no modify | Large types / modify | Optional / arrays |

### Const Reference (Best Practice for Read-Only Large Types)

```cpp
void printVector(const vector<int>& v) {
    // Can read v but cannot modify
    for (auto x : v) cout << x;
}
```

### Return by Reference

```cpp
int& getElement(vector<int>& v, int i) {
    return v[i];   // returns reference to element
}
// Now: getElement(v, 0) = 100;  modifies v[0]
```

### Default Arguments

```cpp
int add(int a, int b = 10) {   // default b = 10
    return a + b;
}
add(5);       // returns 15
add(5, 3);    // returns 8
// Default args must be at the END of parameter list
```

### Function Overloading

```cpp
int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }
int add(int a, int b, int c) { return a + b + c; }
// Compiler picks correct version based on argument types
```

### Inline Functions

```cpp
inline int square(int x) { return x * x; }
// Compiler may replace the call with actual code (no function call overhead)
// Good for tiny, frequently called functions
```

### Templates (Generic Functions)

```cpp
template<typename T>
T maxOf(T a, T b) {
    return (a > b) ? a : b;
}
maxOf(3, 5);        // int version
maxOf(3.14, 2.7);   // double version
maxOf('a', 'z');    // char version

// Multiple types
template<typename T, typename U>
auto add(T a, U b) -> decltype(a+b) {
    return a + b;
}
```

---

## 7. Pointers

```cpp
int x = 10;
int* ptr = &x;       // ptr stores address of x

*ptr          // dereference: get value = 10
ptr           // the address itself
&x            // address of x

*ptr = 20;    // x is now 20

// Pointer to pointer
int** pp = &ptr;
**pp = 30;    // x is now 30

// Null pointer
int* p = nullptr;   // C++11 (prefer over NULL)
if (p == nullptr) { }

// Dynamic memory
int* arr = new int[10];   // allocate array on heap
arr[0] = 5;
delete[] arr;              // MUST free! ([] for arrays)

int* val = new int(42);
delete val;                // for single value

// Pointer arithmetic
int arr2[5] = {1,2,3,4,5};
int* p2 = arr2;
p2++;          // points to arr2[1]
*(p2+2)        // arr2[3]
```

---

## 8. char Array & C-style Strings

```cpp
#include <cstring>    // strlen, strcpy, strcmp, etc.
#include <cctype>     // isalpha, isdigit, toupper, etc.
```

### Declaration

```cpp
char str[10] = "hello";          // automatically adds '\0'
char str[10] = {'h','e','l','l','o','\0'};  // manual
char str[] = "hello";            // size auto = 6 (5 + null)
char* s = "hello";               // pointer to string literal (read-only!)
```

### cstring Functions

| Function | Description | Example |
|---|---|---|
| `strlen(s)` | Length (excl. `\0`) | `strlen("hi")` → `2` |
| `strcpy(dst, src)` | Copy src into dst | `strcpy(a, b)` |
| `strncpy(dst, src, n)` | Copy at most n chars | safer than strcpy |
| `strcat(dst, src)` | Append src to dst | `strcat(a, b)` |
| `strncat(dst, src, n)` | Append at most n chars | safer |
| `strcmp(a, b)` | Compare: `0`=equal, `<0`=a<b, `>0`=a>b | `strcmp("ab","ab")` → `0` |
| `strncmp(a, b, n)` | Compare first n chars | |
| `strchr(s, c)` | Find first occurrence of char `c` | returns pointer or `NULL` |
| `strrchr(s, c)` | Find last occurrence of char `c` | |
| `strstr(hay, needle)` | Find substring | returns pointer or `NULL` |
| `memset(s, c, n)` | Fill n bytes with value c | `memset(arr, 0, sizeof(arr))` |
| `memcpy(dst, src, n)` | Copy n bytes | faster than strcpy for non-strings |

### cctype Character Functions

```cpp
isalpha(c)    // a-z or A-Z
isdigit(c)    // 0-9
isalnum(c)    // a-z, A-Z, or 0-9
islower(c)    // a-z
isupper(c)    // A-Z
isspace(c)    // space, tab, newline etc.
tolower(c)    // convert to lowercase (returns int, cast to char)
toupper(c)    // convert to uppercase

// Usage
char c = 'A';
char lower = (char)tolower(c);   // 'a'

// Traverse char array
char str[] = "hello";
for (int i = 0; str[i] != '\0'; i++) {
    cout << str[i];
}
```

### char ↔ int Conversions

```cpp
char c = '5';
int n = c - '0';          // digit char to int: n = 5

char digit = '0' + 5;     // int to digit char: '5'

int ascii = (int)'A';     // 65
char ch = (char)65;       // 'A'

// 'a' = 97, 'z' = 122
// 'A' = 65, 'Z' = 90
// '0' = 48, '9' = 57
int idx = c - 'a';        // 0-based index for lowercase letter
```

---

## 9. std::string & Library Functions

```cpp
#include <string>
#include <algorithm>   // for sort, reverse on strings
```

### Declaration & Initialization

```cpp
string s;               // empty string
string s = "hello";
string s("hello");
string s(5, 'a');       // "aaaaa"  — fill constructor
string s = s1 + s2;     // concatenation
string s(s1, 2, 3);     // substring: s1 starting at index 2, length 3
```

### Member Functions

| Function | Description | Example |
|---|---|---|
| `s.size()` / `s.length()` | Number of characters | `s.size()` → `5` |
| `s.empty()` | Is empty? | `if (s.empty())` |
| `s[i]` | Access char at index i (no bounds check) | `s[0]` |
| `s.at(i)` | Access with bounds check (throws exception) | `s.at(0)` |
| `s.front()` | First character | `s.front()` |
| `s.back()` | Last character | `s.back()` |
| `s.push_back(c)` | Append char | `s.push_back('!')` |
| `s.pop_back()` | Remove last char | `s.pop_back()` |
| `s.append(t)` | Append string | `s.append(" world")` |
| `s += t` | Concatenate (preferred) | `s += " world"` |
| `s.insert(i, t)` | Insert string t at position i | `s.insert(2, "XX")` |
| `s.erase(i, n)` | Remove n chars starting at i | `s.erase(2, 3)` |
| `s.replace(i, n, t)` | Replace n chars at i with t | `s.replace(0, 3, "bye")` |
| `s.substr(i, n)` | Substring from i, length n | `s.substr(2, 3)` |
| `s.substr(i)` | Substring from i to end | `s.substr(3)` |
| `s.find(t)` | First pos of t (`string::npos` if not found) | `s.find("lo")` |
| `s.rfind(t)` | Last pos of t | `s.rfind('l')` |
| `s.find(t, pos)` | Search from position pos | `s.find('l', 4)` |
| `s.compare(t)` | Like strcmp: 0=equal | `s.compare("hello")` |
| `s.clear()` | Empties the string | `s.clear()` |
| `s.resize(n)` | Resize to n chars | `s.resize(10)` |
| `s.resize(n, c)` | Resize, fill with char c | `s.resize(10, '0')` |
| `s.c_str()` | Convert to C-style `const char*` | `s.c_str()` |
| `s.begin()` / `s.end()` | Iterators | for algorithms |
| `s.rbegin()` / `s.rend()` | Reverse iterators | |

```cpp
// find returns string::npos (-1 cast to size_t) if not found
if (s.find("abc") != string::npos) {
    cout << "found";
}

// String comparison (lexicographic)
s1 == s2    // equal
s1 < s2     // lexicographically smaller
s1 > s2     // lexicographically greater

// Sort a string
sort(s.begin(), s.end());

// Reverse a string
reverse(s.begin(), s.end());

// Convert to int / long long
int n = stoi("123");
long long n = stoll("1234567890");
double d = stod("3.14");

// Convert int to string
string s = to_string(42);
string s = to_string(3.14);

// String stream (for parsing)
#include <sstream>
stringstream ss("hello world 123");
string word;
while (ss >> word) {
    cout << word << "\n";   // prints each token
}

// Build a string via stringstream
stringstream ss2;
ss2 << "Value: " << 42;
string result = ss2.str();

// Split string by delimiter
string line = "a,b,c";
stringstream ss(line);
string token;
while (getline(ss, token, ',')) {
    cout << token << "\n";  // a, b, c
}
```

---

## 10. STL Overview

The Standard Template Library has **4 components**:

```
STL
├── Containers       — data structures (vector, map, set, etc.)
├── Algorithms       — sort, search, transform, etc.
├── Iterators        — pointers into containers
└── Functors         — objects that act like functions
```

### Container Categories

| Category | Containers | Underlying | Ordered? |
|---|---|---|---|
| **Sequence** | `vector`, `deque`, `list`, `forward_list`, `array` | varies | by insertion |
| **Associative** | `set`, `multiset`, `map`, `multimap` | Red-Black Tree | sorted by key |
| **Unordered Assoc.** | `unordered_set`, `unordered_multiset`, `unordered_map`, `unordered_multimap` | Hash Table | no order |
| **Adapters** | `stack`, `queue`, `priority_queue` | wraps deque/vector | — |

<img width="1440" height="2096" alt="image" src="https://github.com/user-attachments/assets/46b7d113-2b08-4bfd-9d09-604fe7e289a6" />

---

## 11. Vector

```cpp
#include <vector>
```

**Internally:** Dynamic array. Doubles capacity when full. Elements stored contiguously.

### Declaration

```cpp
vector<int> v;                    // empty
vector<int> v(5);                 // size 5, all zeros
vector<int> v(5, 10);             // size 5, all 10
vector<int> v = {1, 2, 3, 4, 5};
vector<int> v(another_v);         // copy
vector<int> v(arr, arr + n);      // from C array

vector<vector<int>> mat(n, vector<int>(m, 0));  // 2D, n×m, all 0
```

### Member Functions

| Function | Description | Complexity |
|---|---|---|
| `v.push_back(x)` | Add x at end | O(1) amortized |
| `v.pop_back()` | Remove last element | O(1) |
| `v.size()` | Number of elements | O(1) |
| `v.capacity()` | Allocated capacity | O(1) |
| `v.empty()` | Is empty? | O(1) |
| `v.front()` | First element | O(1) |
| `v.back()` | Last element | O(1) |
| `v[i]` | Access element (no bounds check) | O(1) |
| `v.at(i)` | Access with bounds check | O(1) |
| `v.insert(it, x)` | Insert x before iterator | O(n) |
| `v.insert(it, n, x)` | Insert n copies of x | O(n) |
| `v.erase(it)` | Remove element at iterator | O(n) |
| `v.erase(it1, it2)` | Remove range `[it1, it2)` | O(n) |
| `v.clear()` | Remove all elements | O(n) |
| `v.resize(n)` | Resize to n | O(n) |
| `v.resize(n, x)` | Resize to n, fill new with x | O(n) |
| `v.reserve(n)` | Pre-allocate capacity (no resize) | O(n) |
| `v.shrink_to_fit()` | Reduce capacity to size | O(n) |
| `v.assign(n, x)` | Assign n copies of x | O(n) |
| `v.swap(v2)` | Swap two vectors | O(1) |
| `v.begin()` / `v.end()` | Iterators | O(1) |
| `v.rbegin()` / `v.rend()` | Reverse iterators | O(1) |
| `v.data()` | Pointer to underlying array | O(1) |
| `v.emplace_back(x)` | Construct in-place at end | O(1) amortized |

```cpp
// Common patterns
vector<int> v = {3, 1, 4, 1, 5};
sort(v.begin(), v.end());                         // sort ascending
sort(v.begin(), v.end(), greater<int>());          // sort descending

// Remove element at index i (order not preserved — fast)
v[i] = v.back();
v.pop_back();

// Remove element at index i (order preserved — slow)
v.erase(v.begin() + i);

// Erase-remove idiom (remove all equal to val)
v.erase(remove(v.begin(), v.end(), val), v.end());

// 2D vector access
mat[row][col]

// Fill vector
fill(v.begin(), v.end(), 0);
```

---

## 12. Pair & Tuple

```cpp
#include <utility>   // pair
#include <tuple>     // tuple
```

### pair

```cpp
pair<int, int> p = {3, 5};
pair<int, int> p = make_pair(3, 5);

p.first     // 3
p.second    // 5

// Comparison: compares first, then second
pair<int,int> a = {1, 2}, b = {1, 3};
a < b    // true (same first, 2 < 3)

// Vector of pairs
vector<pair<int,int>> vp;
vp.push_back({1, 2});
vp.emplace_back(3, 4);
sort(vp.begin(), vp.end());    // sorts by first, then second
```

### tuple

```cpp
tuple<int, string, double> t = {1, "hello", 3.14};
tuple<int, string, double> t = make_tuple(1, "hello", 3.14);

get<0>(t)   // 1
get<1>(t)   // "hello"
get<2>(t)   // 3.14

// C++17 structured bindings (very useful!)
auto [a, b, c] = t;     // a=1, b="hello", c=3.14

auto [x, y] = make_pair(3, 5);   // works on pair too

// Tie (unpack)
int a; string b; double c;
tie(a, b, c) = t;
```

---

## 13. Stack

```cpp
#include <stack>
```

**LIFO** — Last In First Out. Implemented over deque by default.

| Function | Description | Complexity |
|---|---|---|
| `s.push(x)` | Push x on top | O(1) |
| `s.pop()` | Remove top (returns void!) | O(1) |
| `s.top()` | Access top element | O(1) |
| `s.empty()` | Is empty? | O(1) |
| `s.size()` | Number of elements | O(1) |
| `s.emplace(x)` | Construct and push | O(1) |
| `s.swap(s2)` | Swap two stacks | O(1) |

```cpp
stack<int> st;
st.push(1); st.push(2); st.push(3);

st.top();   // 3
st.pop();   // removes 3
st.top();   // 2

// Safe pop pattern
while (!st.empty()) {
    int val = st.top();
    st.pop();
    // process val
}

// Stack with pair
stack<pair<int,int>> st;
st.push({1, 2});
auto [x, y] = st.top();
```

---

## 14. Queue

```cpp
#include <queue>
```

**FIFO** — First In First Out. Implemented over deque.

| Function | Description | Complexity |
|---|---|---|
| `q.push(x)` | Enqueue at back | O(1) |
| `q.pop()` | Dequeue from front (returns void!) | O(1) |
| `q.front()` | Access front element | O(1) |
| `q.back()` | Access back element | O(1) |
| `q.empty()` | Is empty? | O(1) |
| `q.size()` | Number of elements | O(1) |
| `q.emplace(x)` | Construct and push | O(1) |

```cpp
queue<int> q;
q.push(1); q.push(2); q.push(3);

q.front()   // 1
q.back()    // 3
q.pop();    // removes 1
q.front()   // 2

// BFS pattern
queue<int> bfs;
bfs.push(start);
while (!bfs.empty()) {
    int node = bfs.front();
    bfs.pop();
    // process node, push neighbors
}
```

---

## 15. Deque

```cpp
#include <deque>
```

**Double-ended queue** — O(1) insert/delete at both front AND back.

| Function | Description | Complexity |
|---|---|---|
| `dq.push_back(x)` | Add at back | O(1) |
| `dq.push_front(x)` | Add at front | O(1) |
| `dq.pop_back()` | Remove from back | O(1) |
| `dq.pop_front()` | Remove from front | O(1) |
| `dq.front()` | Access front | O(1) |
| `dq.back()` | Access back | O(1) |
| `dq[i]` / `dq.at(i)` | Random access | O(1) |
| `dq.size()` | Number of elements | O(1) |
| `dq.empty()` | Is empty? | O(1) |
| `dq.insert(it, x)` | Insert at iterator | O(n) |
| `dq.erase(it)` | Remove at iterator | O(n) |
| `dq.clear()` | Remove all | O(n) |
| `dq.resize(n)` | Resize | O(n) |
| `dq.begin()` / `dq.end()` | Iterators | O(1) |

```cpp
deque<int> dq;
dq.push_back(3);
dq.push_front(1);
dq.push_back(4);
// dq: [1, 3, 4]

// Sliding window maximum: use deque of indices
// Store indices in decreasing order of values
```

---

## 16. Priority Queue (Heap)

```cpp
#include <queue>
```

**MAX HEAP by default** — `top()` gives largest element. Internally a Binary Heap.

### Declaration

```cpp
// MAX HEAP (default) — top() = largest
priority_queue<int> maxpq;

// MIN HEAP — top() = smallest
priority_queue<int, vector<int>, greater<int>> minpq;

// PAIRS — max heap on first element
priority_queue<pair<int,int>> pqpair;

// PAIRS — min heap on first element
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> minPairPQ;
```

### Member Functions

| Function | Description | Complexity |
|---|---|---|
| `pq.push(x)` | Insert element | O(log n) |
| `pq.emplace(x)` | Construct and insert | O(log n) |
| `pq.pop()` | Remove top element | O(log n) |
| `pq.top()` | Access top (max or min) | O(1) |
| `pq.empty()` | Is empty? | O(1) |
| `pq.size()` | Number of elements | O(1) |

### Custom Comparator

```cpp
// Custom struct comparator
struct Comp {
    bool operator()(pair<int,int> a, pair<int,int> b) {
        // Returns true = a has LOWER priority than b
        if (a.first == b.first)
            return a.second > b.second;   // smaller second = higher priority
        return a.first > b.first;          // smaller first = higher priority
    }
};
priority_queue<pair<int,int>, vector<pair<int,int>>, Comp> customPQ;

// Lambda comparator (C++11, needs decltype)
auto cmp = [](int a, int b) { return a > b; };   // min-heap behavior
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
```

### ⚠️ Key Rules

- `pop()` returns **void** — must call `top()` then `pop()` separately.
- `operator()` returning `true` means **lower priority** (same as `<` comparator logic — inverted from sort!).
- Built on a **binary heap**, not BST.

### When to Use

```
✔ Get largest/smallest element fast
✔ Dijkstra's algorithm (min heap on {dist, node})
✔ K largest/smallest elements
✔ Huffman coding
✔ Merge K sorted arrays
✔ Median in a stream (two heaps trick)
✔ Any greedy with priority
```

```cpp
// Pattern: Top K Frequent Elements
// Use min-heap of size K to keep largest K

// Pattern: Dijkstra
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
pq.push({0, src});   // {dist, node}
while (!pq.empty()) {
    auto [d, u] = pq.top(); pq.pop();
    for (auto [w, v] : adj[u])
        if (dist[u] + w < dist[v]) {
            dist[v] = dist[u] + w;
            pq.push({dist[v], v});
        }
}
```

---

## 17. Set & Multiset

```cpp
#include <set>
```

**Underlying:** Red-Black Tree (self-balancing BST). Elements are always **sorted**.

### set (unique elements)

| Function | Description | Complexity |
|---|---|---|
| `s.insert(x)` | Insert x | O(log n) |
| `s.erase(x)` | Remove element equal to x | O(log n) |
| `s.erase(it)` | Remove element at iterator | O(log n) |
| `s.find(x)` | Returns iterator (or `s.end()` if not found) | O(log n) |
| `s.count(x)` | 0 or 1 (unique) | O(log n) |
| `s.contains(x)` | bool (C++20) | O(log n) |
| `s.lower_bound(x)` | Iterator to first element **≥ x** | O(log n) |
| `s.upper_bound(x)` | Iterator to first element **> x** | O(log n) |
| `s.size()` | Number of elements | O(1) |
| `s.empty()` | Is empty? | O(1) |
| `s.clear()` | Remove all | O(n) |
| `s.begin()` / `s.end()` | Iterators (ascending) | O(1) |
| `s.rbegin()` / `s.rend()` | Reverse iterators (descending) | O(1) |

```cpp
set<int> s = {3, 1, 4, 1, 5, 9};
// s contains: {1, 3, 4, 5, 9}  (sorted, no duplicates)

s.insert(2);
s.erase(4);

if (s.find(3) != s.end()) { /* found */ }
if (s.count(3)) { /* found */ }

// Smallest element ≥ x
auto it = s.lower_bound(4);
if (it != s.end()) cout << *it;

// Largest element ≤ x
auto it = s.upper_bound(4);
if (it != s.begin()) {
    --it;
    cout << *it;   // largest element ≤ 4
}

// Iteration (sorted order)
for (auto x : s) cout << x << " ";

// Descending
for (auto it = s.rbegin(); it != s.rend(); ++it) cout << *it;
```

### multiset (allows duplicates)

```cpp
multiset<int> ms;
ms.insert(3); ms.insert(3); ms.insert(1);
// ms: {1, 3, 3}

ms.count(3)       // 2
ms.erase(ms.find(3))   // removes ONE 3 (use find + erase iterator!)
ms.erase(3)            // removes ALL 3s  ⚠️ CAREFUL

// All other functions same as set
```

---

## 18. Unordered Set & Unordered Multiset

```cpp
#include <unordered_set>
```

**Underlying:** Hash Table. **No ordering**. Average O(1) operations.

| Function | Complexity |
|---|---|
| `insert`, `erase`, `find`, `count` | O(1) avg, O(n) worst |
| `size`, `empty`, `clear` | O(1) |

```cpp
unordered_set<int> us;
us.insert(3); us.insert(1); us.insert(4);

us.find(3) != us.end()   // found
us.count(3)               // 1

// Custom hash for pairs (needed!)
struct PairHash {
    size_t operator()(const pair<int,int>& p) const {
        return hash<long long>()(((long long)p.first << 32) | (unsigned)p.second);
    }
};
unordered_set<pair<int,int>, PairHash> us2;

// When to use vs set
// unordered_set: faster average, no order needed
// set: need sorted order or lower/upper bound
```

---

## 19. Map & Multimap

```cpp
#include <map>
```

**Underlying:** Red-Black Tree. Keys are always **sorted**.

### map (unique keys)

| Function | Description | Complexity |
|---|---|---|
| `m[key]` | Access/insert with default value | O(log n) |
| `m.at(key)` | Access, throws if key absent | O(log n) |
| `m.insert({key, val})` | Insert pair | O(log n) |
| `m.insert_or_assign(key, val)` | Insert or update | O(log n) |
| `m.emplace(key, val)` | Construct in-place | O(log n) |
| `m.erase(key)` | Remove by key | O(log n) |
| `m.erase(it)` | Remove by iterator | O(log n) |
| `m.find(key)` | Returns iterator or `m.end()` | O(log n) |
| `m.count(key)` | 0 or 1 | O(log n) |
| `m.contains(key)` | bool (C++20) | O(log n) |
| `m.lower_bound(key)` | Iterator to first key ≥ key | O(log n) |
| `m.upper_bound(key)` | Iterator to first key > key | O(log n) |
| `m.size()` | Number of pairs | O(1) |
| `m.empty()` | Is empty? | O(1) |
| `m.clear()` | Remove all | O(n) |
| `m.begin()` / `m.end()` | Iterators (sorted by key) | O(1) |

```cpp
map<string, int> freq;
freq["apple"]++;         // inserts "apple":0, then increments → 1
freq["banana"] = 5;
freq["apple"] += 3;

// Access — use find to avoid inserting default:
if (freq.find("apple") != freq.end())
    cout << freq["apple"];

// Iterate (sorted by key)
for (auto& [key, val] : freq) {
    cout << key << ": " << val << "\n";
}

// Map with pair key
map<pair<int,int>, int> grid;
grid[{0, 0}] = 1;

// Sorted by key — useful for finding prev/next key
auto it = m.lower_bound(5);  // first key ≥ 5
if (it != m.begin()) { --it; /* largest key < 5 */ }
```

### multimap (duplicate keys)

```cpp
multimap<int, string> mm;
mm.insert({1, "one"});
mm.insert({1, "uno"});

mm.count(1)   // 2

// Get range of equal keys
auto range = mm.equal_range(1);
for (auto it = range.first; it != range.second; ++it)
    cout << it->second;
```

---

## 20. Unordered Map & Unordered Multimap

```cpp
#include <unordered_map>
```

**Underlying:** Hash Table. No ordering. O(1) average.

| Function | Complexity |
|---|---|
| `m[key]`, `m.at(key)` | O(1) avg |
| `m.insert`, `m.erase`, `m.find`, `m.count` | O(1) avg |
| `m.size`, `m.empty`, `m.clear` | O(1) |

```cpp
unordered_map<string, int> freq;
freq["cat"]++;
freq["dog"] = 3;

// Check existence without inserting
if (freq.count("cat")) cout << freq["cat"];
if (freq.find("cat") != freq.end()) cout << freq["cat"];

// Iterate (no guaranteed order)
for (auto& [k, v] : freq) {
    cout << k << ": " << v << "\n";
}

// Reserve to avoid rehashing (performance optimization)
freq.reserve(1000);
freq.max_load_factor(0.25);

// ⚠️ m[key] INSERTS key with default 0 if not present!
// Use find() or count() to check before accessing.
```

### unordered_multimap

```cpp
unordered_multimap<int, int> umm;
umm.insert({1, 10});
umm.insert({1, 20});
auto range = umm.equal_range(1);
for (auto it = range.first; it != range.second; ++it)
    cout << it->second;  // 10, 20
```

---

## 21. List (Doubly Linked List)

```cpp
#include <list>
```

O(1) insert/erase anywhere given an iterator. No random access.

| Function | Description | Complexity |
|---|---|---|
| `l.push_back(x)` | Add at back | O(1) |
| `l.push_front(x)` | Add at front | O(1) |
| `l.pop_back()` | Remove from back | O(1) |
| `l.pop_front()` | Remove from front | O(1) |
| `l.insert(it, x)` | Insert before iterator | O(1) |
| `l.erase(it)` | Remove at iterator | O(1) |
| `l.front()` / `l.back()` | Access ends | O(1) |
| `l.size()` / `l.empty()` | Count / empty | O(1) |
| `l.clear()` | Remove all | O(n) |
| `l.sort()` | Sort | O(n log n) |
| `l.reverse()` | Reverse | O(n) |
| `l.unique()` | Remove consecutive duplicates | O(n) |
| `l.remove(x)` | Remove all equal to x | O(n) |
| `l.splice(it, l2)` | Move elements from l2 | O(1) |
| `l.merge(l2)` | Merge sorted lists | O(n) |

```cpp
list<int> l = {1, 2, 3, 4, 5};
auto it = l.begin();
advance(it, 2);        // move iterator forward 2
l.insert(it, 99);      // {1, 2, 99, 3, 4, 5}
l.erase(it);           // removes element at it

// LRU Cache pattern: unordered_map + list
```

---

## 22. Forward List (Singly Linked List)

```cpp
#include <forward_list>
```

Only forward iteration. More memory-efficient than list.

```cpp
forward_list<int> fl = {1, 2, 3};
fl.push_front(0);
fl.pop_front();
fl.insert_after(fl.begin(), 99);   // insert after iterator
fl.erase_after(fl.begin());        // erase after iterator
fl.front();                        // first element (no back()!)
```

---

## 23. Array (std::array)

```cpp
#include <array>
```

Fixed-size, stack-allocated. Safer wrapper around C arrays.

```cpp
array<int, 5> a = {1, 2, 3, 4, 5};
array<int, 5> a;      // uninitialized
a.fill(0);            // fill all with 0

a[i]           // access
a.at(i)        // access with bounds check
a.front()      // first
a.back()       // last
a.size()       // 5 (compile-time constant)
a.empty()      // false
a.data()       // pointer to underlying array
a.begin() / a.end()
sort(a.begin(), a.end());
```

---

## 24. Iterators

Iterators are pointer-like objects that allow traversal of containers.

### Iterator Types

| Type | Supports | Containers |
|---|---|---|
| **Input** | Read, `++` | istream |
| **Output** | Write, `++` | ostream |
| **Forward** | Read/Write, `++` | `forward_list`, `unordered_*` |
| **Bidirectional** | Read/Write, `++`, `--` | `list`, `set`, `map` |
| **Random Access** | All + `[]`, `+n`, `-n`, `<` | `vector`, `deque`, `array` |

### Usage

```cpp
vector<int> v = {1, 2, 3, 4, 5};

// Basic iteration
for (auto it = v.begin(); it != v.end(); ++it) {
    cout << *it;     // dereference
}

// Reverse iteration
for (auto it = v.rbegin(); it != v.rend(); ++it) {
    cout << *it;
}

// Const iterators
for (auto it = v.cbegin(); it != v.cend(); ++it) { }

// Advance iterator by n
auto it = v.begin();
advance(it, 3);       // now points to index 3
next(it, 2);          // returns iterator 2 positions ahead (doesn't modify it)
prev(it, 1);          // returns iterator 1 position behind

// Distance between iterators
distance(v.begin(), v.end())   // = 5

// Insert/output iterators
back_inserter(v)       // insert at back
front_inserter(lst)    // insert at front (list only)
inserter(s, s.begin()) // insert anywhere
```

### Pre vs Post Increment

```cpp
++it    // pre-increment: faster (no copy made) — PREFER THIS
it++    // post-increment: makes a copy, then increments
```

---

## 25. STL Algorithms

```cpp
#include <algorithm>
```

### Sorting

```cpp
sort(v.begin(), v.end());                         // ascending O(n log n)
sort(v.begin(), v.end(), greater<int>());          // descending
sort(v.begin(), v.end(), [](int a, int b){         // custom
    return a > b;
});

stable_sort(v.begin(), v.end());   // preserves relative order of equals

// Sort array
sort(arr, arr + n);

// Partial sort — first k elements sorted
partial_sort(v.begin(), v.begin() + k, v.end());

// nth_element — O(n) average: element at v[k] is what would be there after sort
nth_element(v.begin(), v.begin() + k, v.end());
```

### Searching

```cpp
// Linear search
find(v.begin(), v.end(), 5);         // returns iterator
find_if(v.begin(), v.end(), [](int x){ return x > 3; });

// Binary search (requires sorted!)
binary_search(v.begin(), v.end(), 5);  // returns bool
lower_bound(v.begin(), v.end(), 5);    // iterator to first ≥ 5
upper_bound(v.begin(), v.end(), 5);    // iterator to first > 5
equal_range(v.begin(), v.end(), 5);    // pair of (lower, upper) bound
```

### Min / Max

```cpp
min(a, b)             // minimum of two values
max(a, b)
min({a, b, c, d})     // C++11: initializer list
max({a, b, c, d})
min_element(v.begin(), v.end())    // iterator to min
max_element(v.begin(), v.end())    // iterator to max
*min_element(v.begin(), v.end())   // dereference for value
minmax(a, b)          // returns pair {min, max}
minmax_element(v.begin(), v.end()) // pair of iterators
clamp(val, lo, hi)    // C++17: clamp val in [lo, hi]
```

### Reverse & Rotate

```cpp
reverse(v.begin(), v.end());
rotate(v.begin(), v.begin() + k, v.end());  // rotate left by k
```

### Copy, Fill, Transform

```cpp
copy(src.begin(), src.end(), dst.begin());
copy_if(src.begin(), src.end(), back_inserter(dst), pred);
fill(v.begin(), v.end(), 0);
fill_n(v.begin(), n, 0);    // fill first n elements
iota(v.begin(), v.end(), 1);  // fill with 1, 2, 3, ... (C++11)

transform(v.begin(), v.end(), result.begin(), [](int x){ return x*2; });
transform(v1.begin(), v1.end(), v2.begin(), result.begin(),
          [](int a, int b){ return a + b; });  // binary transform
```

### Remove & Unique

```cpp
remove(v.begin(), v.end(), val);       // moves non-val elements forward
// Use erase-remove idiom:
v.erase(remove(v.begin(), v.end(), val), v.end());

remove_if(v.begin(), v.end(), pred);   // remove matching predicate

unique(v.begin(), v.end());            // remove consecutive duplicates
v.erase(unique(v.begin(), v.end()), v.end());  // full dedup after sort
```

### Count & Check

```cpp
count(v.begin(), v.end(), val)
count_if(v.begin(), v.end(), pred)
all_of(v.begin(), v.end(), pred)      // all match?
any_of(v.begin(), v.end(), pred)      // any match?
none_of(v.begin(), v.end(), pred)     // none match?
```

### Permutations & Combinations

```cpp
next_permutation(v.begin(), v.end());  // next lexicographic permutation, returns bool
prev_permutation(v.begin(), v.end());

// Generate all permutations:
sort(v.begin(), v.end());
do {
    // process permutation
} while (next_permutation(v.begin(), v.end()));
```

### Merge & Set Operations (on sorted ranges)

```cpp
merge(a.begin(), a.end(), b.begin(), b.end(), result.begin());

set_union(a.begin(), a.end(), b.begin(), b.end(), back_inserter(res));
set_intersection(a.begin(), a.end(), b.begin(), b.end(), back_inserter(res));
set_difference(a.begin(), a.end(), b.begin(), b.end(), back_inserter(res));
```

### Heap Operations (on vector)

```cpp
make_heap(v.begin(), v.end());         // make max heap
push_heap(v.begin(), v.end());         // add last element to heap
pop_heap(v.begin(), v.end());          // move max to end
sort_heap(v.begin(), v.end());         // heap sort
is_heap(v.begin(), v.end());
```

---

## 26. Numeric Algorithms

```cpp
#include <numeric>
```

| Function | Description | Example |
|---|---|---|
| `accumulate(b, e, init)` | Sum of range | `accumulate(v.begin(), v.end(), 0)` |
| `accumulate(b, e, init, op)` | Fold with custom op | `accumulate(v.begin(), v.end(), 1, multiplies<int>())` |
| `inner_product(b1, e1, b2, init)` | Dot product | |
| `partial_sum(b, e, out)` | Prefix sums | |
| `adjacent_difference(b, e, out)` | Differences | |
| `iota(b, e, start)` | Fill with sequence | `iota(v.begin(), v.end(), 0)` |
| `gcd(a, b)` | GCD (C++17) | `gcd(12, 8)` → `4` |
| `lcm(a, b)` | LCM (C++17) | `lcm(4, 6)` → `12` |
| `__gcd(a, b)` | GCD (older) | works before C++17 |

```cpp
// Prefix sum array
vector<int> prefix(n+1, 0);
for (int i = 0; i < n; i++)
    prefix[i+1] = prefix[i] + arr[i];
// Sum of range [l, r] = prefix[r+1] - prefix[l]
```

---

## 27. Lambda Functions

```cpp
// Basic syntax
auto f = [](int x) { return x * 2; };
f(5);   // 10

// With capture
int mult = 3;
auto g = [mult](int x) { return x * mult; };   // capture by value

auto h = [&mult](int x) { mult++; return x; }; // capture by reference

auto all = [=]() { };    // capture all by value
auto allref = [&]() { }; // capture all by reference

// Return type (when needed)
auto sq = [](double x) -> double { return x * x; };

// As comparator
sort(v.begin(), v.end(), [](int a, int b){
    return a > b;   // descending
});

// With multiple parameters
auto add = [](int a, int b) { return a + b; };

// In STL algorithms
count_if(v.begin(), v.end(), [](int x){ return x % 2 == 0; });
transform(v.begin(), v.end(), v.begin(), [](int x){ return x * x; });
```

---

## 28. Auto & Range-based For

```cpp
// auto deduction
auto x = 5;             // int
auto x = 5LL;           // long long
auto x = 5.0;           // double
auto x = "hello";       // const char*
auto x = string("hi");  // string

// auto with containers
vector<int> v;
auto it = v.begin();    // vector<int>::iterator

// Range-based for
for (auto x : v) { }          // copy of each element
for (auto& x : v) { }         // reference — can modify
for (const auto& x : v) { }   // const ref — read-only, efficient

// Structured bindings (C++17)
map<string, int> m;
for (auto& [key, val] : m) {
    cout << key << " " << val;
}

vector<pair<int,int>> pairs;
for (auto [a, b] : pairs) { }

// auto return type (C++14)
auto add(int a, int b) { return a + b; }
```

---

## 29. Useful Macros & Tricks

```cpp
// Essential headers
#include <bits/stdc++.h>  // includes everything (competitive programming only)

// Fast I/O
ios_base::sync_with_stdio(false);
cin.tie(NULL);
// Put these at top of main() for faster input

// Useful defines
#define ll long long
#define ull unsigned long long
#define vi vector<int>
#define vll vector<long long>
#define pii pair<int,int>
#define pll pair<ll,ll>
#define pb push_back
#define mp make_pair
#define all(x) (x).begin(), (x).end()
#define rall(x) (x).rbegin(), (x).rend()
#define sz(x) (int)(x).size()
#define fi first
#define se second
#define INF INT_MAX
#define LINF LLONG_MAX
#define MOD 1e9+7

// Usage:
vi v = {1,2,3};
sort(all(v));
sort(rall(v));

// Integer overflow check
// Use (ll) before multiply
ll ans = (ll)a * b;

// Ceiling division
int ceil_div = (a + b - 1) / b;   // ceiling of a/b

// GCD
int g = __gcd(a, b);

// Swap
swap(a, b);   // O(1)

// Min/Max of multiple
int res = min({a, b, c, d});

// Reading line with spaces
string line;
getline(cin, line);

// Print precision
cout << fixed << setprecision(6) << 3.14159;

// Bit operations on numbers
int msb = 31 - __builtin_clz(n);   // position of most significant bit
int lsb = __builtin_ctz(n);         // position of least significant bit
int bits = __builtin_popcount(n);   // count of set bits
```

---

## 30. Interview Q&A — Core C++ Concepts

### Q1: What is the difference between `++i` and `i++`?

**`++i` (pre-increment):** increments first, then returns the new value.
**`i++` (post-increment):** returns the current value first, then increments.

```cpp
int i = 5;
int a = i++;  // a = 5, i = 6  (old value returned)
int b = ++i;  // b = 7, i = 7  (new value returned)
```

Pre-increment is **faster** for iterators because post-increment must make a copy.

---

### Q2: What is `nullptr` vs `NULL` vs `0`?

| | Type | Safe? |
|---|---|---|
| `0` | `int` | Can be misinterpreted |
| `NULL` | `int` (macro = 0) | Can be misinterpreted |
| `nullptr` | `std::nullptr_t` | ✅ Type-safe, preferred (C++11) |

```cpp
// Always use nullptr in modern C++
int* p = nullptr;
if (p == nullptr) { }
```

---

### Q3: `size_t` — What is it and why does `--sz >= 0` loop forever?

`size_t` is an **unsigned** type. It can never be negative. So `--sz >= 0` is always true, causing infinite loop.

```cpp
// BUG
size_t sz = 5;
while (--sz >= 0) { /* infinite! */ }

// FIX: use int, or check differently
for (int i = (int)v.size() - 1; i >= 0; i--) { }
```

---

### Q4: What is the difference between `vector::size()` and `vector::capacity()`?

- `size()` — number of elements currently in the vector.
- `capacity()` — total allocated memory (can hold this many before reallocation).

```cpp
vector<int> v;
v.reserve(100);   // capacity = 100, size = 0
v.push_back(1);   // capacity = 100, size = 1
```

---

### Q5: Why use `emplace_back` over `push_back`?

`push_back` creates an object then copies/moves it. `emplace_back` constructs directly in-place — avoids extra copy/move.

```cpp
vector<pair<int,int>> v;
v.push_back(make_pair(1, 2));    // creates temp pair then moves
v.emplace_back(1, 2);            // constructs pair in-place directly
```

---

### Q6: When should I use `map` vs `unordered_map`?

| | `map` | `unordered_map` |
|---|---|---|
| Order | Sorted by key | No order |
| Lookup | O(log n) | O(1) avg |
| Worst case | O(log n) | O(n) |
| Memory | Less | More |
| Use when | Need sorted iteration, `lower_bound` | Just need fast lookup |

---

### Q7: What happens when you access `map[key]` for a non-existent key?

It **inserts** the key with a **default value** (0 for int, "" for string, etc.).

```cpp
map<string, int> m;
cout << m["missing"];  // inserts "missing":0, then prints 0
// m now has "missing" in it!

// Safe check without inserting:
if (m.count("missing")) { /* exists */ }
if (m.find("missing") != m.end()) { /* exists */ }
```

---

### Q8: How does the `erase-remove` idiom work?

```cpp
vector<int> v = {1, 2, 3, 2, 4};
v.erase(remove(v.begin(), v.end(), 2), v.end());
// remove() moves non-2 elements to front, returns iterator to "new end"
// erase() then removes from new end to actual end
// Result: {1, 3, 4}
```

---

### Q9: What is `string::npos`?

It's the "not found" sentinel returned by `string::find()`. Its value is `(size_t)-1` = the maximum value of `size_t`.

```cpp
string s = "hello";
if (s.find("xyz") == string::npos)
    cout << "not found";
```

---

### Q10: `lower_bound` vs `upper_bound`?

```cpp
// On sorted vector {1, 2, 2, 3, 4}
//                              ↑ index 3
lower_bound(v.begin(), v.end(), 2);  // → iterator to FIRST  2 (index 1)
upper_bound(v.begin(), v.end(), 2);  // → iterator to AFTER last 2 (index 3)

// Count occurrences of val:
int cnt = upper_bound(v.begin(), v.end(), val) - lower_bound(v.begin(), v.end(), val);
```

---

### Q11: What is the difference between `set::erase(val)` and `multiset::erase(val)`?

```cpp
multiset<int> ms = {1, 2, 2, 2, 3};
ms.erase(2);                    // removes ALL 2s → {1, 3}
ms.erase(ms.find(2));           // removes ONLY ONE 2 → {1, 2, 2, 3}
```

---

### Q12: What is `static` keyword for variables?

For **local variables:** initialized only once, retains value between calls.
For **global variables:** limits scope to that translation unit.

```cpp
void counter() {
    static int count = 0;   // initialized only ONCE
    count++;
    cout << count;
}
counter();  // 1
counter();  // 2
counter();  // 3
```

---

### Q13: What is the `const` keyword?

```cpp
const int x = 5;             // x cannot be modified
const int* p = &x;           // pointer to const int (value can't change)
int* const p = &x;           // const pointer (address can't change)
const int* const p = &x;     // both const

void print(const string& s);  // function cannot modify s
```

---

### Q14: `struct` in C++ for Competitive Programming

```cpp
struct Node {
    int val, idx;
    // Custom comparison for sort
    bool operator<(const Node& o) const {
        return val < o.val;
    }
};

// Usage
vector<Node> nodes;
nodes.push_back({5, 0});
sort(nodes.begin(), nodes.end());   // uses operator<
```

---

### Q15: Common C++ Gotchas in LeetCode

```cpp
// 1. Integer overflow in intermediate calculation
int a = 100000, b = 100000;
long long ans = (long long)a * b;   // cast BEFORE multiply

// 2. Signed/unsigned comparison warning
for (int i = 0; i < (int)v.size(); i++) { }  // cast size() to int

// 3. Modifying container while iterating — undefined behavior
// Use iterator erase carefully:
for (auto it = v.begin(); it != v.end(); ) {
    if (*it == val) it = v.erase(it);   // erase returns next valid iterator
    else ++it;
}

// 4. map[] vs map.find()
// m[key] inserts default value if key absent — use find() to check

// 5. Empty container — calling top()/front()/back() on empty → undefined
if (!pq.empty()) pq.top();

// 6. Infinite loop with unsigned
for (int i = n-1; i >= 0; i--)   // OK if i is int, not size_t

// 7. String substr bounds
s.substr(pos, len)   // pos can be s.size() (returns ""), but NOT > s.size()

// 8. Vector of vectors reuse — clear vs new
v.clear();    // size = 0, capacity retained (faster reuse)
v = {};       // assigns new empty vector
```

---

## Quick Reference: Complexity Summary

| Container | Insert | Delete | Search | Access | Notes |
|---|---|---|---|---|---|
| `vector` | O(1) back / O(n) mid | O(1) back / O(n) mid | O(n) | O(1) | Cache-friendly |
| `deque` | O(1) ends / O(n) mid | O(1) ends / O(n) mid | O(n) | O(1) | |
| `list` | O(1) given iter | O(1) given iter | O(n) | O(n) | No random access |
| `stack` | O(1) | O(1) | — | O(1) top | LIFO |
| `queue` | O(1) | O(1) | — | O(1) front | FIFO |
| `priority_queue` | O(log n) | O(log n) | — | O(1) top | Max/Min heap |
| `set` | O(log n) | O(log n) | O(log n) | — | Sorted, unique |
| `multiset` | O(log n) | O(log n) | O(log n) | — | Sorted, dup ok |
| `map` | O(log n) | O(log n) | O(log n) | O(log n) | Sorted by key |
| `unordered_set` | O(1) avg | O(1) avg | O(1) avg | — | Hash, no order |
| `unordered_map` | O(1) avg | O(1) avg | O(1) avg | O(1) avg | Hash, no order |

---

*Last updated for C++17. All examples assume `using namespace std;`.*
