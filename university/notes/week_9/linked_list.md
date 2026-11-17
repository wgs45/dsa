# 💠 Linked Lists — Dynamic Data Structures

> **Domain:** Data Structures & Algorithms (DSA)
> **Core Concept:** Dynamic Memory Allocation · Pointer-Based Structures
> **Focus:** Array Limitations → Linked List Architecture → Insertion & Deletion

---

# 💠 01. Intuition — Why Linked Lists?

## 🧠 The Problem with Array-Based Lists

An Array-Based List ADT stores elements in **contiguous memory locations** and accesses them through an index.

Each element is identified by its position:

```c
Items[i]  // Access the ith element
```

### 📦 Array-Based List Architecture

```text
Index:   0    1    2    3    4    5
       +----+----+----+----+----+----+
Items: | 45 | 21 | 33 | 16 | ?? | ?? |
       +----+----+----+----+----+----+
                  Size = 4
```

### ⚠️ Two Fundamental Limitations

| Limitation         | Explanation                                    | Impact                                                  |
| ------------------ | ---------------------------------------------- | ------------------------------------------------------- |
| Fixed Size         | Array capacity must be declared in advance.    | Cannot grow beyond allocated capacity without resizing. |
| Insertion Overhead | Existing elements must shift to create space.  | Requires moving multiple elements.                      |
| Deletion Overhead  | Remaining elements must shift to fill the gap. | Requires additional data movement.                      |

> [!IMPORTANT]
> Array-based lists provide efficient indexed access, but inserting or deleting elements in the middle introduces unnecessary movement.

---

# 🧪 02. Formal Logic — Array-Based Implementation

## 🛠️ Structure Definition

A typical array-based list can be implemented using a C structure.

```c
#define MAX_ITEM 100

typedef int itemType;

typedef struct {
    int Size;
    itemType Items[MAX_ITEM];
} list;
```

### 🔍 Component Breakdown

| Component  | Purpose                                   |
| ---------- | ----------------------------------------- |
| `MAX_ITEM` | Defines the maximum array capacity.       |
| `itemType` | Defines the data type stored in the list. |
| `Size`     | Tracks the current number of elements.    |
| `Items[]`  | Stores the actual list elements.          |

> [!NOTE]
> `Size` represents the number of occupied positions, not the maximum capacity.

---

## ⚡ 03. Array Insertion — Element Shifting

### 💠 Intuition

Suppose we want to insert `44` at index `2`.

```c
ListInsert(&L, 2, 44);
```

The array must shift existing elements to the right to create an empty position.

### 🔄 Step-by-Step Execution

**Initial State**

```text
Index:   0    1    2    3    4    5
       +----+----+----+----+----+----+
       | 12 |  3 | 19 | 18 | 10 | ?? |
       +----+----+----+----+----+----+
```

**Step 1 — Shift elements right**

```text
Index:   0    1    2    3    4    5
       +----+----+----+----+----+----+
       | 12 |  3 | 19 | 18 | 10 | ?? |
       +----+----+----+----+----+----+
                       ↑    ↑
                  Shift right
```

**Step 2 — Insert the new element**

```text
Index:   0    1    2    3    4    5
       +----+----+----+----+----+----+
       | 12 |  3 | 44 | 19 | 18 | 10 |
       +----+----+----+----+----+----+
```

### 🛠️ C Implementation

```c
void ListInsert(list *L, int position, itemType value) {
    if (L->Size >= MAX_ITEM) return;

    if (position < 0 || position > L->Size) return;

    // Shift elements right to create space
    for (int i = L->Size; i > position; i--) {
        L->Items[i] = L->Items[i - 1];
    }

    // Insert the new element
    L->Items[position] = value;

    // Update the list size
    L->Size++;
}
```

**System Impact:** Inserting at the beginning or middle requires shifting elements, resulting in O(n) worst-case time complexity.

---

## ⚡ 04. Array Deletion — Element Shifting

### 💠 Intuition

Deleting an element creates an empty position.

To maintain a continuous sequence, every subsequent element must shift one position to the left.

### 🔄 Step-by-Step Execution

**Initial State**

```text
Index:   0    1    2    3    4
       +----+----+----+----+----+
       | 12 |  3 | 19 | 18 | 10 |
       +----+----+----+----+----+
                  ↑
             Delete index 2
```

**Step 1 — Remove the target element**

```text
Index:   0    1    2    3    4
       +----+----+----+----+----+
       | 12 |  3 | ?? | 18 | 10 |
       +----+----+----+----+----+
```

**Step 2 — Shift elements left**

```text
Index:   0    1    2    3
       +----+----+----+----+
       | 12 |  3 | 18 | 10 |
       +----+----+----+----+

              Size = 4
```

### 🛠️ C Implementation

```c
void ListDelete(list *L, int position) {
    if (position < 0 || position >= L->Size) return;

    // Shift subsequent elements left
    for (int i = position; i < L->Size - 1; i++) {
        L->Items[i] = L->Items[i + 1];
    }

    // Update the list size
    L->Size--;
}
```

**System Impact:** Deleting an element requires O(n) worst-case time because subsequent elements must be shifted.

---

# 💠 05. Linked Lists — The Solution

## 🧠 Intuition

A Linked List solves the element-shifting problem by replacing contiguous storage with **individually allocated nodes connected through pointers**.

Instead of relying on physical positions, each node stores:

1. **Data:** The actual value.
2. **Next:** A pointer to the next node.

### 🔗 Linked List Architecture

```text
 HEAD
  |
  v
+------+------+
|  20  |  •---|---->
+------+------+
                  +------+------+
                  |  45  |  •---|---->
                  +------+------+
                                    +------+------+
                                    |  51  |  •---|---->
                                    +------+------+
                                                      +------+------+
                                                      |  76  |  •---|---->
                                                      +------+------+
                                                                        +------+------+
                                                                        |  84  | NULL |
                                                                        +------+------+
```

### 🧪 Node Structure

```c
typedef struct Node {
    int data;             // Stores the node's value
    struct Node *next;    // Points to the next node
} Node;
```

### 🔍 Node Anatomy

| Component | Function                              |
| --------- | ------------------------------------- |
| `data`    | Stores the actual element.            |
| `next`    | Stores the address of the next node.  |
| `NULL`    | Indicates the end of the linked list. |
| `HEAD`    | Points to the first node.             |

> [!IMPORTANT]
> Unlike arrays, linked-list nodes do not need to occupy consecutive memory addresses. Their logical order is maintained through pointers.

---

# ⚡ 06. Applied Example — Insertion

## 🧠 Intuition

To insert a new node, we modify pointer connections rather than shifting existing elements.

Consider inserting `60` between `51` and `76`.

### 🔄 Before Insertion

```text
 HEAD
  |
  v
[20] ---> [45] ---> [51] ---> [76] ---> [84] ---> NULL
```

### 🛠️ Step 1 — Create the New Node

```text
       +------+
       |  60  |
       |  •---|---> NULL
       +------+
```

### 🛠️ Step 2 — Connect the New Node

First, make the new node point to the node currently following `51`.

```c
newNode->next = current->next;
```

Then redirect `51` to the new node.

```c
current->next = newNode;
```

### 🏁 After Insertion

```text
 HEAD
  |
  v
[20] ---> [45] ---> [51] ---> [60] ---> [76] ---> [84] ---> NULL
```

### 🧪 Complete C Implementation

```c
void InsertAfter(Node *current, int value) {
    if (current == NULL) return;

    // Allocate memory for the new node
    Node *newNode = malloc(sizeof(Node));

    if (newNode == NULL) return;

    // Store the new value
    newNode->data = value;

    // Preserve the existing connection
    newNode->next = current->next;

    // Connect the current node to the new node
    current->next = newNode;
}
```

**System Impact:** Inserting after an already-known node takes O(1) time because only two pointer assignments are required.

---

# ⚡ 07. Applied Example — Deletion

## 🧠 Intuition

Deleting a node requires bypassing it by connecting its predecessor directly to its successor.

### 🔄 Before Deletion

Suppose we want to delete node `60`.

```text
[20] ---> [45] ---> [51] ---> [60] ---> [76] ---> [84] ---> NULL
                              ^
                         Target Node
```

### 🛠️ Step 1 — Locate the Target and Its Predecessor

```text
[51] ---> [60] ---> [76]
  ^
 Previous Node
```

### 🛠️ Step 2 — Bypass the Target

```c
previous->next = target->next;
```

### 🛠️ Step 3 — Release Allocated Memory

```c
free(target);
```

### 🏁 After Deletion

```text
[20] ---> [45] ---> [51] ---> [76] ---> [84] ---> NULL
```

### 🧪 Complete C Implementation

```c
void DeleteAfter(Node *previous) {
    if (previous == NULL || previous->next == NULL) return;

    // Identify the node to remove
    Node *target = previous->next;

    // Bypass the target node
    previous->next = target->next;

    // Release the allocated memory
    free(target);
}
```

**System Impact:** Deleting a node after an already-known predecessor takes O(1) time, without shifting any elements.

---

# 💠 08. Array vs Linked List — Architecture Comparison

| Property                | Array-Based List     | Linked List            |
| ----------------------- | -------------------- | ---------------------- |
| Memory Layout           | Contiguous           | Non-contiguous         |
| Capacity                | Fixed (static array) | Dynamic                |
| Random Access           | O(1)                 | O(n)                   |
| Search                  | O(n)                 | O(n)                   |
| Insert at Beginning     | O(n)                 | O(1) with head pointer |
| Insert After Known Node | O(n)                 | O(1)                   |
| Delete at Beginning     | O(n)                 | O(1)                   |
| Delete After Known Node | O(n)                 | O(1)                   |
| Memory Overhead         | Low                  | Extra pointer per node |

> [!NOTE]
> Complexity assumes ordinary singly linked lists and excludes allocation costs. Finding the insertion or deletion position still requires traversal and may take O(n).

---

# ⚡ 09. Optimization — Complexity Analysis

| Operation       |   Array | Singly Linked List |
| --------------- | ------: | -----------------: |
| Access by Index |    O(1) |               O(n) |
| Search by Value |    O(n) |               O(n) |
| Insert at Head  |    O(n) |               O(1) |
| Insert at Tail  |   O(1)* |             O(n)** |
| Delete at Head  |    O(n) |               O(1) |
| Delete at Tail  | O(1)*** |               O(n) |

- `*` Array has available capacity and insertion is at the logical end.
- `**` A tail pointer can make insertion at the end O(1).
- `***` Array deletion at the end requires no shifting.

### 🧠 Key Architectural Trade-off

- Arrays optimize indexed access and cache locality.
- Linked lists optimize structural modification when the relevant node is already known.
- Linked lists consume additional memory for pointers and require traversal for indexed access.

---

# 🏁 10. Recap — Mental Model

## 🔄 The Core Transformation

```text
ARRAY-BASED LIST
       |
       v
Contiguous Memory
       |
       v
Insertion / Deletion
       |
       v
Element Shifting
       |
       v
O(n) Structural Modification


LINKED LIST
       |
       v
Independent Nodes
       |
       v
Pointer Connections
       |
       v
Insertion / Deletion
       |
       v
Pointer Reassignment
       |
       v
O(1) Modification When Position Is Known
```

## 🧠 Five Things to Remember

1. **Array:** Elements occupy contiguous memory locations.
2. **Linked List:** Nodes are connected through pointers.
3. **Insertion:** Connect the new node before redirecting the previous node.
4. **Deletion:** Bypass the target node, then release its memory.
5. **Trade-off:** Linked lists improve certain modifications but sacrifice efficient random access.

> [!IMPORTANT]
> **The most important distinction:**
>
> Arrays organize data through indices and contiguous memory. Linked lists organize data through nodes and pointer relationships.

---

## 🧪 Knowledge Check — Interview Revision

- [ ] Why does array insertion require element shifting?
- [ ] What are the two components of a linked-list node?
- [ ] Why is linked-list insertion O(1) when the target position is already known?
- [ ] Why is linked-list indexed access O(n)?
- [ ] What happens if `free()` is omitted after deleting a dynamically allocated node?
- [ ] Why must `newNode->next` be assigned before `current->next`?

## 🏁 Final System Takeaway

**Array = Index-Based Access**

**Linked List = Pointer-Based Connectivity**

Choose the structure according to the operations that matter most, not simply because one is dynamically sized.
