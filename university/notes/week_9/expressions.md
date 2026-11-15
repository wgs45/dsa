# 💠 Compiler Design — Evaluation of Expressions

> **Domain:** Data Structures & Algorithms / Compiler Fundamentals
> **Core Concepts:** Operator Precedence · Associativity · Infix · Postfix · Prefix · Stack Evaluation
> **Complexity:** O(n) time for expression conversion and evaluation
> **Implementation:** C
> **Revision Level:** University Examination / Technical Interview

---

## 📡 01. Intuition — Why Expression Evaluation Matters

Computers cannot interpret mathematical expressions exactly as humans naturally read them. A compiler must determine the correct order of operations and translate expressions into an executable instruction sequence.

Consider:

```c
X = a / b - c + d * e - a * c;

a = 4;
b = c = 2;
d = e = 3;
```

The expression contains several operators with different precedence levels.

### 💠 Operator Interpretation

**Interpretation 1 — Standard precedence and associativity**

```text
X = a / b - c + d * e - a * c

  = 4 / 2 - 2 + 3 * 3 - 4 * 2

  = 2 - 2 + 9 - 8

  = 1
```

**Interpretation 2 — Different parenthesization**

```text
X = (4 / (2 - 2 + 3)) * (3 - 4) * 2

  = (4 / 3) * (-1) * 2

  = -8 / 3

  ≈ -2.6667
```

[!IMPORTANT]
The same operands and operators can produce different results when the grouping changes.

**System Impact:** Correct operator precedence and associativity are essential for generating instructions that preserve the intended mathematical meaning.

---

## 🧪 02. Formal Logic — Operator Precedence & Associativity

### 💠 2.1 Operator Precedence

Operator precedence determines which operator is evaluated first.

For example:

```c
a + b * c

// Multiplication has higher precedence.
// Equivalent to:
a + (b * c)
```

### 💠 2.2 Associativity

Associativity determines the evaluation direction when operators have the same precedence.

| Associativity | Direction           | Example                     |
| ------------- | ------------------- | --------------------------- |
| Left-to-right | Evaluate from left  | `a - b - c` → `(a - b) - c` |
| Right-to-left | Evaluate from right | `a = b = c` → `a = (b = c)` |

[!NOTE]
Precedence determines priority between different operators. Associativity determines grouping between operators of equal precedence.

### 🧪 2.3 C Operator Precedence Table

Higher precedence appears first.

| Level | Operators                                  | Description             | Associativity |
| ----: | ------------------------------------------ | ----------------------- | ------------- |
|    17 | `()` `[]` `->` `.` `x++` `x--`             | Postfix / member access | Left → Right  |
|    16 | `++` `--` `!` `~` `+` `-` `*` `&` `sizeof` | Unary operators         | Right → Left  |
|    15 | `(type)`                                   | Type cast               | Right → Left  |
|    14 | `*` `/` `%`                                | Multiplicative          | Left → Right  |
|    13 | `+` `-`                                    | Additive                | Left → Right  |
|    12 | `<<` `>>`                                  | Bitwise shift           | Left → Right  |
|    11 | `<` `<=` `>` `>=`                          | Relational              | Left → Right  |
|    10 | `==` `!=`                                  | Equality                | Left → Right  |
|     9 | `&`                                        | Bitwise AND             | Left → Right  |
|     8 | `^`                                        | Bitwise XOR             | Left → Right  |
|     7 | `\|`                                       | Bitwise OR              | Left → Right  |
|     6 | `&&`                                       | Logical AND             | Left → Right  |
|     5 | `\|\|`                                     | Logical OR              | Left → Right  |
|     4 | `?:`                                       | Conditional             | Right → Left  |
|     3 | `=` `+=` `-=` `*=` `/=` `%=`               | Assignment              | Right → Left  |
|     2 | `<<=` `>>=` `&=` `^=` `\|=`                | Compound assignment     | Right → Left  |
|     1 | `,`                                        | Comma operator          | Left → Right  |

[!WARNING]
This table follows the conventional C precedence hierarchy. Parentheses explicitly override normal grouping. The postfix and prefix forms of increment/decrement have different semantics.

### ⚡ 2.4 Practical Precedence Examples

```c
int x = 2 + 3 * 4;       // 14
int y = (2 + 3) * 4;     // 20

int z = 10 - 5 - 2;      // 3
int w = 10 / 2 * 5;      // 25

int a = 5;
int b = 3;
int c = 2;

a = b = c;               // Right-to-left assignment
```

**System Impact:** Understanding the precedence hierarchy prevents incorrect expression parsing and unexpected evaluation results.

---

## 🔄 03. Expression Notations

Three major expression representations are used in compiler design.

| Notation | Operator Position | Example |
| -------- | ----------------- | ------- |
| Infix    | Between operands  | `A + B` |
| Prefix   | Before operands   | `+AB`   |
| Postfix  | After operands    | `AB+`   |

### 💠 3.1 Infix Notation

The conventional mathematical notation used by humans.

```text
A + B
A * B + C
(A + B) * C
```

**Limitation:** The expression requires precedence rules and parentheses to determine the intended evaluation order.

### 💠 3.2 Prefix Notation

The operator appears before its operands.

```text
Infix:  A + B
Prefix: +AB

Infix:  A * B + C
Prefix: +*ABC
```

### 💠 3.3 Postfix Notation

The operator appears after its operands.

```text
Infix:   A + B
Postfix: AB+

Infix:   A * B + C
Postfix: AB*C+
```

[!IMPORTANT]
Prefix and postfix expressions do not require parentheses or conventional precedence rules during evaluation because their structure already determines the operation order.

---

## 🧪 04. Infix → Postfix Conversion

### 💠 4.1 The Fundamental Principle

The order of operands in infix and postfix notation remains unchanged.

Only the position of operators changes.

**Conversion strategy:**

1. Fully parenthesize the infix expression.
2. Move each operator to the position immediately after its corresponding right parenthesis.
3. Remove all parentheses.

### 🛠️ 4.2 Applied Example — Metal

Original expression:

```text
a / b - c + d * e - a * c
```

**Step 1 — Fully parenthesize**

```text
((((a / b) - c) + (d * e)) - (a * c))
```

**Step 2 — Move operators**

```text
((ab/ c-) de*+ ac* -)
```

**Step 3 — Remove parentheses**

```text
ab/c-de*+ac*-
```

<badge color="success">Final Postfix</badge>

```text
ab/c-de*+ac*-
```

### 🧪 4.3 Additional Conversion Examples

| Infix                           | Postfix         |
| ------------------------------- | --------------- |
| `2 + 3 * 4`                     | `234*+`         |
| `a * b + 5`                     | `ab*5+`         |
| `(1 + 2) * 7`                   | `12+7*`         |
| `a * b / c`                     | `ab*c/`         |
| `a / (b - c + d) * (e - a) * c` | `abc-d+/ea-*c*` |
| `a / b - c + d * e - a * c`     | `ab/c-de*+ac*-` |

[!NOTE]
For the expression `a * b / c`, multiplication and division have equal precedence and associate left-to-right. Therefore, its postfix representation is `ab*c/`.

---

## 🛠️ 05. Postfix Evaluation Using a Stack

### 💠 5.1 Intuition — Why a Stack?

Postfix expressions are evaluated using a Last-In, First-Out (LIFO) structure.

The algorithm is straightforward:

1. Read the expression from left to right.
2. If the token is an operand, push it onto the stack.
3. If the token is an operator, pop two operands.
4. Perform the operation.
5. Push the result back onto the stack.
6. Repeat until the expression ends.

[!IMPORTANT]
The first popped operand is `op2`, while the second popped operand is `op1`.

```c
op2 = pop();
op1 = pop();

result = op1 - op2;
```

This distinction is critical for subtraction and division.

### 🧪 5.2 Applied Example

Evaluate:

```text
6 / 2 - 3 + 4 * 2
```

**Step 1 — Convert to postfix**

```text
62/3-42*+
```

**Step 2 — Evaluate using a stack**

| Token | Operation | Stack (Bottom → Top) |
| ----- | --------- | -------------------- |
| `6`   | Push 6    | `[6]`                |
| `2`   | Push 2    | `[6, 2]`             |
| `/`   | 6 / 2 = 3 | `[3]`                |
| `3`   | Push 3    | `[3, 3]`             |
| `-`   | 3 - 3 = 0 | `[0]`                |
| `4`   | Push 4    | `[0, 4]`             |
| `2`   | Push 2    | `[0, 4, 2]`          |
| `*`   | 4 * 2 = 8 | `[0, 8]`             |
| `+`   | 0 + 8 = 8 | `[8]`                |

<badge color="success">Final Result = 8</badge>

### ⚡ 5.3 Evaluation Algorithm

```text
POSTFIX_EVALUATION(expression):

    Initialize an empty stack

    FOR each token in expression:

        IF token is an operand:
            PUSH token onto stack

        ELSE IF token is an operator:

            op2 = POP()
            op1 = POP()

            result = op1 operator op2

            PUSH result onto stack

    RETURN POP()
```

**System Impact:** Each operand is pushed once and each operator performs two pops and one push, resulting in O(n) time complexity.

---

## 🐍 06. C Implementation — Postfix Evaluation

### 🧪 6.1 Assumptions

| Component            | Constraint                    |
| -------------------- | ----------------------------- |
| Operators            | `+`, `-`, `*`, `/`, `%`       |
| Operands             | Single-digit integers (`0–9`) |
| Data structure       | Integer stack                 |
| Stack capacity       | 100                           |
| Expression capacity  | 100                           |
| Evaluation direction | Left → Right                  |

### 🛠️ 6.2 Stack Configuration

```c
#define MAX_STACK_SIZE 100
#define MAX_EXPR_SIZE 100

typedef enum {
    lparen,
    rparen,
    plus,
    minus,
    times,
    divide,
    mod,
    eos,
    operand
} precedence;

int stack[MAX_STACK_SIZE];
char expr[MAX_EXPR_SIZE];
```

**System Impact:** The enumerated token type provides readable operator identification, while the integer stack stores intermediate evaluation results.

### 🧪 6.3 Token Extraction

```c
precedence get_token(char *symbol, int *n)
{
    *symbol = expr[(*n)++];

    switch (*symbol) {
        case '(':
            return lparen;

        case ')':
            return rparen;

        case '+':
            return plus;

        case '-':
            return minus;

        case '*':
            return times;

        case '/':
            return divide;

        case '%':
            return mod;

        case '\0':
            return eos;

        default:
            return operand;
    }
}
```

**System Impact:** Token extraction converts individual characters into meaningful token categories that the expression-processing algorithm can recognize.

### 🛠️ 6.4 Stack Operations

```c
#include <stdio.h>
#include <stdlib.h>

void push(int *top, int value)
{
    if (*top >= MAX_STACK_SIZE - 1) {
        fprintf(stderr, "Stack overflow\n");
        exit(EXIT_FAILURE);
    }

    stack[++(*top)] = value;
}

int pop(int *top)
{
    if (*top < 0) {
        fprintf(stderr, "Stack underflow\n");
        exit(EXIT_FAILURE);
    }

    return stack[(*top)--];
}
```

**System Impact:** Centralized push/pop operations maintain stack integrity and prevent invalid memory access caused by overflow or underflow.

### 🧪 6.5 Postfix Evaluation Function

```c
int eval(void)
{
    precedence token;
    char symbol;

    int op1, op2;
    int n = 0;
    int top = -1;

    token = get_token(&symbol, &n);

    while (token != eos) {

        if (token == operand) {
            if (symbol < '0' || symbol > '9') {
                fprintf(stderr, "Invalid operand\n");
                exit(EXIT_FAILURE);
            }

            push(&top, symbol - '0');
        }
        else {
            op2 = pop(&top);
            op1 = pop(&top);

            switch (token) {

                case plus:
                    push(&top, op1 + op2);
                    break;

                case minus:
                    push(&top, op1 - op2);
                    break;

                case times:
                    push(&top, op1 * op2);
                    break;

                case divide:
                    if (op2 == 0) {
                        fprintf(stderr, "Division by zero\n");
                        exit(EXIT_FAILURE);
                    }

                    push(&top, op1 / op2);
                    break;

                case mod:
                    if (op2 == 0) {
                        fprintf(stderr, "Modulo by zero\n");
                        exit(EXIT_FAILURE);
                    }

                    push(&top, op1 % op2);
                    break;

                default:
                    fprintf(stderr, "Invalid operator\n");
                    exit(EXIT_FAILURE);
            }
        }

        token = get_token(&symbol, &n);
    }

    if (top != 0) {
        fprintf(stderr, "Invalid postfix expression\n");
        exit(EXIT_FAILURE);
    }

    return pop(&top);
}
```

**System Impact:** The evaluation function executes postfix expressions in linear time using a single auxiliary stack, with integer division following C's truncation rules.

[!WARNING]
This implementation assumes a valid, null-terminated postfix expression and single-digit operands. A production implementation should additionally validate expression length, token count, malformed input, and arithmetic overflow.

---

## 🔄 07. Infix → Postfix Using Operator Stacks

### 💠 7.1 Core Concept

Instead of manually parenthesizing an expression, a stack can temporarily hold operators until they are ready to be emitted.

**Rules:**

1. Output operands immediately.
2. Push incoming operators onto the stack.
3. Pop operators while the top operator has higher or equal precedence than the incoming operator.
4. When encountering `)`, pop until `(` is found.
5. Discard the matching `(`.
6. After reaching the end, pop all remaining operators.

### 🧪 7.2 In-Stack vs Incoming Precedence

Two precedence values control the conversion algorithm.

| Term | Meaning             |
| ---- | ------------------- |
| ISP  | In-Stack Precedence |
| ICP  | Incoming Precedence |

For the operator set `+`, `-`, `*`, `/`, `%`:

| Token | `(` | `)` | `+` | `-` | `*` | `/` | `%` | EOS |
| ----- | --: | --: | --: | --: | --: | --: | --: | --: |
| ISP   |   0 |  19 |  12 |  12 |  13 |  13 |  13 |   0 |
| ICP   |  20 |  19 |  12 |  12 |  13 |  13 |  13 |   0 |

**Why are parentheses special?**

- `(` has low ISP = 0.
- `(` has high ICP = 20.
- This allows it to be pushed without removing existing operators.
- `)` triggers removal of operators until the matching `(` is encountered.

### 🛠️ 7.3 Precedence Arrays in C

```c
static int isp[] = {
    0, 19, 12, 12, 13, 13, 13, 0
};

static int icp[] = {
    20, 19, 12, 12, 13, 13, 13, 0
};
```

**System Impact:** Separate ISP and ICP values allow the algorithm to handle parentheses and operator priority using a consistent comparison mechanism.

### 🧪 7.4 Conversion Algorithm

```text
INFIX_TO_POSTFIX(expression):

    Initialize stack
    PUSH EOS

    FOR each token:

        IF operand:
            OUTPUT token

        ELSE IF right parenthesis:
            WHILE top != left parenthesis:
                OUTPUT POP()

            POP()  // Discard left parenthesis

        ELSE:
            WHILE ISP(top) >= ICP(token):
                OUTPUT POP()

            PUSH token

    WHILE top != EOS:
        OUTPUT POP()
```

### 🛠️ 7.5 C Implementation

```c
void postfix(void)
{
    char symbol;
    precedence token;

    int n = 0;
    int top = 0;

    stack[0] = eos;

    for (
        token = get_token(&symbol, &n);
        token != eos;
        token = get_token(&symbol, &n)
    ) {

        if (token == operand) {
            printf("%c", symbol);
        }

        else if (token == rparen) {

            while (stack[top] != lparen) {
                printf("%d", stack[top--]);
            }

            top--;  // Discard left parenthesis
        }

        else {

            while (isp[stack[top]] >= icp[token]) {
                printf("%d", stack[top--]);
            }

            stack[++top] = token;
        }
    }

    while (stack[top] != eos) {
        printf("%d", stack[top--]);
    }

    printf("\n");
}
```

**Implementation Note:** This is a structural illustration of the original algorithm. A complete implementation should use a character-based output function such as `print_token()`, handle stack capacity, and ensure the EOS sentinel is not treated as a normal operator.

**System Impact:** The operator stack ensures that every operator is inserted and removed at most once, achieving O(n) conversion time.

---

## 🧪 08. Conversion Walkthrough — Stack Behavior

Consider:

```text
a + b * c
```

Because multiplication has higher precedence than addition, `*` must be emitted before `+`.

| Token | Stack (Bottom → Top) | Output  |
| ----- | -------------------- | ------- |
| Start | `[EOS]`              |         |
| `a`   | `[EOS]`              | `a`     |
| `+`   | `[EOS, +]`           | `a`     |
| `b`   | `[EOS, +]`           | `ab`    |
| `*`   | `[EOS, +, *]`        | `ab`    |
| `c`   | `[EOS, +, *]`        | `abc`   |
| EOS   | `[EOS]`              | `abc*+` |

<badge color="success">Final Postfix = abc*+</badge>

### 💠 Parenthesis Example

Consider:

```text
a * (b + c) * d
```

The conversion process:

```text
a       → a
*       → a
(       → a
b       → ab
+       → ab
c       → abc
)       → abc+
*       → abc+*
d       → abc+*d
EOS     → abc+*d*
```

<badge color="success">Final Postfix = abc+_d_</badge>

[!IMPORTANT]
When `)` arrives, the algorithm pops operators until it finds the matching `(`. The left parenthesis is discarded rather than included in the output.

---

## 💠 09. Infix → Prefix Conversion

### 🧪 9.1 Fundamental Principle

Prefix notation places operators before their operands.

A common conversion technique is:

1. Reverse the infix expression.
2. Exchange every left parenthesis with a right parenthesis.
3. Convert the resulting expression into postfix.
4. Reverse the postfix result.

[!NOTE]
When implementing the reverse-based algorithm, equal-precedence handling must be adjusted for the reversed associativity. Simply applying the ordinary postfix algorithm without adjustment can produce incorrect results for expressions involving repeated operators.

### 🛠️ 9.2 Applied Examples

| Infix                       | Prefix          |
| --------------------------- | --------------- |
| `a * b / c`                 | `/*abc`         |
| `a / b - c + d * e - a * c` | `-+-/abc*de*ac` |
| `a * (b + c) / d - g`       | `-/*a+bcdg`     |

### 💠 9.3 Expression Structure

For:

```text
a / b - c + d * e - a * c
```

The fully parenthesized expression:

```text
((((a / b) - c) + (d * e)) - (a * c))
```

Its prefix representation:

```text
- + - / a b c * d e * a c
```

Without spaces:

```text
-+-/abc*de*ac
```

**System Impact:** Prefix representation makes the operation hierarchy explicit, allowing expression evaluation without relying on infix precedence rules.

---

## ⚡ 10. Complexity Analysis & Optimization

### 💠 Time and Space Complexity

Let `n` represent the number of tokens in an expression.

| Operation          | Time Complexity | Auxiliary Space |
| ------------------ | --------------- | --------------- |
| Postfix evaluation | O(n)            | O(n)            |
| Infix → Postfix    | O(n)            | O(n)            |
| Infix → Prefix     | O(n)            | O(n)            |
| Stack PUSH         | O(1)            | O(1)            |
| Stack POP          | O(1)            | O(1)            |

### ⚡ Optimization Insights

- Each token should be processed exactly once.
- Every operator is pushed and popped at most once.
- Stack operations should remain O(1).
- Avoid repeatedly scanning previously processed tokens.
- Use dynamic stack allocation when expression size is unknown.
- Validate malformed expressions before evaluation.

[!IMPORTANT]
The O(n) conversion complexity assumes constant-time token processing and stack operations. The auxiliary stack can grow proportionally to the expression size.

---

## 🏁 11. Final Recap — Interview & Examination Revision

### 💠 Essential Concepts

| Concept            | Remember                                           |
| ------------------ | -------------------------------------------------- |
| Precedence         | Determines operator priority                       |
| Associativity      | Determines grouping direction                      |
| Infix              | Operator between operands                          |
| Prefix             | Operator before operands                           |
| Postfix            | Operator after operands                            |
| Stack              | LIFO data structure                                |
| ISP                | In-stack precedence                                |
| ICP                | Incoming precedence                                |
| Postfix evaluation | Pop two operands, calculate, push result           |
| Infix → Postfix    | Operator stack + precedence                        |
| Infix → Prefix     | Reverse + exchange parentheses + convert + reverse |

### 🧪 Five Critical Rules

1. **Precedence:** `*` and `/` have higher precedence than `+` and `-`.
2. **Associativity:** `a - b - c` means `(a - b) - c`.
3. **Operand order:** Always pop `op2` first, then `op1`.
4. **Parentheses:** They override ordinary precedence and control grouping.
5. **Complexity:** Stack-based expression conversion and evaluation are O(n).

### 🛠️ Quick Practice

Try solving these without looking at the answers.

**Q1. Convert to postfix**

```text
a + b * c
```

<details>
<summary>🏁 Reveal Answer</summary>

```text
abc*+
```

</details>

**Q2. Convert to postfix**

```text
(a + b) * (c - d)
```

<details>
<summary>🏁 Reveal Answer</summary>

```text
ab+cd-*
```

</details>

**Q3. Evaluate postfix**

```text
82/34*+
```

<details>
<summary>🏁 Reveal Answer</summary>

```text
8 / 2 + 3 * 4
= 4 + 12
= 16
```

</details>

**Q4. Convert to prefix**

```text
a + b * c
```

<details>
<summary>🏁 Reveal Answer</summary>

```text
+a*bc
```

</details>

**Q5. Evaluate postfix**

```text
82-42/+
```

<details>
<summary>🏁 Reveal Answer</summary>

```text
8 - 2 + 4 / 2
= 6 + 2
= 8
```

</details>

---

## 🏁 12. Knowledge Checkpoint

> [!SUCCESS]
> **Core Takeaway**
>
> Expression evaluation is fundamentally a problem of understanding operation order and managing intermediate results.
>
> - Precedence and associativity establish the correct expression structure.
> - Infix, prefix, and postfix represent the same logical expression differently.
> - Stacks provide an efficient mechanism for expression conversion and evaluation.
> - Linear-time algorithms make stack-based expression processing efficient and predictable.

**Mental Model:**

```text
INFIX
  │
  ▼
PRECEDENCE + ASSOCIATIVITY
  │
  ▼
EXPRESSION CONVERSION
  │
  ├── POSTFIX
  │     │
  │     ▼
  │   STACK EVALUATION
  │     │
  │     ▼
  │   RESULT
  │
  └── PREFIX
        │
        ▼
      STACK EVALUATION
        │
        ▼
      RESULT
```

**Revision Priority:** Understand operand ordering, precedence comparisons, parenthesis handling, and stack state transitions. These are the most important foundations for solving expression-conversion and evaluation problems in C.
