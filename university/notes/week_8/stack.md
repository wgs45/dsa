# 💠 Data Structures & Algorithms — Stack

> [!ABSTRACT]
> **Domain:** Data Structures & Algorithms (DSA)
> **Topic:** Stack — Linear Data Structure
> **Core Principle:** Last-In, First-Out (LIFO)
> **Implementation:** Array-Based Stack
> **Language:** C

---

# 💠 01. Concept — Understanding Stack

## 🧠 Intuition: Why Does It Exist?

A **Stack** is a linear data structure that follows the **Last-In, First-Out (LIFO)** principle.

Think of a stack of plates:

- The first plate placed at the bottom is the last one removed.
- The newest plate is always the first one removed.
- Both insertion and deletion happen at the same end, called `TOP`.

### 📦 Core Operations

| Operation | Description                      | Time Complexity |
| :-------- | :------------------------------- | :-------------: |
| `Push`    | Insert an element at the top     |      O(1)       |
| `Pop`     | Remove the top element           |      O(1)       |
| `Peek`    | Retrieve the top element         |      O(1)       |
| `IsEmpty` | Check whether the stack is empty |      O(1)       |
| `IsFull`  | Check whether the stack is full  |      O(1)       |

### 🔍 Visual Representation

```text
        TOP
         |
      +-----+
      |  C  |  <-- Last inserted
      +-----+
      |  B  |
      +-----+
      |  A  |  <-- First inserted
      +-----+
```

**Insertion (Push C):**

```text
Before:          After:

  TOP              TOP
   |                |
 +-----+          +-----+
 |  B  |          |  C  |
 +-----+          +-----+
 |  A  |          |  B  |
 +-----+          +-----+
                  |  A  |
                  +-----+
```

**Deletion (Pop C):**

```text
Before:          After:

  TOP              TOP
   |                |
 +-----+          +-----+
 |  C  |          |  B  |
 +-----+          +-----+
 |  B  |          |  A  |
 +-----+          +-----+
 |  A  |
 +-----+
```

> [!IMPORTANT]
> A stack only allows access to its top element. You cannot directly remove an element from the middle or bottom.

---

# 🧪 02. Formal Logic — Abstract Data Type (ADT)

## 🧠 Intuition: Why Separate Logic from Implementation?

An **Abstract Data Type (ADT)** defines what a data structure does without specifying exactly how it is implemented.

For a stack, the ADT describes:

- What objects it stores.
- Which operations are available.
- What each operation should return.
- What conditions must be satisfied.

The same Stack ADT can be implemented using an array or a linked list.

## ⚙️ Stack ADT Specification

**Structure:**

`Stack` is a finite ordered list containing zero or more elements.

**Objects:**

- `stack`: A collection of stack elements.
- `item`: An individual element.
- `max_stack_size`: Maximum permitted capacity.

### Core Functions

| Function           | Purpose               | Condition          |
| :----------------- | :-------------------- | :----------------- |
| `CreateS(n)`       | Create an empty stack | n > 0              |
| `IsFull(stack)`    | Check capacity        | size == max        |
| `IsEmpty(stack)`   | Check whether empty   | size == 0          |
| `Add(stack, item)` | Insert an element     | Stack is not full  |
| `Delete(stack)`    | Remove top element    | Stack is not empty |

### 📜 Pseudocode

```text
CreateS(max_stack_size):
    Create an empty stack
    Set maximum capacity
    Return stack

IsFull(stack):
    If number of elements == max_stack_size:
        Return TRUE
    Return FALSE

IsEmpty(stack):
    If number of elements == 0:
        Return TRUE
    Return FALSE

Add(stack, item):
    If IsFull(stack):
        Return STACK_FULL
    Insert item at TOP
    Return SUCCESS

Delete(stack):
    If IsEmpty(stack):
        Return STACK_EMPTY
    Remove and return TOP element
```

> [!NOTE]
> The ADT is the contract. The implementation is the mechanism that fulfills that contract.

---

# 🛠️ 03. Applied Example — Array Implementation in C

## 🧠 Intuition: How Does an Array Store a Stack?

An array provides a fixed amount of memory for storing stack elements.

We use a variable called `top` to track the index of the most recently inserted element.

**Initial condition:**

`top = -1`

Why -1?

Because array indexing starts at 0. An index of -1 indicates that no valid element currently exists in the stack.

### 🔧 Stack Initialization

```c
#include <stdio.h>
#include <stdbool.h>

#define MAX_STACK_SIZE 100

typedef struct {
    int key;
    // Other fields can be added here
} element;

element stack[MAX_STACK_SIZE];

int top = -1;  // Stack starts empty
```

### ⚙️ Stack Conditions

```c
bool IsEmpty(void) {
    return top < 0;
    // No elements exist in the stack
}

bool IsFull(void) {
    return top >= MAX_STACK_SIZE - 1;
    // Top has reached the final valid index
}
```

### ⚡ System Impact

The `top` variable provides constant-time access to stack boundaries without needing to traverse the array.

---

## 🔄 04. Push — Adding Elements to a Stack

### 🧠 Intuition: How Does Insertion Work?

The `Push` operation inserts a new element at the top of the stack.

Execution sequence:

1. Check whether the stack is full.
2. If full, reject the insertion.
3. Increment `top`.
4. Store the new element at `stack[top]`.

### 📜 C Implementation

```c
bool Push(element item) {

    if (IsFull()) {
        return false;
        // Stack overflow: insertion is impossible
    }

    stack[++top] = item;
    // Increment TOP first, then insert the element

    return true;
}
```

### 🔍 Execution Example

Suppose we insert three elements: A, B, C.

| Operation | TOP | Stack   |
| :-------- | :-: | :------ |
| Initial   | -1  | Empty   |
| Push A    |  0  | A       |
| Push B    |  1  | A, B    |
| Push C    |  2  | A, B, C |

```text
Index       0       1       2
          +-------+-------+-------+
Array     |   A   |   B   |   C   |
          +-------+-------+-------+
                                  ^
                                 TOP
```

### ⚡ System Impact

`stack[++top]` performs index increment and insertion in one expression, giving Push an O(1) time complexity.

---

## 🔄 05. Pop — Deleting Elements from a Stack

### 🧠 Intuition: How Does Deletion Work?

The `Pop` operation removes and returns the element currently located at the top.

Execution sequence:

1. Check whether the stack is empty.
2. If empty, reject the deletion.
3. Retrieve the current top element.
4. Decrement `top`.
5. Return the retrieved element.

### 📜 C Implementation

```c
bool Pop(element *item) {

    if (IsEmpty()) {
        return false;
        // Stack underflow: no element can be removed
    }

    *item = stack[top--];
    // Retrieve current TOP, then decrement its index

    return true;
}
```

### 🔍 Execution Example

Starting with A, B, C:

| Operation | TOP | Removed |
| :-------- | :-: | :------ |
| Initial   |  2  | —       |
| Pop       |  1  | C       |
| Pop       |  0  | B       |
| Pop       | -1  | A       |

```text
Before Pop:             After Pop:

    TOP                     TOP
     |                       |
  +-----+                 +-----+
  |  C  |                 |  B  |
  +-----+                 +-----+
  |  B  |                 |  A  |
  +-----+                 +-----+
  |  A  |
  +-----+
```

> [!WARNING]
> **Stack Overflow:** Attempting to insert into a full stack.
> **Stack Underflow:** Attempting to remove from an empty stack.

### ⚡ System Impact

`stack[top--]` retrieves the current top element before decrementing the index, preserving the LIFO ordering with O(1) complexity.

---

# 📡 06. Real-World Application — Function Call Stack

## 🧠 Intuition: Why Do Programming Languages Need Stacks?

Whenever a function is called, the runtime environment needs to remember information about its execution.

This information is stored in a **Stack Frame**, also known as an **Activation Record**.

A stack frame typically contains:

- Previous frame pointer.
- Return address.
- Local variables.
- Other execution-related information.

### 🔬 Stack Frame Structure

```text
       Higher Memory Addresses
       +----------------------+
       | Previous Frame       |
       | Pointer              |
       +----------------------+
       | Return Address       |
       +----------------------+
       | Local Variables      |
       +----------------------+
       | Function Arguments   |
       +----------------------+
       Lower Memory Addresses

       FP = Frame Pointer
```

_Conceptual representation; actual frame layout depends on the architecture and calling convention._

### 🔄 Function Call Example

Consider the following program:

```c
#include <stdio.h>

void SUB(void) {
    int x = 10;
    // Local variable belongs to SUB's frame

    printf("%d\n", x);
}

int main(void) {
    SUB();
    // Execution transfers to SUB

    return 0;
}
```

### Execution Sequence

**1. Before SUB is invoked**

```text
+----------------------+
| main() Stack Frame   |
+----------------------+
```

**2. After SUB is invoked**

```text
+----------------------+
| SUB() Stack Frame    |
| Local variable: x    |
| Return Address       |
| Previous Frame       |
+----------------------+
| main() Stack Frame   |
+----------------------+
```

**3. After SUB returns**

```text
+----------------------+
| main() Stack Frame   |
+----------------------+
```

The SUB frame is removed, and execution resumes in `main()` at the return address.

> [!IMPORTANT]
> Function calls follow the same LIFO principle: the most recently invoked function generally returns first.

### ⚡ System Impact

The call stack enables nested function execution, local variable management, and return-address tracking through automatic stack-frame allocation and deallocation.

---

# 🧪 07. Technical Comparison — Array vs Linked List

A stack is an ADT, meaning its implementation can vary.

| Characteristics   | Array Stack           | Linked List Stack           |
| :---------------- | :-------------------- | :-------------------------- |
| Memory allocation | Fixed capacity        | Dynamic                     |
| Memory layout     | Contiguous            | Non-contiguous              |
| Push              | O(1)                  | O(1)                        |
| Pop               | O(1)                  | O(1)                        |
| Capacity          | Limited by array size | Limited by available memory |
| Memory overhead   | Low                   | Additional pointer storage  |
| Overflow          | When array is full    | When allocation fails       |

> [!TIP]
> An array-based stack is straightforward and cache-friendly. A linked-list stack is useful when dynamic capacity is required.

---

# 🏁 08. Recap — System Knowledge

## 💠 Essential Concepts

- **Stack:** A linear data structure following LIFO.
- **TOP:** Tracks the most recently inserted element.
- **Push:** Inserts an element at the top.
- **Pop:** Removes and returns the top element.
- **Overflow:** Insertion into a full stack.
- **Underflow:** Deletion from an empty stack.
- **Stack Frame:** Memory structure associated with a function invocation.
- **ADT:** Defines the behavior of a stack independently of its implementation.

## ⚡ Complexity Summary

| Operation        | Time Complexity |
| :--------------- | :-------------: |
| Create           |      O(1)       |
| Push             |      O(1)       |
| Pop              |      O(1)       |
| Peek             |      O(1)       |
| IsEmpty          |      O(1)       |
| IsFull           |      O(1)       |
| Space Complexity |      O(n)       |

## 🧠 Interview Revision — Quick Questions

1. What is the fundamental principle behind a stack?
2. Why is `top` initialized to -1?
3. What is the difference between Stack Overflow and Stack Underflow?
4. Why does `stack[++top]` increment before insertion?
5. What is the difference between an ADT and its implementation?
6. How does a function call stack work?
7. Why does a stack support O(1) insertion and deletion?
8. What are the advantages of an array stack over a linked-list stack?

---

> [!SUCCESS]
> **🏁 Final Takeaway**
>
> A Stack is a simple but fundamental data structure. Its LIFO behavior makes it essential for function calls, recursion, expression evaluation, backtracking, and many other algorithms.
>
> **Remember:** Push adds to TOP. Pop removes from TOP. The last element inserted is always the first element removed.
