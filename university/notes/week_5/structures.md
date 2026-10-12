# 💠 Structures & Ordered Lists — Data Organization

> [!NOTE]
> **Domain:** Data Structures · C/C++ · Abstract Data Types
> **Core Concepts:** Records, `struct`, `typedef`, self-referential structures, linked lists, sequential mapping
> **Objective:** Understand how structures organize related data and how the underlying representation affects list operations.

---

## 💠 01. Intuition — What Is a Structure?

A **structure**, also called a _record_, groups multiple fields that may have different data types into one logical object.

Instead of maintaining a person's name, age, and salary separately, a structure packages them together.

### 🧪 Formal Logic

```c
struct {
    char name[10];  // Character array for the name
    int age;        // Integer field
    float salary;   // Floating-point field
} person;           // Declare a structure variable
```

The variable `person` contains three members:

| Member          | Type       | Purpose                     |
| --------------- | ---------- | --------------------------- |
| `person.name`   | `char[10]` | Stores a character sequence |
| `person.age`    | `int`      | Stores an age               |
| `person.salary` | `float`    | Stores a salary value       |

### 🛠️ Applied Example — Assigning Members

```c
#include <stdio.h>
#include <string.h>

int main(void) {
    struct {
        char name[10];
        int age;
        float salary;
    } person;

    strcpy(person.name, "james"); // Copy the string
    person.age = 10;              // Assign the age
    person.salary = 35000.0f;     // Assign the salary

    printf("%s, %d, %.2f\n",
           person.name, person.age, person.salary);

    return 0;
}
```

Output:

```text
james, 10, 35000.00
```

> [!IMPORTANT]
> `char name[10]` can hold at most nine ordinary characters plus the terminating `'\0'`. The string `"james"` fits safely.

**System Impact:** Structures create a coherent data model, making related values easier to pass, store, and maintain as a single object.

---

## 🧪 02. Formal Logic — Creating a Reusable Structure Type

A structure definition describes a type. You can use that type to declare multiple objects with the same fields.

### 💠 Method A — Named Structure

```c
struct human_being {
    char name[10];
    int age;
    float salary;
};

struct human_being person1; // First object
struct human_being person2; // Second object
```

In C, variables declared with this type use the `struct human_being` name.

### 🛠️ Method B — `typedef`

```c
typedef struct {
    char name[10];
    int age;
    float salary;
} human_being;

human_being person1; // No struct keyword required
human_being person2;
```

`typedef` introduces the alias `human_being`, allowing shorter declarations.

### ⚡ Comparison

| Feature                                        | Named `struct`             | `typedef struct`                            |
| ---------------------------------------------- | -------------------------- | ------------------------------------------- |
| Type declaration                               | `struct human_being`       | `human_being`                               |
| Requires `struct` in C declarations            | Yes                        | No                                          |
| Creates a new underlying struct representation | Defines the structure type | Defines the structure type and a type alias |
| Common use                                     | Explicit type naming       | Convenient type declarations                |

> [!NOTE]
> In C++, a declared structure name can generally be used directly as a type without `typedef`. In C, `typedef` provides the alias that removes the need for the `struct` keyword.

**System Impact:** Reusable structure types standardize data layouts and reduce duplicated declarations across a program.

---

## 💠 03. Self-Referential Structures — The Foundation of Linked Lists

A **self-referential structure** contains a pointer to an object of its own structure type.

This allows objects to reference other objects, forming chains, trees, and other dynamic data structures.

### 🧪 Formal Logic

```c
typedef struct list {
    char data;             // Stores one character
    struct list *link;     // Points to another list node
} list;
```

The pointer is allowed because its size is known independently of the size of the object it points to.

However, embedding a complete object of the same type directly inside itself would require infinitely recursive storage.

> [!IMPORTANT]
> In a C structure definition, use `struct list *link` unless an alias is already available. The `typedef` alias in this example becomes available after the closing brace.

### 🛠️ Applied Example — Three Nodes

```c
list item1, item2, item3;

item1.data = 'a';
item2.data = 'b';
item3.data = 'c';

// Initialize the links
item1.link = NULL;
item2.link = NULL;
item3.link = NULL;

// Connect the nodes
item1.link = &item2;
item2.link = &item3;
```

Logical representation:

```text
┌───────┐    ┌───────┐    ┌───────┐
│ a  ───┼───>│ b  ───┼───>│ c  ───┼───> NULL
└───────┘    └───────┘    └───────┘
  item1        item2        item3
```

Each node stores its own data and a pointer to the next node. The final node points to `NULL`, indicating the end of the list.

**System Impact:** Self-referential structures enable linked data structures whose nodes can be connected dynamically without requiring contiguous storage.

---

## 💠 04. Ordered Lists — Understanding the Abstract Data Type

An **ordered linear list** is a sequence of elements in which each item has a defined position relative to the others.

Here, _ordered_ means that the sequence has a meaningful positional order; the values do not necessarily need to be numerically sorted.

### 🧪 Examples

```text
Days:
(MONDAY, TUESDAY, WEDNESDAY, THURSDAY,
 FRIDAY, SATURDAY, SUNDAY)

Playing cards:
(2, 3, 4, 5, 6, 7, 8, 9, 10, JACK, QUEEN, KING, ACE)

Years:
(1941, 1942, 1943, 1944, 1945)

General form:
(a₁, a₂, a₃, ..., aₙ₋₁, aₙ)
```

The list's logical order is independent of how its elements are physically stored.

### 💠 Abstract Operations

| Operation      | Description                                        |
| -------------- | -------------------------------------------------- |
| `Length`       | Determine the number of elements, `n`              |
| `Read`         | Traverse items from left to right or right to left |
| `Retrieve(i)`  | Access the item at position `i`                    |
| `Store(i, x)`  | Replace the item at position `i` with `x`          |
| `Insert(i, x)` | Insert a new item at position `i`                  |
| `Delete(i)`    | Remove the item at position `i`                    |

These operations define what a list can do. Their time complexity depends on the implementation.

**System Impact:** Separating the abstract list interface from its representation allows the same logical operations to be implemented using arrays or linked lists.

---

## ⚡ 05. Sequential Mapping — Arrays vs. Ordered Lists

Your lecture's final note distinguishes operations that work efficiently with sequential mapping from operations that require moving elements.

In an array, elements occupy consecutive memory locations, and their positions are directly calculable.

### 🧪 Array Operation Complexity

Assume a fixed-size or suitably allocated array with `n` elements.

| Operation              | Typical complexity | Reason                                    |
| ---------------------- | ------------------ | ----------------------------------------- |
| Find length            | `O(1)`             | Length is stored or known                 |
| Read all elements      | `O(n)`             | Every element must be visited             |
| Retrieve element `i`   | `O(1)`             | Calculate its address directly            |
| Store at position `i`  | `O(1)`             | Directly access the target element        |
| Insert at position `i` | `O(n)`             | Shift later elements when necessary       |
| Delete at position `i` | `O(n)`             | Shift subsequent elements to fill the gap |

> [!IMPORTANT]
> The lecture's `(1)–(4) O, (5)–(6) X` is shorthand for **efficient direct operations versus insertion/deletion that may require shifting**. Inserting or deleting at the end can be `O(1)` when spare capacity and the relevant conditions permit it; arbitrary-position operations are typically `O(n)`.

### 🛠️ Applied Example — Insertion

Suppose the array contains:

```text
Before:
Index:  0   1   2   3   4
Value: [10, 20, 30, 40, 50]
```

Insert `25` at index `2`:

```text
After:
Index:  0   1   2   3   4   5
Value: [10, 20, 25, 30, 40, 50]
```

Elements `30`, `40`, and `50` must move one position to the right. If the array has no spare capacity, additional allocation may also be necessary.

### 🛠️ Applied Example — Deletion

Delete the element at index `1`:

```text
Before:
[10, 20, 30, 40, 50]

Delete 20 and shift subsequent elements:

After:
[10, 30, 40, 50]
```

The elements after the deleted item move left to preserve contiguous storage.

**System Impact:** Arrays excel at random access but incur linear-time movement for arbitrary-position insertion and deletion.

---

## ⚡ 06. Optimization — When Linked Lists Help

Linked lists use pointers to connect nodes rather than requiring all elements to occupy consecutive memory.

| Property                                 | Array                               | Singly linked list          |
| ---------------------------------------- | ----------------------------------- | --------------------------- |
| Memory layout                            | Contiguous elements                 | Independently located nodes |
| Retrieve by index                        | `O(1)`                              | `O(n)`                      |
| Insert after a known position/node       | Usually `O(n)` for shifting         | `O(1)` link updates         |
| Delete a known node with its predecessor | Usually `O(n)` for shifting         | `O(1)` link updates         |
| Extra per-element memory                 | Usually none beyond element storage | Pointer storage             |
| Cache locality                           | Generally strong                    | Often weaker                |

> [!NOTE]
> Linked-list insertion and deletion are `O(1)` only when the required position or node references are already available. Finding the position still costs `O(n)` in a singly linked list.

**System Impact:** Choosing between arrays and linked lists is a trade-off between fast indexed access, memory locality, and efficient structural modifications.

---

## 🏁 07. Recap — Interview Flashcards

- 💠 **What is a structure?** A user-defined type that groups related fields, potentially of different types.
- 💠 **What is `typedef`?** A mechanism for defining a type alias.
- 🧪 **What is a self-referential structure?** A structure containing a pointer to its own type.
- 🛠️ **What does `NULL` mean in a linked list?** No next node is referenced by that pointer.
- ⚡ **Why are arrays good for retrieval?** Their contiguous storage permits constant-time index-to-address calculation.
- ⚡ **Why is arbitrary-position array insertion expensive?** Existing elements may need to shift.
- 🔒 **When is linked-list insertion `O(1)`?** When the necessary node references are already available.
- 🏁 **What determines list-operation complexity?** The data structure's representation and the information available when the operation begins.

> [!IMPORTANT]
> **Memory anchor**
>
> - `struct` → Group related fields.
> - Self-reference → Connect objects through pointers.
> - Ordered list → Define a logical sequence.
> - Array → Fast indexed access, expensive arbitrary-position shifts.
> - Linked list → Flexible links, slower indexed traversal.

---

## 🧪 08. Knowledge Check

1. Why can a structure contain a pointer to its own type but not a complete object of its own type directly?
2. What is the difference between `struct human_being` and a `typedef` alias in C?
3. Why is retrieving `list[i]` generally `O(1)` in an array but `O(n)` in a singly linked list?
4. When can linked-list insertion be performed in `O(1)` time?
5. Why does inserting into the middle of a full array require more work than updating an existing element?

<details>
<summary>💠 Reveal Answers</summary>

1. A pointer has a known size; embedding a complete object of the same type would create an infinitely recursive object layout.
2. `struct human_being` is the structure type name. A `typedef` alias allows the type to be written without `struct` in C.
3. Array access calculates an address directly; a linked list must traverse nodes from a known starting point.
4. When the insertion location is already known and only a constant number of links need updating.
5. Subsequent elements must shift, and a full array may require a new allocation.

</details>

---

**🏁 Final Takeaway:** Structures organize data into meaningful records; self-referential structures connect records into linked data structures; and the choice between sequential arrays and pointer-based lists determines the performance of retrieval, insertion, and deletion.
