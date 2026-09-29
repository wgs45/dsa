# 💠 Week 2 — Algorithm Analysis: Time & Space Complexity

> [!NOTE]
> **Objective:** Understand how programs consume **memory** and **execution steps** as input grows.

---

## 💠 01. Space Complexity — Memory Footprint

### 🧪 Intuition — Why?

**Space complexity** measures how much additional memory a program requires during execution.

Think of memory as the program's **workspace**:

- 🧠 Variables occupy memory.
- 📦 Arrays occupy memory proportional to their size.
- 🔄 Recursive calls create additional stack frames.
- ⚡ A program can therefore use more memory even when its algorithmic result is simple.

The key question is:

> **How much memory does the program need as the input size `n` changes?**

---

### 🧪 Formal Logic — How?

For an algorithm `P`:

```text
S(P) = Fixed Space + Variable Space
# Total memory = memory independent of input + memory dependent on input
```

If the algorithm only uses a fixed number of variables:

```text
S(P) = O(1)
# Constant auxiliary space
```

If memory grows with the input:

```text
S(P) = O(n)
# Linear auxiliary space
```

---

### 🛠️ Applied Example — Constant Space

```c
float abc(float a, float b, float c) {
    return a + b + b * c + (a + b - c) / (a + b) + 4.00;
    # Uses only a fixed number of parameters and temporary values
}
```

```text
S_abc(n) = 0
# No additional variable-sized storage is required
```

The important idea is not the exact byte count, but that the required memory **does not grow with `n`**.

---

### 🛠️ Applied Example — Iterative Summation

```c
float sum(float list[], int n) {
    float tempsum = 0;
    int i;

    for (i = 0; i < n; i++)
        tempsum += list[i];

    return tempsum;
}
```

The input array `list[]` already exists, while the function itself only needs:

- `tempsum`
- `i`
- fixed bookkeeping

```text
S_sum(n) = O(1)
# The loop increases execution time, not the function's auxiliary memory
```

> [!IMPORTANT]
> **Iteration does not automatically mean extra space.**
> A loop can execute `n` times while still using `O(1)` auxiliary space.

---

## 🔄 02. Recursion & Stack Space

### 💠 Intuition — Why?

Recursion behaves differently because **every active function call needs its own stack frame**.

Consider:

```c
float rsum(float list[], int n) {
    if (n)
        return rsum(list, n - 1) + list[n - 1];

    return 0;
}
```

For `n = 4`, the calls conceptually form:

```text
rsum(4)
 └─ rsum(3)
     └─ rsum(2)
         └─ rsum(1)
             └─ rsum(0)
```

The calls cannot disappear until deeper calls return.

---

### 🧪 Formal Logic — Stack Growth

Each recursive call requires approximately:

| Stack component     |         Size |
| ------------------- | -----------: |
| `float` parameter   |      4 bytes |
| `integer` parameter |      4 bytes |
| Return address      |      4 bytes |
| **Total**           | **12 bytes** |

Therefore:

```text
S_rsum(n) = 12n
# There are approximately n active recursive stack frames
```

So asymptotically:

```text
S_rsum(n) = O(n)
# Recursive depth grows linearly with n
```

---

### ⚡ Optimization — Iteration vs Recursion

| Approach           | Time | Auxiliary Space | Main Cost      |
| ------------------ | ---: | --------------: | -------------- |
| Iterative `sum()`  | O(n) |            O(1) | Loop execution |
| Recursive `rsum()` | O(n) |            O(n) | Call stack     |

> [!NOTE]
> Both approaches perform roughly the same amount of summation work, but recursion introduces **stack-space overhead**.

**System Impact:** Recursion can make code elegant, but deep recursion increases memory consumption and may eventually cause stack overflow.

---

# ⚡ 03. Time Complexity

### 💠 Intuition — Why?

Space complexity asks:

> **"How much memory?"**

Time complexity asks:

> **"How much computational work?"**

A program's execution time can be represented conceptually as:

```text
T(P) = C + T_P(I)
# Total time = compile-time component + execution-time component
```

Where:

- `C` → compile-time cost, independent of the input instance.
- `T_P(I)` → runtime cost for program `P` on input `I`.

For algorithm analysis, the runtime component is usually the important part.

---

### 🧪 Formal Logic — Program Steps

A **program step** is a syntactically or semantically meaningful operation whose execution time is independent of the input characteristics.

For example:

```c
abc = a + b + b * c + (a + b - c) / (a + b) + 4.0;
```

and

```c
abc = a + b + c;
```

may contain different numbers of machine instructions, but algorithm analysis can treat each meaningful operation as a unit-cost step.

```text
T_P(n) ≈ Number of executed program steps
# Machine-dependent instruction details are abstracted away
```

---

### 🧪 Machine Independence

A more detailed model can express execution time as:

```text
T_P(n) = c_add × ADD(n)
       + c_sub × SUB(n)
       + c_lda × LDA(n)
       + c_sta × STA(n)
# Different machine instructions may have different execution costs
```

For algorithm analysis, we generally focus on **growth with input size** rather than hardware-specific constants.

> [!IMPORTANT]
> The goal is not to predict the exact number of CPU cycles. The goal is to understand **how execution grows as `n` grows**.

---

# 💠 04. Selection Structure — `if`

### 🧪 Intuition — Why?

A **selection structure** determines which statements execute based on a condition.

```text
Condition → True  → Execute block
          → False → Skip block
```

---

### 🛠️ Applied Example

**C / C++ / Java**

```cpp
if (condition) {
    statements;
    // Execute only when condition is true
}
```

**Python**

```python
if condition:
    statements
    # Execute only when condition is true
```

Example:

```cpp
if (x > 0) {
    x++;
    y++;
    z++;
    // Three statements execute when x > 0
}
```

If `x > 0`:

```text
1 + 3 = 4 steps
# One condition check + three executed statements
```

If `x <= 0`, the three statements are skipped.

**System Impact:** Selection structures introduce conditional execution, so the exact runtime depends on which branch is taken.

---

# 🔄 05. Repetition Structure — `for`

### 💠 Intuition — Why?

A repetition structure executes the same logical segment multiple times.

The important question becomes:

> **How many times does the loop body execute?**

---

### 🛠️ Applied Example

```cpp
for (i = 1; i <= 10; i++) {
    printf("%d", i);
    // Loop body executes 10 times
}
```

A detailed counting model gives:

```text
Initialization      = 1
Condition checks    = 11
Body + increment    = 20

Total = 1 + 11 + 20
      = 32 steps
# The condition is checked one final time after the last iteration
```

Thus:

```text
T(n) = O(n)
# The number of iterations grows linearly with n
```

> [!NOTE]
> Depending on the course's step-counting convention, you may see a simplified count such as `2 × 10 + 1 = 21`. **Follow your instructor's counting model when answering exam questions.**

---

# 🐍 06. Repetition Structure — `while`

### 🧪 Intuition — Why?

A `while` loop repeatedly checks a condition before executing its body.

**C / C++ / Java**

```cpp
while (condition) {
    statements;
}
```

**Python**

```python
while condition:
    statements
```

---

### 🛠️ Applied Example

```cpp
i = 1;

while (i <= 10) {
    printf("%d", i);
    i = i + 1;
    // Body executes 10 times
}
```

Using detailed step counting:

```text
Initialization  = 1
Condition       = 11
Print           = 10
Increment       = 10

Total = 1 + 11 + 10 + 10
      = 32 steps
# The condition is evaluated once more when the loop terminates
```

**System Impact:** A `while` loop's complexity depends on how many times its condition becomes true before termination.

---

# ⚡ 07. Loop Counting — Exam Patterns

### 🧪 Example A — Increment by 2

```cpp
for (i = 1; i <= 10; i = i + 2)
    printf("%d", i);
// i takes values 1, 3, 5, 7, 9
```

Number of body executions:

```text
5 iterations
# Only odd values from 1 through 9 are processed
```

Using a detailed counting model:

```text
1 + 2 × 5 + 5 + 1 = 17
# Initialization + condition/body-related operations + final condition check
```

The important algorithmic result is:

```text
T(n) = O(n)
# Increasing by a constant factor does not change linear complexity
```

---

# 💠 08. Nested Loops

### 🧪 Intuition — Why?

When one loop is placed inside another, the inner loop runs **for every iteration of the outer loop**.

Think:

```text
Outer loop: 10 times
    ↓
Inner loop: 5 times each
    ↓
Total: 10 × 5 = 50
```

---

### 🛠️ Applied Example

```cpp
count = 0;

for (i = 1; i <= 10; i++) {
    for (j = 1; j <= 5; j++) {
        count++;
        // Executes 5 times for each outer iteration
    }
}

printf("%d", count);
// Final value = 50
```

Therefore:

```text
count = 10 × 5
      = 50
# The inner loop executes completely for every outer-loop iteration
```

For variable input sizes:

```text
Outer loop → n iterations
Inner loop → n iterations

Total → n × n
      = n²

T(n) = O(n²)
# Nested linear loops produce quadratic growth
```

---

# 🧪 09. Complexity Pattern Recognition

| Code Pattern               |   Approx. Work | Complexity |
| -------------------------- | -------------: | ---------: |
| Single statement           |              1 |       O(1) |
| Fixed number of statements |       constant |       O(1) |
| Single loop `1 → n`        |              n |       O(n) |
| Loop `1 → n`, step 2       |          ≈ n/2 |       O(n) |
| Two independent loops      |          n + n |       O(n) |
| Nested loops               |          n × n |      O(n²) |
| Three nested loops         |      n × n × n |      O(n³) |
| Recursion depth `n`        | n stack frames | O(n) space |

> [!IMPORTANT]
> **Constants disappear in Big-O.**
> `O(2n)`, `O(n/2)`, and `O(100n)` are all classified as **O(n)**.

---

# 🏁 10. Week 2 — Recap

### 💠 Core Mental Model

```text
                    ALGORITHM
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
       🧠 SPACE              ⚡ TIME
       Memory usage          Work performed
             │                   │
       ┌─────┴─────┐       ┌─────┴─────┐
       ↓           ↓       ↓           ↓
   Variables    Recursion  Selection  Repetition
   O(1)         O(n)       Branches   Loops
                                           │
                                      ┌────┴────┐
                                      ↓         ↓
                                   Single    Nested
                                   O(n)      O(n²)
```

### 🏁 Exam Memory Hooks

- 🧠 **Variables only → usually `O(1)` space.**
- 🔄 **Recursion → think stack frames.**
- ⚡ **Loop → count iterations.**
- 📦 **Nested loops → multiply iteration counts.**
- 🔍 **`if` → count the condition + executed branch.**
- 🧮 **Constant loop-step changes do not change Big-O.**
- 🏁 **Focus on growth, not machine-specific constants.**

> [!NOTE]
> **Golden Rule:**
> **Time = how much work happens. Space = how much memory must remain alive while it happens.**

---

## 💫 One-Minute Interview Revision

```text
Q: What is space complexity?
A: The memory required by an algorithm as input size grows.

Q: Why does recursion consume extra space?
A: Each active recursive call creates a stack frame.

Q: What is the space complexity of recursive rsum(n)?
A: O(n), because recursion depth grows with n.

Q: What is the complexity of one loop from 1 to n?
A: O(n).

Q: What about two nested loops, each running n times?
A: O(n²).

Q: Does looping n/2 times make an algorithm O(n/2)?
A: No. Big-O ignores constant factors → O(n).

Q: Why can iteration use less space than recursion?
A: Iteration can reuse the same variables instead of creating n stack frames.
```

**System Impact:** These patterns form the foundation for recognizing algorithmic complexity quickly without manually counting every machine instruction.
