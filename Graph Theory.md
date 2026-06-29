
## 🟩 Breadth-First Search (BFS)

### A. Example & Drawing

BFS explores the graph **layer by layer**, visiting all neighbors of a node before moving to the next depth level. It uses a **Queue (FIFO - First In, First Out)** data structure.

Plaintext

```
      [A]          <-- Level 0 (Start)
     /   \
   [B]   [C]       <-- Level 1
   / \     \
 [D] [E]   [F]     <-- Level 2
```

**Execution Order from [A]:** `A ➔ B ➔ C ➔ D ➔ E ➔ F`

### B. Metaphor

> [!info] **The Ripple in a Pond** 🌊
> 
> Imagine dropping a stone into a calm pond at node `A`. The water ripples outward in perfect, concentric circles. It hits every immediate neighbor simultaneously (Level 1), then expands to hit the next boundary of nodes (Level 2). It never goes deep into one channel until it has washed over everything closer.

### C. Pseudo Code

Plaintext

```c
procedure BFS(Graph, start_node)
    let Q be a queue
    label start_node as discovered
    Q.enqueue(start_node)
    
    while Q is not empty do
        v := Q.dequeue()
        for all edges from v to w in Graph.adjacentEdges(v) do
            if w is not labeled as discovered then
                label w as discovered
                Q.enqueue(w)
            end if
        end for
    end while
end procedure
```

---

## 🟪 Depth-First Search (DFS)

### A. Example & Drawing

DFS explores the graph by diving **as deep as possible** along each branch before backtracking. It uses a **Stack (LIFO - Last In, First Out)** data structure, typically implemented via recursion.

Plaintext

```
      [A]1
     /   \
   [B]2   [E]6
   / \
 [C]3 [D]5
   |
 [D]4  (Target or Dead End)
```

**Execution Order from [A]:** `A ➔ B ➔ C ➔ D ➔ E`

_(It deep-dives to the bottom of the left branch before ever looking at the right branch node `E`)._

### B. Metaphor

> [!info] **The Labyrinth Explorer** 🧵
> 
> Think of entering a dark maze holding a spool of thread. You pick a path and walk straight down it blindly until you hit a dead end. Only when you cannot go any further do you roll up your thread, backtrack to the last intersection you passed, and try the next unexplored corridor.

### C. Pseudo Code

Plaintext

```c
procedure DFS(Graph, v)
    label v as discovered
    for all directed edges from v to w in Graph.adjacentEdges(v) do
        if w is not labeled as discovered then
            recursive DFS(Graph, w)
        end if
    end for
end procedure
```

---

## 📈 Search Comparison Cheatsheet

| **Metric**           | **Breadth-First Search (BFS)**                     | **Depth-First Search (DFS)**                       |
| -------------------- | -------------------------------------------------- | -------------------------------------------------- |
| **Data Structure**   | Queue (FIFO)                                       | Stack / Recursion (LIFO)                           |
| **Core Strategy**    | Explores wide before deep                          | Explores deep before wide                          |
| **Space Complexity** | $O(V)$ where $V$ is vertices (stores a layer)      | $O(H)$ where $H$ is max tree height                |
| **Time Complexity**  | $O(V + E)$                                         | $O(V + E)$                                         |
| **Best Used For**    | Finding the **shortest path** on unweighted graphs | Topological sorting, finding cycles, solving mazes |

