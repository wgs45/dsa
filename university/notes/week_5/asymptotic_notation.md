# 💠 Asymptotic Notation — Ω (Omega) & Θ (Theta)

> [!NOTE]
> **Domain:** Data Structures & Algorithms · Complexity Analysis
> **Objective:** Understand lower bounds and tight asymptotic bounds for algorithm growth.

---

## 💠 01. Intuition — Why It Matters

Asymptotic notation describes how an algorithm's resource usage grows as the input size `n` becomes large.

It helps us compare algorithms without being distracted by constant factors or lower-order terms.

| Notation      | Meaning                    | Intuition                       |
| ------------- | -------------------------- | ------------------------------- |
| **Ω (Omega)** | Asymptotic lower bound     | At least this fast-growing      |
| **Θ (Theta)** | Asymptotically tight bound | Grows at the same order         |
| **O (Big O)** | Asymptotic upper bound     | Grows no faster than this order |

> [!IMPORTANT]
> **Omega is a lower bound; Theta is a matching upper and lower bound.** These describe mathematical growth bounds, not automatically the best-case or worst-case runtime.

---

## 🧪 02. Formal Logic — Ω (Omega)

### Definition

A function `f(n)` belongs to `Ω(g(n))` if there exist positive constants `c` and `n₀` such that:

```text
f(n) >= c * g(n)    # For every n >= n₀
```

Formally:

```text
f(n) = Ω(g(n))
```

- 💠 `f(n)`: Function being analyzed.
- 💠 `g(n)`: Asymptotic lower-bound function.
- 💠 `c > 0`: Positive scaling constant.
- 💠 `n₀`: Threshold beyond which the bound holds.

### Intuition

After some threshold `n₀`, the function `f(n)` never falls below `c * g(n)`.

The lower bound describes a guaranteed minimum growth rate for sufficiently large inputs.

### 🛠️ Polynomial Rule

For a polynomial:

```text
f(n) = aₘnᵐ + ... + a₁n + a₀

# If aₘ > 0 and m >= 0:
f(n) = Ω(nᵐ)
```

The highest-degree term determines the asymptotic growth rate of a polynomial with a positive leading coefficient.

### ⚡ Applied Example — Metal

Consider:

```python
def process(n):
    total = 0

    for i in range(n):
        total += i  # Executes n times

    return total
```

The loop performs `n` iterations, so its runtime is linear.

```text
T(n) = an + b, where a > 0

# For sufficiently large n:
T(n) = Ω(n)
```

**System Impact:** Omega establishes that the runtime grows at least linearly, excluding implementations whose runtime eventually grows more slowly than a positive linear function.

---

## 🧪 03. Formal Logic — Θ (Theta)

### Definition

A function `f(n)` belongs to `Θ(g(n))` if there exist positive constants `c₁`, `c₂`, and `n₀` such that:

```text
c₁ * g(n) <= f(n) <= c₂ * g(n)  # For every n >= n₀
```

Formally:

```text
f(n) = Θ(g(n))
```

- 💠 `c₁ * g(n)`: Asymptotic lower bound.
- 💠 `c₂ * g(n)`: Asymptotic upper bound.
- 💠 `n₀`: Threshold beyond which both bounds hold.

### Intuition

After `n₀`, the function stays between two positive constant multiples of `g(n)`.

Therefore, `f(n)` and `g(n)` have the same asymptotic order of growth.

### 🛠️ Polynomial Rule

For a polynomial:

```text
f(n) = aₘnᵐ + ... + a₁n + a₀

# If aₘ > 0 and m >= 0:
f(n) = Θ(nᵐ)
```

### ⚡ Applied Example — Metal

Consider the same linear-time algorithm:

```python
def process(n):
    total = 0

    for i in range(n):
        total += i  # Linear work

    return total
```

Its runtime can be expressed as:

```text
T(n) = an + b, where a > 0

# For sufficiently large n:
T(n) = Θ(n)
```

For sufficiently large `n`, positive constants can bound `T(n)` both above and below by multiples of `n`.

**System Impact:** Theta gives a tight asymptotic characterization of the runtime, showing that its growth is linear rather than merely bounded below by a linear function.

---

## 💠 04. Visual Logic — Bound Comparison

Imagine `f(n)` as the runtime curve and `g(n)` as the reference growth curve.

### Ω — Lower Bound

```text
Runtime
  ^
  |              f(n)
  |            /
  |          /       # f(n) stays above c * g(n)
  |   c*g(n)/
  |       /
  +--------------------> n
          n₀
```

### Θ — Tight Bound

```text
Runtime
  ^
  |             c₂*g(n)
  |           /  f(n)
  |         /  /
  |        / /       # f(n) stays between both bounds
  |       / c₁*g(n)
  +--------------------> n
          n₀
```

> [!NOTE]
> These diagrams are conceptual. Formal asymptotic bounds depend on inequalities holding for every `n >= n₀`, not merely on how curves appear in a sketch.

---

## ⚡ 05. Optimization — Ω vs. Θ

| Property      | Ω (Omega)                                                 | Θ (Theta)                                             |
| ------------- | --------------------------------------------------------- | ----------------------------------------------------- |
| Bound type    | Lower                                                     | Tight                                                 |
| Inequality    | `c * g(n) <= f(n)`                                        | `c₁ * g(n) <= f(n) <= c₂ * g(n)`                      |
| Constants     | One positive constant                                     | Two positive constants                                |
| Main question | How slowly can the function grow, relative to this bound? | Does the function grow at this same asymptotic order? |
| Example       | `3n² + 5n + 2 = Ω(n²)`                                    | `3n² + 5n + 2 = Θ(n²)`                                |

### 🔒 Critical Distinction

A lower bound does not need to be tight.

For example:

```text
f(n) = 3n² + 5n + 2

# All of these are valid lower bounds:
f(n) = Ω(n)
f(n) = Ω(n²)

# But the tight polynomial order is:
f(n) = Θ(n²)
```

**System Impact:** Recognizing valid but non-tight bounds prevents overly vague complexity claims and helps you identify the most informative asymptotic classification.

---

## 🔄 06. Workflow — How to Analyze a Polynomial

Follow this procedure when determining the asymptotic complexity of a polynomial.

1. 🧪 Identify the highest-degree term.
2. 💠 Confirm that its leading coefficient is positive.
3. ⚡ Ignore lower-order terms when determining the polynomial's asymptotic order.
4. 🛠️ State the tight bound using `Θ`.
5. 🏁 State an `Ω` lower bound when the problem asks for one.

### Example

```text
f(n) = 7n³ + 4n² + 9n + 12

# Highest-degree term:
7n³

# Tight asymptotic bound:
f(n) = Θ(n³)

# Valid lower bound:
f(n) = Ω(n³)
```

**System Impact:** This workflow reduces polynomial complexity analysis to identifying the dominant term instead of comparing every term individually.

---

## 🏁 07. Recap — Interview Flashcards

- 💠 **What does Ω mean?** A function eventually grows at least as fast as a positive constant multiple of its reference function.
- 💠 **What does Θ mean?** A function is eventually bounded both above and below by positive constant multiples of its reference function.
- ⚡ **Why does the highest-degree polynomial term dominate?** Lower-order terms become insignificant relative to the leading term as `n` approaches infinity.
- 🔒 **Does `Ω(n²)` mean the function is exactly quadratic?** No. It establishes a quadratic lower bound, not a tight growth classification.
- 🛠️ **When can you claim `Θ(n²)` for a polynomial?** When the polynomial has degree two and a positive leading coefficient.

> [!IMPORTANT]
> **Memory anchor**
>
> - `Ω` → Lower bound.
> - `Θ` → Tight bound.
> - Highest polynomial degree → Tight asymptotic order, assuming a positive leading coefficient.

---

## 🔄 08. Knowledge Check

Determine the tight asymptotic bound for each function.

1. `f(n) = 5n + 100`
2. `f(n) = 4n² + 3n + 8`
3. `f(n) = 9n³ + 2n² + n`

<details>
<summary>🧪 Reveal Answers</summary>

1. `Θ(n)` — The linear term dominates.
2. `Θ(n²)` — The quadratic term dominates.
3. `Θ(n³)` — The cubic term dominates.

All three functions also belong to their corresponding `Ω` classes.

</details>

---

**🏁 Final Takeaway:** Omega tells you what growth rate a function eventually cannot fall below; Theta tells you its exact asymptotic order up to constant factors. Use the dominant polynomial term to identify the tight bound quickly.
