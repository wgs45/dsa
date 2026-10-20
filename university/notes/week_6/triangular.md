# 💠 Special Matrices — Compact Storage & Index Mapping

> **Domain:** Data Structures & Algorithms · Matrix Representation
> **Core Theme:** Memory Optimization · Index Arithmetic · Space Complexity

---

## 💠 01. Intuition — Why Special Matrices Matter

A general `n × n` matrix requires `n²` memory slots. However, special matrices contain predictable zeros or repeated values.

By storing only meaningful elements, we reduce memory consumption and improve cache efficiency.

| Matrix Type      | Stored Elements | Space Complexity |
| ---------------- | --------------: | ---------------- |
| General          |            `n²` | `O(n²)`          |
| Lower triangular |      `n(n+1)/2` | `O(n²)`          |
| Upper triangular |      `n(n+1)/2` | `O(n²)`          |
| Diagonal         |             `n` | `O(n)`           |
| Tridiagonal      |        `3n - 2` | `O(n)`           |
| Symmetric        |      `n(n+1)/2` | `O(n²)`          |

> [!IMPORTANT]
> Triangular matrices reduce storage by approximately half. Diagonal and tridiagonal matrices achieve linear storage because their meaningful elements grow proportionally to `n`.

---

## 🧪 02. Formal Logic — Matrix Definitions

Assume `M` is an `n × n` matrix and `M[i][j]` denotes the element at row `i`, column `j`.

### 💠 Lower Triangular Matrix

A matrix is lower triangular when every element above the main diagonal is zero.

```text
# Example: 4 × 4 lower triangular matrix

[ a  0  0  0 ]
[ b  c  0  0 ]
[ d  e  f  0 ]
[ g  h  i  j ]

# Condition: i < j  =>  M[i][j] = 0
```

**Storage rule:** Keep elements on and below the main diagonal.

Number of stored elements:

```text
# Row lengths: 1 + 2 + ... + n
count = n * (n + 1) / 2
```

### 💠 Upper Triangular Matrix

A matrix is upper triangular when every element below the main diagonal is zero.

```text
# Example: 4 × 4 upper triangular matrix

[ a  b  c  d ]
[ 0  e  f  g ]
[ 0  0  h  i ]
[ 0  0  0  j ]

# Condition: i > j  =>  M[i][j] = 0
```

**Storage rule:** Keep elements on and above the main diagonal.

Number of stored elements:

```text
# Row lengths: n + (n-1) + ... + 1
count = n * (n + 1) / 2
```

### 🔒 Diagonal Matrix

A diagonal matrix contains nonzero values only on the main diagonal.

```text
# Example: 4 × 4 diagonal matrix

[ a  0  0  0 ]
[ 0  b  0  0 ]
[ 0  0  c  0 ]
[ 0  0  0  d ]

# Condition: i != j  =>  M[i][j] = 0
```

**Storage rule:** Store only `M[i][i]`.

```text
# Compact index
index(i, i) = i
```

Storage requirement: `n` elements.

### 📡 Tridiagonal Matrix

A tridiagonal matrix allows nonzero values only on the main diagonal and its two neighboring diagonals.

```text
# Example: 4 × 4 tridiagonal matrix

[ a  b  0  0 ]
[ c  d  e  0 ]
[ 0  f  g  h ]
[ 0  0  i  j ]

# Condition: abs(i - j) > 1  =>  M[i][j] = 0
```

**Storage rule:** Store the lower diagonal, main diagonal, and upper diagonal.

```text
# Lower diagonal: n - 1 elements
# Main diagonal:  n elements
# Upper diagonal: n - 1 elements

count = 3 * n - 2
```

### 💠 Symmetric Matrix

A symmetric matrix equals its own transpose.

```text
# Example: 3 × 3 symmetric matrix

[ a  b  c ]
[ b  d  e ]
[ c  e  f ]

# Condition: M[i][j] = M[j][i]
```

**Storage rule:** Store either the upper or lower triangle, including the diagonal. The other half can be reconstructed by mirroring indices.

```text
# Symmetry lookup
M[i][j] = M[j][i]
```

Storage requirement: `n(n+1)/2` elements.

---

## ⚡ 03. Compact Storage — Row Major vs. Column Major

### 🛠️ Indexing Conventions

- **Row major:** Store one complete row before moving to the next.
- **Column major:** Store one complete column before moving to the next.
- **Compact storage:** Skip the guaranteed-zero elements entirely.

> [!WARNING]
> The following formulas use **0-based matrix indices** and **0-based array indices**. If your lecture slides use 1-based indexing, subtract one from the final array position to convert it to 0-based indexing.

### 💠 Lower Triangular — Row Major

Rows contain `1, 2, 3, ..., n` elements.

```cpp
// Valid when i >= j
index = i * (i + 1) / 2 + j;

// Example: n = 4, M[2][1]
// index = 2 * 3 / 2 + 1 = 4
```

**System Impact:** Row-major packing gives contiguous storage for each lower-triangular row, enabling constant-time element access.

### 💠 Lower Triangular — Column Major

Column `j` contains `n - j` stored elements.

```cpp
// Valid when i >= j
index = j * n - j * (j - 1) / 2 + (i - j);

// Example: n = 4, M[2][1]
// index = 1 * 4 - 1 * 0 / 2 + (2 - 1) = 5
```

**System Impact:** Column-major packing places each lower-triangular column contiguously, making column-wise traversal efficient.

### 💠 Upper Triangular — Row Major

Row `i` contains `n - i` stored elements.

```cpp
// Valid when i <= j
index = i * n - i * (i - 1) / 2 + (j - i);

// Example: n = 4, M[1][2]
// index = 1 * 4 - 1 * 0 / 2 + (2 - 1) = 5
```

**System Impact:** Row-major packing places each upper-triangular row contiguously, supporting efficient row-wise processing.

### 💠 Upper Triangular — Column Major

Column `j` contains `j + 1` stored elements.

```cpp
// Valid when i <= j
index = j * (j + 1) / 2 + i;

// Example: n = 4, M[1][2]
// index = 2 * 3 / 2 + 1 = 4
```

**System Impact:** Column-major packing places each upper-triangular column contiguously, supporting efficient column-wise processing.

---

## 🧪 04. Worked Example — Compact Array Mapping

Consider a `4 × 4` lower-triangular matrix.

```text
# Original matrix

[ 1  0  0  0 ]
[ 2  3  0  0 ]
[ 4  5  6  0 ]
[ 7  8  9 10 ]
```

### 🔄 Row-Major Layout

```text
# Store one row at a time

A = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Row 0: [1]
# Row 1: [2, 3]
# Row 2: [4, 5, 6]
# Row 3: [7, 8, 9, 10]
```

### 🔄 Column-Major Layout

```text
# Store one column at a time

A = [1, 2, 4, 7, 3, 5, 8, 6, 9, 10]

# Column 0: [1, 2, 4, 7]
# Column 1: [3, 5, 8]
# Column 2: [6, 9]
# Column 3: [10]
```

**System Impact:** Both layouts store the same ten values, but their ordering determines which traversal pattern has better memory locality.

---

## 🛠️ 05. Applied Example — C++ Implementation

This implementation stores a lower-triangular matrix in compact row-major order.

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>
using namespace std;

class LowerTriangular {
private:
    int n;
    vector<int> data;

    // Maps a valid matrix coordinate to compact storage.
    size_t index(int i, int j) const {
        return static_cast<size_t>(i) * (i + 1) / 2 + j;
    }

public:
    explicit LowerTriangular(int size)
        : n(size),
          data(size >= 0
                   ? static_cast<size_t>(size) * (size + 1) / 2
                   : 0, 0) {
        if (size < 0) {
            throw invalid_argument("Matrix size cannot be negative");
        }
    }

    int get(int i, int j) const {
        if (i < 0 || i >= n || j < 0 || j >= n) {
            throw out_of_range("Invalid matrix coordinate");
        }

        // Elements above the diagonal are implicit zeros.
        if (i < j) return 0;

        return data[index(i, j)];
    }

    void set(int i, int j, int value) {
        if (i < 0 || i >= n || j < 0 || j >= n) {
            throw out_of_range("Invalid matrix coordinate");
        }

        // Nonzero values cannot exist above the diagonal.
        if (i < j) {
            if (value != 0) {
                throw invalid_argument(
                    "Cannot store nonzero above the diagonal");
            }
            return;
        }

        data[index(i, j)] = value;
    }
};

int main() {
    LowerTriangular matrix(4);

    matrix.set(2, 1, 5);

    cout << matrix.get(2, 1) << '\n'; // 5
    cout << matrix.get(1, 2) << '\n'; // 0
}
```

**System Impact:** The implementation uses `n(n+1)/2` integers instead of `n²`, while providing constant-time access and validation of matrix coordinates.

---

## ⚡ 06. Complexity & Optimization

| Operation             | Time Complexity | Explanation                                 |
| --------------------- | --------------- | ------------------------------------------- |
| Read valid element    | `O(1)`          | Calculate its compact index                 |
| Write valid element   | `O(1)`          | Calculate its compact index                 |
| Read implicit zero    | `O(1)`          | Return zero without accessing storage       |
| Initialize storage    | `O(n²)`         | Allocate and initialize `n(n+1)/2` elements |
| Full matrix traversal | `O(n²)`         | Visit all logical coordinates               |
| Compact storage       | `O(n²)`         | Store only triangular elements              |

For diagonal and tridiagonal matrices, compact storage reduces to `O(n)`.

> [!NOTE]
> Big-O notation describes growth rate, not exact memory usage. Triangular storage remains quadratic even though it uses approximately half as many elements as a dense square matrix.

---

## 🏁 07. Recap — Interview Revision

- 💠 **Lower triangular:** Store when `i >= j`.
- 💠 **Upper triangular:** Store when `i <= j`.
- 💠 **Diagonal:** Store only when `i == j`.
- 💠 **Tridiagonal:** Store when `abs(i - j) <= 1`.
- 💠 **Symmetric:** Reconstruct the mirrored element using `M[i][j] = M[j][i]`.
- ⚡ **Triangular count:** `n * (n + 1) / 2`.
- ⚡ **Tridiagonal count:** `3 * n - 2` for `n >= 1`.
- 🛠️ **Indexing rule:** Confirm the storage order and index base before applying a formula.

### 🧠 Self-Test

1. How many values must be stored in a `6 × 6` lower-triangular matrix?
2. What is the compact row-major index of `M[4][2]` when `n = 6`?
3. Why does a tridiagonal matrix require linear rather than quadratic storage?
4. How can a symmetric matrix reconstruct an element that was not explicitly stored?
5. What changes when the same compact matrix switches from row-major to column-major storage?

> [!IMPORTANT]
> **Final Principle:** Identify the structural zeros first, derive the storage order second, and calculate the compact index last. This sequence prevents most matrix-mapping errors.
