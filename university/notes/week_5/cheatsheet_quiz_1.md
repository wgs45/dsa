# 📚 Ultimate Algorithm Step-Counting & Big-O Cheat Sheet

## 1. The Line-by-Line Statement Frequency Table Method (Tabular Method)

When your professor asks for a statement frequency table, break down every single line of code.

### Standard Line Defaults (Linear Loop)

| Statement / Line Type               | Cost | Frequency (How many times it runs) | Explanation                                                                 |
| :---------------------------------- | :--: | :--------------------------------- | :-------------------------------------------------------------------------- |
| **Initialization** (`int i = 0`)    | $1$  | $1$                                | Runs once when entering the block.                                          |
| **Loop Condition** (`i < n`)        | $1$  | $n + 1$                            | Checked $n$ times when true, plus $1$ extra final check when false to exit. |
| **Loop Increment** (`i++` or `i--`) | $1$  | $n$                                | Runs exactly once per successful iteration.                                 |
| **Loop Body** (Simple code)         | $1$  | $n$                                | Executes once per iteration.                                                |

---

## 2. The 4 Golden Shapes of Code (Quick Recognition)

When you look at a loop or function, match it to one of these core patterns:

### Shape 1: The Straight Line $\rightarrow$ Linear $\mathcal{O}(n)$

- **Code Example:** Single loop (`for i = 0 to n: i++`)
- **Meaning:** Moves across $n$ items one by one.
- **Total Steps:** $\sim n$

### Shape 2: The Grid / Triangle $\rightarrow$ Quadratic $\mathcal{O}(n^2)$

- **Code Example:** Nested loops where inner loop depends on outer loop (`for i = 1 to n: for j = 1 to i`)
- **Meaning:** Forms a 2D square grid or a triangle staircase.
- **Total Steps:** $\frac{n(n+1)}{2} = \frac{n^2 + n}{2}$

### Shape 3: The Halving / Doubling $\rightarrow$ Logarithmic $\mathcal{O}(\log n)$

- **Code Example:** Multiplying/dividing loop (`i *= 2` or `i /= 2`)
- **Meaning:** Cuts the problem in half or doubles it instantly (paper-folding / binary halving).
- **Total Steps:** $\log_2(n)$

### Shape 4: Sequential Side-by-Side Loops $\rightarrow$ $\mathcal{O}(n)$

- **Code Example:** Loop A, then immediately Loop B after it (`for i...; for j...;`)
- **Meaning:** Doing tasks one after another **adds** their steps ($n + n = 2n$).
- **Total Steps:** $2n \rightarrow$ Drop constants to get $\mathcal{O}(n)$.

---

## 3. Essential Math Series & Formulas

- **Arithmetic Series (Triangles):**
  $$1 + 2 + 3 + \dots + n = \frac{n(n+1)}{2} = \frac{1}{2}n^2 + \frac{1}{2}n$$
- **Geometric Series (Powers of 2 / Doubling):**
  $$1 + 2 + 4 + 8 + \dots + n = 2n - 1$$
- **Branching Recursion (Binary Tree of Calls):**
  $$T(n) = 1 + 2 + 4 + \dots + 2^{n-1} = 2^n - 1 \quad \rightarrow \quad \mathcal{O}(2^n)$$

---

## 4. The 3 Golden Rules of Big-O Conversion

When you have a raw step count expression (e.g., $3n^2 + 5n + 10$), convert it to Big-O instantly:

1. **Find the Boss Term:** Identify the fastest-growing term as $n$ approaches infinity ($n^2$ beats $n$, $n$ beats constants).
2. **Drop the Constants:** Remove all coefficients and constant multipliers (e.g., $5n^2 \rightarrow n^2$).
3. **Wrap in $\mathcal{O}(\dots)$:** Place your dominant term inside the asymptotic notation.

### Big-O Growth Hierarchy (Fastest to Slowest)

$$\mathcal{O}(1) < \mathcal{O}(\log n) < \mathcal{O}(n) < \mathcal{O}(n \log n) < \mathcal{O}(n^2) < \mathcal{O}(2^n)$$

---

## 5. Recursion Quick Reference

| Recursion Pattern               | Behavior                          | Big-O Complexity      |
| :------------------------------ | :-------------------------------- | :-------------------- |
| `f(n - 1)` (1 call)             | Decreases by 1 each step          | $\mathcal{O}(n)$      |
| `f(n / 2)` (1 call)             | Halves input each step            | $\mathcal{O}(\log n)$ |
| `f(n - 1) + f(n - 1)` (2 calls) | Branches into two calls per level | $\mathcal{O}(2^n)$    |
