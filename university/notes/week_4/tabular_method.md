# 💠 Week 4 — Step Count Analysis

> [!NOTE]
> **Theme:** Analyze an algorithm by counting how many times each statement executes.
>
> **Mental Model:**
> `Step Cost × Execution Frequency = Total Contribution`

---

## 💠 01 — Intuition: Why Count Steps?

Algorithm performance can be estimated without running the program.

Instead of measuring **seconds**, we count the number of fundamental operations performed as the input size grows.

### 🧠 The Core Idea

For every statement:

1. Determine its **step cost**.
2. Determine its **execution frequency**.
3. Multiply them.
4. Add every statement's contribution.

```text
Total Steps = Σ (Step Cost × Frequency)
# Count the work performed by every statement.
```

**System Impact:** Step counting converts source code into a mathematical model of algorithmic cost.

---

## 🧪 02 — Formal Logic: Tabular Method

The **Tabular Method** organizes statement execution into a table.

| Component       | Meaning                                |
| --------------- | -------------------------------------- |
| **s/e**         | Steps executed by the statement        |
| **Frequency**   | Number of times the statement executes |
| **Total Steps** | s/e × Frequency                        |

### 🔄 General Workflow

```text
For each statement:
    identify step cost
    identify execution frequency
    calculate contribution
    accumulate total
# The final sum is the exact step-count function.
```

**System Impact:** The tabular method makes nested-loop and recursive costs explicit instead of relying on intuition.

---

# 💠 03 — Example: Iterative Sum

```c
float sum(float list[], int n)
{
    float tempsum = 0;
    int i;

    for (i = 0; i < n; i++)
        tempsum += list[i];

    return tempsum;
}
```

### 🧪 Step Count

| Statement            | s/e | Frequency |      Total |
| -------------------- | --: | --------: | ---------: |
| `tempsum = 0`        |   1 |         1 |          1 |
| `i = 0`              |   1 |         1 |          1 |
| `i < n`              |   1 |     n + 1 |      n + 1 |
| `i++`                |   1 |         n |          n |
| `tempsum += list[i]` |   1 |         n |          n |
| `return tempsum`     |   1 |         1 |          1 |
| **Total**            |     |           | **3n + 4** |

> [!IMPORTANT]
> Depending on the course's definition of a "basic step," initialization or return operations may be excluded. The important skill is understanding **frequency × cost** and deriving the dominant growth rate.

### ⚡ Complexity

```text
T(n) = 3n + 4
T(n) = O(n)
# Linear growth because the loop executes proportional to n.
```

**System Impact:** Doubling the input roughly doubles the dominant amount of work.

---

# 🧪 04 — Recursive Function: Sum of a List

```c
float rsum(float list[], int n)
{
    if (n)
        return rsum(list, n - 1) + list[n - 1];

    return list[0];
}
```

### 🔍 Execution Pattern

The function recursively decreases `n`:

```text
rsum(n)
  ↓
rsum(n - 1)
  ↓
rsum(n - 2)
  ↓
...
  ↓
rsum(0)
# One recursive call is created for each level.
```

### 🧪 Step Count

| Statement        | Frequency | Contribution |
| ---------------- | --------: | -----------: |
| `if (n)`         |     n + 1 |        n + 1 |
| Recursive call   |         n |            n |
| `return list[0]` |         1 |            1 |
| **Total**        |           |   **2n + 2** |

### ⚡ Complexity

```text
T(n) = 2n + 2
T(n) = O(n)
# The recursion depth grows linearly with n.
```

> [!NOTE]
> Recursive code can have the same asymptotic complexity as iterative code while introducing **call-stack overhead**.

**System Impact:** Recursion does not automatically mean exponential complexity; the recursive structure must be analyzed.

---

# 💠 05 — Matrix Addition

```c
void add(int a[][MAX_SIZE], int b[][MAX_SIZE],
         int c[][MAX_SIZE], int rows, int cols)
{
    int i, j;

    for (i = 0; i < rows; i++)
        for (j = 0; j < cols; j++)
            c[i][j] = a[i][j] + b[i][j];
}
```

### 🧪 Execution Structure

There are **two nested loops**:

```text
rows
 └── cols
      └── matrix operation
# Every row processes every column.
```

### ⚡ Complexity

```text
T(rows, cols) ≈ 2 × rows × cols
T = O(rows × cols)
# Work grows with the total number of matrix elements.
```

For a square `n × n` matrix:

```text
T(n) = O(n²)
# rows = cols = n, therefore n × n = n².
```

**System Impact:** Matrix-wide operations usually become quadratic when both dimensions scale together.

---

# 💠 06 — Exercise 1: Print Matrix

```c
void Printmatrix(int matrix[][MAX_SIZE],
                 int rows, int cols)
{
    int i, j;

    for (i = 0; i < rows; i++)
    {
        for (j = 0; j < cols; j++)
            printf("%5d", matrix[i][j]);

        printf("\n");
    }
}
```

### 🧪 Step Count Pattern

For a square matrix where:

```text
rows = cols = n
# Both dimensions grow together.
```

The important frequencies are:

| Operation                 | Frequency |
| ------------------------- | --------: |
| Outer-loop test           |     n + 1 |
| Inner-loop initialization |         n |
| Inner-loop test           |  n(n + 1) |
| Matrix printing           |        n² |
| Inner-loop increment      |        n² |
| Newline                   |         n |

The provided course calculation gives:

```text
T(n) = 2n² + 3n + 1
T(n) = O(n²)
# The n² terms dominate the linear terms.
```

> [!IMPORTANT]
> **Revision rule:** Nested loops do not automatically mean `O(n²)`. Here they do because each of the `n` outer iterations performs approximately `n` inner iterations.

**System Impact:** Printing every element of an `n × n` matrix requires quadratic work because every element must be visited.

---

# ⚡ 07 — Exercise 2: Square Matrix Multiplication

```c
void mult(int a[][MAX_SIZE], int b[][MAX_SIZE],
          int c[][MAX_SIZE])
{
    int i, j, k;

    for (i = 0; i < MAX_SIZE; i++)
        for (j = 0; j < MAX_SIZE; j++)
        {
            c[i][j] = 0;

            for (k = 0; k < MAX_SIZE; k++)
                c[i][j] += a[i][k] * b[k][j];
        }
}
```

### 🧠 Intuition

Matrix multiplication introduces **three nested loops**:

```text
i
└── j
    └── k
        └── multiplication + accumulation
# Three dimensions of iteration produce cubic growth.
```

### 🧪 Step Count

Let:

```text
n = MAX_SIZE
# Treat MAX_SIZE as the input dimension.
```

The provided calculation gives:

```text
T(n) = 2n³ + 3n² + 2n + 1
```

Therefore:

```text
T(n) = O(n³)
# The cubic term dominates all lower-order terms.
```

> [!IMPORTANT]
> The original note writes `O(n)^3`; the standard notation is **O(n³)**.

**System Impact:** Increasing matrix dimension makes multiplication dramatically more expensive because the dominant work grows cubically.

---

# ⚡ 08 — Exercise 3: General Matrix Multiplication

```c
void prod(int a[][MAX_SIZE], int b[][MAX_SIZE],
          int c[][MAX_SIZE],
          int rowsA, int colsB, int colsA)
{
    int i, j, k;

    for (i = 0; i < rowsA; i++)
        for (j = 0; j < colsB; j++)
        {
            c[i][j] = 0;

            for (k = 0; k < colsA; k++)
                c[i][j] += a[i][k] * b[k][j];
        }
}
```

### 💠 Dimensions

For:

```text
A = rowsA
B = colsB
K = colsA
# A × K multiplied by K × B produces A × B.
```

The dominant operation occurs:

```text
A × B × K
# Every output element performs K multiply-accumulate operations.
```

### 🧪 Step Count

The provided formula is:

```text
T(A, B, K) = 2ABK + 3AB + 2A + 1
# ABK dominates when the dimensions become large.
```

Therefore:

```text
T(A, B, K) = O(ABK)
# Complexity depends on all three matrix dimensions.
```

For square matrices:

```text
A = B = K = n

T(n) = O(n³)
# The general case collapses to the familiar cubic square-matrix case.
```

**System Impact:** General matrix multiplication exposes why rectangular matrices cannot always be described simply as `O(n³)`.

---

# 💠 09 — Matrix Transposition

```c
void transpose(int a[][MAX_SIZE])
{
    int i, j;
    int temp;

    for (i = 0; i <= MAX_SIZE - 1; i++)
        for (j = i + 1; j < MAX_SIZE; j++)
            SWAP(a[i][j], a[j][i], temp);
}
```

### 🧠 Intuition

Unlike ordinary nested loops, the inner loop does **not** always execute `n` times.

Its range shrinks:

```text
i = 0 → n - 1 swaps
i = 1 → n - 2 swaps
i = 2 → n - 3 swaps
...
i = n - 1 → 0 swaps
# The triangular iteration pattern avoids processing both matrix halves.
```

### 🧪 Frequency

The swap operation occurs:

```text
(n - 1) + (n - 2) + ... + 1
```

Using the arithmetic-series formula:

```text
1 + 2 + ... + (n - 1)
= n(n - 1) / 2
# Only half of the matrix pairs need to be swapped.
```

The provided step-count result is:

```text
T(n) = 2n² + 1
T(n) = O(n²)
# Despite the triangular loop structure, the dominant growth remains quadratic.
```

> [!NOTE]
> The exact constant depends on how the course counts `SWAP`, loop initialization, comparisons, and increments. The asymptotic result remains **O(n²)**.

**System Impact:** Exploiting matrix symmetry reduces unnecessary work while preserving quadratic complexity.

---

# 💠 10 — Complexity Pattern Recognition

| Code Structure               | Typical Growth |
| ---------------------------- | -------------: |
| Single loop over `n`         |         `O(n)` |
| Two independent nested loops |        `O(n²)` |
| Three nested loops           |        `O(n³)` |
| Matrix addition              |        `O(n²)` |
| Matrix multiplication        |        `O(n³)` |
| Recursive decrement by 1     |         `O(n)` |
| Triangular nested iteration  |        `O(n²)` |

> [!IMPORTANT]
> **Do not blindly count loop depth.** Always determine the actual execution frequency.

---

# 🧪 11 — The Most Important Formulas

### Linear

```text
1 + 1 + ... + 1
= n
# Repeated constant work produces linear growth.
```

### Arithmetic Series

```text
1 + 2 + ... + n
= n(n + 1) / 2
# Triangular iteration patterns commonly produce this form.
```

### Nested Loops

```text
n × n = n²
n × n × n = n³
# Independent loop dimensions multiply their execution frequencies.
```

### Dominant Term

```text
T(n) = 2n³ + 3n² + 2n + 1

T(n) = O(n³)
# Ignore constants and lower-order terms when determining asymptotic growth.
```

---

# 🔄 12 — Analysis Workflow

```text
Source Code
    ↓
Identify statements
    ↓
Assign step cost
    ↓
Determine execution frequency
    ↓
Multiply cost × frequency
    ↓
Sum all contributions
    ↓
Obtain T(n)
    ↓
Extract dominant term
    ↓
Determine Big-O
# This workflow converts implementation into mathematical complexity.
```

---

# 🛠️ 13 — Rapid Exam Strategy

When given unfamiliar code:

### ① Find the loops

```text
How many times can each loop execute?
# Start with control flow before calculating individual instructions.
```

### ② Check dependencies

```text
for (i = 0; i < n; i++)
    for (j = i + 1; j < n; j++)
# j depends on i, so the inner loop does not execute n times every iteration.
```

### ③ Find the dominant operation

```text
c[i][j] += a[i][k] * b[k][j];
# In matrix multiplication, this operation sits inside the deepest loop.
```

### ④ Write the exact function

```text
T(n) = 2n³ + 3n² + 2n + 1
# Exact step count is useful before simplifying.
```

### ⑤ Reduce to Big-O

```text
T(n) = O(n³)
# Keep only the fastest-growing term.
```

---

# 🏁 14 — Recap

> [!IMPORTANT]
> **The central idea of Week 4:**
>
> **Step Count = Step Cost × Frequency**

### 💠 Remember

- 🧮 **Tabular Method** → calculate each statement separately.
- 🔄 **Frequency matters** → nested loops multiply execution counts.
- 🧪 **Exact count first** → derive `T(n)` before simplifying.
- ⚡ **Dominant term wins** → `2n³ + 3n² + 2n + 1 → O(n³)`.
- 📐 **Dimensions matter** → matrix multiplication can be `O(ABK)`, not always simply `O(n³)`.
- 🔺 **Triangular loops** → `1 + 2 + ... + n = n(n + 1)/2`.
- 🧠 **Never judge complexity from syntax alone** → trace actual execution frequency.

---

## 🏁 One-Minute Revision Card

```text
TABULAR METHOD
────────────────────────────────
1. Identify statement
2. Count step cost
3. Count frequency
4. Multiply
5. Sum
6. Extract dominant term
7. Write Big-O

COMMON PATTERNS
────────────────────────────────
Single loop          → O(n)
Nested 2 loops       → O(n²)
Nested 3 loops       → O(n³)
Matrix addition      → O(n²)
Matrix multiplication→ O(n³)
General multiplication
                     → O(ABK)

KEY FORMULA
────────────────────────────────
1 + 2 + ... + n
= n(n + 1) / 2
```

**System Impact:** Mastering frequency analysis lets you estimate algorithmic scalability directly from source code—an essential foundation for algorithm design and systems engineering.
