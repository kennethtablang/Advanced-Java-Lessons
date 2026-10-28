# Data Structures Discussion: Hashing, HashSets, and HashMaps

## Learning Objectives

At the end of this discussion, students should be able to:

1. Explain hashing and the purpose of a hash function.
2. Design a basic hash table and calculate table indexes.
3. Identify collisions and apply collision-handling techniques.
4. Implement basic set operations using a HashSet.
5. Store, retrieve, update, and remove key-value pairs using a HashMap.
6. Follow implementation rules when choosing and using hash-based data structures.

---

## 1. Introduction to Hashing

**Hashing** is a technique that transforms a key into a number that can be used as an index in a table. This number is produced by a **hash function**.

A **hash table** stores data in positions called *slots* or *buckets*. Instead of checking every item one by one, a program uses the hash function to estimate where an item belongs.

### Why do we use hashing?

Hash-based structures are useful when a program needs to:

- Check quickly whether an item exists.
- Find a value using a unique key.
- Prevent duplicate values.
- Count or group items by key.
- Add and remove items efficiently.

With a good hash function and a well-managed table, lookup, insertion, and deletion take **O(1) average time**. In the worst case, they may take **O(n)** time if many keys collide.

### Everyday examples

| Application | How hashing is used |
|---|---|
| Student information system | Find a student record using a student number |
| Attendance system | Check whether a student number has already been recorded |
| Inventory system | Find a product using its product code |
| Online store | Look up a cart item using a product ID |
| Word counter | Store each word and its occurrence count |
| Duplicate checker | Detect repeated email addresses or filenames |

> Important: A hash table is a data structure, not encryption. A normal hash function used for table indexing is not designed to hide or protect data.

---

## 2. Hash Functions and Table Design

### 2.1 What is a hash function?

A **hash function** accepts a key and returns a hash value. The table then converts that value into a valid array index.

For a table with `m` slots, a common simple index formula for a non-negative hash value is:

```text
index = hash(key) % m
```

The `%` operator returns the remainder after division.

### 2.2 Step-by-step: Calculate a table index

Suppose a table has **10 slots**, numbered `0` through `9`, and the keys are integers.

Use this simple example hash function:

```text
hash(key) = key
index = hash(key) % 10
```

Calculate the index for each key:

| Key | Calculation | Index |
|---:|---|---:|
| 23 | 23 % 10 | 3 |
| 45 | 45 % 10 | 5 |
| 67 | 67 % 10 | 7 |
| 32 | 32 % 10 | 2 |

**Procedure:**

1. Choose the key you want to store.
2. Apply the hash function to the key.
3. Use the modulo operation with the table size to calculate an index.
4. Check whether the slot is available.
5. If it is available, store the item there.
6. If it is occupied by another key, handle the collision using a defined strategy.

### 2.3 Qualities of a good hash function

A useful hash function should:

1. Be **deterministic**: the same key produces the same hash value while the hashing rules and key remain unchanged.
2. Be **fast to compute**.
3. Distribute keys across the table as evenly as practical.
4. Use the key's relevant information so that many different keys do not cluster in the same slots.
5. Be consistent with the equality rule used to compare keys.

A hash function does **not** have to give a unique hash value to every key. Different keys can produce the same index; that is a collision, and the table must handle it.

### 2.4 Table size and load factor

The **load factor** measures how full a hash table is.

```text
load factor = number of stored entries / number of table slots
```

For example, if a table has 10 slots and stores 7 entries:

```text
load factor = 7 / 10 = 0.7
```

A high load factor can increase collisions and slow down operations. Many implementations resize their tables when the load factor passes an internal threshold.

**Table-design procedure:**

1. Estimate how many entries the program may store.
2. Select a suitable initial table capacity.
3. Choose a hash function that distributes expected keys well.
4. Choose a collision-handling technique.
5. Monitor the load factor.
6. Resize and reinsert entries when necessary, preserving the same key-to-value relationships.

In real applications, prefer a tested language-library implementation unless the learning task specifically requires building a hash table from scratch.

---

## 3. Collisions and Collision-Handling Techniques

### 3.1 What is a collision?

A **collision** happens when two different keys map to the same table index.

Using the example `index = key % 10`:

| Key | Index |
|---:|---:|
| 23 | 3 |
| 33 | 3 |
| 43 | 3 |

All three keys map to index `3`. The program needs a collision-handling method so that none of the entries is accidentally lost.

### 3.2 Separate chaining

**Separate chaining** stores multiple entries in a collection (often a linked list) associated with each bucket.

Example for keys `23`, `33`, and `43`:

```text
Bucket 3: [23] -> [33] -> [43]
```

**Insertion procedure:**

1. Calculate the bucket index.
2. Go to that bucket.
3. Search the bucket to determine whether the key already exists.
4. If the key exists, update or reject it according to the data structure's rules.
5. If the key does not exist, add the new entry to the bucket.

**Search procedure:**

1. Calculate the bucket index from the search key.
2. Visit the entries in that bucket.
3. Compare the actual keys, not only their hash values.
4. Return the matching entry if found; otherwise report that it is absent.

**Advantage:** Multiple entries can share a bucket without searching for another empty table slot.

**Limitation:** Long chains can make operations slower.

### 3.3 Open addressing

With **open addressing**, all entries are stored directly in the table. When a collision occurs, the algorithm probes other slots.

#### A. Linear probing

Try the next slot until an available one is found.

```text
index = (hash(key) + i) % tableSize
```

Here, `i` begins at `0` and increases by `1`.

**Example:** With table size `10`, keys `23`, `33`, and `43` all initially map to index `3`.

- Store `23` at index `3`.
- `33` tries index `3`, finds it occupied, then tries index `4`.
- `43` tries index `3`, then `4`, then `5`.

Result:

```text
Index 3: 23
Index 4: 33
Index 5: 43
```

**Limitation:** Linear probing can create clusters of occupied slots.

#### B. Quadratic probing

Quadratic probing uses a growing quadratic offset, for example:

```text
index = (hash(key) + i * i) % tableSize
```

The exact formula and table-size requirements depend on the implementation. Quadratic probing can reduce some clustering, but it must be designed so that probing can reach enough slots.

#### C. Double hashing

Double hashing uses a second hash function to determine the step size:

```text
index = (hash1(key) + i * hash2(key)) % tableSize
```

The second hash must produce a valid, non-zero step size that works with the table capacity.

### 3.4 Deletion with open addressing

In open-addressed tables, simply marking a removed slot as empty may break searches for entries placed farther along the probe sequence.

Implementations commonly use a **deleted marker** (often called a tombstone) or a carefully designed reorganization procedure.

**Rules for handling collisions:**

1. Never overwrite a different key just because it has the same index.
2. Compare actual keys to determine whether an entry matches.
3. Use one collision strategy consistently within a table.
4. Ensure search and deletion follow the same probing rules used during insertion.
5. Handle full tables and unsuccessful searches so they cannot loop forever.
6. When resizing, recalculate the indexes for all entries using the new capacity.

---

## 4. HashSet: Set Operations and Implementation

### 4.1 What is a HashSet?

A **set** is a collection of distinct elements. A **HashSet** implements a set using hashing, making membership checks and common updates efficient on average.

A HashSet stores **values**, not separate key-value pairs. Adding a value that is already present does not create a duplicate.

### 4.2 Common set operations

Let:

```text
A = {10, 20, 30, 40}
B = {30, 40, 50, 60}
```

| Operation | Meaning | Result |
|---|---|---|
| Add | Insert an element if it is not already present | Add `50` to A → `{10, 20, 30, 40, 50}` |
| Contains | Check membership | `A contains 20` → `true` |
| Remove | Delete an element | Remove `20` from A → `{10, 30, 40}` |
| Union | Elements in A or B, without duplicates | `{10, 20, 30, 40, 50, 60}` |
| Intersection | Elements present in both sets | `{30, 40}` |
| Difference | Elements in A but not in B | `{10, 20}` |
| Subset check | Check whether every element of one set is in another | `{30, 40}` is a subset of A → `true` |

### 4.3 Step-by-step: Build a HashSet

Example scenario: an attendance system must avoid recording the same student number more than once.

**Procedure:**

1. Create an empty HashSet.
2. Read a student's ID.
3. Attempt to add the ID to the set.
4. If the ID was added, treat it as a new attendance entry.
5. If it was already present, treat it as a duplicate.
6. Use a membership check when the program needs to verify whether an ID was recorded.

### 4.4 Java example

```java
import java.util.HashSet;
import java.util.Set;

public class AttendanceExample {
    public static void main(String[] args) {
        Set<String> attendedStudentIds = new HashSet<>();

        // Add student IDs. Duplicate IDs are not stored twice.
        boolean firstAdd = attendedStudentIds.add("STU-101");
        boolean secondAdd = attendedStudentIds.add("STU-102");
        boolean duplicateAdd = attendedStudentIds.add("STU-101");

        System.out.println(firstAdd);      // true
        System.out.println(secondAdd);     // true
        System.out.println(duplicateAdd);  // false

        // Check whether a student has already been recorded.
        System.out.println(
            attendedStudentIds.contains("STU-101")
        ); // true

        // Remove an ID if necessary.
        attendedStudentIds.remove("STU-102");

        System.out.println(attendedStudentIds);
    }
}
```

**Important:** A HashSet does not guarantee iteration order. Do not rely on its elements appearing in insertion order or sorted order.

### 4.5 Java set-operation example

```java
import java.util.HashSet;
import java.util.Set;

public class SetOperationsExample {
    public static void main(String[] args) {
        Set<Integer> a = new HashSet<>();
        Set<Integer> b = new HashSet<>();

        a.add(10);
        a.add(20);
        a.add(30);
        a.add(40);

        b.add(30);
        b.add(40);
        b.add(50);
        b.add(60);

        // Work on copies so the original sets remain unchanged.
        Set<Integer> union = new HashSet<>(a);
        union.addAll(b);

        Set<Integer> intersection = new HashSet<>(a);
        intersection.retainAll(b);

        Set<Integer> difference = new HashSet<>(a);
        difference.removeAll(b);

        System.out.println("Union: " + union);
        System.out.println("Intersection: " + intersection);
        System.out.println("A - B: " + difference);
    }
}
```

The order shown in the console may vary because HashSet does not promise a particular order.

---

## 5. HashMap: Map Data Structures and Implementation

### 5.1 What is a Map?

A **Map** stores entries as **key-value pairs**. Each key identifies a value. In a HashMap, hashing helps locate the bucket associated with the key.

Examples:

- Student ID → Student name
- Product code → Product details
- Username → User profile
- Word → Number of occurrences
- Course code → Course information

In Java, `HashMap<K, V>` uses a key type `K` and a value type `V`.

### 5.2 Common Map operations

| Operation | Purpose | Example |
|---|---|---|
| `put(key, value)` | Add a pair or replace the value for an existing key | `put("STU-101", "Ana")` |
| `get(key)` | Retrieve a value by key | `get("STU-101")` |
| `containsKey(key)` | Check whether a key exists | `containsKey("STU-101")` |
| `remove(key)` | Remove a pair by key | `remove("STU-101")` |
| `getOrDefault(key, default)` | Return a default if no value is mapped to the key | `getOrDefault("Unknown", 0)` |
| `size()` | Count the number of key-value pairs | `size()` |

### 5.3 Step-by-step: Insert and retrieve a pair

1. Create an empty HashMap with suitable key and value types.
2. Choose a key that identifies the record.
3. Choose the value to associate with that key.
4. Call `put(key, value)` to insert the pair.
5. To retrieve the record, call `get(key)`.
6. To check whether the key exists, use `containsKey(key)`.
7. To update the record, call `put` with the same key and a new value.
8. To delete the record, call `remove(key)`.

**Rule:** Keys are unique within a Map. Adding an existing key replaces its mapped value; it does not create a second entry with the same key.

### 5.4 Java example: Student records

```java
import java.util.HashMap;
import java.util.Map;

public class StudentMapExample {
    public static void main(String[] args) {
        Map<String, String> students = new HashMap<>();

        // Insert key-value pairs: student ID -> student name.
        students.put("STU-101", "Ana");
        students.put("STU-102", "Ben");
        students.put("STU-103", "Carlo");

        // Retrieve a value using its key.
        System.out.println(students.get("STU-101")); // Ana

        // Check whether a key exists before relying on its mapping.
        if (students.containsKey("STU-102")) {
            System.out.println("Student ID exists.");
        }

        // A repeated key updates the value associated with that key.
        students.put("STU-102", "Benjamin");
        System.out.println(students.get("STU-102")); // Benjamin

        // A missing key returns null with this example.
        System.out.println(students.get("STU-999")); // null

        // Remove an entry.
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

### 5.5 Example: Count word occurrences

A word counter is a common use of a HashMap. Each word is a key, and its occurrence count is the value.

```java
import java.util.HashMap;
import java.util.Map;

public class WordCounter {
    public static void main(String[] args) {
        String[] words = {
            "java", "hashing", "java", "map", "java", "hashing"
        };

        Map<String, Integer> counts = new HashMap<>();

        for (String word : words) {
            // Use 0 when the word has not been counted yet.
            int currentCount = counts.getOrDefault(word, 0);

            // Store the updated count for this word.
            counts.put(word, currentCount + 1);
        }

        System.out.println(counts);
        // Counts: java=3, hashing=2, map=1
        // The printed entry order is not guaranteed.
    }
}
```

**Procedure:**

1. Start with an empty map.
2. Read one word.
3. Look up the current count; use zero if the word is not present.
4. Add one to the count.
5. Store the updated count using the word as the key.
6. Repeat for every word.

---

## 6. How HashSet and HashMap Work Internally

Both structures use hashing, but they serve different purposes.

| Feature | HashSet | HashMap |
|---|---|---|
| Stores | Distinct values | Key-value pairs |
| Main question | “Have I seen this value?” | “What value belongs to this key?” |
| Duplicate handling | Duplicate values are not added | An existing key's value is replaced by `put` |
| Typical use | Unique student IDs, unique tags | Student ID-to-name mapping, word counts |
| Average lookup | O(1) | O(1) |

Conceptually, a HashSet can be implemented using a HashMap internally, storing each set element as a map key with a placeholder value. This is an implementation idea; programmers should use the library's HashSet directly when they need a set.

### General insertion procedure

1. Receive a value or key-value pair.
2. Compute the key's hash code.
3. Convert the hash code into a bucket or table position.
4. Inspect that bucket or follow the probing sequence.
5. Compare keys using the structure's equality rules.
6. If the key/value already exists, apply the correct duplicate or update rule.
7. Otherwise, insert the new entry.
8. If the table becomes too full, resize and redistribute entries as needed.

### General search procedure

1. Receive the target value or key.
2. Compute its hash code using the same hashing rules.
3. Find the corresponding bucket or probe sequence.
4. Compare candidate entries using equality.
5. Return the matching result or report that no match exists.

---

## 7. Implementation Rules and Best Practices

### 7.1 Rules for hash functions and keys

1. **Use consistent hashing.** Equal keys must produce equal hash codes.
2. **Use equality checks after hashing.** A matching hash code alone does not prove two keys are equal.
3. **Keep key equality stable.** Avoid changing fields that affect a key's hash code or equality while it is stored in a HashSet or used as a HashMap key.
4. **Choose a suitable key.** Use a stable identifier, such as a student ID or product code, when appropriate.
5. **Do not expect unique hash values.** Collisions are normal and must be handled.
6. **Do not confuse hashing with encryption.** Hash-table hashing is for efficient data access, not for hiding information.

### 7.2 Rules for table design and collisions

1. Select a table capacity suitable for the expected number of entries.
2. Use a hash function that distributes expected keys reasonably evenly.
3. Define a collision strategy, such as separate chaining or open addressing.
4. Ensure that insertion, search, and deletion use compatible collision-handling rules.
5. Monitor the load factor and resize when necessary.
6. Recalculate table positions after resizing.
7. Ensure probing or bucket search terminates when an item is absent or the table is full.
8. Never overwrite a different entry merely because it collides with the new key.

### 7.3 Rules for HashSet

1. Use HashSet when only unique values are needed.
2. Use `add()` to insert and check its Boolean result when duplicate detection matters.
3. Use `contains()` to test membership.
4. Use `remove()` to delete a value.
5. Use set-operation methods such as `addAll()`, `retainAll()`, and `removeAll()` for union, intersection, and difference.
6. Do not assume iteration order.
7. Ensure objects stored in a set have consistent `equals()` and `hashCode()` behavior in Java.

### 7.4 Rules for HashMap

1. Use HashMap when each key maps to a value.
2. Use a key type that represents a stable identifier.
3. Remember that each key can have only one current mapping.
4. Use `put()` to insert or update.
5. Use `get()` to retrieve; in Java, a `null` result can mean either no mapping or a mapping to `null`, so use `containsKey()` when that distinction matters.
6. Use `getOrDefault()` for a fallback value when appropriate.
7. Use `remove()` to delete a mapping.
8. Do not assume iteration order.
9. Ensure custom key classes implement `equals()` and `hashCode()` consistently.
10. Do not rely on a normal HashMap for thread-safe concurrent updates; use suitable synchronization or a concurrent map when needed.

### 7.5 Java-specific `equals()` and `hashCode()` rules

When creating a custom Java class to use as a HashSet element or HashMap key:

1. Override `equals()` to define when two objects represent the same logical key.
2. Override `hashCode()` so that objects considered equal by `equals()` return the same hash code.
3. Use the same meaningful fields in both methods.
4. Avoid mutating those fields while the object is stored as a key or set element.
5. Prefer immutable key objects when practical.

Example principle:

```text
If keyA.equals(keyB) is true,
then keyA.hashCode() must equal keyB.hashCode().
```

The reverse is not required: two unequal keys may share a hash code.

---

## 8. Performance Summary

| Operation | Typical average time | Possible worst case |
|---|---:|---:|
| HashSet add | O(1) | O(n) |
| HashSet contains | O(1) | O(n) |
| HashSet remove | O(1) | O(n) |
| HashMap put | O(1) | O(n) |
| HashMap get | O(1) | O(n) |
| HashMap remove | O(1) | O(n) |

These are typical complexity summaries, not guarantees for every implementation or workload. Actual performance depends on the hash function, collisions, resizing, key equality, and implementation details.

---

## 9. Guided Practice Activities

### Activity A: Calculate hash indexes

A hash table has 11 slots, indexed from `0` to `10`. Use:

```text
index = key % 11
```

Calculate the indexes for `18`, `29`, `40`, `51`, and `62`.

**Procedure:**

1. Divide each key by 11.
2. Record the remainder.
3. Compare the results.
4. Identify which keys collide.

### Activity B: Handle a collision

Use a table of size 10 and the hash function:

```text
index = key % 10
```

Insert the keys `12`, `22`, `32`, and `42` using linear probing.

**Procedure:**

1. Calculate the initial index for each key.
2. Try the initial slot.
3. If it is occupied, move to the next slot.
4. Continue until an available slot is found.
5. Draw the final table and show the index of every key.

### Activity C: Use a HashSet

A class has student IDs:

```text
STU-01, STU-02, STU-03, STU-02, STU-04, STU-01
```

Write a Java program that:

1. Creates a HashSet.
2. Adds each ID.
3. Displays the unique IDs.
4. Checks whether `STU-03` exists.
5. Removes `STU-02`.
6. Displays the remaining set.

### Activity D: Use a HashMap

Create a map for product inventory where the product code is the key and the stock quantity is the value.

Use these starting entries:

```text
P-100 -> 15
P-200 -> 8
P-300 -> 20
```

Write a Java program that:

1. Inserts the three entries.
2. Retrieves the stock for `P-200`.
3. Updates `P-200` to a quantity of `12`.
4. Adds 5 to the quantity for `P-100`.
5. Checks whether `P-400` exists.
6. Removes `P-300`.
7. Displays the remaining entries.

### Activity E: Word frequency

Given the sentence:

```text
data structures use data hashing structures
```

Use a HashMap to count how many times each word appears. Show the sequence of updates to the map after each word is processed.

---

## 10. Review Questions

1. What is hashing, and why is it useful?
2. What is the difference between a hash code and a table index?
3. What is a collision? Give an example.
4. How does separate chaining differ from linear probing?
5. What does the load factor measure?
6. Why must keys be compared for equality after a hash match?
7. How is a HashSet different from a HashMap?
8. What are the results of union, intersection, and difference?
9. What happens when `put()` is called with a key that already exists?
10. Why should a key's equality and hash code remain stable while it is stored?
11. Why should a program not rely on HashSet or HashMap iteration order?
12. When would you choose a HashSet instead of a HashMap?

---

## Key Takeaways

- **Hashing** maps keys to positions that help a program locate data efficiently.
- **Hash functions** should be fast, deterministic, and distribute keys well.
- **Collisions** are expected; use a defined technique such as separate chaining or open addressing.
- **HashSet** stores distinct values and supports set operations.
- **HashMap** stores key-value pairs and supports lookup by key.
- Average operations are often O(1), but poor distribution and many collisions can make them slower.
- Correct implementation depends on consistent equality and hashing, stable keys, appropriate resizing, and not relying on an unspecified iteration order.
