# 💠 Asymptotic Notation — Big-O

> [!NOTE]
> **Core Theme:** As `n` becomes large, lower-order terms and constant factors become less important compared with the dominant term.

---

## 💠 01 — Intuition: What Happens as `n` Grows?

Consider:

```text
f(n) = 5n + 100
# The linear term grows with n, while 100 remains constant.
```

When `n` is small, `100` has a noticeable effect.

When `n` becomes very large, `100` becomes insignificant compared with `5n`.

### 📈 Growth Observation

|    n |        `5n + 100` | `(5n + 100) / 5n` |
| ---: | ----------------: | ----------------: |
|    1 |               105 |            21.000 |
|  10² |               600 |             1.200 |
|  10⁴ |            50,100 |             1.002 |
|  10⁶ |         5,000,100 |           1.00002 |
|  10⁸ |       500,000,100 |         ≈ 1.00000 |
| 10¹⁰ |    50,000,000,100 |         ≈ 1.00000 |
| 10¹² | 5,000,000,000,100 |         ≈ 1.00000 |

As `n → ∞`:

```text
5n + 100
      ↓
     5n
# The constant term becomes negligible.
```

Therefore:

```text
5n + 100 = O(n)
# Both functions have linear asymptotic growth.
```

**System Impact:** Big-O lets us ignore details that become insignificant at large input sizes.

---

# 🧪 02 — Lower-Order Terms Disappear

Consider:

```text
f(n) = 3n² - 6n + 50
# The quadratic term grows much faster than the linear and constant terms.
```

Compare the complete function with its dominant term:

```text
(3n² - 6n + 50) / 3n²
# The ratio approaches 1 as n becomes very large.
```

|    n | `3n² - 6n + 50` | Ratio to `3n²` |
| ---: | --------------: | -------------: |
|    1 |              47 |          15.67 |
|  10² |          29,450 |       ≈ 0.9817 |
|  10⁴ |     299,940,050 |       ≈ 0.9998 |
|  10⁶ |      ≈ 3 × 10¹² |       ≈ 1.0000 |
|  10⁸ |      ≈ 3 × 10¹⁶ |       ≈ 1.0000 |
| 10¹² |      ≈ 3 × 10²⁴ |       ≈ 1.0000 |

Therefore:

```text
3n² - 6n + 50 = O(n²)
# The n² term dominates as n grows.
```

> [!IMPORTANT]
> **Dominant-term rule:**
> `3n² - 6n + 50 → O(n²)`
> because `n²` grows faster than `n` and constants.

**System Impact:** Asymptotic analysis focuses on scalability rather than the exact operation count for small inputs.

---

# 💠 03 — Formal Definition of Big-O

The formal definition is:

```text
f(n) = O(g(n))

iff there exist positive constants c and n₀
such that:

f(n) ≤ c · g(n)

for every n ≥ n₀.
# g(n) provides an asymptotic upper bound for f(n).
```

### 🔐 Three Important Components

| Symbol | Meaning                                  |
| ------ | ---------------------------------------- |
| `f(n)` | Function being analyzed                  |
| `g(n)` | Proposed upper-bound function            |
| `c`    | Positive constant scaling the bound      |
| `n₀`   | Threshold from which the bound must hold |

Visually:

```text
          c·g(n)
         ╱
        ╱
       ╱
      ╱
 f(n) ─────────────
      ↑
     n₀

# After n₀, f(n) remains at or below c·g(n).
```

> [!NOTE]
> Big-O does **not** describe the exact runtime. It describes an asymptotic **upper bound**.

**System Impact:** Big-O provides a machine-independent way to reason about algorithm scalability.

---

# 🧪 04 — Proving Big-O

To prove:

```text
f(n) = O(g(n))
# We need to find valid c and n₀.
```

The general process is:

```text
1. Start with f(n)
2. Find a constant c
3. Find a threshold n₀
4. Show f(n) ≤ c·g(n)
5. Confirm the inequality holds for every n ≥ n₀
# A Big-O proof establishes an upper bound mathematically.
```

---

# ⚡ 05 — Example: `3n + 2 = O(n)`

We want:

```text
3n + 2 ≤ c·n
# Find constants c and n₀ that make this true.
```

Choose:

```text
c = 4
# We need 3n + 2 ≤ 4n.
```

This becomes:

```text
3n + 2 ≤ 4n
2 ≤ n
# Therefore the inequality holds when n ≥ 2.
```

So:

```text
c = 4
n₀ = 2

3n + 2 = O(n)
# A valid upper bound has been established.
```

**System Impact:** Constant factors do not change the asymptotic classification.

---

# ⚡ 06 — Example: `3n + 3 = O(n)`

Choose:

```text
c = 4
# We want 3n + 3 ≤ 4n.
```

Then:

```text
3n + 3 ≤ 4n
3 ≤ n
# The inequality holds for n ≥ 3.
```

Therefore:

```text
3n + 3 = O(n)
# Linear growth remains linear regardless of the constant addition.
```

---

# ⚡ 07 — Example: `100n + 6 = O(n)`

Choose:

```text
c = 101
# We want 100n + 6 ≤ 101n.
```

Then:

```text
100n + 6 ≤ 101n
6 ≤ n
# The inequality holds for n ≥ 6.
```

Therefore:

```text
100n + 6 = O(n)
# Even a large constant multiplier does not change the growth class.
```

> [!IMPORTANT]
> The original notes use `n ≥ 10`, which is also valid because `n ≥ 10` satisfies `n ≥ 6`. Big-O proofs can have multiple valid choices of `c` and `n₀`.

**System Impact:** Big-O deliberately abstracts away constant factors because they do not affect asymptotic growth.

---

# 🧪 08 — Example: `10n² + 4n + 2 = O(n²)`

We want:

```text
10n² + 4n + 2 ≤ c·n²
# Convert lower-order terms into multiples of n².
```

For `n ≥ 1`:

```text
4n ≤ 4n²
2 ≤ 2n²

Therefore:

10n² + 4n + 2
≤ 10n² + 4n² + 2n²
= 16n²
# Every lower-order term can be bounded by n².
```

Thus:

```text
c = 16
n₀ = 1

10n² + 4n + 2 = O(n²)
# The quadratic term determines the asymptotic class.
```

> [!NOTE]
> The original example uses a tighter bound (`11n²` for sufficiently large `n`). The exact constant is irrelevant to the final Big-O classification.

**System Impact:** Polynomial expressions are classified by their highest-order term.

---

# ⚡ 09 — Example: `6·2ⁿ + n² = O(2ⁿ)`

This example demonstrates an important comparison:

```text
2ⁿ grows faster than n²
# Exponential growth eventually dominates polynomial growth.
```

For sufficiently large `n`:

```text
n² ≤ 2ⁿ
# The polynomial term can be bounded by the exponential term.
```

Therefore:

```text
6·2ⁿ + n²
≤ 6·2ⁿ + 2ⁿ
= 7·2ⁿ
# Both terms can be expressed using the exponential upper bound.
```

Thus:

```text
6·2ⁿ + n² = O(2ⁿ)
# Exponential growth dominates the polynomial term.
```

**System Impact:** Recognizing growth-rate relationships is essential when simplifying complex runtime functions.

---

# 💠 10 — The Dominant-Term Rule

For a polynomial:

```text
f(n) = aₘnᵐ + ... + a₂n² + a₁n + a₀
# Assume the leading coefficient aₘ is positive.
```

The highest-order term dominates:

```text
f(n) = O(nᵐ)
# Lower-order terms become asymptotically insignificant.
```

### Example

```text
f(n) = 7n⁴ + 20n³ + 100n² + 500n + 42

f(n) = O(n⁴)
# n⁴ grows faster than every lower-order term.
```

### ⚡ Mental Shortcut

```text
7n⁴ + 20n³ + 100n² + 500n + 42
 ↓
keep the fastest-growing term
 ↓
n⁴
 ↓
O(n⁴)
# Remove coefficients, lower-order terms, and constants.
```

**System Impact:** The dominant-term rule turns complicated polynomial runtimes into rapidly recognizable complexity classes.

---

# 🧪 11 — Common Complexity Classes

| Complexity   | Name         | Growth                 |
| ------------ | ------------ | ---------------------- |
| `O(1)`       | Constant     | Does not grow with `n` |
| `O(log n)`   | Logarithmic  | Very slow growth       |
| `O(n)`       | Linear       | Proportional to `n`    |
| `O(n log n)` | Linearithmic | Slightly above linear  |
| `O(n²)`      | Quadratic    | Proportional to `n²`   |
| `O(n³)`      | Cubic        | Proportional to `n³`   |
| `O(2ⁿ)`      | Exponential  | Extremely rapid growth |

### 📈 Growth Hierarchy

```text
O(1)
  ↓
O(log n)
  ↓
O(n)
  ↓
O(n log n)
  ↓
O(n²)
  ↓
O(n³)
  ↓
O(2ⁿ)
# Each step generally scales worse as n becomes large.
```

> [!IMPORTANT]
> This ordering describes **asymptotic growth**, not necessarily actual runtime on every machine or for every input size.

**System Impact:** Complexity classes allow algorithms to be compared independently of hardware and implementation details.

---

# 💠 12 — What Big-O Ignores

Consider:

```text
T₁(n) = 100n + 50
T₂(n) = n + 5000
# Both are linear functions.
```

Asymptotically:

```text
T₁(n) = O(n)
T₂(n) = O(n)
# Big-O assigns both functions the same growth class.
```

Big-O ignores:

- ⚡ Constant multipliers
- 📉 Lower-order terms
- 🔢 Constant additions
- 🖥️ Machine-specific execution time

> [!NOTE]
> This does **not** mean constants are irrelevant in real systems. A `100n` algorithm can be much slower than an `n` algorithm for practical input sizes.

**System Impact:** Big-O captures scalability, while real-world performance also depends on constants, hardware, memory access, compiler behavior, and implementation.

---

# 🔄 13 — From Exact Count → Big-O

This is the connection to **Week 4 Step Counting**.

Suppose step counting gives:

```text
T(n) = 2n³ + 3n² + 2n + 1
# This is the exact mathematical step-count model.
```

Now apply asymptotic simplification:

```text
T(n)
    ↓
2n³ + 3n² + 2n + 1
    ↓
keep dominant term
    ↓
2n³
    ↓
ignore constant factor
    ↓
O(n³)
# Exact analysis becomes asymptotic classification.
```

**System Impact:** Step counting tells us **how much work** occurs; Big-O tells us **how that work scales**.

---

# 🛠️ 14 — Exam / Interview Workflow

```text
SOURCE CODE
    ↓
Count operations
    ↓
Determine frequency
    ↓
Build T(n)
    ↓
Identify highest-growth term
    ↓
Remove coefficient
    ↓
Write Big-O
# Convert implementation into an asymptotic growth class.
```

### ⚡ Rapid Recognition

```text
5n + 100
→ O(n)

3n² - 6n + 50
→ O(n²)

2n³ + 10n² + 7
→ O(n³)

6·2ⁿ + n²
→ O(2ⁿ)
# Always identify which term grows fastest.
```

---

# 🏁 15 — Recap

> [!IMPORTANT]
> **Big-O answers one fundamental question:**
>
> **"How does the amount of work grow as the input size becomes very large?"**

### 💠 Core Rules

- 📈 **Lower-order terms fade:** `n² + n → n²`
- 🔢 **Constants disappear:** `100n → O(n)`
- 🧪 **Dominant term wins:** `3n³ + 2n² + n → O(n³)`
- 🔒 **Big-O is an upper bound:** `f(n) ≤ c·g(n)` for `n ≥ n₀`
- 🧠 **Exact count ≠ Big-O:** `2n³ + 3n² + 1 → O(n³)`
- ⚡ **Exponential beats polynomial:** `n² → O(2ⁿ)`
- 📐 **Growth matters more than exact values** when `n` becomes large.

---

## 🏁 One-Minute Revision Card

```text
BIG-O
────────────────────────────────
f(n) = O(g(n))

Means:
∃ c > 0 and n₀ > 0 such that

f(n) ≤ c·g(n)
for all n ≥ n₀

POLYNOMIAL RULE
────────────────────────────────
aₘnᵐ + ... + a₁n + a₀
            ↓
          O(nᵐ)

COMMON CLASSES
────────────────────────────────
O(1)       Constant
O(log n)   Logarithmic
O(n)       Linear
O(n log n) Linearithmic
O(n²)      Quadratic
O(n³)      Cubic
O(2ⁿ)      Exponential

MENTAL MODEL
────────────────────────────────
Exact Steps
     ↓
T(n)
     ↓
Dominant Term
     ↓
Big-O
     ↓
Scalability
```

**System Impact:** Big-O is the bridge between low-level operation counting and high-level algorithmic scalability—the foundation for evaluating whether a system will remain efficient as data grows.
