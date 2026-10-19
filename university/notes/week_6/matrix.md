# 💠 Matrix ADT

> [!NOTE]
> **Domain:** Data Structures & Algorithms
> **Core Systems:** Matrix Representation · Sparse Storage · Abstract Data Types · Space Optimization
> **Mission:** Represent matrices efficiently while minimizing unnecessary memory usage.

---

## 🧭 01 · Intuition — Why Sparse Matrices Matter

A matrix is a two-dimensional collection of values arranged in rows and columns.

A **sparse matrix** contains mostly zero values. Storing every element wastes memory because the zero values often carry no useful information that needs to be stored explicitly.

### 💠 Core Concepts

- 💠 **Matrix:** A rectangular arrangement of values organized by row and column.
- 💠 **Sparse matrix:** A matrix containing many zero elements.
- 💠 **Nonzero term:** An element whose value is not zero.
- 💠 **Dimensions:** The number of rows and columns.
- 💠 **Term count:** The number of nonzero elements stored explicitly.

### 🧪 Metal — Example Matrix

Consider this 6 × 6 matrix:

```text
       Col 0  Col 1  Col 2  Col 3  Col 4  Col 5
Row 0    15      0      0     22      0    -15
Row 1     0     11      3      0      0      0
Row 2     0      0      0     -6      0      0
Row 3     0      0      0      0      0      0
Row 4    91      0      0      0      0      0
Row 5     0      0     28      0      0      0
```

The matrix contains:

| Property         |       Value |
| ---------------- | ----------: |
| Rows             |           6 |
| Columns          |           6 |
| Total elements   |          36 |
| Nonzero elements |           8 |
| Zero elements    |          28 |
| Sparsity         | 77.8% zeros |

**System Impact:** Because 28 of the 36 elements are zero, storing only the eight nonzero elements can reduce storage requirements.

---

## 🧱 02 · Traditional Representation — Two-Dimensional Array

### 💠 Intuition — Direct Access

The traditional representation stores every matrix element in a two-dimensional array, including all zeros.

### 🛠️ C Implementation

```c
#define MAX_ROW 100
#define MAX_COL 100

typedef int Matrix[MAX_ROW][MAX_COL];

Matrix m1, m2;
```

**System Impact:** Direct indexing provides O(1) element access, but each matrix reserves space for 10,000 integers regardless of how many elements are nonzero.

### 🧪 Metal — Memory Cost

For a matrix with 100 rows and 100 columns:

```text
Total elements = 100 * 100
               = 10,000 integers

Memory = 10,000 * sizeof(int)
```

Assuming `sizeof(int) == 4` bytes:

```text
Memory = 40,000 bytes
       ≈ 39.1 KiB
```

This estimate excludes additional overhead and assumes the array occupies the full declared capacity.

### ⚠️ The Problem

- 🔹 Most allocated positions may contain zero.
- 🔹 Memory consumption depends on matrix dimensions.
- 🔹 Very large, mostly empty matrices can consume excessive memory.

> [!IMPORTANT]
> A traditional array is not inherently inefficient. It is often the better choice when most elements are nonzero or when fast, predictable access to every position is important.

---

## 🧬 03 · Sparse Matrix ADT — Formal Logic

### 💠 Intuition — Store Only Nonzero Values

A sparse matrix can be represented by its dimensions and a collection of nonzero terms.

Each term records three values:

- `row`: The row index.
- `col`: The column index.
- `value`: The element stored at that position.

### 🛠️ Abstract Data Type

| Operation                  | Purpose                       | Result            |
| -------------------------- | ----------------------------- | ----------------- |
| `Create(max_row, max_col)` | Create an empty sparse matrix | Sparse matrix     |
| `Transpose(sm)`            | Exchange rows and columns     | Transposed matrix |
| `Add(a, b)`                | Add two matrices              | Sum matrix        |
| `Multiply(a, b)`           | Multiply compatible matrices  | Product matrix    |

### 🔄 Operation Contracts

**`Create(rows, cols)`**

- Initializes the matrix dimensions.
- Sets the nonzero term count to zero.

**`Transpose(sm)`**

- Converts each term at `(row, col)` into `(col, row)`.
- Exchanges the matrix's row and column dimensions.

**`Add(a, b)`**

- Requires equal dimensions.
- Adds values at matching positions.
- Omits zero-valued results from canonical sparse storage.

**`Multiply(a, b)`**

- Requires the number of columns in `a` to equal the number of rows in `b`.
- Produces a matrix with the row count of `a` and column count of `b`.

**System Impact:** The ADT separates matrix operations from the storage mechanism, allowing implementations to change without changing the operation contracts.

---

## 🛠️ 04 · Sparse Matrix Representation — C Structure

### 💠 Data Model

A sparse matrix stores its metadata and nonzero terms in a single structure.

```c
#define MAX_TERM 100

typedef struct {
    int row;                    // Row index
    int col;                    // Column index
    int value;                  // Nonzero element value
} Term;

typedef struct {
    int nRow;                   // Number of rows
    int nCol;                   // Number of columns
    int nTerm;                  // Number of stored nonzero terms
    Term tArray[MAX_TERM];      // Array of nonzero terms
} SparseMatrix;
```

**System Impact:** Each nonzero element requires one `Term` record instead of a separate storage slot for every possible matrix position.

### 🧪 Metal — Triplet Representation

The 6 × 6 example can be stored as eight triplets.

Terms are ordered first by row and then by column.

```text
Index   Row   Col   Value
  0      0     0      15
  1      0     3      22
  2      0     5     -15
  3      1     1      11
  4      1     2       3
  5      2     3      -6
  6      4     0      91
  7      5     2      28
```

Metadata:

```text
nRow  = 6
nCol  = 6
nTerm = 8
```

The element at row 0, column 3 has value 22:

```c
// Access the second stored term
int value = sm.tArray[1].value;  // 22
```

**System Impact:** Storage grows with the number of nonzero terms, but locating an arbitrary element may require searching the term array.

### 🔒 Representation Invariants

- [ ] `nRow` and `nCol` are non-negative.
- [ ] `nTerm` is between zero and `MAX_TERM`.
- [ ] Each stored row index is valid.
- [ ] Each stored column index is valid.
- [ ] Every stored value is nonzero.
- [ ] Each coordinate appears at most once.
- [ ] Terms are ordered by row, then by column.

> [!NOTE]
> The original structure uses a fixed-capacity array. A matrix with more than `MAX_TERM` nonzero elements cannot be represented without increasing capacity or changing the storage strategy.

---

## ⚡ 05 · Optimization — Dense vs. Sparse Storage

### 💠 Intuition

The most suitable representation depends on the number of matrix elements that are actually nonzero.

Let:

```text
R = Number of rows
C = Number of columns
T = Number of nonzero elements
```

### 🧪 Storage Comparison

| Property              | Dense 2D Array       | Sparse Triplet Array    |
| --------------------- | -------------------- | ----------------------- |
| Storage requirement   | O(R * C)             | O(T)                    |
| Stored values         | All elements         | Nonzero elements        |
| Metadata per element  | Implicit coordinates | Explicit row and column |
| Random element access | O(1)                 | O(T) with linear search |
| Sequential traversal  | O(R * C)             | O(T)                    |
| Best suited for       | Dense matrices       | Sparse matrices         |

### 🧪 Memory Example

Assume each matrix element uses four bytes and each triplet uses three four-byte integers.

For the 6 × 6 example:

```text
Dense representation:
36 elements * 4 bytes = 144 bytes

Sparse representation:
8 terms * 12 bytes = 96 bytes
```

These estimates exclude structure padding, metadata, and unused fixed-array capacity.

For a 100 × 100 matrix containing only eight nonzero elements:

```text
Dense representation:
10,000 * 4 = 40,000 bytes

Sparse terms:
8 * 12 = 96 bytes
```

The sparse term records are dramatically smaller in this example, provided the implementation stores only the eight terms rather than reserving a large fixed-capacity term array.

> [!IMPORTANT]
> A fixed-size `tArray[MAX_TERM]` still reserves space for `MAX_TERM` entries. The structure above is a simple teaching implementation, not a dynamically sized representation.

**System Impact:** Sparse storage can dramatically reduce memory use when `T` is much smaller than `R * C`, at the cost of more complex access and update logic.

---

## 🔄 06 · Special Square Matrices — One-Dimensional Storage

### 💠 Intuition

A square matrix has the same number of rows and columns.

```text
Rows = Columns = n
Dimensions = n * n
```

Some square matrices have predictable structures, such as diagonal or triangular matrices. These structures allow their elements to be stored in one-dimensional arrays without reserving space for every position.

### 🧪 Example — General Square Matrix

A 3 × 3 matrix contains nine elements:

```text
M =       Col 0  Col 1  Col 2
Row 0       a      b      c
Row 1       d      e      f
Row 2       g      h      i
```

A row-major one-dimensional representation stores the elements in this order:

```text
Index:  0  1  2  3  4  5  6  7  8
Value:  a  b  c  d  e  f  g  h  i
```

The position of matrix element `(row, col)` is:

```text
index = row * number_of_columns + col
```

For a 3 × 3 matrix:

```text
M[1][2] -> index = 1 * 3 + 2
         -> index = 5
```

**System Impact:** Row-major flattening stores a general matrix in one contiguous array, but it does not reduce the total number of elements.

### ⚡ Special Structures

| Matrix Type             | Stored Elements | Potential Benefit                 |
| ----------------------- | --------------- | --------------------------------- |
| General square matrix   | n * n           | Contiguous storage                |
| Diagonal matrix         | n               | Stores only diagonal values       |
| Upper triangular matrix | n * (n + 1) / 2 | Omits elements below the diagonal |
| Lower triangular matrix | n * (n + 1) / 2 | Omits elements above the diagonal |
| Sparse matrix           | T               | Omits all zero elements           |

> [!NOTE]
> One-dimensional storage alone does not save memory for a general matrix. Space savings come from exploiting structural properties or omitting zero elements.

---

## 🧪 07 · Applied Example — Flattening a Matrix in C

```c
#include <stdio.h>

#define N 3

int main(void)
{
    // Store a 3x3 matrix in row-major order
    int matrix[N * N] = {
        1, 2, 3,
        4, 5, 6,
        7, 8, 9
    };

    int row = 1;
    int col = 2;

    // Convert 2D coordinates to a 1D index
    int index = row * N + col;

    printf("%d\n", matrix[index]);  // Prints 6

    return 0;
}
```

**System Impact:** The row-major index formula provides constant-time element access without requiring a native two-dimensional array declaration.

---

## 🏁 08 · Recap — Interview Quick Reference

### 💠 Core Takeaways

- **Sparse matrix:** A matrix containing many zero elements.
- **Dense representation:** Stores every matrix element.
- **Triplet representation:** Stores row, column, and value for each nonzero term.
- **Sparse matrix ADT:** Defines creation, transpose, addition, and multiplication.
- **Term ordering:** Row first, then column.
- **Space optimization:** Sparse storage is beneficial when the nonzero count is small relative to the total number of positions.
- **Square matrix:** Has equal numbers of rows and columns.
- **One-dimensional mapping:** Row-major index equals `row * number_of_columns + col`.

### ⚡ Rapid Revision

| Interview Question                                       | Answer                                                         |
| -------------------------------------------------------- | -------------------------------------------------------------- |
| What makes a matrix sparse?                              | It contains many zeros.                                        |
| Why is a traditional array wasteful for sparse matrices? | It stores zero elements explicitly.                            |
| What fields make up a triplet?                           | Row, column, and value.                                        |
| Why sort sparse terms by row and column?                 | It enables ordered traversal and simplifies matrix operations. |
| What does transpose do?                                  | Swaps each term's row and column indices.                      |
| What dimensions are required for addition?               | Both matrices must have equal dimensions.                      |
| What dimensions are required for multiplication?         | Columns of the first matrix must equal rows of the second.     |
| Does flattening a matrix reduce storage?                 | Not by itself; it changes the layout, not the element count.   |
| How can triangular matrices save space?                  | Store only the triangle that contains the required values.     |

> [!SUCCESS]
> **Architect's Principle:** Optimize the representation around the structure of the data. Use dense arrays for direct, uniform access; use sparse storage when most positions contain zero; and use specialized layouts when matrix structure permits further compression.
