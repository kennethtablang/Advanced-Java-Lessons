# Graphs and Network Data Structures
## Data Structures Subject — Discussion Notes

## 1. Learning Objectives

After studying this topic, students should be able to:

1. Define a graph and explain its basic components.
2. Identify common types of graphs and networks.
3. Represent a graph using an adjacency matrix, adjacency list, or edge list.
4. Perform Breadth-First Search (BFS) and Depth-First Search (DFS).
5. Explain common applications of graphs.
6. Implement a graph carefully by following a set of rules and procedures.

---

## 2. What Is a Graph?

A **graph** is a non-linear data structure used to represent relationships or connections between objects.

A graph contains:

- **Vertex (or node):** An object or location in the graph.
- **Edge:** A connection between two vertices.
- **Weight:** An optional value attached to an edge, such as distance, cost, or travel time.
- **Path:** A sequence of connected vertices.
- **Degree:** The number of edges connected to a vertex. In a directed graph, we distinguish *in-degree* (edges entering a vertex) and *out-degree* (edges leaving it).

Graphs are useful when information is about how things are connected, rather than simply arranged one after another.

### Example: A small campus network

Imagine four locations on a campus:

- A — Gate
- B — Registrar
- C — Library
- D — Laboratory

Suppose the paths are:

- A connects to B.
- A connects to C.
- B connects to D.
- C connects to D.

The graph can be drawn conceptually as:

```text
       B (Registrar)
      / \
A (Gate)  D (Laboratory)
      \ /
       C (Library)
```

The diagram is only a visual representation. The actual graph is stored in a data structure, such as an adjacency list or adjacency matrix.

### Basic graph notation

A graph is often written as **G = (V, E)**:

- **V** is the set of vertices.
- **E** is the set of edges.

For the campus example:

```text
V = {A, B, C, D}
E = {(A, B), (A, C), (B, D), (C, D)}
```

If edges have weights, the graph may also store a value for each edge, such as the distance between two campus buildings.

---

## 3. Types of Graphs

### 3.1 Undirected graph

An **undirected graph** has edges that work in both directions.

Example: If A is connected to B, a person can travel from A to B and from B to A.

```text
A ----- B
```

**Common use:** Two-way roads, mutual friendships, or two-way network links.

### 3.2 Directed graph

A **directed graph** (or digraph) has edges with a direction.

```text
A -----> B
```

This means the connection goes from A to B. It does not automatically mean B can go back to A.

**Common use:** One-way streets, website links, task dependencies, or following relationships on social media.

### 3.3 Weighted graph

A **weighted graph** assigns a value to each edge.

```text
A --5-- B
```

The value 5 might mean five kilometres, five minutes, or a cost of five units. The meaning depends on the application.

**Common use:** GPS routes, delivery planning, transportation costs, and network latency.

### 3.4 Unweighted graph

An **unweighted graph** treats every edge as an ordinary connection without a stored cost.

**Common use:** Finding the fewest connections between two users in a social network.

### 3.5 Connected and disconnected graphs

In an undirected graph, a graph is **connected** if every vertex can be reached from every other vertex. Otherwise, it is **disconnected**.

A graph can contain separate groups of vertices with no path between them. Each fully connected group is called a **connected component**.

### 3.6 Cyclic and acyclic graphs

A **cycle** is a path that returns to its starting vertex without repeating intermediate vertices.

- A **cyclic graph** contains at least one cycle.
- An **acyclic graph** contains no cycles.
- A **directed acyclic graph (DAG)** is a directed graph with no directed cycles.

**Common use:** A DAG can represent tasks that must be completed in a particular order, such as course prerequisites or software build dependencies.

---

## 4. Graph Representation

Graph representation is the way a program stores vertices and edges in memory. The choice affects memory use and the speed of operations.

Consider this undirected graph:

```text
A ----- B
|       |
|       |
C ----- D
```

Its edges are A–B, A–C, B–D, and C–D.

### 4.1 Adjacency matrix

An **adjacency matrix** is a two-dimensional table. Rows and columns represent vertices.

A cell contains `1` if an edge exists between the two vertices and `0` if it does not. For a weighted graph, a cell may store the weight instead, with a clearly defined value for “no edge.”

For the example:

| Vertex | A | B | C | D |
|---|---:|---:|---:|---:|
| A | 0 | 1 | 1 | 0 |
| B | 1 | 0 | 0 | 1 |
| C | 1 | 0 | 0 | 1 |
| D | 0 | 1 | 1 | 0 |

#### Procedure: Build an adjacency matrix

1. List every vertex in a fixed order: A, B, C, D.
2. Create a square matrix with one row and one column per vertex.
3. Initialize every cell to `0` (or the chosen “no edge” value).
4. For every edge, set the corresponding cell or cells.
5. For an undirected edge A–B, set both matrix entries `[A][B]` and `[B][A]`.
6. For a directed edge A → B, set `[A][B]`; do not set `[B][A]` unless a reverse edge also exists.
7. For a weighted graph, store the edge's weight rather than simply `1`.

**Advantages**
- Checking whether an edge exists is very fast: typically **O(1)**.
- The structure is straightforward for dense graphs (graphs with many edges).

**Disadvantages**
- It uses **O(V²)** space, even when few edges exist.
- Listing all neighbours of a vertex takes **O(V)** time.

### 4.2 Adjacency list

An **adjacency list** stores each vertex together with the vertices directly connected to it.

For the same undirected graph:

```text
A: B, C
B: A, D
C: A, D
D: B, C
```

#### Procedure: Build an adjacency list

1. Create an empty neighbour list for every vertex.
2. Read each edge one at a time.
3. For an undirected edge A–B, add B to A's list and A to B's list.
4. For a directed edge A → B, add B to A's list only.
5. For a weighted graph, store both the neighbour and the edge weight.
6. When adding an edge, decide whether duplicate edges are allowed. If not, check for an existing edge before inserting it.

**Advantages**
- Usually uses **O(V + E)** space.
- Efficient for sparse graphs (graphs with relatively few edges).
- Traversal can visit only the actual neighbours.

**Disadvantages**
- Checking whether a particular edge exists may take time proportional to the degree of a vertex.
- An adjacency list can be less convenient than a matrix when the graph is dense.

### 4.3 Edge list

An **edge list** stores the edges as pairs or triples.

```text
(A, B)
(A, C)
(B, D)
(C, D)
```

For a weighted graph, an edge might be stored as `(A, B, 5)`.

#### Procedure: Build an edge list

1. Store or identify the vertices.
2. Read every connection.
3. Record each edge as `(source, destination)`.
4. For a weighted graph, include the weight: `(source, destination, weight)`.
5. If the graph is undirected, define whether each edge is stored once or twice. Storing it once is common, provided the rest of the program treats it as undirected.

**Advantages**
- Simple and compact: **O(E)** space for the edges, plus any separate vertex storage.
- Useful for processing or sorting edges, such as in minimum spanning tree algorithms.

**Disadvantages**
- Finding all neighbours of one vertex may require scanning the entire edge list.
- Checking whether a specific edge exists is generally slower than in an adjacency matrix.

### 4.4 Representation comparison

| Representation | Typical space | Best suited for | Main trade-off |
|---|---|---|---|
| Adjacency matrix | O(V²) | Dense graphs; frequent edge checks | Uses more memory for sparse graphs |
| Adjacency list | O(V + E) | Sparse graphs; traversals | Edge checks may require searching a neighbour list |
| Edge list | O(V + E) including vertices | Edge-oriented processing | Neighbour lookup may require scanning edges |

Here, **V** is the number of vertices and **E** is the number of edges.

---

## 5. Graph Traversal

**Graph traversal** means visiting vertices in a systematic order. Two fundamental traversal techniques are Breadth-First Search and Depth-First Search.

A traversal must keep track of visited vertices. Without this, a graph containing a cycle could cause the algorithm to revisit vertices indefinitely.

Use the following undirected graph for the examples:

```text
       A
      / \
     B   C
     |   |
     D---E
```

Edges: A–B, A–C, B–D, C–E, and D–E.

The exact traversal order can depend on the order in which neighbours are stored. For the examples below, process neighbours alphabetically.

### 5.1 Breadth-First Search (BFS)

**Breadth-First Search** visits vertices in layers: first the starting vertex, then its immediate neighbours, then vertices two edges away, and so on.

BFS uses a **queue**, which follows FIFO: *First In, First Out*.

**Typical use:** Finding the path with the fewest edges in an unweighted graph.

#### Procedure: Perform BFS

1. Choose a starting vertex.
2. Create an empty queue and an empty visited set.
3. Mark the starting vertex as visited.
4. Enqueue the starting vertex.
5. While the queue is not empty:
   - Dequeue the front vertex and process it.
   - Examine each of its neighbours.
   - If a neighbour has not been visited, mark it visited immediately and enqueue it.
6. Stop when the queue becomes empty, or stop earlier if the task is to find a particular target and the algorithm has reached it.

**Example: BFS starting at A**

1. Visit A; enqueue B and C.
2. Dequeue B; visit D and enqueue it.
3. Dequeue C; visit E and enqueue it.
4. Dequeue D; E is already visited.
5. Dequeue E; its neighbours are already visited.

**BFS order:** `A, B, C, D, E`

**Important:** Mark a vertex visited when it is enqueued, not only when it is dequeued. This prevents the same vertex from being added to the queue multiple times.

**Complexity with an adjacency list:** O(V + E).

### 5.2 Depth-First Search (DFS)

**Depth-First Search** explores one branch as far as possible before returning (backtracking) to explore another branch.

DFS can be implemented with **recursion** (the function calls itself) or with an explicit **stack**. A stack follows LIFO: *Last In, First Out*.

**Typical use:** Exploring connected components, detecting cycles, maze exploration, and analysing dependencies.

#### Procedure: Perform DFS recursively

1. Choose a starting vertex.
2. Create an empty visited set.
3. Mark the current vertex as visited and process it.
4. Examine each neighbour in the chosen order.
5. For each unvisited neighbour, call DFS on that neighbour.
6. When a vertex has no unvisited neighbours, return to the previous call. This is backtracking.
7. Continue until all vertices reachable from the start have been visited.

**Example: DFS starting at A**, processing neighbours alphabetically:

1. Visit A.
2. Visit B.
3. Visit D.
4. Visit E.
5. Visit C.
6. Backtrack when no unvisited neighbours remain.

**DFS order:** `A, B, D, E, C`

Another neighbour order may produce a different valid DFS order.

**Complexity with an adjacency list:** O(V + E).

### BFS and DFS comparison

| Feature | BFS | DFS |
|---|---|---|
| Main structure | Queue | Stack or recursion |
| Exploration | Level by level | Deep along one branch, then backtracks |
| Fewest edges in an unweighted graph | Finds such a path | Does not guarantee it |
| Common uses | Shortest unweighted paths, levels, reachability | Cycle detection, components, maze exploration |
| Complexity with adjacency list | O(V + E) | O(V + E) |

### What if the graph is disconnected?

Starting BFS or DFS at one vertex visits only the vertices reachable from that start. To traverse every vertex in a disconnected graph:

1. Mark all vertices as unvisited.
2. Loop through every vertex.
3. Whenever a vertex is still unvisited, start a new BFS or DFS from it.
4. Each new search explores another connected component.

For a directed graph, reachability follows edge directions. Connected-component terminology must be chosen carefully because directed graphs have different notions, including strongly connected components.

---

## 6. Example Implementation in Java

The following example uses an **adjacency list** and demonstrates adding undirected edges, BFS, and DFS. It uses letters for vertex names and processes neighbours in insertion order.

```java
import java.util.*;

public class Graph {
    // Each vertex maps to a list of its neighbouring vertices.
    private final Map<String, List<String>> adjacencyList = new LinkedHashMap<>();

    // Add a vertex if it does not already exist.
    public void addVertex(String vertex) {
        adjacencyList.putIfAbsent(vertex, new ArrayList<>());
    }

    // Add an undirected edge between two vertices.
    public void addEdge(String source, String destination) {
        addVertex(source);
        addVertex(destination);

        // Add both directions because this is an undirected graph.
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
                    visited.add(neighbour); // Mark when enqueued.
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

### Expected output

```text
BFS: A B C D E
DFS: A B D E C
```

### Understanding the implementation

- `Map<String, List<String>>` stores each vertex and its neighbours.
- `addVertex()` ensures a vertex exists before its edges are added.
- `addEdge()` adds both directions because this example is undirected.
- `Queue` supports BFS's first-in, first-out processing.
- `Set` records visited vertices and prevents repeated processing.
- `dfsRecursive()` follows one branch and then backtracks.
- `LinkedHashMap` and the neighbour lists preserve insertion order, making this example's output predictable.

**Note:** This implementation is for an undirected, unweighted graph. A directed graph should add only the source-to-destination connection. A weighted graph must store edge weights as well. Recursive DFS can also use significant call-stack space on a very deep graph; an explicit stack is an alternative.

---

## 7. Applications of Graphs and Networks

### 7.1 GPS and route planning

**Vertices:** Places, intersections, or landmarks.  
**Edges:** Roads connecting those places.  
**Weights:** Distance, travel time, toll, or cost.

**Procedure for a simple route problem:**

1. Represent each location as a vertex.
2. Add an edge for each permitted road.
3. Store a weight if roads have different costs or travel times.
4. Choose the starting location and destination.
5. Use BFS for the fewest edges when the graph is unweighted.
6. Use an appropriate weighted shortest-path algorithm, such as Dijkstra's algorithm for non-negative edge weights, when minimising distance or cost.
7. Reconstruct and display the route.

**Important:** BFS does not generally find the shortest-distance route in a weighted road network.

### 7.2 Social networks

**Vertices:** People or accounts.  
**Edges:** Friendships, follows, or other relationships.

**Example procedure:**

1. Create a vertex for each user.
2. Add edges to represent relationships.
3. Use BFS to discover users within a certain number of connections.
4. Use graph analysis to explore mutual connections or communities.

For a directed “follows” network, A following B does not automatically mean B follows A.

### 7.3 Computer networks

**Vertices:** Computers, routers, switches, or servers.  
**Edges:** Communication links.  
**Weights:** Delay, bandwidth-related cost, or link cost.

Graphs can help model connectivity, find routes, and identify links whose failure may separate parts of a network.

### 7.4 Course prerequisites and task scheduling

**Vertices:** Courses or tasks.  
**Directed edges:** A prerequisite or dependency.

For example, if Programming Fundamentals must be completed before Data Structures, add an edge:

```text
Programming Fundamentals ---> Data Structures
```

**Procedure:**

1. Create a vertex for each course or task.
2. Draw a directed edge from each prerequisite to the task that depends on it.
3. Check for cycles. A cycle may indicate impossible prerequisites or circular dependencies.
4. If the graph is a DAG, use a topological ordering to find an order that respects the dependencies.

### 7.5 Recommendation systems

**Vertices:** Users and items, such as movies or products.  
**Edges:** Interactions, such as a user rating or purchasing an item.

A recommendation system can examine shared connections to identify items that may interest a user. Real systems may use additional data and more advanced algorithms.

### 7.6 Delivery and logistics

**Vertices:** Warehouses, stores, customers, or delivery points.  
**Edges:** Available travel links.  
**Weights:** Distance, delivery time, or cost.

Graphs help compare routes and model connections between locations. Finding the best route for multiple deliveries may require more advanced optimisation than a single shortest-path search.

### 7.7 Web pages and hyperlinks

**Vertices:** Web pages.  
**Directed edges:** Links from one page to another.

Search engines can model the web as a directed graph and analyse links to help understand relationships among pages.

---

## 8. Rules for Implementing Graphs

Follow these rules when designing and coding a graph.

1. **Define what a vertex represents.** Every vertex should have a clear meaning, such as a campus building or a user.
2. **Choose the graph type first.** Decide whether it is directed or undirected, and weighted or unweighted.
3. **Choose a suitable representation.** Use an adjacency list for many sparse graphs, an adjacency matrix for dense graphs or frequent edge-existence checks, and an edge list for edge-oriented processing.
4. **Use stable vertex identifiers.** Each vertex should have a unique identifier or a clearly defined mapping from identifiers to indices.
5. **Validate vertices and edges.** Decide whether adding an edge automatically creates missing vertices or rejects the operation, and apply that rule consistently.
6. **Respect edge direction.** For an undirected edge, store both directions in an adjacency list. For a directed edge, store only the specified direction.
7. **Store weights consistently.** Define what a weight means and how a missing edge is represented. Do not confuse a valid weight such as `0` with “no edge.”
8. **Prevent unwanted duplicate edges.** Decide whether parallel edges are allowed. If they are not, check before adding an edge.
9. **Handle self-loops deliberately.** Decide whether a vertex may connect to itself and validate edge additions accordingly.
10. **Track visited vertices during traversal.** Mark each vertex at the appropriate time so cycles do not cause endless traversal.
11. **Use the correct structure for each traversal.** BFS uses a queue; iterative DFS uses a stack, while recursive DFS uses the call stack.
12. **Understand traversal scope.** BFS or DFS from one starting vertex does not necessarily visit a disconnected graph's other components.
13. **Do not assume traversal order is universal.** The order depends on how neighbours are stored or sorted.
14. **Choose algorithms based on the problem.** BFS can find a fewest-edge path in an unweighted graph; weighted shortest paths require an appropriate weighted algorithm.
15. **Consider memory and performance.** Estimate the number of vertices and edges before selecting a representation.
16. **Test boundary cases.** Test an empty graph, a single vertex, a graph with no edges, a cycle, a disconnected graph, and invalid vertex input.
17. **Keep the implementation consistent.** Use the same vertex identifiers, edge rules, and weight conventions throughout the program.
18. **Document assumptions.** Explain whether edges are directed, whether duplicates are allowed, and what edge weights mean.

---

## 9. Common Mistakes and How to Avoid Them

| Mistake | Why it is a problem | Correct approach |
|---|---|---|
| Forgetting the reverse edge in an undirected adjacency list | The connection appears to work in only one direction | Add both neighbour entries |
| Adding a reverse edge to every directed edge | It changes the meaning of the graph | Add only the specified direction |
| Not using a visited set | Cycles can cause repeated visits or endless processing | Track visited vertices |
| Marking BFS vertices visited too late | A vertex may enter the queue multiple times | Mark it when enqueued |
| Using BFS for weighted shortest distance | Fewest edges may not mean lowest cost | Use a suitable weighted shortest-path algorithm |
| Assuming every vertex is reachable | Some graphs are disconnected | Start additional searches from unvisited vertices |
| Treating `0` as “no edge” in every weighted matrix | A valid zero-weight edge may be lost | Use a separate presence marker or a safe sentinel |
| Ignoring graph size | A matrix may waste memory on a large sparse graph | Compare expected vertices and edges before choosing a representation |

---

## 10. Suggested Classroom Activity

### Scenario: Campus Navigation Network

A campus has five locations:

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

### Tasks

1. List the vertices and edges.
2. Draw the graph.
3. Construct its adjacency matrix.
4. Construct its adjacency list.
5. Starting from A, perform BFS and record the order.
6. Starting from A, perform DFS and record the order. State the neighbour order you used.
7. Explain what changes if the path from B to D becomes a one-way path from B to D.
8. Suppose each edge has a distance. Explain why BFS alone may not find the route with the shortest total distance.
9. Modify the Java program to include E and the edge D–E.
10. Test the program with a starting vertex that does not exist and explain how the program responds.

### Suggested answers for checking

Using alphabetical neighbour order and the edges listed above:

- **Vertices:** `{A, B, C, D, E}`
- **Edges:** `{(A,B), (A,C), (B,D), (C,D), (D,E)}`
- **Adjacency list:**
  ```text
  A: B, C
  B: A, D
  C: A, D
  D: B, C, E
  E: D
  ```
- **BFS from A:** `A, B, C, D, E`
- **DFS from A:** `A, B, D, C, E` when neighbours are processed alphabetically and DFS recursively follows the first unvisited neighbour.

Traversal orders can differ if the neighbour order differs, while still being valid.

---

## 11. Summary

A graph is a data structure for modelling objects and their relationships. Its vertices represent the objects, and its edges represent connections. Edges may be directed or undirected and may carry weights.

- **Adjacency matrix:** Good for dense graphs and fast edge checks.
- **Adjacency list:** Commonly suitable for sparse graphs and traversal.
- **Edge list:** Simple representation for processing edges.
- **BFS:** Uses a queue and explores level by level; it finds fewest-edge paths in unweighted graphs.
- **DFS:** Uses a stack or recursion and explores deeply before backtracking.
- **Applications:** Route planning, social networks, computer networks, scheduling, recommendations, logistics, and web links.

The most important implementation habit is to define the graph's rules before writing code. Decide what vertices and edges mean, select the correct representation, handle direction and weights consistently, and use visited tracking during traversal.
