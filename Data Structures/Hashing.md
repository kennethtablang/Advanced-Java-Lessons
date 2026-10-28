# Data Structures Discussion: Hashing, HashSets, and HashMaps

## Learning Objectives

At the end of this discussion, students should be able to:

1. Explain hashing and the role of a hash function.
2. Calculate hash values and table indexes.
3. Design and illustrate a basic hash table.
4. Identify collisions and apply different collision-handling techniques.
5. Implement separate chaining and linear probing conceptually.
6. Explain and perform HashSet operations.
7. Explain and implement HashMap operations using Java.
8. Apply hashing to practical applications such as attendance systems, student records, and inventory management.
9. Follow the implementation rules for hash functions, sets, and maps.

---

# PART I. UNDERSTANDING HASHING

## 1. What Is Hashing?

**Hashing** is a technique used to convert a key into a value that helps determine where data should be stored in a table.

A hash-based data structure uses a **hash function** to calculate a location where an element or key-value pair can be stored or retrieved.

Without hashing, a program might need to examine many records one by one. With hashing, it can often locate the correct area directly.

### Illustration 1: Searching Without Hashing

Suppose a student information system stores these student IDs:

```text
STU-101
STU-102
STU-103
STU-104
STU-105
STU-106
STU-107
STU-108
```

The program needs to find `STU-107`.

Using a sequential search, it might check the records one by one:

```text
Check STU-101  -> Not found
Check STU-102  -> Not found
Check STU-103  -> Not found
Check STU-104  -> Not found
Check STU-105  -> Not found
Check STU-106  -> Not found
Check STU-107  -> FOUND!
```

The program has to inspect seven records in this example.

### Illustration 2: Searching With Hashing

Instead, suppose a hash function calculates a table index from the student's numeric ID.

```text
Student ID: 107
      |
      v
Hash Function
      |
      v
Index = 107 % 10
      |
      v
Index = 7
      |
      v
Check Table Slot 7
      |
      v
Find the Record
```

The program goes directly to the calculated slot, provided the entry has been stored there and no collision-handling step is required.

**Important:** Hashing does not guarantee that every item will have a unique location. Collisions can occur, so a proper implementation must handle them.

## 2. Applications of Hashing

| Application | Key or Input | Purpose |
|---|---|---|
| Student attendance | Student ID | Detect duplicate attendance entries |
| Student information system | Student ID | Retrieve student details |
| Inventory management | Product code | Find a product record |
| Online shopping cart | Product ID | Retrieve cart item information |
| Word counter | Word | Count word occurrences |
| Email registration | Email address | Check whether an address already exists |
| Caching | Request identifier | Locate previously computed results |
| Duplicate detection | File identifier or content hash | Help identify possible duplicate content |

Hashing is useful when a program frequently needs to check membership or retrieve information by key.

---

# PART II. HASH FUNCTIONS AND TABLE DESIGN

## 3. What Is a Hash Function?

A **hash function** accepts a key and calculates a hash value. The hash value is then used to determine a position in a hash table.

A simple hash function for integer keys is:

```text
hash(key) = key
```

If the table contains 10 slots, the table index can be calculated using:

```text
index = hash(key) % tableSize
```

Here, `%` is the modulo operator, which returns the remainder after division.

### Illustration 3: Hash Function Process

```text
             INPUT KEY
                 |
                 v
          +---------------+
          | Hash Function |
          +---------------+
                 |
                 v
            Hash Value
                 |
                 v
       +-------------------+
       | Modulo Operation  |
       | hash % tableSize  |
       +-------------------+
                 |
                 v
            Table Index
                 |
                 v
       +-------------------+
       | Hash Table Slot   |
       +-------------------+
```

### Step-by-step procedure

1. Receive the key.
2. Apply the selected hash function.
3. Calculate the table index.
4. Check the corresponding table slot.
5. Store the element if the slot is available.
6. If the slot is occupied by another key, apply a collision-handling technique.

## 4. Designing a Basic Hash Table

Suppose we have a hash table with 10 slots.

The valid indexes are:

```text
0, 1, 2, 3, 4, 5, 6, 7, 8, 9
```

Use this hash function:

```text
index = key % 10
```

### Example 1: Calculate the indexes

Calculate the index of each key.

| Key | Division | Remainder | Index |
|---:|---|---:|---:|
| 23 | 23 ÷ 10 | 3 | 3 |
| 45 | 45 ÷ 10 | 5 | 5 |
| 67 | 67 ÷ 10 | 7 | 7 |
| 32 | 32 ÷ 10 | 2 | 2 |
| 18 | 18 ÷ 10 | 8 | 8 |

### Illustration 4: Hash Table Before Insertion

```text
Index     Stored Value
----------------------
  0       EMPTY
  1       EMPTY
  2       EMPTY
  3       EMPTY
  4       EMPTY
  5       EMPTY
  6       EMPTY
  7       EMPTY
  8       EMPTY
  9       EMPTY
```

### Illustration 5: Hash Table After Insertion

After inserting `23`, `45`, `67`, `32`, and `18`:

```text
Index     Stored Value
----------------------
  0       EMPTY
  1       EMPTY
  2       32
  3       23
  4       EMPTY
  5       45
  6       EMPTY
  7       67
  8       18
  9       EMPTY
```

Notice that each key is placed at the index calculated by the hash function.

### Insertion procedure

For every key:

1. Calculate the index using `key % 10`.
2. Locate the calculated index.
3. If the slot is empty, insert the key.
4. If the slot contains the same key, follow the duplicate-handling rule.
5. If the slot contains a different key, handle the collision.
6. Repeat for the remaining keys.

## 5. Qualities of a Good Hash Function

A good hash function should be:

- **Deterministic:** The same key produces the same hash value under the same hashing rules.
- **Fast:** The function should not take excessive time to calculate.
- **Well distributed:** Keys should spread across the table rather than cluster in a few slots.
- **Consistent with equality:** Keys considered equal must have equal hash codes.

A hash function does not need to produce a unique result for every key.

For example:

```text
23 % 10 = 3
33 % 10 = 3
43 % 10 = 3
```

All three keys map to index 3. This is a collision, not necessarily an error.

## 6. Table Capacity and Load Factor

The **table capacity** is the number of slots or buckets available.

The **load factor** indicates how full a hash table is.

Formula:

```text
Load Factor = Number of Stored Entries / Table Capacity
```

### Example

A table has 10 slots and contains 7 entries.

```text
Load Factor = 7 / 10
            = 0.7
            = 70%
```

| Table Capacity | Stored Entries | Load Factor |
|---:|---:|---:|
| 10 | 2 | 0.20 |
| 10 | 4 | 0.40 |
| 10 | 7 | 0.70 |
| 10 | 9 | 0.90 |
| 10 | 10 | 1.00 |

A high load factor can increase collisions and slow down some implementations.

### Illustration 6: Table Occupancy

```text
Capacity = 10

Low occupancy:
[23][  ][  ][45][  ][  ][  ][  ][  ][  ]

Higher occupancy:
[23][33][43][53][63][  ][  ][  ][  ][  ]

Nearly full:
[23][33][43][53][63][73][83][93][13][  ]
```

These illustrations show occupancy only. Whether the entries are valid at those positions depends on the collision-handling strategy.

### Table-design rules

1. Estimate the number of expected entries.
2. Choose an appropriate initial capacity.
3. Select a suitable hash function.
4. Decide how collisions will be handled.
5. Monitor the load factor when implementing the table yourself.
6. Resize when needed and recalculate entry positions.
7. Use tested library implementations when a custom implementation is not required.

---

# PART III. COLLISIONS AND HANDLING TECHNIQUES

## 7. What Is a Collision?

A **collision** occurs when two different keys produce the same table index.

Suppose we have the following keys:

```text
23, 33, 43, 53
```

Using:

```text
index = key % 10
```

we get:

| Key | Calculation | Index |
|---:|---|---:|
| 23 | 23 % 10 | 3 |
| 33 | 33 % 10 | 3 |
| 43 | 43 % 10 | 3 |
| 53 | 53 % 10 | 3 |

Every key maps to index 3.

### Illustration 7: Collision Occurrence

```text
       Key 23 --------+
                      |
       Key 33 --------+
                      |
       Key 43 --------+----> Index 3
                      |      +---------+
       Key 53 --------+      |    23   |
                             +---------+
                             Slot Occupied!
```

The table cannot simply overwrite `23` with `33`, because the original entry would be lost.

Instead, we need a collision-handling technique.

The two major approaches discussed here are:

1. Separate chaining
2. Open addressing

---

## 8. Collision Technique 1: Separate Chaining

**Separate chaining** stores multiple entries in a collection associated with the same bucket.

A bucket may contain a linked list or another collection of entries.

### Example

Insert these keys:

```text
23, 33, 43, 53
```

Hash function:

```text
index = key % 10
```

All four keys map to bucket 3.

### Illustration 8: Separate Chaining

```text
Index 0  ---> EMPTY

Index 1  ---> EMPTY

Index 2  ---> EMPTY

Index 3  ---> [23] ---> [33] ---> [43] ---> [53] ---> NULL

Index 4  ---> EMPTY

Index 5  ---> EMPTY

Index 6  ---> EMPTY

Index 7  ---> EMPTY

Index 8  ---> EMPTY

Index 9  ---> EMPTY
```

The bucket at index 3 stores all four keys.

### Insertion procedure using separate chaining

Suppose the program needs to insert `63`.

1. Calculate its index:

   ```text
   63 % 10 = 3
   ```

2. Go to bucket 3.
3. Examine the existing entries.
4. Check whether key `63` already exists.
5. If it does not exist, add it to the bucket.

The resulting chain becomes:

```text
Index 3 ---> [23] ---> [33] ---> [43] ---> [53] ---> [63] ---> NULL
```

### Search procedure

Search for key `43`.

```text
Search Key: 43
      |
      v
43 % 10 = 3
      |
      v
Go to Bucket 3
      |
      v
Compare 23 -> Not equal
      |
      v
Compare 33 -> Not equal
      |
      v
Compare 43 -> MATCH!
```

### Deletion procedure

To remove key `33`:

1. Calculate `33 % 10 = 3`.
2. Visit bucket 3.
3. Find the node containing `33`.
4. Remove that node from the chain.
5. Keep the other entries.

Before deletion:

```text
[23] ---> [33] ---> [43] ---> [53]
```

After deletion:

```text
[23] ---> [43] ---> [53]
```

### Advantages

- Multiple keys can occupy the same bucket.
- Deletion can be straightforward.
- The table does not need an empty slot for every stored entry.

### Limitations

- Long chains can slow down searching.
- Additional memory may be needed for linked nodes or other bucket collections.

---

## 9. Collision Technique 2: Open Addressing

**Open addressing** stores all entries directly in the table.

When a collision occurs, the algorithm searches for another slot according to a probing rule.

The common methods are:

1. Linear probing
2. Quadratic probing
3. Double hashing

### Illustration 9: General Open Addressing Process

```text
         Calculate Index
                |
                v
          Is Slot Empty?
           /         \
         YES          NO
          |            |
          v            v
      Insert Key   Try Another Slot
                       |
                       v
                Apply Probe Rule
                       |
                       v
                 Check Again
```

## 10. Linear Probing

**Linear probing** checks the next slot whenever a collision occurs.

Formula:

```text
index = (hash(key) + i) % tableSize
```

Where:

- `hash(key)` is the initial hash value.
- `i` is the probe number, starting at `0`.
- `tableSize` is the number of slots.

### Example: Insert 23, 33, 43, and 53

Table size:

```text
10
```

Hash function:

```text
index = key % 10
```

### Step 1: Insert 23

```text
23 % 10 = 3
```

Slot 3 is empty.

```text
Index:  0  1  2  3  4  5  6  7  8  9
Value:  -  -  - 23  -  -  -  -  -  -
```

### Step 2: Insert 33

```text
33 % 10 = 3
```

Slot 3 is occupied by `23`.

Try the next slot:

```text
(3 + 1) % 10 = 4
```

Slot 4 is empty, so insert `33`.

```text
Index:  0  1  2  3  4  5  6  7  8  9
Value:  -  -  - 23 33  -  -  -  -  -
```

### Step 3: Insert 43

```text
43 % 10 = 3
```

Slot 3 is occupied.

Try slot 4. It is also occupied.

Try slot 5:

```text
(3 + 2) % 10 = 5
```

Slot 5 is empty.

```text
Index:  0  1  2  3  4  5  6  7  8  9
Value:  -  -  - 23 33 43  -  -  -  -
```

### Step 4: Insert 53

```text
53 % 10 = 3
```

Check slots 3, 4, and 5. All are occupied.

The next available slot is 6.

```text
Index:  0  1  2  3  4  5  6  7  8  9
Value:  -  -  - 23 33 43 53  -  -  -
```

### Illustration 10: Linear Probing Summary

| Key | Initial Index | Probe Sequence | Final Index |
|---:|---:|---|---:|
| 23 | 3 | 3 | 3 |
| 33 | 3 | 3 → 4 | 4 |
| 43 | 3 | 3 → 4 → 5 | 5 |
| 53 | 3 | 3 → 4 → 5 → 6 | 6 |

### Searching with linear probing

To search for `43`:

1. Calculate `43 % 10 = 3`.
2. Check slot 3. It contains `23`.
3. Check slot 4. It contains `33`.
4. Check slot 5. It contains `43`.
5. Return the matching entry.

**Important:** Search must follow the same probe sequence used by insertion.

### Deleting with linear probing

Suppose slot 4 contains `33`, and we delete it.

Do not automatically mark slot 4 as completely empty if later entries depend on the same probe sequence. A search for `43` could incorrectly stop at slot 4 and report that `43` is missing.

Implementations commonly use a deleted marker called a **tombstone**.

Before deletion:

```text
Index 3: 23
Index 4: 33
Index 5: 43
Index 6: 53
```

After deletion:

```text
Index 3: 23
Index 4: DELETED
Index 5: 43
Index 6: 53
```

A search can continue past the deleted slot.

---

## 11. Quadratic Probing

Quadratic probing changes the probing distance using a quadratic expression.

One example formula is:

```text
index = (hash(key) + i * i) % tableSize
```

The probing offsets are:

```text
i = 0  -> Offset 0
i = 1  -> Offset 1
i = 2  -> Offset 4
i = 3  -> Offset 9
i = 4  -> Offset 16
```

### Example

Assume:

```text
Initial index = 3
Table size = 10
```

The probe sequence is:

| Probe Number | Calculation | Index |
|---:|---|---:|
| 0 | (3 + 0²) % 10 | 3 |
| 1 | (3 + 1²) % 10 | 4 |
| 2 | (3 + 2²) % 10 | 7 |
| 3 | (3 + 3²) % 10 | 2 |
| 4 | (3 + 4²) % 10 | 9 |

### Illustration 11: Quadratic Probe Sequence

```text
Start at 3
    |
    v
Index 3
    |
    v
Index 4
    |
    v
Index 7
    |
    v
Index 2
    |
    v
Index 9
```

Quadratic probing can reduce primary clustering compared with linear probing. However, it does not guarantee that every slot can be reached for every table capacity and formula. The table size and probing rules must be selected carefully.

---

## 12. Double Hashing

**Double hashing** uses two hash functions.

The first calculates the starting index. The second calculates the step size.

Example formula:

```text
index = (hash1(key) + i * hash2(key)) % tableSize
```

For illustration, use:

```text
hash1(key) = key % 10
hash2(key) = 7 - (key % 7)
```

For key `23`:

```text
hash1(23) = 23 % 10 = 3
hash2(23) = 7 - (23 % 7)
           = 7 - 2
           = 5
```

The probe sequence becomes:

| Probe Number | Calculation | Index |
|---:|---|---:|
| 0 | (3 + 0 × 5) % 10 | 3 |
| 1 | (3 + 1 × 5) % 10 | 8 |
| 2 | (3 + 2 × 5) % 10 | 3 |
| 3 | (3 + 3 × 5) % 10 | 8 |

This example demonstrates an important design issue: the step size and table capacity can cause a probe sequence to revisit the same positions.

For a practical double-hashing implementation, select the second hash function and table capacity so that the step size is non-zero and can reach every required slot. For example, a prime table capacity with a suitable step size relatively prime to the capacity can provide a full probe cycle.

### Comparison of collision-handling methods

| Method | How It Works | Main Advantage | Main Concern |
|---|---|---|---|
| Separate chaining | Stores multiple entries in a bucket collection | Handles multiple entries per bucket | Long chains |
| Linear probing | Checks consecutive slots | Simple to implement | Primary clustering |
| Quadratic probing | Checks slots using quadratic offsets | Reduces some clustering | May not reach all slots |
| Double hashing | Uses a second hash function for the step size | Can improve distribution | Requires careful function design |

---

# PART IV. HASHSET: SET OPERATIONS AND IMPLEMENTATION

## 13. What Is a Set?

A **set** is a collection of distinct elements. It does not contain duplicate elements.

For example:

```text
Set A = {10, 20, 30, 40}
```

Adding `20` again does not create another copy.

```text
Before: {10, 20, 30, 40}

Add 20

After:  {10, 20, 30, 40}
```

A **HashSet** implements a set using hashing. It is useful for fast membership checks and duplicate detection on average.

## 14. HashSet Operations

Suppose:

```text
A = {10, 20, 30, 40}
B = {30, 40, 50, 60}
```

### Operation 1: Add

Add `50` to A.

```text
Before:
A = {10, 20, 30, 40}

Add 50

After:
A = {10, 20, 30, 40, 50}
```

If we add `30` again, A remains unchanged.

### Operation 2: Contains

Check whether `20` belongs to A.

```text
A = {10, 20, 30, 40}

Contains(20) = TRUE
Contains(99) = FALSE
```

### Operation 3: Remove

Remove `20` from A.

```text
Before:
A = {10, 20, 30, 40}

Remove(20)

After:
A = {10, 30, 40}
```

### Operation 4: Union

The **union** contains all distinct elements found in either set.

Notation:

```text
A ∪ B
```

Illustration:

```text
A = {10, 20, 30, 40}
B = {30, 40, 50, 60}

A ∪ B = {10, 20, 30, 40, 50, 60}
```

The shared elements `30` and `40` appear only once.

### Operation 5: Intersection

The **intersection** contains only elements shared by both sets.

Notation:

```text
A ∩ B
```

Illustration:

```text
A = {10, 20, 30, 40}
B = {30, 40, 50, 60}

A ∩ B = {30, 40}
```

### Operation 6: Difference

The difference `A - B` contains elements in A that are not in B.

```text
A = {10, 20, 30, 40}
B = {30, 40, 50, 60}

A - B = {10, 20}
B - A = {50, 60}
```

### Illustration 12: Set Operations Table

| Operation | Input | Result |
|---|---|---|
| Union | A ∪ B | `{10, 20, 30, 40, 50, 60}` |
| Intersection | A ∩ B | `{30, 40}` |
| Difference | A − B | `{10, 20}` |
| Difference | B − A | `{50, 60}` |

## 15. Venn Diagram Illustration

The following diagram illustrates how the regions of two sets relate to each other.

```text
              SET A                 SET B
          _____________         _____________
         /             \       /             \
        /   A only      \_____/    B only     \
       /                 / 30  \               \
      |       10        |  40  |       50       |
      |       20        |      |       60       |
       \                \_____/               /
        \                                     /
         \___________________________________/
```

For the example sets:

```text
A only          = {10, 20}
Shared region   = {30, 40}
B only          = {50, 60}
```

Therefore:

```text
Union        = A only + Shared region + B only
Intersection = Shared region
A - B        = A only
B - A        = B only
```

## 16. Step-by-Step: Implement a HashSet in Java

### Scenario: Student Attendance

A teacher needs to record which students attended class. Each student has a unique ID, and an ID should not be recorded twice.

### Procedure

1. Import `HashSet` and `Set`.
2. Create an empty set of student IDs.
3. Read each student ID.
4. Use `add()` to record the ID.
5. Check the Boolean result returned by `add()`.
6. If it returns `true`, the ID was newly added.
7. If it returns `false`, the ID was already present.
8. Use `contains()` to check attendance.
9. Use `remove()` if an entry must be removed.

### Java implementation

```java
import java.util.HashSet;
import java.util.Set;

public class AttendanceExample {
    public static void main(String[] args) {
        Set<String> attendedStudentIds = new HashSet<>();

        // Add the first student.
        boolean first = attendedStudentIds.add("STU-101");

        // Add another student.
        boolean second = attendedStudentIds.add("STU-102");

        // Attempt to add a duplicate.
        boolean duplicate = attendedStudentIds.add("STU-101");

        System.out.println("First ID added: " + first);
        System.out.println("Second ID added: " + second);
        System.out.println("Duplicate added: " + duplicate);

        // Check whether a student is recorded.
        if (attendedStudentIds.contains("STU-101")) {
            System.out.println("STU-101 is recorded.");
        }

        // Remove a student ID.
        attendedStudentIds.remove("STU-102");

        System.out.println("Remaining IDs: " + attendedStudentIds);
    }
}
```

### Expected behavior

```text
First ID added: true
Second ID added: true
Duplicate added: false
STU-101 is recorded.
Remaining IDs: [STU-101]
```

The exact order of the printed set is not guaranteed. In this example, only one element remains after removal.

## 17. Java Implementation of Set Operations

```java
import java.util.HashSet;
import java.util.Set;

public class SetOperationsExample {
    public static void main(String[] args) {
        Set<Integer> a = new HashSet<>();
        Set<Integer> b = new HashSet<>();

        // Populate set A.
        a.add(10);
        a.add(20);
        a.add(30);
        a.add(40);

        // Populate set B.
        b.add(30);
        b.add(40);
        b.add(50);
        b.add(60);

        // UNION: Elements from either set.
        Set<Integer> union = new HashSet<>(a);
        union.addAll(b);

        // INTERSECTION: Elements shared by both sets.
        Set<Integer> intersection = new HashSet<>(a);
        intersection.retainAll(b);

        // DIFFERENCE: Elements in A but not in B.
        Set<Integer> difference = new HashSet<>(a);
        difference.removeAll(b);

        System.out.println("A: " + a);
        System.out.println("B: " + b);
        System.out.println("Union: " + union);
        System.out.println("Intersection: " + intersection);
        System.out.println("A - B: " + difference);
    }
}
```

The copies are important. They allow the program to calculate each operation without unintentionally modifying the original sets.

---

# PART V. HASHMAP: MAP DATA STRUCTURES

## 18. What Is a Map?

A **Map** stores information as key-value pairs.

Each key identifies the value associated with it.

Example:

```text
Student ID  -> Student Name
STU-101     -> Ana
STU-102     -> Ben
STU-103     -> Carlo
```

A **HashMap** uses hashing to help locate the value associated with a key.

### Illustration 13: HashMap Structure

```text
              KEY
               |
               v
         +------------+
         | Hash       |
         | Function   |
         +------------+
               |
               v
          Table Index
               |
               v
         +-------------------+
         | Hash Table        |
         +-------------------+
         | Key   | Value     |
         +-------------------+
         | 101   | Ana       |
         | 102   | Ben       |
         | 103   | Carlo     |
         +-------------------+
```

The table shown is a conceptual representation. A real HashMap organizes entries according to its implementation, not necessarily in the visual order shown.

## 19. Map Operations

| Method | Purpose |
|---|---|
| `put(key, value)` | Add a mapping or update an existing key |
| `get(key)` | Retrieve the mapped value |
| `containsKey(key)` | Check whether a key exists |
| `containsValue(value)` | Check whether a value exists |
| `remove(key)` | Remove a mapping |
| `getOrDefault(key, defaultValue)` | Return a fallback value if no mapping exists |
| `size()` | Return the number of mappings |
| `isEmpty()` | Check whether the map contains no mappings |
| `clear()` | Remove all mappings |

### Important rule

Map keys are unique. If a program calls `put()` with an existing key, the value associated with that key is replaced.

Example:

```text
Before:
STU-102 -> Ben

Execute:
put("STU-102", "Benjamin")

After:
STU-102 -> Benjamin
```

The map does not create two entries for `STU-102`.

## 20. Step-by-Step: Insert and Retrieve Map Entries

### Scenario: Student Records

We need to associate each student ID with a student name.

### Step 1: Create the map

```java
Map<String, String> students = new HashMap<>();
```

The key type is `String`, and the value type is also `String`.

### Step 2: Insert records

```java
students.put("STU-101", "Ana");
students.put("STU-102", "Ben");
students.put("STU-103", "Carlo");
```

Conceptual table:

| Key | Value |
|---|---|
| STU-101 | Ana |
| STU-102 | Ben |
| STU-103 | Carlo |

### Step 3: Retrieve a value

```java
String studentName = students.get("STU-102");
```

Result:

```text
Ben
```

### Step 4: Update a value

```java
students.put("STU-102", "Benjamin");
```

Updated conceptual table:

| Key | Value |
|---|---|
| STU-101 | Ana |
| STU-102 | Benjamin |
| STU-103 | Carlo |

### Step 5: Remove a record

```java
students.remove("STU-103");
```

Remaining mappings:

| Key | Value |
|---|---|
| STU-101 | Ana |
| STU-102 | Benjamin |

## 21. Complete Java HashMap Example

```java
import java.util.HashMap;
import java.util.Map;

public class StudentMapExample {
    public static void main(String[] args) {
        Map<String, String> students = new HashMap<>();

        // Insert key-value pairs.
        students.put("STU-101", "Ana");
        students.put("STU-102", "Ben");
        students.put("STU-103", "Carlo");

        // Retrieve a student name.
        System.out.println(students.get("STU-101"));

        // Check whether a student ID exists.
        if (students.containsKey("STU-102")) {
            System.out.println("Student ID exists.");
        }

        // Update the value for an existing key.
        students.put("STU-102", "Benjamin");

        // Retrieve a value with a fallback.
        String unknown = students.getOrDefault(
            "STU-999",
            "Student not found"
        );

        System.out.println(unknown);

        // Remove a mapping.
        students.remove("STU-103");

        // Traverse the remaining key-value pairs.
        for (Map.Entry<String, String> entry : students.entrySet()) {
            System.out.println(
                entry.getKey() + " -> " + entry.getValue()
            );
        }
    }
}
```

A missing key returns `null` from `get()` in this example because no value was mapped to it. A map can also explicitly associate a key with `null`, so use `containsKey()` when it is necessary to distinguish these cases.

## 22. HashMap Application: Product Inventory

An inventory system can use product codes as keys and stock quantities as values.

Initial data:

```text
P-100 -> 15
P-200 -> 8
P-300 -> 20
```

Conceptual table:

| Product Code | Stock Quantity |
|---|---:|
| P-100 | 15 |
| P-200 | 8 |
| P-300 | 20 |

### Required procedures

1. Insert the product codes and quantities.
2. Retrieve the stock of `P-200`.
3. Update `P-200` to 12 units.
4. Add 5 units to `P-100`.
5. Check whether `P-400` exists.
6. Remove `P-300`.
7. Display the remaining inventory.

### Java implementation

```java
import java.util.HashMap;
import java.util.Map;

public class InventoryExample {
    public static void main(String[] args) {
        Map<String, Integer> inventory = new HashMap<>();

        // Insert initial inventory records.
        inventory.put("P-100", 15);
        inventory.put("P-200", 8);
        inventory.put("P-300", 20);

        // Retrieve the quantity for P-200.
        System.out.println(
            "P-200 stock: " + inventory.get("P-200")
        );

        // Update P-200 stock to 12.
        inventory.put("P-200", 12);

        // Add five units to P-100.
        int currentStock = inventory.getOrDefault("P-100", 0);
        inventory.put("P-100", currentStock + 5);

        // Check for an unknown product.
        if (inventory.containsKey("P-400")) {
            System.out.println("P-400 exists.");
        } else {
            System.out.println("P-400 does not exist.");
        }

        // Remove a product.
        inventory.remove("P-300");

        // Display the remaining inventory.
        for (Map.Entry<String, Integer> entry : inventory.entrySet()) {
            System.out.println(
                entry.getKey() + " -> " + entry.getValue()
            );
        }
    }
}
```

Expected final mappings, in no guaranteed order:

```text
P-100 -> 20
P-200 -> 12
```

---

# PART VI. WORD FREQUENCY USING HASHMAP

## 23. Counting Word Occurrences

A word counter demonstrates why HashMaps are useful when a program needs to associate each distinct item with a count.

Input:

```text
java hashing java map java hashing
```

We want to calculate the frequency of each word.

### Step-by-step table

| Step | Word Read | Previous Count | New Count |
|---:|---|---:|---:|
| 1 | java | 0 | 1 |
| 2 | hashing | 0 | 1 |
| 3 | java | 1 | 2 |
| 4 | map | 0 | 1 |
| 5 | java | 2 | 3 |
| 6 | hashing | 1 | 2 |

### Illustration 14: Map Changes During Processing

Initial state:

```text
{}
```

Read `java`:

```text
{java=1}
```

Read `hashing`:

```text
{java=1, hashing=1}
```

Read `java` again:

```text
{java=2, hashing=1}
```

Read `map`:

```text
{java=2, hashing=1, map=1}
```

Read `java` again:

```text
{java=3, hashing=1, map=1}
```

Read `hashing` again:

```text
{java=3, hashing=2, map=1}
```

### Java implementation

```java
import java.util.HashMap;
import java.util.Map;

public class WordCounter {
    public static void main(String[] args) {
        String[] words = {
            "java", "hashing", "java",
            "map", "java", "hashing"
        };

        Map<String, Integer> counts = new HashMap<>();

        for (String word : words) {
            // Return zero when the word has no current mapping.
            int currentCount = counts.getOrDefault(word, 0);

            // Increase the frequency of the word.
            counts.put(word, currentCount + 1);
        }

        // Display each word and its frequency.
        for (Map.Entry<String, Integer> entry : counts.entrySet()) {
            System.out.println(
                entry.getKey() + " -> " + entry.getValue()
            );
        }
    }
}
```

Expected frequencies:

```text
java -> 3
hashing -> 2
map -> 1
```

The output order may differ.

---

# PART VII. HASHSET VS. HASHMAP

## 24. Comparison of the Data Structures

| Feature | HashSet | HashMap |
|---|---|---|
| Main purpose | Store distinct values | Store key-value pairs |
| Stored item | Element | Key and value |
| Duplicate behavior | Duplicate elements are not added | Existing key's value is replaced by `put()` |
| Membership or lookup | `contains(element)` | `containsKey(key)` and `get(key)` |
| Removal | `remove(element)` | `remove(key)` |
| Typical example | Unique student IDs | Student ID mapped to student name |
| Average lookup time | O(1) | O(1) |

### Illustration 15: Choosing the Correct Structure

```text
What information must be stored?
                |
                v
       +-------------------+
       | Only unique items?|
       +-------------------+
          /           \
        YES            NO
         |              |
         v              v
      HashSet     Need key-value
                     mapping?
                         |
                         v
                     HashMap
```

Use a HashSet when the main requirement is uniqueness or membership.

Use a HashMap when a key must identify a corresponding value.

---

# PART VIII. GENERAL IMPLEMENTATION PROCEDURES

## 25. Procedure for Implementing a Hash Table From Scratch

When an exercise requires students to implement a hash table without using a built-in collection, follow these steps.

### Step 1: Define the storage

Create an array or another structure that represents the table.

```text
table[0]
table[1]
table[2]
...
table[m - 1]
```

### Step 2: Define the hash function

For a simple integer-key exercise:

```text
index = key % tableSize
```

### Step 3: Implement insertion

1. Calculate the index.
2. Check whether the slot is available.
3. If available, store the entry.
4. If occupied, use the selected collision strategy.
5. Prevent duplicate keys according to the table's rules.

### Step 4: Implement searching

1. Calculate the index.
2. Inspect the correct bucket or probe sequence.
3. Compare actual keys.
4. Return the value or indicate that the key does not exist.

### Step 5: Implement deletion

1. Locate the entry using the same lookup rules.
2. Remove it using the deletion procedure for the selected collision strategy.
3. Preserve the ability to find other entries.

### Step 6: Handle capacity

1. Determine whether the table is becoming too full.
2. Allocate a larger table when needed.
3. Recalculate indexes using the new capacity.
4. Insert all existing entries into their new positions.

### Step 7: Test edge cases

Test:

- An empty table
- Duplicate keys
- Colliding keys
- Missing keys
- Deleting an existing key
- Deleting a missing key
- A table that becomes full
- Resizing, if implemented

---

# PART IX. IMPLEMENTATION RULES

## 26. Rules for Hash Functions and Keys

1. The same key must produce the same hash code under the same hashing rules.
2. Equal keys must have equal hash codes.
3. A matching hash code does not prove that two keys are equal.
4. Compare actual keys when searching for a matching entry.
5. Do not assume every key produces a unique index.
6. Use a stable key whose equality does not change while stored.
7. Use a hash function appropriate for the key type.
8. Remember that table indexing and cryptographic hashing serve different purposes.

## 27. Rules for Collision Handling

1. Never overwrite a different key simply because it occupies the same index.
2. Select and consistently apply a collision-handling strategy.
3. Make searching follow the same bucket or probe rules as insertion.
4. Handle deletion without breaking future searches.
5. Ensure probing stops safely when a key is absent or the table is full.
6. Choose table capacities and probing formulas carefully.
7. Recalculate entry positions after resizing.
8. Consider clustering and load factor when evaluating table performance.

## 28. Rules for HashSet

1. Use HashSet for distinct values.
2. Use `add()` to insert an element.
3. Check the result of `add()` when duplicate detection matters.
4. Use `contains()` to check membership.
5. Use `remove()` to delete an element.
6. Use `addAll()` for union, `retainAll()` for intersection, and `removeAll()` for difference.
7. Do not assume the collection is sorted.
8. Do not rely on insertion order.
9. In Java, implement `equals()` and `hashCode()` consistently for custom objects stored in a HashSet.

## 29. Rules for HashMap

1. Use HashMap for key-value associations.
2. Choose meaningful and stable keys.
3. Remember that each key has at most one current mapping.
4. Use `put()` to insert or update a mapping.
5. Use `get()` to retrieve a mapped value.
6. Use `containsKey()` when the program must distinguish a missing key from a key mapped to `null`.
7. Use `getOrDefault()` when a default value is appropriate.
8. Use `remove()` to delete a mapping.
9. Do not assume iteration order.
10. Implement `equals()` and `hashCode()` consistently for custom Java key classes.
11. Avoid changing fields that determine a key's equality or hash code while it is stored.
12. Use synchronization or a suitable concurrent map when thread-safe concurrent updates are required.

## 30. Java `equals()` and `hashCode()` Rules

For custom Java objects used as HashSet elements or HashMap keys:

1. Override `equals()` to define logical equality.
2. Override `hashCode()` so equal objects return equal hash codes.
3. Base both methods on consistent identifying fields.
4. Keep those identifying fields stable while the object is stored.
5. Prefer immutable key objects when practical.

The fundamental rule is:

```text
If:

keyA.equals(keyB) == true

Then:

keyA.hashCode() == keyB.hashCode()
```

However, the reverse is not required. Two unequal objects may have the same hash code.

---

# PART X. PERFORMANCE AND COMPLEXITY

## 31. Time Complexity

| Operation | Average Time | Worst Case |
|---|---:|---:|
| HashSet insertion | O(1) | O(n) |
| HashSet membership check | O(1) | O(n) |
| HashSet removal | O(1) | O(n) |
| HashMap insertion or update | O(1) | O(n) |
| HashMap lookup | O(1) | O(n) |
| HashMap removal | O(1) | O(n) |

These are general educational summaries. Exact performance depends on the implementation, hash distribution, collisions, resizing, and key comparisons. Some library implementations have additional collision-handling optimizations.

### Illustration 16: Sequential Search vs. Hash Lookup

```text
Sequential Search:

Start -> Record 1 -> Record 2 -> Record 3 -> ... -> Match
              May inspect many records


Hash-Based Lookup:

Key -> Hash Function -> Table Position -> Check Candidate
```

Hashing is especially useful for repeated membership checks and lookups by key.

---

# PART XI. GUIDED PRACTICE ACTIVITIES

## Activity A: Calculate Hash Indexes

A hash table has 11 slots, indexed from 0 to 10.

Use:

```text
index = key % 11
```

Calculate the index of:

```text
18, 29, 40, 51, 62
```

### Procedure

1. Divide each key by 11.
2. Record the remainder.
3. Identify keys that produce the same index.
4. Draw the resulting table before handling collisions.

## Activity B: Simulate Linear Probing

Use a table with 10 slots.

Hash function:

```text
index = key % 10
```

Insert:

```text
12, 22, 32, 42, 52
```

### Procedure

1. Calculate the initial index for every key.
2. Attempt to insert each key.
3. When a collision occurs, check the next slot.
4. Continue until an available slot is found.
5. Draw the final table.
6. Show the probe sequence for each key.

## Activity C: Implement Student Attendance

Create a Java program that:

1. Creates a HashSet of student IDs.
2. Adds `STU-01`, `STU-02`, `STU-03`, `STU-02`, `STU-04`, and `STU-01`.
3. Displays the unique student IDs.
4. Checks whether `STU-03` exists.
5. Removes `STU-02`.
6. Displays the remaining set.

## Activity D: Implement Product Inventory

Create a HashMap that stores product codes and stock quantities.

Initial records:

```text
P-100 -> 15
P-200 -> 8
P-300 -> 20
```

The program must:

1. Insert all records.
2. Retrieve the stock quantity for `P-200`.
3. Update `P-200` to 12.
4. Add 5 to `P-100`.
5. Check whether `P-400` exists.
6. Remove `P-300`.
7. Display the remaining records.

## Activity E: Word Frequency

Input:

```text
data structures use data hashing structures
```

Use a HashMap to count each word.

Students must show:

1. The initial empty map.
2. The map after processing each word.
3. The final frequency of every distinct word.
4. Why repeated words update an existing mapping rather than creating duplicate keys.

## Activity F: Compare Collision Techniques

Insert the keys:

```text
15, 25, 35, 45
```

Use a table size of 10 and the hash function:

```text
index = key % 10
```

Perform the insertion using:

1. Separate chaining
2. Linear probing
3. Quadratic probing, using the formula given in this discussion

Draw the resulting table for each method and compare the locations of the entries.

---

# PART XII. REVIEW QUESTIONS

1. What is hashing?
2. What is the difference between a hash code and a table index?
3. How does a hash function determine where an entry should be stored?
4. What is a collision? Give an example.
5. Explain separate chaining using a table illustration.
6. How does linear probing resolve collisions?
7. How does quadratic probing differ from linear probing?
8. Why must a double-hashing implementation choose its step size carefully?
9. What is the load factor?
10. What happens when a HashSet receives a duplicate element?
11. What is the difference between union, intersection, and difference?
12. What is the difference between a HashSet and a HashMap?
13. What happens when `put()` receives an existing key?
14. Why must equal Java keys have equal hash codes?
15. Why can deleting an entry incorrectly break searching in an open-addressed table?
16. Why should a program not depend on the iteration order of a HashSet or HashMap?
17. What is the purpose of resizing a hash table?
18. Which data structure would you use to count the frequency of words, and why?

---

# KEY TAKEAWAYS

- **Hashing** calculates a value that helps locate stored data efficiently.
- **Hash functions** should be deterministic, fast, and reasonably distribute keys.
- **Hash tables** use buckets or slots to store entries.
- **Collisions** occur when different keys map to the same index.
- **Separate chaining** stores multiple entries in a bucket collection.
- **Open addressing** searches for other slots using a probing strategy.
- **Linear probing** checks consecutive slots.
- **Quadratic probing** uses quadratic offsets.
- **Double hashing** uses a second hash function to determine the probe step.
- **HashSet** stores distinct values and supports set operations.
- **HashMap** stores key-value pairs and retrieves values by key.
- Average hash-based operations are commonly O(1), but collisions and other implementation factors can increase the cost.
- Correct implementation requires consistent hashing and equality, stable keys, appropriate collision handling, and careful resizing.
