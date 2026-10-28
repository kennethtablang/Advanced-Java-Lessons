# Graphs and Network Data Structures
## Data Structures Subject — Discussion Notes with Illustrations

## 1. Learning Objectives

After studying this topic, students should be able to:

1. Define a graph and explain its components.
2. Identify common types of graphs and networks.
3. Represent graphs using adjacency matrices, adjacency lists, and edge lists.
4. Perform Breadth-First Search (BFS) and Depth-First Search (DFS).
5. Explain common graph applications.
6. Implement graph operations by following clear procedures and rules.

---

## 2. What Is a Graph?

A **graph** is a non-linear data structure that represents relationships or connections between objects.

A graph contains:

- **Vertex (node):** An object, person, place, or device.
- **Edge:** A connection between two vertices.
- **Weight:** An optional value associated with an edge, such as distance, time, or cost.
- **Path:** A sequence of vertices connected by edges.
- **Degree:** The number of edges connected to a vertex. For directed graphs, we distinguish **in-degree** and **out-degree**.

A graph is commonly written as:

\[
G = (V, E)
\]

- \(V\) is the set of vertices.
- \(E\) is the set of edges.

### Illustration 1: A simple graph

```text
      (A)------(B)
       |        |
       |        |
      (C)------(D)
```

Vertices: `V = {A, B, C, D}`

Edges: `E = {(A,B), (A,C), (B,D), (C,D)}`

The graph describes four objects and the connections between them. The letters could represent campus locations, people, or computers.

### Example: Campus locations

| Vertex | Location |
|---|---|
| A | Gate |
| B | Registrar |
| C | Library |
| D | Laboratory |

In this example, A–B means that the Gate is connected to the Registrar. If the graph is undirected, the connection can be used in either direction.

---

## 3. Types of Graphs

### 3.1 Undirected graph

An **undirected graph** has edges without a specified direction.

```text
(A) -------- (B)
```

The connection allows movement from A to B and from B to A.

**Examples:** Two-way roads, mutual friendships, and two-way network links.

### 3.2 Directed graph

A **directed graph** has edges with arrows indicating direction.

```text
(A) --------> (B)
```

This represents a connection from A to B. It does not automatically mean B connects back to A.

**Examples:** One-way streets, web links, and social media following relationships.

### 3.3 Weighted graph

A **weighted graph** assigns a value to an edge.

```text
             5 km
      (A) ----------- (B)
```

The weight `5 km` represents the distance between A and B.

A weighted graph can also represent cost or travel time. Always define what the weight means.

### 3.4 Unweighted graph

An **unweighted graph** records connections without assigning costs.

```text
(A) -------- (B) -------- (C)
```

Each edge represents a connection. If finding the fewest number of edges is the goal, BFS can find a shortest path in an unweighted graph.

### 3.5 Connected and disconnected graphs

A connected undirected graph has a path between every pair of vertices.

**Connected:**

```text
(A)----(B)----(C)----(D)
```

**Disconnected:**

```text
(A)----(B)       (C)----(D)
```

There is no path from A to C in the disconnected example. Each separate connected group is a **connected component**.

### 3.6 Cyclic and acyclic graphs

A **cycle** is a path that returns to its starting vertex.

**Cyclic graph:**

```text
       (A)
      /   \
    (B)---(C)
```

One cycle is A → B → C → A (the arrows here simply describe the sequence around the undirected cycle).

**Acyclic example:**

```text
(A)----(B)----(C)----(D)
```

This example has no cycle.

A **directed acyclic graph (DAG)** is a directed graph with no directed cycles. DAGs can represent task dependencies and course prerequisites.

### 3.7 Complete graph

A **complete graph** has an edge between every pair of distinct vertices.

```text
       (A)
      / | \
     /  |  \
   (B)---+--(C)
     \       /
      \     /
       (D)
```

In a complete graph, every vertex is directly connected to all other vertices. The drawing is only illustrative; every pair must have an edge.

**Example:** A small model in which every participant has a direct connection to every other participant.

---

## 4. Graph Representation

Graph representation describes how a program stores vertices and edges in memory.

We will use this graph for all three representations:

```text
       (A)------(B)
        |        |
        |        |
       (C)------(D)
```

Edges: A–B, A–C, B–D, and C–D.

### 4.1 Adjacency matrix

An **adjacency matrix** is a two-dimensional table. Rows and columns represent vertices. In a simple unweighted graph, `1` means an edge exists and `0` means it does not.

| Vertex | A | B | C | D |
|---|---:|---:|---:|---:|
| A | 0 | 1 | 1 | 0 |
| B | 1 | 0 | 0 | 1 |
| C | 1 | 0 | 0 | 1 |
| D | 0 | 1 | 1 | 0 |

#### How to read the matrix

- Row A, column B is `1`, so A connects to B.
- Row A, column D is `0`, so A does not directly connect to D.
- Because this graph is undirected, the matrix is symmetrical: `[A][B]` equals `[B][A]`.

#### Procedure: Build an adjacency matrix

1. List the vertices in a fixed order: A, B, C, D.
2. Create a square matrix with one row and column for each vertex.
3. Initialize all cells to `0`.
4. Read every edge.
5. For an undirected edge A–B, set both `[A][B]` and `[B][A]` to `1`.
6. For a directed edge A → B, set only `[A][B]` to `1`.
7. For a weighted graph, store the weight and use a clearly defined way to represent missing edges.

**Advantages:** Edge checks are typically O(1); suitable for dense graphs.

**Disadvantages:** Uses O(V²) memory, including when there are few edges.

### 4.2 Adjacency list

An **adjacency list** stores each vertex and the vertices directly connected to it.

```text
A: B, C
B: A, D
C: A, D
D: B, C
```

#### How to read the list

- `A: B, C` means A is directly connected to B and C.
- `D: B, C` means D is directly connected to B and C.
- In an undirected graph, each edge appears in both endpoints' lists.

#### Procedure: Build an adjacency list

1. Create an empty list for every vertex.
2. Read each edge.
3. For an undirected edge A–B, add B to A's list and A to B's list.
4. For a directed edge A → B, add B to A's list only.
5. For a weighted graph, store each neighbour with its weight.
6. Check for existing edges before insertion if duplicate edges are not allowed.

**Advantages:** Usually O(V + E) space; efficient for sparse graphs and traversal.

**Disadvantages:** Checking whether a particular edge exists may require searching a neighbour list.

### 4.3 Edge list

An **edge list** stores each connection as a pair of vertices.

```text
(A, B)
(A, C)
(B, D)
(C, D)
```

For a weighted graph:

```text
(A, B, 5)
(A, C, 8)
```

#### Procedure: Build an edge list

1. Identify the vertices.
2. Read each connection.
3. Record each edge as `(source, destination)`.
4. Add the weight when needed.
5. For an undirected graph, decide whether each edge is stored once or twice. Storing it once is common when the program treats it as undirected.

**Advantages:** Simple and useful for algorithms that process or sort edges.

**Disadvantages:** Finding all neighbours of a vertex may require scanning the entire edge list.

### 4.4 Representation comparison

| Representation | Typical space | Best suited for |
|---|---|---|
| Adjacency matrix | O(V²) | Dense graphs and frequent edge checks |
| Adjacency list | O(V + E) | Sparse graphs and traversal |
| Edge list | O(V + E), including vertex storage | Edge-oriented processing |

`V` is the number of vertices and `E` is the number of edges.

---

## 5. Graph Traversal

**Graph traversal** means visiting vertices in a systematic order.

The two fundamental techniques are:

- **BFS:** Uses a queue and explores level by level.
- **DFS:** Uses a stack or recursion and explores deeply before backtracking.

A visited set prevents the algorithm from repeatedly visiting vertices in cycles.

Use this graph for the traversal illustrations:

```text
       (A)
       / \
     (B) (C)
      |    |
     (D)---(E)
```

Edges: A–B, A–C, B–D, C–E, D–E.

For predictable examples, process neighbours alphabetically.

### 5.1 Breadth-First Search (BFS)

BFS visits the starting vertex, then its immediate neighbours, then vertices farther away.

A **queue** follows FIFO: First In, First Out.

#### Procedure: BFS

1. Choose a starting vertex.
2. Create an empty queue and visited set.
3. Mark the start as visited.
4. Enqueue the start.
5. While the queue is not empty:
   - Dequeue the front vertex and process it.
   - Examine its neighbours.
   - Mark each unvisited neighbour as visited and enqueue it.
6. Stop when the queue is empty, or when a target is reached if the task allows early stopping.

#### Illustration: BFS from A

Initial state:

```text
Queue:   [A]
Visited: {A}
Output:  (empty)
```

**Step 1 — Remove A**

A's neighbours are B and C.

```text
Queue:   [B, C]
Visited: {A, B, C}
Output:  A
```

**Step 2 — Remove B**

B's neighbours are A and D. A is already visited, so add D.

```text
Queue:   [C, D]
Visited: {A, B, C, D}
Output:  A, B
```

**Step 3 — Remove C**

C's neighbours are A and E. A is visited, so add E.

```text
Queue:   [D, E]
Visited: {A, B, C, D, E}
Output:  A, B, C
```

**Step 4 — Remove D**

D's neighbours B and E have already been visited.

```text
Queue:   [E]
Output:  A, B, C, D
```

**Step 5 — Remove E**

All of E's neighbours are already visited.

```text
Queue:   []
Output:  A, B, C, D, E
```

**BFS order:** `A, B, C, D, E`

**Important rule:** Mark a vertex visited when it is enqueued, not when it is dequeued. This avoids adding the same vertex to the queue multiple times.

**Time complexity with an adjacency list:** O(V + E).

### 5.2 Depth-First Search (DFS)

DFS follows one branch as far as possible before backtracking.

A **stack** follows LIFO: Last In, First Out. Recursive DFS uses the programming language's call stack.

#### Procedure: Recursive DFS

1. Choose a starting vertex.
2. Create an empty visited set.
3. Mark the current vertex visited and process it.
4. Examine its neighbours in a defined order.
5. Recursively visit each unvisited neighbour.
6. If no unvisited neighbours remain, return to the previous call.
7. Continue until all vertices reachable from the start have been visited.

#### Illustration: DFS from A

With neighbours processed alphabetically:

```text
Start at A
   |
   v
   A
   |
   v
   B
   |
   v
   D
   |
   v
   E
   |
   v
   C
```

The actual graph connections still determine which move is legal. DFS does not move from E to C unless that edge exists; in this example, E–C is an edge.

Traversal order:

```text
A, B, D, E, C
```

**Backtracking example:**

1. Visit A.
2. Follow A–B.
3. Follow B–D.
4. Follow D–E.
5. Follow E–C.
6. C has no unvisited neighbours, so return through the previous calls until the search is complete.

**Time complexity with an adjacency list:** O(V + E).

### 5.3 BFS and DFS compared

| Feature | BFS | DFS |
|---|---|---|
| Data structure | Queue | Stack or recursion |
| Search strategy | Level by level | Deep branch, then backtracking |
| Fewest-edge path in an unweighted graph | Yes | Not guaranteed |
| Common uses | Reachability and shortest unweighted paths | Cycle detection, maze exploration, components |
| Complexity with adjacency list | O(V + E) | O(V + E) |

### 5.4 Traversing a disconnected graph

A search from one vertex does not necessarily visit every vertex in a disconnected graph.

```text
(A)----(B)       (C)----(D)       (E)
```

There are three components: `{A, B}`, `{C, D}`, and `{E}`.

Procedure:

1. Mark every vertex unvisited.
2. Loop through all vertices.
3. If a vertex is unvisited, start a new BFS or DFS from it.
4. Repeat until all vertices have been visited.

---

## 6. Java Implementation: Adjacency List, BFS, and DFS

This example uses an undirected, unweighted graph. It demonstrates how the graph is stored and traversed.

```java
import java.util.*;

public class Graph {

    // Each vertex maps to a list of its neighbours.
    private final Map<String, List<String>> adjacencyList =
            new LinkedHashMap<>();

    // Add a vertex if it does not already exist.
    public void addVertex(String vertex) {
        adjacencyList.putIfAbsent(vertex, new ArrayList<>());
    }

    // Add an undirected edge.
    public void addEdge(String source, String destination) {
        addVertex(source);
        addVertex(destination);

        // An undirected edge must be stored in both directions.
        if (!adjacencyList.get(source).contains(destination)) {
            adjacencyList.get(source).add(destination);
        }

        if (!adjacencyList.get(destination).contains(source)) {
            adjacencyList.get(destination).add(source);
        }
    }

    // Breadth-First Search using a queue.
    public void bfs(String start) {
        if (!adjacencyList.containsKey(start)) {
            System.out.println("Starting vertex does not exist.");
            return;
        }

        Set<String> visited = new HashSet<>();
        Queue<String> queue = new ArrayDeque<>();

        visited.add(start);
        queue.offer(start);

        while (!queue.isEmpty()) {
            String current = queue.poll();
            System.out.print(current + " ");

            for (String neighbour : adjacencyList.get(current)) {
                if (!visited.contains(neighbour)) {
                    // Mark immediately when enqueued.
                    visited.add(neighbour);
                    queue.offer(neighbour);
                }
            }
        }

        System.out.println();
    }

    // Depth-First Search using recursion.
    public void dfs(String start) {
        if (!adjacencyList.containsKey(start)) {
            System.out.println("Starting vertex does not exist.");
            return;
        }

        Set<String> visited = new HashSet<>();
        dfsRecursive(start, visited);
        System.out.println();
    }

    private void dfsRecursive(String current, Set<String> visited) {
        visited.add(current);
        System.out.print(current + " ");

        for (String neighbour : adjacencyList.get(current)) {
            if (!visited.contains(neighbour)) {
                dfsRecursive(neighbour, visited);
            }
        }
    }

    public static void main(String[] args) {
        Graph graph = new Graph();

        graph.addEdge("A", "B");
        graph.addEdge("A", "C");
        graph.addEdge("B", "D");
        graph.addEdge("C", "E");
        graph.addEdge("D", "E");

        System.out.print("BFS: ");
        graph.bfs("A");

        System.out.print("DFS: ");
        graph.dfs("A");
    }
}
```

Expected output:

```text
BFS: A B C D E
DFS: A B D E C
```

### Reading the code step by step

1. `adjacencyList` stores each vertex and its neighbours.
2. `addVertex()` creates a vertex only if it is not already present.
3. `addEdge()` creates an undirected connection in both lists.
4. `bfs()` uses a queue to process vertices in arrival order.
5. `dfs()` creates a visited set and calls the recursive helper.
6. `dfsRecursive()` follows an unvisited neighbour until it must backtrack.

`LinkedHashMap` preserves vertex insertion order, and each neighbour list preserves insertion order. This makes the example's output reproducible.

For a directed graph, change `addEdge()` so it adds only the destination to the source's list. For a weighted graph, store a neighbour together with its weight.

---

## 7. Applications of Graphs and Networks

### 7.1 GPS and route planning

```text
       4 km
 (A)----------(B)
  |             |
  | 2 km        | 3 km
  |             |
 (C)----------(D)
       5 km
```

Vertices represent locations; edges represent roads; weights represent distances.

**Procedure:**

1. Create a vertex for each location.
2. Add edges for roads that can be used.
3. Store the distance or cost on each edge.
4. Select a starting point and destination.
5. Use a suitable shortest-path algorithm.
6. Reconstruct and display the selected route.

BFS finds a path with the fewest edges in an unweighted graph. It does not generally find the minimum-distance route in a weighted graph. Dijkstra's algorithm is suitable for non-negative edge weights.

### 7.2 Social networks

```text
        (Ana)
        /   \
    (Ben)---(Cara)
       \
       (Dan)
```

Vertices represent users and edges represent relationships. If the relationship is mutual, use an undirected graph. If it means “follows,” use a directed graph.

Applications include finding mutual connections, discovering reachable users, and identifying communities.

### 7.3 Computer networks

```text
(PC-1)----(Router)----(Server)
              |
           (PC-2)
```

Vertices represent computers, routers, or servers. Edges represent communication links. Weights may represent delay or link cost.

Graphs can help model network connectivity and determine possible communication paths.

### 7.4 Course prerequisites

```text
(Programming Basics)
          |
          v
    (Data Structures)
          |
          v
      (Algorithms)
```

Vertices represent courses; directed edges indicate prerequisite relationships.

Procedure:

1. Create a vertex for each course.
2. Draw an arrow from a prerequisite to the dependent course.
3. Check for directed cycles.
4. If the graph is a DAG, use topological sorting to determine a valid order.

### 7.5 Delivery and logistics

```text
(Warehouse)----(Store A)
      |             |
      |             |
   (Store B)----(Store C)
```

Vertices represent warehouses and delivery locations. Edges represent available routes, and weights may represent travel time or cost.

Graphs can help compare routes and identify connections between locations. Planning the best order for many deliveries can require more advanced optimisation.

### 7.6 Web pages and hyperlinks

```text
(Page A) -----> (Page B)
    |               |
    v               v
(Page C) -----> (Page D)
```

Vertices represent web pages. Directed edges represent hyperlinks from one page to another. Graph analysis can help describe relationships between pages.

### 7.7 Recommendation systems

A graph can connect users to products they interacted with.

```text
(User A) ---- (Movie 1)
    |               |
    |               |
(Movie 2) ---- (User B)
```

A system can analyse shared interactions to suggest products or content. Real recommendation systems may also use ratings, user attributes, and machine-learning models.

---

## 8. Rules for Implementing Graphs

1. **Define every vertex.** Each vertex must have a clear meaning.
2. **Choose the graph type first.** Decide whether it is directed or undirected, weighted or unweighted.
3. **Choose the representation based on needs.** Consider graph density, memory use, and the operations performed most often.
4. **Use unique vertex identifiers.** Avoid ambiguous identifiers.
5. **Validate vertices and edges.** Decide how the program handles unknown vertices.
6. **Respect direction.** Undirected adjacency lists store both directions; directed lists store only the specified direction.
7. **Store weights consistently.** Clearly define units and how missing edges are represented.
8. **Decide how duplicates are handled.** Prevent duplicate edges if the graph does not permit them.
9. **Decide whether self-loops are allowed.** Handle edges from a vertex to itself deliberately.
10. **Track visited vertices.** Use a visited set or array in traversal algorithms.
11. **Use the correct data structure.** BFS requires a queue; iterative DFS uses a stack.
12. **Handle disconnected graphs.** Start new searches from remaining unvisited vertices when every component must be processed.
13. **Do not assume traversal order is universal.** It depends on neighbour ordering.
14. **Choose the algorithm for the actual goal.** Use BFS for fewest-edge paths in unweighted graphs; use a weighted shortest-path algorithm for weighted costs.
15. **Consider complexity.** Estimate the number of vertices and edges before choosing a representation.
16. **Test boundary cases.** Include an empty graph, isolated vertices, cycles, disconnected graphs, and invalid input.
17. **Maintain consistency.** Apply the same vertex, edge, and weight rules throughout the implementation.
18. **Document assumptions.** Explain whether duplicate edges, self-loops, and zero-weight edges are permitted.

---

## 9. Common Mistakes

| Mistake | Why it is a problem | Correct approach |
|---|---|---|
| Forgetting the reverse edge in an undirected adjacency list | The edge behaves like a one-way connection | Add both neighbour entries |
| Adding reverse edges to every directed edge | Changes the intended graph | Store only the specified direction |
| Not tracking visited vertices | May cause repeated processing or endless traversal | Use a visited set |
| Marking BFS vertices visited too late | A vertex can be enqueued multiple times | Mark it when enqueued |
| Using BFS to minimize weighted distance | Fewest edges may not be lowest cost | Use a suitable weighted algorithm |
| Assuming every vertex is reachable | The graph may be disconnected | Start additional searches |
| Using zero to mean “no edge” in every weighted matrix | A real zero-weight edge may be lost | Store edge presence separately |
| Choosing a matrix without considering graph density | Memory may be wasted | Compare expected V and E |

---

## 10. Classroom Activity: Campus Navigation

A campus contains five locations:

- A — Gate
- B — Registrar
- C — Library
- D — Laboratory
- E — Cafeteria

The available two-way paths are:

```text
A-B
A-C
B-D
C-D
D-E
```

### Student Tasks

1. Draw the graph.
2. Identify its vertices and edges.
3. Construct an adjacency matrix.
4. Construct an adjacency list.
5. Perform BFS starting from A and show the queue after each step.
6. Perform DFS starting from A and show when backtracking occurs.
7. Change edge B–D to a directed edge B → D. Explain how the adjacency list changes.
8. Assign different distances to the edges. Explain why BFS alone cannot guarantee the shortest total distance.
9. Modify the Java program to include all five locations.
10. Test the program with a nonexistent starting vertex.

### Suggested Answers

Vertices:

```text
{A, B, C, D, E}
```

Adjacency list:

```text
A: B, C
B: A, D
C: A, D
D: B, C, E
E: D
```

BFS from A, using alphabetical neighbour order:

```text
A, B, C, D, E
```

DFS from A, using alphabetical neighbour order:

```text
A, B, D, C, E
```

Traversal orders may differ when neighbour ordering differs. Students should explain the process, not only write the final order.

---

## 11. Final Summary

A graph models relationships between objects through vertices and edges. An edge may be directed or undirected, and it may have a weight.

- **Adjacency matrix:** Uses a two-dimensional table.
- **Adjacency list:** Stores each vertex's neighbours.
- **Edge list:** Stores the connections directly.
- **BFS:** Uses a queue to explore level by level.
- **DFS:** Uses a stack or recursion to explore deeply and backtrack.

Graphs are used in GPS navigation, social networks, computer networks, course scheduling, delivery systems, recommendations, and web analysis.

**Implementation principle:** Before writing code, decide what the vertices and edges mean, choose an appropriate representation, define the rules for direction and weights, and use visited tracking during traversal.