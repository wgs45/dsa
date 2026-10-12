# 💠 Arrays — Memory Architecture & Pointer Mechanics

> [!NOTE]
> **Domain:** Data Structures · C/C++ · Memory Management
> **Core Concepts:** Indexing, contiguous storage, pointers, row-major layout, address calculation
> **Objective:** Understand how arrays map logical indices to physical memory.

---

## 💠 01. Intuition — What Is an Array?

An **array** is a data structure that associates each valid index with an element.

```text
Index:    0    1    2    3    4
Value:    7   51   22   43    9
          │    │    │    │    │
Memory:  [7]  [51] [22] [43] [9]
```

In C and C++, ordinary built-in arrays store their elements in contiguous memory.

This means consecutive elements are separated by exactly `sizeof(element_type)` bytes.

### 🧪 Formal Structure

An array can be modeled as a collection of index-value pairs:

`(index, value)`

For a one-dimensional array of length `n`:

```text
Index set = {0, 1, 2, ..., n - 1}

A = {
    (0, A[0]),
    (1, A[1]),
    ...
    (n-1, A[n-1])
}
```

For a two-dimensional array with three rows and three columns:

```text
Index set = {
    (0,0), (0,1), (0,2),
    (1,0), (1,1), (1,2),
    (2,0), (2,1), (2,2)
}
```

### 🛠️ Abstract Array Operations

| Operation        | Purpose                                   | Expected complexity                      |
| ---------------- | ----------------------------------------- | ---------------------------------------- |
| `Create`         | Create an array with specified dimensions | Depends on allocation and initialization |
| `Retrieve(A, i)` | Read the element at index `i`             | `O(1)`                                   |
| `Store(A, i, x)` | Write `x` at index `i`                    | `O(1)`                                   |

> [!IMPORTANT]
> The abstract data type defines valid operations and their behavior. The implementation determines how storage is managed. For built-in C/C++ arrays, valid indexing provides constant-time access.

**System Impact:** Direct indexing enables predictable, constant-time element access without scanning preceding elements.

---

## 🧪 02. Formal Logic — One-Dimensional Memory Layout

Consider:

```cpp
int iArray[100];  // Reserve space for 100 integers
```

Assume `sizeof(int) == 4` and let `m` represent the address of `iArray[0]`.

| Element      | Index | Address   |
| ------------ | ----: | --------- |
| `iArray[0]`  |     0 | `m`       |
| `iArray[1]`  |     1 | `m + 4`   |
| `iArray[2]`  |     2 | `m + 8`   |
| `iArray[3]`  |     3 | `m + 12`  |
| `iArray[99]` |    99 | `m + 396` |

### ⚡ Address Formula

For an array whose base address is `m`:

```text
Address(A[i]) = m + i * sizeof(A[0])

# For 4-byte integers:
Address(A[i]) = m + 4*i
```

The last element begins at byte offset `396`, because the offset is `(100 - 1) × 4`.

The total storage for the 100 integer elements is `400` bytes, excluding any surrounding objects or implementation-specific overhead.

### 🧪 Applied Example — C++

```cpp
#include <iostream>
using namespace std;

int main() {
    int values[5] = {7, 51, 22, 43, 9};

    for (int i = 0; i < 5; i++) {
        cout << "Address: " << &values[i]
             << " Value: " << values[i] << '\n';
    }

    return 0;
}
```

The actual addresses depend on where the array is placed in memory. The important invariant is that consecutive elements have consecutive addresses separated by `sizeof(int)` bytes.

**System Impact:** Understanding contiguous storage explains both constant-time indexing and why array traversal often benefits from CPU cache locality.

---

## 💠 03. C/C++ Arrays vs. Pointers

### 🧪 Declaration Comparison

```cpp
int list[5];      // Five actual int objects
int* plist[5];    // Five pointers to int objects

int* list1;       // One pointer to int
int list2[5];     // Five actual int objects
```

| Declaration     | Meaning                        | Storage reserved by declaration |
| --------------- | ------------------------------ | ------------------------------- |
| `int list[5]`   | Array of five integers         | Space for five integers         |
| `int* plist[5]` | Array of five integer pointers | Space for five pointers         |
| `int* list1`    | Pointer to an integer          | Space for one pointer           |
| `int list2[5]`  | Array of five integers         | Space for five integers         |

> [!WARNING]
> `int* plist[5]` is an **array of pointers**, not a pointer to an array. The five pointers do not automatically allocate five integers for their targets.

### 🔒 Array-to-Pointer Conversion

In most expressions, an array expression converts to a pointer to its first element. This is often called _array decay_.

```cpp
int list2[5] = {10, 20, 30, 40, 50};

int* p = list2;          // Points to list2[0]
int* q = list2 + 2;      // Points to list2[2]

cout << *p << '\n';      // 10
cout << *q << '\n';      // 30
cout << *(list2 + 3);    // 40
```

### ⚡ Pointer Arithmetic

| Expression     | Meaning                                     |
| -------------- | ------------------------------------------- |
| `list2`        | Usually converts to a pointer to `list2[0]` |
| `list2 + i`    | Pointer to `list2[i]`                       |
| `*(list2 + i)` | Value stored in `list2[i]`                  |
| `&list2[i]`    | Address of `list2[i]`                       |
| `list2[i]`     | Equivalent to `*(list2 + i)`                |

Pointer arithmetic advances in **elements**, not individual bytes.

For example, if `sizeof(int) == 4`, adding `1` to an `int*` advances its address by four bytes.

### 🔒 The Critical Difference

```cpp
int* list1 = nullptr;  // Pointer variable; no array allocated
int list2[5] = {};     // Actual array of five integers

// list1 = list2;      // Valid: pointer can point to list2[0]
// sizeof(list2);      // Total size of five integers
// sizeof(list1);      // Size of the pointer itself
```

- `list1` is a pointer variable. It does not reserve an array of five integers.
- `list2` is an array object with storage for five integers.
- `list2` often converts to a pointer, but it is not itself a pointer variable.

**System Impact:** Distinguishing arrays from pointers prevents incorrect assumptions about memory ownership, object size, and the lifetime of stored data.

---

## 🛠️ 04. Applied Example — Printing Addresses with Pointers

The expression `one + i` identifies the address of element `i`, while `*(one + i)` retrieves its value.

```cpp
#include <iostream>
using namespace std;

int main() {
    int one[] = {0, 1, 2, 3, 4};

    cout << "Address\t\tContents\n";

    for (int i = 0; i < 5; i++) {
        cout << static_cast<const void*>(one + i)
             << '\t'
             << *(one + i) << '\n';
    }

    return 0;
}
```

### Expected Logical Output

```text
Address         Contents
m               0
m + 4           1
m + 8           2
m + 12          3
m + 16          4
```

Here, `m` is a symbolic base address, not a literal output value. Real pointer addresses vary by execution.

The original lecture's sample addresses increase by two hexadecimal units (`0x1228`, `0x1230`, and so on), which is consistent with four-byte elements because hexadecimal `0x4` equals four decimal bytes.

> [!NOTE]
> In C++, `one` and `&one[0]` point to the first element after the relevant conversions. However, `one` and `&one` have different pointer types: `int*` and `int (*)[5]`, respectively.

**System Impact:** Pointer-based traversal reveals that array indexing is fundamentally an address calculation followed by an element access.

---

## 💠 05. Multidimensional Arrays — Logical Structure

Consider a two-dimensional array:

```cpp
int mesh[7][11];  // Seven rows, eleven columns
```

Valid indices are:

- Rows: `0` through `6`
- Columns: `0` through `10`

For example:

```cpp
mesh[4][2];  // Row 4, column 2
mesh[2][7];  // Row 2, column 7
```

The array contains `7 × 11 = 77` integers.

### 🧪 Memory Layout — Row Major

Built-in C and C++ multidimensional arrays use **row-major order**: all elements of one row are stored before the next row begins.

```text
Logical matrix:

Row 0: [0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10]
Row 1: [11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21]
Row 2: [22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32]

Physical order:

[0, 1, 2, ..., 10, 11, 12, ..., 21, 22, ...]
```

### ⚡ Address Formula

For `int mesh[7][11]`, assuming four-byte integers and base address `m`:

```text
Address(mesh[i][j])
    = m + (11*i + j) * 4

# Row stride = 11 * 4 = 44 bytes
# Column stride = 4 bytes
```

| Element       | Linear offset in elements | Byte offset |
| ------------- | ------------------------: | ----------: |
| `mesh[0][0]`  |                         0 |           0 |
| `mesh[0][1]`  |                         1 |           4 |
| `mesh[1][0]`  |                        11 |          44 |
| `mesh[2][7]`  |                        29 |         116 |
| `mesh[4][2]`  |                        46 |         184 |
| `mesh[6][10]` |                        76 |         304 |

The complete array occupies `7 × 11 × 4 = 308` bytes, assuming `sizeof(int) == 4`.

> [!IMPORTANT]
> **Row-major vs. column-major:** Column-major storage places all rows of a column together before advancing to the next column. It is used by some languages and numerical libraries, but not by built-in C/C++ multidimensional arrays.

**System Impact:** Knowing the storage order lets you calculate element addresses directly and choose traversal patterns that improve memory locality.

---

## 🧪 06. Three-Dimensional Arrays — Address Mapping

Consider:

```cpp
int cube[6][8][3];  // Six blocks, eight rows, three columns
```

The array contains:

`6 × 8 × 3 = 144` integer elements.

The indices are:

- `i`: Block index, `0`–`5`
- `j`: Row index, `0`–`7`
- `k`: Column index, `0`–`2`

### ⚡ Address Formula

In row-major order, the final dimension varies fastest.

```text
Address(cube[i][j][k])
    = m + ((8*3)*i + 3*j + k) * 4

# Simplified:
Address(cube[i][j][k])
    = m + (24*i + 3*j + k) * 4
```

Each block contains `8 × 3 = 24` integers, occupying `96` bytes.

### 🛠️ Memory Boundaries

| Element         | Element offset | Byte offset |
| --------------- | -------------: | ----------: |
| `cube[0][0][0]` |              0 |           0 |
| `cube[0][0][1]` |              1 |           4 |
| `cube[0][1][0]` |              3 |          12 |
| `cube[1][0][0]` |             24 |          96 |
| `cube[2][0][0]` |             48 |         192 |
| `cube[5][7][2]` |            143 |         572 |

The final element starts at offset `572` bytes and ends immediately before byte offset `576`.

```cpp
#include <iostream>
using namespace std;

int main() {
    int cube[6][8][3] = {};

    // Traverse in row-major order
    for (int i = 0; i < 6; i++) {
        for (int j = 0; j < 8; j++) {
            for (int k = 0; k < 3; k++) {
                cube[i][j][k] = i * 100 + j * 10 + k;
            }
        }
    }

    cout << cube[2][4][1] << '\n';  // 241
    return 0;
}
```

**System Impact:** Multidimensional arrays remain contiguous, so the same address-calculation principle scales to any number of dimensions.

---

## ⚡ 07. Optimization — Cache-Friendly Traversal

Since the last dimension varies fastest in C/C++, iterating over it in the innermost loop usually gives better spatial locality.

```cpp
int mesh[7][11] = {};

// Preferred: traverse consecutive elements
for (int i = 0; i < 7; i++) {
    for (int j = 0; j < 11; j++) {
        mesh[i][j] = i + j;
    }
}

// Less cache-friendly access pattern:
for (int j = 0; j < 11; j++) {
    for (int i = 0; i < 7; i++) {
        mesh[i][j] = i + j;
    }
}
```

| Pattern      | Inner-loop movement           | Typical locality |
| ------------ | ----------------------------- | ---------------- |
| Row-first    | `mesh[i][j]` → `mesh[i][j+1]` | Strong           |
| Column-first | `mesh[i][j]` → `mesh[i+1][j]` | Weaker           |

The second loop is not inherently incorrect or always slower; actual performance depends on the data size, cache behavior, compiler, and workload.

**System Impact:** Matching traversal order to physical storage can reduce cache misses and improve performance in large matrix-processing workloads.

---

## 🔒 08. Common Pitfalls — Interview Traps

| Mistake                                                        | Correct understanding                                                              |
| -------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Accessing `array[n]` in an array of length `n`                 | Out of bounds; valid indices end at `n - 1`                                        |
| Treating `int* plist[5]` as five integers                      | It is an array of five pointers                                                    |
| Assuming `int* p` allocates an array                           | A pointer alone allocates no target array                                          |
| Thinking `p + 1` advances one byte                             | It advances by `sizeof(*p)` bytes for a valid pointer to an element                |
| Assuming C/C++ built-in 2D arrays use column-major order       | They use row-major order                                                           |
| Assuming `sizeof(array)` always returns a pointer size         | In the scope of an actual array object, it returns the array's total size in bytes |
| Assuming a function parameter `int a[]` retains its array size | It is adjusted to a pointer parameter                                              |

> [!WARNING]
> Out-of-bounds access and invalid pointer arithmetic can cause undefined behavior. A pointer must refer to an appropriate object or its permitted one-past-the-end position for pointer arithmetic to be valid.

---

## 🏁 09. Recap — Rapid Revision

- 💠 **Array:** A collection of elements accessed through valid indices.
- ⚡ **Contiguous memory:** Consecutive elements are separated by `sizeof(element)`.
- 🛠️ **One-dimensional address:** `base + i * element_size`.
- 🔒 **Pointer arithmetic:** Advances in units of the pointed-to type.
- 💠 **Two-dimensional address:** `base + (i * columns + j) * element_size`.
- 🧪 **Three-dimensional address:** `base + (i * rows * columns + j * columns + k) * element_size`.
- ⚡ **C/C++ storage order:** Row-major, with the last index varying fastest.
- 🏁 **Optimization:** Traverse consecutive elements in the innermost loop whenever practical.

> [!IMPORTANT]
> **Memory anchor:** Index → Linear offset → Byte offset → Address.
>
> For a row-major array, multiply each index by the product of the dimensions that follow it, sum those contributions, then multiply by the element size.

---

## 🧪 10. Knowledge Check

Try solving these before revealing the answers.

1. What is the byte offset of `A[8]` when `A` contains four-byte integers?
2. How many bytes does `int matrix[4][6]` occupy if `sizeof(int) == 4`?
3. What is the element offset of `mesh[3][5]` in `int mesh[7][11]`?
4. What is the element offset of `cube[2][3][1]` in `int cube[6][8][3]`?
5. Why is `int* p` different from `int array[5]`?

<details>
<summary>💠 Reveal Answers</summary>

1. `8 × 4 = 32` bytes.
2. `4 × 6 × 4 = 96` bytes.
3. `3 × 11 + 5 = 38` elements.
4. `2 × 8 × 3 + 3 × 3 + 1 = 58` elements.
5. `p` is a pointer variable; `array` is an array object containing five integers.

</details>

---

**🏁 Final Takeaway:** Arrays bridge logical indexing and physical memory. Mastering their layout and pointer arithmetic provides the foundation for understanding cache behavior, matrices, strings, low-level data structures, and memory-safe C/C++ programming.
