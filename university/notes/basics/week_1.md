# 💠 Data Structures — Week 1

> **Foundations of Algorithms, ADTs & Complexity**

---

## 💠 Intuition — Why This Matters

Data structures are not just containers for data. They define **what data means**, **what operations are allowed**, and **how efficiently programs can process it**.

A reliable program follows a pipeline:

```text
Requirements
    ↓
Analysis
    ↓
Design
    ↓
Refinement & Coding
    ↓
Verification
    ↓
Program Proving / Testing / Debugging
```

> [!NOTE]
> Think of the workflow as **intent → model → implementation → evidence**.

### 🔄 Program Construction Pipeline

| Stage               | Core Question                               | Output                   |
| ------------------- | ------------------------------------------- | ------------------------ |
| Requirements        | What must the system accomplish?            | Problem constraints      |
| Analysis            | How should the problem be understood?       | Problem model            |
| Design              | What data + operations are needed?          | Algorithm / ADT design   |
| Refinement & Coding | How do we implement it?                     | Source code              |
| Verification        | Does it satisfy the specification?          | Correctness evidence     |
| Program Proving     | Can correctness be reasoned about formally? | Proof                    |
| Testing             | Does it work on selected inputs?            | Test evidence            |
| Debugging           | Why does it fail?                           | Corrected implementation |

> [!IMPORTANT]
> **Testing finds failures; verification/proving establishes whether the implementation satisfies its specification.**

---

## 🧪 Formal Logic — Algorithms

### Definition

An **algorithm** is a **finite set of instructions that accomplishes a particular task**.

A valid algorithm must satisfy five core criteria:

| Criterion        | Meaning                                              |
| ---------------- | ---------------------------------------------------- |
| 📥 Input         | Accepts zero or more clearly specified inputs        |
| 📤 Output        | Produces one or more specified results               |
| 🔒 Definiteness  | Every instruction is clear and unambiguous           |
| 🏁 Finiteness    | Terminates after a finite number of steps            |
| ⚡ Effectiveness | Each instruction is basic enough to actually execute |

### Algorithm Mental Model

```text
Input
  ↓
[ Deterministic / Unambiguous Steps ]
  ↓
Finite Execution
  ↓
Output
```

> [!NOTE]
> **Finite + definite + effective** separates an executable algorithm from vague problem-solving instructions.

---

## 💠 Intuition — Data Types

A **data type** combines two things:

1. A collection of **objects**
2. A set of **operations** that act on those objects

```text
Data Type
├── Objects
└── Operations
```

For example, an integer type contains integer values and operations such as addition, subtraction, comparison, and equality.

---

## 🧪 Formal Logic — Abstract Data Types

An **Abstract Data Type (ADT)** separates:

```text
WHAT
├── Objects
└── Operations
        ↓
Specification
        ║
        ║ abstraction barrier
        ▼
HOW
├── Representation
└── Implementation
```

> [!IMPORTANT]
> **ADT = specification independent of representation and implementation.**

### Specification vs. Implementation

| Specification                   | Implementation           |
| ------------------------------- | ------------------------ |
| Defines what an operation means | Defines how it works     |
| Function name                   | Algorithm / code         |
| Argument types                  | Data representation      |
| Result type                     | Memory / machine details |
| Implementation-independent      | Representation-dependent |

For an operation, the specification should identify:

- 🔹 Function name
- 🔹 Types of arguments
- 🔹 Type of result

> [!NOTE]
> The same ADT can have multiple implementations as long as they obey the same specification.

---

## 🛠️ Applied Example — ADT `Natural_Number`

The lecture's `Natural_Number` ADT models values from:

```text
0 → INT_MAX
```

Its operations include:

| Operation        | Purpose                                    |
| ---------------- | ------------------------------------------ |
| `Zero()`         | Return `0`                                 |
| `Is_Zero(x)`     | Determine whether `x` is zero              |
| `Add(x, y)`      | Add values with overflow saturation        |
| `Equal(x, y)`    | Compare equality                           |
| `Successor(x)`   | Increment unless already `INT_MAX`         |
| `Subtract(x, y)` | Subtract without producing negative values |

### Metal — Pseudocode

```text
Zero() ::= 0
# Return the smallest Natural_Number.

Is_Zero(x) ::= if x == 0
                 return TRUE
               else
                 return FALSE
# Test whether x represents zero.

Add(x, y) ::= if x + y <= INT_MAX
                return x + y
              else
                return INT_MAX
# Saturate at INT_MAX instead of overflowing.

Equal(x, y) ::= if x == y
                  return TRUE
                else
                  return FALSE
# Compare two Natural_Number values.

Successor(x) ::= if x == INT_MAX
                   return x
                 else
                   return x + 1
# Prevent the successor from exceeding INT_MAX.

Subtract(x, y) ::= if x < y
                      return 0
                    else
                      return x - y
# Clamp negative results to zero.
```

**System Impact:** The ADT defines predictable mathematical behavior while hiding the machine-level representation and implementation details.

> [!NOTE]
> `::=` means **"is defined as"**.

---

## ⚡ Optimization — Measuring Programs

A program should be evaluated beyond simply asking whether it runs.

Core quality questions:

```text
Correctness
    ↓
Readability
    ↓
Performance
├── Space Complexity
└── Time Complexity
```

### Performance Analysis

**Machine-independent analysis** focuses on how resource requirements grow rather than on one specific computer.

| Metric              | Measures            | Core Question                |
| ------------------- | ------------------- | ---------------------------- |
| ⏱️ Time complexity  | Computing time      | How much work is required?   |
| 💾 Space complexity | Storage requirement | How much memory is required? |

> [!IMPORTANT]
> Complexity analysis is about **resource growth with input characteristics**, not simply measuring elapsed seconds on one machine.

---

## 🧪 Formal Logic — Space Complexity

The general model is:

```text
S(P) = C + SP(I)
# Total space = fixed space + input-dependent space.
```

Where:

- `S(P)` = total space required by program `P`
- `C` = fixed space requirement
- `SP(I)` = variable space requirement for input instance `I`

### Fixed Space — `C`

Fixed space does **not depend on the characteristics of the input or output**.

Typical components:

- 🧩 Instruction space
- 📦 Simple variables
- 📦 Fixed-size structured variables
- 🔢 Constants

### Variable Space — `SP(I)`

Variable space **depends on the input instance `I`**.

Typical components:

- 📥 Number of inputs
- 📐 Size of inputs
- 🔢 Values of inputs / outputs
- 🔄 Recursive stack space
- 📦 Formal parameters
- 📦 Local variables
- ↩️ Return addresses

---

## 🛠️ Applied Example — Recursive Space

Consider a recursive function:

```text
factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
# Each recursive call adds stack-frame space.
```

For input `n`, the recursion creates approximately `n` active stack frames before returning.

```text
S(P) = C + SP(I)

C       → fixed program/storage requirements
SP(I)   → recursive stack grows with n
```

**System Impact:** Even when an algorithm's data structures remain small, recursion can increase memory usage through the call stack.

---

## 💠 Core Connections — The Big Picture

```text
Problem
   ↓
Requirements
   ↓
Analysis
   ↓
ADT / Algorithm
   ↓
Specification
   ↓
Implementation
   ↓
Verification + Testing
   ↓
Complexity Analysis
   ↓
Reliable Program
```

The conceptual separation is crucial:

```text
ADT
│
├── WHAT
│   ├── Objects
│   └── Operations
│
└── HOW
    ├── Representation
    └── Implementation
```

---

## 🏁 Recap — Interview Mode

### ⚡ 10-Second Recall

- 💠 **Data type** = objects + operations.
- 💠 **ADT** = data type whose specification is separated from representation and implementation.
- 🧪 **Algorithm** = finite instructions accomplishing a task.
- 🔒 Algorithm criteria = **input, output, definiteness, finiteness, effectiveness**.
- 🛠️ **Specification** describes what an operation provides.
- 🛠️ **Implementation** describes how the operation works.
- ⚡ **Time complexity** measures computing-time growth.
- ⚡ **Space complexity** measures storage requirements.
- 💾 `S(P) = C + SP(I)`.
- 🔄 `C` is input-independent; `SP(I)` depends on the input instance.

### 🧠 Interview Triggers

| Question                         | Answer Pattern                                                 |
| -------------------------------- | -------------------------------------------------------------- |
| What is an algorithm?            | Finite instructions that accomplish a task                     |
| What makes an algorithm valid?   | Input, output, definiteness, finiteness, effectiveness         |
| What is an ADT?                  | Specification separated from representation and implementation |
| Why use an ADT?                  | Encapsulation + implementation independence                    |
| Specification vs implementation? | **What** vs **how**                                            |
| Time vs space complexity?        | Computing time vs storage requirement                          |
| Space formula?                   | `S(P) = C + SP(I)`                                             |
| What is `SP(I)`?                 | Input-dependent space requirement                              |

> [!IMPORTANT]
> **Master the abstraction boundary:** algorithms describe computation, ADTs describe behavior, implementations realize that behavior, and complexity describes resource cost.

---

## 🏁 Final Mental Model

```text
          ┌───────────────────────┐
          │       PROBLEM         │
          └───────────┬───────────┘
                      ↓
          ┌───────────────────────┐
          │   ALGORITHM / ADT     │
          │       WHAT + WHY      │
          └───────────┬───────────┘
                      ↓
          ┌───────────────────────┐
          │   IMPLEMENTATION      │
          │        HOW            │
          └───────────┬───────────┘
                      ↓
          ┌───────────────────────┐
          │ VERIFICATION / TEST   │
          └───────────┬───────────┘
                      ↓
          ┌───────────────────────┐
          │  TIME + SPACE COST    │
          └───────────────────────┘
```

> **Foundation Principle:** Good data-structure design separates **behavior from implementation** and evaluates the resulting program by **correctness, clarity, and resource usage**.
