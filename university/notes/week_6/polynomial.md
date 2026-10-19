# 💠 Polynomial ADT

> [!NOTE]
> **Domain:** Data Structures & Algorithms
> **Core Systems:** Abstract Data Types · Polynomial Representation · Sparse Storage · Polynomial Addition
> **Mission:** Represent, manipulate, and add polynomials efficiently.

---

## 🧭 01 · Intuition — Why Polynomials Matter

A polynomial is a collection of terms. Each term consists of a coefficient and a non-negative integer exponent.

### 💠 Core Concepts

- 💠 **Coefficient:** The numerical multiplier of a term.
- 💠 **Exponent:** The power of the variable, where `exponent >= 0`.
- 💠 **Term:** A coefficient–exponent pair, represented as `<coefficient, exponent>`.
- 💠 **Degree:** The largest exponent with a nonzero coefficient.
- 💠 **Zero polynomial:** A polynomial containing no nonzero terms.

### 🧪 Metal — Example Polynomials

```text
A(x) = 2x^1000 + 1

B(x) = x^4 + 10x^3 + 3x^2 + 1
```

| Property             |        A(x) |             B(x) |
| -------------------- | ----------: | ---------------: |
| Degree               |        1000 |                4 |
| Nonzero terms        |           2 |                4 |
| Constant coefficient |           1 |                1 |
| Storage challenge    | Very sparse | Relatively dense |

> [!IMPORTANT]
> **Degree is not the number of terms.** `A(x)` has degree 1000 but only two nonzero terms. This distinction determines which storage representation is most efficient.

---

## ⚙️ 02 · Formal Logic — Polynomial ADT

An Abstract Data Type (ADT) defines what operations are available without requiring a specific storage implementation.

### 💠 Object Model

A polynomial is represented conceptually as a collection of pairs:

```text
<exponent, coefficient>
```

Each pair represents one term. The following rules define a canonical polynomial:

- 🔹 Coefficients belong to a chosen numerical domain.
- 🔹 Exponents are non-negative integers.
- 🔹 Each exponent appears at most once.
- 🔹 Terms with zero coefficients are omitted.

### 🛠️ Operation Contract

| Operation                | Purpose                                  | Expected Result    |
| ------------------------ | ---------------------------------------- | ------------------ |
| `Zero()`                 | Create a zero polynomial                 | `P(x) = 0`         |
| `IsZero(poly)`           | Check whether a polynomial is zero       | Boolean            |
| `Coef(poly, e)`          | Retrieve the coefficient at exponent `e` | Coefficient or `0` |
| `Lead_Exp(poly)`         | Find the largest exponent                | Maximum exponent   |
| `Attach(poly, c, e)`     | Insert a term                            | Updated polynomial |
| `Remove(poly, e)`        | Delete a term                            | Updated polynomial |
| `SingleMult(poly, c, e)` | Multiply by `c * x^e`                    | Product polynomial |
| `Add(poly1, poly2)`      | Add two polynomials                      | Sum polynomial     |
| `Mult(poly1, poly2)`     | Multiply two polynomials                 | Product polynomial |

> [!NOTE]
> `Attach()` should reject duplicate exponents if the ADT requires unique exponents. `Remove()` should report an error when the requested exponent does not exist.

### 🔄 Canonical Representation

A canonical polynomial contains no duplicate exponents and no zero-coefficient terms.

For example:

```text
Original:
P(x) = 3x^4 + 2x^2 + 0x + 5

Canonical:
P(x) = 3x^4 + 2x^2 + 5
```

**System Impact:** A canonical representation simplifies addition, comparison, and multiplication by eliminating redundant terms.

---

## 🧱 03 · Storage Architecture I — Dense Coefficient Array

### 💠 Intuition — Direct Indexing

Store each coefficient at the array index matching its exponent.

For a polynomial of degree `d`, allocate `d + 1` coefficient slots.

### 🛠️ C Implementation

```c
#define MAX_DEGREE 101

typedef struct {
    int degree;                 // Highest nonzero exponent
    float coef[MAX_DEGREE];     // coef[i] is the coefficient of x^i
} Polynomial;
```

**System Impact:** Direct indexing provides O(1) coefficient access, but memory consumption depends on the maximum degree rather than the number of nonzero terms.

### 🧪 Metal — Array Representation

Given:

```text
B(x) = x^4 + 10x^3 + 3x^2 + 1
```

The coefficients are stored by exponent:

| Exponent / Index |   0 |   1 |   2 |   3 |   4 |   5 |   6 |   7 |   8 |
| ---------------- | --: | --: | --: | --: | --: | --: | --: | --: | --: |
| `coef[i]`        |   1 |   0 |   3 |  10 |   1 |   0 |   0 |   0 |   0 |

```c
Polynomial b = {
    .degree = 4,
    .coef = {1, 0, 3, 10, 1}
};

// b.coef[0] = 1;   // Constant term
// b.coef[2] = 3;   // Coefficient of x^2
// b.coef[4] = 1;   // Leading coefficient
```

**System Impact:** The exponent is implicit in the array index, so no separate exponent field is needed for each coefficient.

### ⚡ Sparse-Polynomial Stress Test

Consider:

```text
A(x) = 2x^1000 + 1
```

A dense array needs 1001 coefficient slots because the exponent ranges from 0 to 1000.

| Metric                     | Value |
| -------------------------- | ----: |
| Maximum exponent           |  1000 |
| Nonzero terms              |     2 |
| Required coefficient slots |  1001 |
| Useful coefficient slots   |     2 |
| Zero-valued slots          |   999 |

> [!WARNING]
> `MAX_DEGREE 101` permits indices 0 through 100. It cannot represent `x^1000`. The capacity must be increased or another representation selected.

### ⚖️ Dense Array — Trade-offs

| Advantages                                      | Disadvantages                        |
| ----------------------------------------------- | ------------------------------------ |
| Simple implementation                           | Wastes memory for sparse polynomials |
| O(1) coefficient lookup                         | Maximum degree limited by capacity   |
| Easy coefficient updates                        | Traversal may visit many zeros       |
| Convenient representation for dense polynomials | Storage grows with degree            |

---

## 🧬 04 · Storage Architecture II — Sparse Term Array

### 💠 Intuition — Store Only What Exists

Instead of allocating a slot for every possible exponent, store only nonzero terms.

Each entry contains two fields:

- `coef`: The coefficient.
- `expon`: The exponent.

### 🛠️ C Implementation

```c
#define MAX_TERMS 100

typedef struct {
    float coef;                 // Coefficient of this term
    int expon;                  // Exponent of this term
} Term;

Term terms[MAX_TERMS];          // Shared term storage
int avail = 0;                  // Index of the next free slot
```

**System Impact:** Memory usage scales with the number of stored terms, making sparse arrays efficient for high-degree polynomials with few nonzero coefficients.

### 🧪 Metal — Representation

Given:

```text
A(x) = 2x^1000 + 1

B(x) = x^4 + 10x^3 + 3x^2 + 1
```

Assume terms are stored in ascending exponent order.

| Index | `coef` | `expon` | Polynomial |
| ----: | -----: | ------: | ---------- |
|     0 |      1 |       0 | A          |
|     1 |      2 |    1000 | A          |
|     2 |      1 |       0 | B          |
|     3 |      3 |       2 | B          |
|     4 |     10 |       3 | B          |
|     5 |      1 |       4 | B          |

Each polynomial is described by an inclusive index range.

| Polynomial | `start` | `finish` | Number of terms |
| ---------- | ------: | -------: | --------------: |
| A          |       0 |        1 |               2 |
| B          |       2 |        5 |               4 |

Here, `avail = 6`, meaning index 6 is the next free slot.

### ⚡ Storage Comparison

Let:

- `d` = maximum degree.
- `t` = number of nonzero terms.

| Property                  | Dense Array       | Sparse Term Array               |
| ------------------------- | ----------------- | ------------------------------- |
| Storage complexity        | O(d + 1)          | O(t)                            |
| Coefficient lookup        | O(1)              | O(t) with linear search         |
| Sequential term traversal | O(d + 1)          | O(t)                            |
| Best use case             | Dense polynomials | Sparse polynomials              |
| Metadata overhead         | Low               | Stores coefficient and exponent |
| Main limitation           | Many unused slots | Capacity and index management   |

> [!IMPORTANT]
> Sparse storage is not always better. When most coefficients are nonzero, storing a separate exponent for every term may consume more memory than a dense coefficient array.

### 🛠️ Append a Term

```c
#include <stdio.h>
#include <stdlib.h>

void attach(float coefficient, int exponent)
{
    // Check whether shared storage has reached capacity
    if (avail >= MAX_TERMS) {
        fprintf(stderr, "Too many polynomial terms\n");
        exit(EXIT_FAILURE);
    }

    // Store the term and advance the next-free index
    terms[avail].coef = coefficient;
    terms[avail].expon = exponent;
    avail++;
}
```

**System Impact:** Appending a term takes O(1) time while capacity remains available.

### 🔄 Shared-Array Lifecycle

Several polynomials can share the same `terms[]` array by maintaining their own `start` and `finish` indices.

When a polynomial is no longer needed, its slots may remain occupied. Reclaiming those slots can require **compaction**, which moves live terms to eliminate gaps.

> [!WARNING]
> Moving terms can invalidate stored indices. Every affected polynomial range must be updated, or a different memory-management strategy must be used.

---

## 🔗 05 · Polynomial Addition — Merge Algorithm

### 💠 Intuition — Merge Two Sorted Sequences

When both polynomials are sorted by exponent, addition resembles merging two sorted arrays.

- If the exponents differ, copy the term with the smaller exponent.
- If the exponents match, add their coefficients.
- If the resulting coefficient is zero, omit that term.
- Once one polynomial is exhausted, copy the remaining terms from the other.

### 🧪 Formal Logic

Example input:

```text
A(x) = 2x^1000 + 1

B(x) = x^4 + 10x^3 + 3x^2 + 1
```

Expected result:

```text
D(x) = A(x) + B(x)

D(x) = 2x^1000 + x^4 + 10x^3 + 3x^2 + 2
```

The constant terms combine:

```text
1 + 1 = 2
```

All other exponents differ, so those terms are copied unchanged.

### 🔄 Merge Workflow

1. Initialize the result's starting index.
2. Compare the current exponents of both input polynomials.
3. Append the term with the smaller exponent.
4. When exponents match, add coefficients and skip a zero sum.
5. Advance both input indices after matching exponents.
6. Append any remaining terms.
7. Record the result's final index.

> [!NOTE]
> The implementation below assumes ascending exponent order. If terms are stored in descending order, reverse the comparison logic.

### 🛠️ C Implementation

```c
#include <stdio.h>
#include <stdlib.h>

#define MAX_TERMS 100

typedef struct {
    float coef;                 // Term coefficient
    int expon;                  // Term exponent
} Term;

Term terms[MAX_TERMS];
int avail = 0;

// Append a term to the shared array
void attach(float coefficient, int exponent)
{
    if (avail >= MAX_TERMS) {
        fprintf(stderr, "Too many polynomial terms\n");
        exit(EXIT_FAILURE);
    }

    terms[avail++] = (Term){coefficient, exponent};
}

// Add A and B, storing the result in the shared array
void padd(int starta, int finisha,
          int startb, int finishb,
          int *startd, int *finishd)
{
    int a = starta;
    int b = startb;

    *startd = avail;            // Record result's first index

    while (a <= finisha && b <= finishb) {
        if (terms[a].expon < terms[b].expon) {
            // A has the smaller exponent
            attach(terms[a].coef, terms[a].expon);
            a++;
        } else if (terms[a].expon > terms[b].expon) {
            // B has the smaller exponent
            attach(terms[b].coef, terms[b].expon);
            b++;
        } else {
            // Matching exponents: combine coefficients
            float sum = terms[a].coef + terms[b].coef;

            if (sum != 0.0f) {
                attach(sum, terms[a].expon);
            }

            a++;
            b++;
        }
    }

    // Copy remaining terms from A
    while (a <= finisha) {
        attach(terms[a].coef, terms[a].expon);
        a++;
    }

    // Copy remaining terms from B
    while (b <= finishb) {
        attach(terms[b].coef, terms[b].expon);
        b++;
    }

    *finishd = avail - 1;       // Record result's last index
}
```

**System Impact:** Each input term is processed at most once, so addition runs in linear time relative to the combined number of terms.

### ⚡ Complexity Analysis

Let:

- `n` = number of nonzero terms in A.
- `m` = number of nonzero terms in B.

| Operation            | Time Complexity      |
| -------------------- | -------------------- |
| Main merge loop      | O(n + m)             |
| Copy remaining terms | O(n + m) worst case  |
| Total addition       | O(n + m)             |
| Output storage       | O(n + m) upper bound |

The result may contain fewer than `n + m` terms because matching exponents can cancel to zero.

### 🔧 Important Corrections to the Original Notes

- 💠 The output parameter must be `int *finishd`, not `int _finishd`, because the function must return the result's ending index to its caller.
- 💠 Both output indices are passed by address.
- 💠 The result must have enough free shared-array capacity.
- 💠 Inputs must be sorted consistently by exponent.
- 💠 The output must not overwrite input terms that have yet to be processed.

---

## 🚧 06 · Edge Cases & System Invariants

### 🔒 Correctness Checklist

- [ ] Input terms are sorted in the same exponent order.
- [ ] Each input exponent occurs at most once.
- [ ] Zero-coefficient terms are excluded from canonical output.
- [ ] Array capacity is checked before every append.
- [ ] Result `start` and `finish` indices are correct.
- [ ] The zero polynomial has a defined representation.
- [ ] Input ranges remain valid while the result is appended.

### ⚠️ Implementation Details

**1. Empty output**

If no terms are appended, `finishd = avail - 1` produces an ending index smaller than the starting index.

This is acceptable only if the ADT defines that convention for an empty polynomial.

**2. Floating-point cancellation**

The condition `sum != 0.0f` uses exact floating-point comparison. For applications requiring numerical tolerance, a documented threshold may be more appropriate.

**3. Shared-array capacity**

The input polynomials may fit in the array while the output does not. The implementation must reserve enough free slots for the result.

**4. Input/output overlap**

The algorithm assumes output terms are appended after the input ranges. Writing into an input range prematurely could corrupt terms that have not yet been processed.

**System Impact:** Clearly defined invariants prevent memory errors, malformed representations, and incorrect polynomial results.

---

## 🏁 07 · Recap — Interview Quick Reference

### 💠 Core Takeaways

- **ADT:** Defines polynomial operations independently of the storage implementation.
- **Dense array:** Stores coefficient `a[i]` at index `i`.
- **Sparse array:** Stores only nonzero coefficient–exponent pairs.
- **Addition:** Merges sorted terms and combines matching exponents.
- **Time complexity:** O(n + m) for two sorted sparse polynomials.
- **Memory management:** Shared arrays require capacity checks and a strategy for reclaiming unused slots.

### ⚡ Rapid Revision Table

| Interview Question                           | Answer                                                                     |
| -------------------------------------------- | -------------------------------------------------------------------------- |
| Why can dense storage waste memory?          | It allocates slots for zero coefficients.                                  |
| Why store exponents in a sparse array?       | Terms can have widely separated exponents.                                 |
| Why sort terms before addition?              | Sorting enables linear-time merging.                                       |
| When are coefficients combined?              | When their exponents match.                                                |
| Why omit a zero sum?                         | To preserve canonical form.                                                |
| What is addition's time complexity?          | O(n + m).                                                                  |
| What is the main weakness of shared storage? | Capacity management and compaction.                                        |
| Why does degree differ from term count?      | Degree is the highest exponent; term count is the number of nonzero terms. |
| When is a dense array preferable?            | When most coefficients are nonzero and the degree is manageable.           |
| When is a sparse array preferable?           | When the degree is high but only a few terms are nonzero.                  |

> [!SUCCESS]
> **Architect's Principle:** Choose a data representation based on the shape of the data, not merely its maximum size. Dense storage optimizes direct access; sparse storage optimizes space for missing terms.
