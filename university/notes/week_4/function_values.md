# 💠 Function Values & Growth-Rate Reality

> [!NOTE]
> **Idea:** Two functions can look similar for small `n`, but their execution time can become radically different as `n` grows.

> [!IMPORTANT]
> Algorithm analysis is fundamentally about understanding **how computation scales**, not merely how fast one input executes.

---

## 💠 1. Intuition — Why Growth Matters

When analyzing an algorithm, `n` represents the **problem size**.

The important question is:

> **What happens to the computation when `n` becomes very large?**

Different functions grow at dramatically different rates:

| Function   |            Growth | Intuition                                        |
| ---------- | ----------------: | ------------------------------------------------ |
| `log₂ n`   | 🟢 Extremely slow | Input can grow enormously with little extra work |
| `n`        |         🟢 Linear | Work grows directly with input                   |
| `n log₂ n` |    🟡 Near-linear | Slightly faster than linear                      |
| `n²`       |      🟠 Quadratic | Doubling `n` → ~4× work                          |
| `n³`       |          🔴 Cubic | Doubling `n` → ~8× work                          |
| `2ⁿ`       |    🔥 Exponential | Each extra input roughly doubles the work        |

> [!NOTE]
> **Growth rate dominates performance.** Constant hardware improvements cannot rescue an algorithm whose complexity explodes with `n`.

---

## 🧪 2. Formal Logic — Function Values

For `n = 0 ... 5`:

| `n` | `log₂ n` | `n` | `n log₂ n` | `n²` | `n³` | `2ⁿ` |
| --: | -------: | --: | ---------: | ---: | ---: | ---: |
|   0 |        — |   0 |          — |    0 |    0 |    1 |
|   1 |        0 |   1 |          0 |    1 |    1 |    2 |
|   2 |        1 |   2 |          2 |    4 |    8 |    4 |
|   3 |    1.585 |   3 |      4.755 |    9 |   27 |    8 |
|   4 |        2 |   4 |          8 |   16 |   64 |   16 |
|   5 |    2.322 |   5 |      11.61 |   25 |  125 |   32 |

> [!IMPORTANT]
> `log₂ n` is undefined at `n = 0`. In algorithm analysis, logarithmic functions normally assume `n ≥ 1`.

### 📈 Growth Pattern

```text
log₂ n   → very slow growth
n        → linear growth
n log n  → near-linear growth
n²       → quadratic growth
n³       → cubic growth
2ⁿ       → exponential growth
```

**System Impact:** As `n` increases, faster-growing functions quickly become the dominant performance bottleneck.

---

## ⚡ 3. Optimization — Why Small `n` Can Mislead

Consider:

```text
n = 10

n² = 100
2ⁿ = 1,024
```

The difference already exists, but it may still appear manageable.

Now consider:

```text
n = 100

n² = 10,000
2ⁿ ≈ 1.27 × 10³⁰
```

The exponential function has escaped practical scale.

### 🔄 Scaling Rule

If `n` doubles:

| Function  | Approximate increase |
| --------- | -------------------: |
| `log n`   |   +1 constant amount |
| `n`       |                 `2×` |
| `n log n` |                ~`2×` |
| `n²`      |                 `4×` |
| `n³`      |                 `8×` |
| `2ⁿ`      |   `(2ⁿ)²` → enormous |

> [!NOTE]
> This is why algorithm designers care about **asymptotic growth** rather than only benchmarking a few inputs.

---

# 🧪 4. Applied Example — 1 Billion Instructions / Second

Assume a theoretical computer executes:

```text
1,000,000,000 instructions / second
= 10⁹ instructions / second
```

Therefore:

```text
1 instruction = 10⁻⁹ seconds
              = 0.001 μs
```

If an algorithm performs `f(n)` instructions:

```text
Time ≈ f(n) / 10⁹ seconds
```

**System Impact:** The function itself determines how quickly execution time grows as the workload expands.

---

## 📊 Execution-Time Reality

Representative values from the `1 GHz / 10⁹ instructions-per-second` model:

|       `n` |     `n` | `n log₂ n` |      `n²` |        `n³` |
| --------: | ------: | ---------: | --------: | ----------: |
|        10 | 0.01 μs |    0.03 μs |    0.1 μs |        1 μs |
|        20 | 0.02 μs |    0.09 μs |    0.4 μs |        8 μs |
|        30 | 0.03 μs |    0.15 μs |    0.9 μs |       27 μs |
|        40 | 0.04 μs |    0.21 μs |    1.6 μs |       64 μs |
|        50 | 0.05 μs |    0.28 μs |    2.5 μs |      125 μs |
|       100 | 0.10 μs |    0.66 μs |     10 μs |        1 ms |
|     1,000 |    1 μs |    9.96 μs |      1 ms |         1 s |
|    10,000 |   10 μs |     130 μs |    100 ms |   16.67 min |
|   100,000 |  100 μs |    1.66 ms |      10 s |  11.57 days |
| 1,000,000 |    1 ms |   19.92 ms | 16.67 min | 31.71 years |

> [!IMPORTANT]
> Notice the transition: at `n = 1,000,000`, `n³` requires **decades**, while `n log n` still requires only milliseconds.

**System Impact:** Choosing `O(n log n)` instead of `O(n³)` can transform an otherwise impossible workload into a practical one.

---

## 🔥 5. Exponential Explosion

The most dramatic example is `2ⁿ`.

| `n` |        `2ⁿ` | Approximate Time |
| --: | ----------: | ---------------: |
|  10 |       1,024 |             1 μs |
|  20 |   1,048,576 |             1 ms |
|  30 |  1.07 × 10⁹ |             ~1 s |
|  40 | 1.10 × 10¹² |          ~18 min |
|  50 | 1.13 × 10¹⁵ |         ~13 days |
| 100 | 1.27 × 10³⁰ |  ~4 × 10¹³ years |

For comparison:

```text
Age of universe ≈ 1.38 × 10¹⁰ years
```

So an `O(2ⁿ)` algorithm at `n = 100` can require **thousands of times longer than the age of the universe** under this simplified model.

**System Impact:** Exponential algorithms become computationally infeasible surprisingly quickly, even on extremely fast hardware.

---

# 🛠️ 6. Engineering Interpretation

A useful mental hierarchy:

```text
        BEST
          │
          ▼
       log n       ← scalable
          │
          ▼
         n         ← excellent
          │
          ▼
       n log n     ← usually excellent
          │
          ▼
         n²        ← manageable at moderate n
          │
          ▼
         n³        ← dangerous at large n
          │
          ▼
         2ⁿ        ← explosive
          │
          ▼
        WORST
```

### 🧠 Practical Rule

> [!TIP]
> When optimizing an algorithm, **changing the growth rate** is usually far more powerful than optimizing individual instructions.

For example:

```text
O(n³) → O(n²)       # major algorithmic improvement
O(n²) → O(n log n)  # major scalability improvement
O(n log n) → O(n)   # excellent scalability improvement
```

**System Impact:** Algorithmic optimization attacks the source of scaling problems rather than merely making individual operations faster.

---

# 🏁 7. Recap — Interview Memory

### 💠 Core Principle

> **Small `n` hides bad algorithms. Large `n` reveals them.**

Remember the hierarchy:

```text
log n < n < n log n < n² < n³ < 2ⁿ
```

### ⚡ Fast Recall

- `log n` → barely grows
- `n` → proportional growth
- `n log n` → efficient large-scale processing
- `n²` → grows quickly
- `n³` → becomes expensive rapidly
- `2ⁿ` → exponential explosion

> [!IMPORTANT]
> **Asymptotic complexity tells you how an algorithm behaves as the input approaches infinity.**

### 🏁 Final Takeaway

```text
Hardware determines how fast we execute.

Algorithmic complexity determines
whether the computation remains feasible.
```

**System Impact:** The most valuable optimization is often not faster hardware—it is selecting an algorithm with a better asymptotic growth rate.

### 🧭 Interview Mental Model

**`O(n log n)` is not merely "a little slower than `O(n)`."** The real question is how the gap evolves as `n → ∞`.

That distinction is the foundation for understanding sorting, searching, graph algorithms, dynamic programming, and system scalability.
