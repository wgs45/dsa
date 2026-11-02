# 💠 Queue — First-In, First-Out (FIFO)

> [!ABSTRACT] SYSTEM OVERVIEW
> **Domain:** Data Structures & Algorithms
> **Structure:** Linear Data Structure
> **Principle:** First-In, First-Out (FIFO)
> **Core Operations:** Enqueue (Add) · Dequeue (Delete)
> **Implementations:** Sequential Array · Circular Array
> **Primary Application:** Job Scheduling

---

# 💠 01. Intuition — Understanding the Queue

## 🧠 What Is a Queue?

A **Queue** is a linear data structure that follows the **First-In, First-Out (FIFO)** principle.

- The first element inserted is the first element removed.
- Insertion occurs at the **Rear**.
- Deletion occurs at the **Front**.

Think of a queue as a line of people waiting for service: the person who arrives first gets served first.

### 📡 Queue Structure

```text
        Dequeue                         Enqueue
           ↓                               ↓
       FRONT                           REAR
         |                               |
         v                               v
       +----+----+----+----+
       | J1 | J2 | J3 | J4 |
       +----+----+----+----+
         ↑
     First Out

     FIFO: J1 → J2 → J3 → J4
```

> [!IMPORTANT]
>
> - **Front:** Identifies the element to be removed.
> - **Rear:** Identifies the most recently inserted element.
> - **Enqueue:** Inserts an element at the rear.
> - **Dequeue:** Removes an element from the front.

## 🔄 Core Operations

| Operation | Description                      | Time Complexity |
| --------- | -------------------------------- | --------------- |
| CreateQ   | Initialize an empty queue        | O(1)            |
| IsEmptyQ  | Check whether the queue is empty | O(1)            |
| IsFullQ   | Check whether the queue is full  | O(1)            |
| AddQ      | Insert an element at rear        | O(1)            |
| DeleteQ   | Remove an element from front     | O(1)            |

---

# 🧪 02. Formal Logic — Abstract Data Type (ADT)

An **Abstract Data Type (ADT)** defines what operations a data structure supports without specifying how those operations are implemented.

## 📐 Queue ADT Specification

**Objects:**

A finite ordered list containing zero or more elements.

**Functions:**

| Function            | Definition                                        |
| ------------------- | ------------------------------------------------- |
| `CreateQ(size)`     | Create an empty queue with maximum capacity       |
| `IsFullQ(queue)`    | Return TRUE if the queue reaches maximum capacity |
| `IsEmptyQ(queue)`   | Return TRUE if the queue contains no elements     |
| `AddQ(queue, item)` | Insert an item at the rear                        |
| `DeleteQ(queue)`    | Remove and return the front item                  |

### 🛠️ ADT Pseudocode

```c
// Create an empty queue
Queue CreateQ(int max_queue_size);

// Check whether the queue is full
Boolean IsFullQ(Queue queue);

// Check whether the queue is empty
Boolean IsEmptyQ(Queue queue);

// Insert an item at the rear
Queue AddQ(Queue queue, Element item);

// Remove and return the front item
Element DeleteQ(Queue queue);
```

> [!NOTE]
> The ADT describes the queue's behavior, not its underlying memory representation. Arrays and linked lists are possible implementations.

---

# 🛠️ 03. Applied Example — Job Scheduling

## 💻 FIFO Scheduling Simulation

A job scheduling system can use a queue to manage incoming tasks.

Jobs are processed in the same order in which they arrive.

### 🔄 Execution Sequence

| Step | Operation  | Front | Rear | Queue Contents | Comments         |
| ---- | ---------- | ----: | ---: | -------------- | ---------------- |
| 0    | Initialize |    -1 |   -1 | Empty          | Queue is empty   |
| 1    | Add J1     |     0 |    0 | J1             | Job 1 is added   |
| 2    | Add J2     |     0 |    1 | J1, J2         | Job 2 is added   |
| 3    | Add J3     |     0 |    2 | J1, J2, J3     | Job 3 is added   |
| 4    | Delete J1  |     1 |    2 | J2, J3         | Job 1 is deleted |
| 5    | Delete J2  |     2 |    2 | J3             | Job 2 is deleted |

### 📡 Execution Visualization

```text
INITIAL
Front = -1, Rear = -1
[ Empty ]

ADD J1
Front = 0, Rear = 0
[J1]

ADD J2
Front = 0, Rear = 1
[J1] [J2]

ADD J3
Front = 0, Rear = 2
[J1] [J2] [J3]

DELETE J1
Front = 1, Rear = 2
     [J2] [J3]

DELETE J2
Front = 2, Rear = 2
           [J3]
```

> [!IMPORTANT]
> Queue ordering guarantees that J1 is processed before J2 and J3, regardless of their individual processing requirements.

**System Impact:** FIFO scheduling provides predictable task ordering and is useful in operating systems, printer queues, and request-processing systems.

---

# 🧪 04. Implementation I — Sequential Array Queue

## 💠 Concept

A sequential queue uses a fixed-size array to store elements.

Two integer variables maintain the queue's boundaries:

- `front`: Position of the first element.
- `rear`: Position of the last element.

### 📐 Initial Configuration

```c
#define MAX_QUEUE_SIZE 100

typedef struct {
    int key;
    // Other fields
} element;

element queue[MAX_QUEUE_SIZE];

// -1 indicates an empty queue
int front = -1;
int rear = -1;
```

### 🔍 Queue Conditions

| Condition | Expression                   | Meaning                     |
| --------- | ---------------------------- | --------------------------- |
| Empty     | `front == rear`              | No elements available       |
| Full      | `rear == MAX_QUEUE_SIZE - 1` | Rear reached array boundary |
| Insert    | `++rear`                     | Move rear forward           |
| Delete    | `++front`                    | Move front forward          |

> [!NOTE]
> This implementation uses `front = rear = -1` initially. After deleting the final element, both pointers become equal, indicating an empty queue.

## 🛠️ Enqueue — Add to Queue

Insert an element at the rear of the array.

```c
void addq(element item) {

    // Check whether the array boundary is reached
    if (rear == MAX_QUEUE_SIZE - 1) {
        queue_full();
        return;
    }

    // Move rear and insert the new element
    queue[++rear] = item;
}
```

**System Impact:** Enqueue takes O(1) time because it inserts directly into the next available array position.

## 🛠️ Dequeue — Delete from Queue

Remove and return the element at the front.

```c
element deleteq(void) {

    // Check whether the queue is empty
    if (front == rear) {
        return queue_empty();
    }

    // Move front and return the next element
    return queue[++front];
}
```

**System Impact:** Dequeue takes O(1) time because it advances the front index without shifting remaining elements.

## ⚠️ Critical Limitation — Wasted Array Space

A sequential queue has a major limitation: **False Overflow**.

Even when elements have been deleted, the queue may report FULL because `rear` has already reached the last array position.

### 📡 Example

```text
Initial:
+----+----+----+----+----+
| J1 | J2 | J3 | J4 | J5 |
+----+----+----+----+----+
  F                   R

After deleting J1, J2, J3:

+----+----+----+----+----+
| -- | -- | -- | J4 | J5 |
+----+----+----+----+----+
                  F    R

Problem:
The first three positions are empty,
but rear is already at MAX_QUEUE_SIZE - 1.
```

> [!WARNING]
> The queue cannot insert new elements even though unused array positions exist at the beginning.

**System Impact:** Sequential queues waste reusable memory space, motivating the circular queue implementation.

---

# ⚡ 05. Optimization — Circular Array Queue

## 💠 Concept

A **Circular Queue** solves the wasted-space problem by treating the array as a circular structure.

Instead of stopping at the final array position, the rear pointer wraps around to index `0`.

This allows previously occupied positions to be reused.

### 🔄 Circular Index Calculation

The next position is calculated using the modulo operator:

```c
// Move to the next array position
next = (current + 1) % MAX_QUEUE_SIZE;
```

| Current Index | Next Index | Explanation  |
| ------------: | ---------: | ------------ |
|             0 |          1 | Move forward |
|             1 |          2 | Move forward |
|             2 |          3 | Move forward |
|             3 |          4 | Move forward |
|             4 |          0 | Wrap around  |

> [!IMPORTANT]
> Circular queues connect the final array position back to the first position, allowing efficient memory reuse.

## 📐 Front and Rear Representation

A circular queue uses two pointers:

- **Front:** One position counterclockwise (behind) the first element.
- **Rear:** Position of the last element.

This representation reserves one unused position to distinguish FULL from EMPTY.

### 🧪 Empty Queue

```text
        [2]
    [3]     [1]
   [4]       [0]
        [5]

    front = 0
    rear  = 0

    Queue = EMPTY
```

### 🧪 Partially Filled Queue

```text
        [2]
    [3]     [1]
   [4]       [0] ← Front
        [5]

    front = 0
    rear  = 3

    Queue contains J1, J2, J3
```

### 🧪 Full Queue

```text
        [2]  J2
    [3]     [1]  J1
   [4]       [0] ← Front
        [5]  J5

    front = 0
    rear  = 5

    Next rear position = 0
    Queue = FULL
```

---

## 🔍 Circular Queue Conditions

Assuming an array capacity of `MAX_QUEUE_SIZE`:

| Condition  | Expression                             |
| ---------- | -------------------------------------- |
| Empty      | `front == rear`                        |
| Full       | `(rear + 1) % MAX_QUEUE_SIZE == front` |
| Next Rear  | `(rear + 1) % MAX_QUEUE_SIZE`          |
| Next Front | `(front + 1) % MAX_QUEUE_SIZE`         |

> [!WARNING]
> One array position is deliberately left unused. Therefore, an array of size N can store at most N - 1 elements.

## 🛠️ Enqueue — Circular AddQ

```c
void addq(element item) {

    // Calculate the next rear position
    int next = (rear + 1) % MAX_QUEUE_SIZE;

    // Check whether advancing rear would collide with front
    if (next == front) {
        queue_full();
        return;
    }

    // Update rear and insert the item
    rear = next;
    queue[rear] = item;
}
```

**System Impact:** Modulo arithmetic enables rear to wrap around, making previously freed array positions reusable in O(1) time.

## 🛠️ Dequeue — Circular DeleteQ

```c
element deleteq(void) {

    element item;

    // Check whether the queue is empty
    if (front == rear) {
        return queue_empty();
    }

    // Move front to the next circular position
    front = (front + 1) % MAX_QUEUE_SIZE;

    // Retrieve the removed element
    item = queue[front];

    return item;
}
```

**System Impact:** Dequeue advances the front pointer circularly without shifting elements, maintaining O(1) deletion complexity.

---

# ⚡ 06. Optimization — Sequential vs Circular Queue

| Feature          | Sequential Queue   | Circular Queue            |
| ---------------- | ------------------ | ------------------------- |
| Memory structure | Linear array       | Circular array            |
| Insertion        | `rear++`           | `(rear + 1) % N`          |
| Deletion         | `front++`          | `(front + 1) % N`         |
| Memory reuse     | No automatic reuse | Reuses freed positions    |
| False overflow   | Possible           | Avoided                   |
| Empty condition  | `front == rear`    | `front == rear`           |
| Full condition   | `rear == N - 1`    | `(rear + 1) % N == front` |
| Maximum capacity | N elements         | N - 1 elements            |
| Enqueue          | O(1)               | O(1)                      |
| Dequeue          | O(1)               | O(1)                      |

> [!TIP]
> Both implementations offer constant-time insertion and deletion. The circular queue improves space utilization rather than asymptotic time complexity.

---

# 🧪 07. Formal Logic — Circular Queue State Transitions

## 🔄 Pointer Movement

Consider a circular queue with an array capacity of 5.

Initially:

```text
front = 0
rear  = 0

Queue = EMPTY
```

Perform the following operations:

| Operation  | Front | Rear | Contents   |
| ---------- | ----: | ---: | ---------- |
| Initialize |     0 |    0 | Empty      |
| Add A      |     0 |    1 | A          |
| Add B      |     0 |    2 | A, B       |
| Add C      |     0 |    3 | A, B, C    |
| Delete A   |     1 |    3 | B, C       |
| Add D      |     1 |    4 | B, C, D    |
| Add E      |     1 |    0 | B, C, D, E |
| Add F      |     - |    - | FULL       |

Notice that `rear` wraps from index 4 to index 0.

The queue can hold four elements because one position must remain unused.

> [!IMPORTANT]
> The circular queue is FULL when the next rear position equals front. This is different from simply checking whether rear equals the final array index.

---

# 🏁 08. Recap — Interview & Examination Revision

## 💠 Essential Concepts

- Queue follows FIFO: First-In, First-Out.
- Enqueue inserts at the rear.
- Dequeue removes from the front.
- Sequential queues may experience false overflow.
- Circular queues reuse previously freed array positions.
- Modulo arithmetic enables circular pointer movement.
- Circular queues reserve one unused position when using the single-empty-slot technique.

## ⚡ Must-Memorize Formulas

```c
// Sequential Queue
EMPTY: front == rear
FULL:  rear == MAX_QUEUE_SIZE - 1

// Circular Queue
EMPTY: front == rear
FULL:  (rear + 1) % MAX_QUEUE_SIZE == front

// Circular Pointer Movement
NEXT:  (index + 1) % MAX_QUEUE_SIZE

// Maximum Circular Queue Capacity
CAPACITY: MAX_QUEUE_SIZE - 1
```

## 🧠 Common Examination Traps

| Question                               | Correct Answer                     |
| -------------------------------------- | ---------------------------------- |
| Where does insertion occur?            | Rear                               |
| Where does deletion occur?             | Front                              |
| What is FIFO?                          | First-In, First-Out                |
| Why does sequential queue waste space? | Deleted positions cannot be reused |
| How does circular queue solve this?    | Wrap-around indexing               |
| Why is one position unused?            | To distinguish FULL from EMPTY     |
| What does modulo do?                   | Wraps index back to zero           |
| Time complexity of enqueue?            | O(1)                               |
| Time complexity of dequeue?            | O(1)                               |

## 🏁 Final Takeaway

> [!SUCCESS] SYSTEM RECAP
> A queue is a FIFO data structure that processes elements in arrival order.
>
> - **Sequential Queue:** Simple implementation, but suffers from false overflow.
> - **Circular Queue:** Efficient array-space reuse through modulo arithmetic.
> - **Core Principle:** Insert at rear, remove from front.
>
> **Remember:** Circular queues optimize memory utilization without changing the O(1) time complexity of basic queue operations.

---

# 🔗 09. Knowledge Connections

| Related Concept   | Connection                                     |
| ----------------- | ---------------------------------------------- |
| Stack             | Uses LIFO instead of FIFO                      |
| Linked List       | Can implement a dynamically sized queue        |
| Deque             | Supports insertion and deletion at both ends   |
| Priority Queue    | Processes elements according to priority       |
| BFS               | Uses a queue to explore graph levels           |
| CPU Scheduling    | Manages processes waiting for execution        |
| Producer-Consumer | Uses queues to exchange data between processes |

**Study Direction:** Understand FIFO → Master sequential arrays → Identify false overflow → Understand modulo arithmetic → Implement circular queues → Apply queues to BFS and scheduling.
