# 0-1 BFS and Queue-based Shortest Path Algorithms: Explanation and Comparison

[GFG article Link](https://www.geeksforgeeks.org/dsa/0-1-bfs-shortest-path-binary-graph/)
---

## 1. Key Property of 0-1 BFS

The 0-1 BFS algorithm is an optimization for shortest paths in graphs where edge weights are only 0 or 1. It uses a **deque** (double-ended queue) to maintain nodes in a specific order based on their distance from the source.

### 🔄 How the deque maintains order:
The deque contains nodes whose distances differ by at most 1 at any point.

It looks like this (distance values of nodes inside deque):

[ d, d, d, d, d+1, d+1, d+1 ]


All nodes with distance `d` appear before nodes with distance `d+1`.

---

### 🤔 Why does this happen?

- When we relax an edge with weight **0**, the neighbor node has the **same distance** as the current node. So we **add it to the front** of the deque, maintaining the order of distance `d`.

- When we relax an edge with weight **1**, the neighbor node's distance becomes `d + 1`, so we **add it to the back** of the deque.

Because of this insertion rule, we process all nodes with distance `d` **before any node with distance `d+1`**.

Nodes with distance `d+2` can only appear after processing all `d+1` nodes, and so on.

---

### 💥 Effect:

This deque structure acts like a **priority queue** with only two "levels" of priority (`d` and `d+1`) and ensures efficient shortest path computation in **O(N + M)** time.

---

## 2. Comparison: 0-1 BFS vs Queue-based SPFA vs Bellman-Ford

| Algorithm         | Data Structure     | Weight Support                | Time Complexity     | Key Property                                                   |
|------------------|--------------------|-------------------------------|---------------------|----------------------------------------------------------------|
| **Bellman-Ford** | None (all edges)   | General weights (incl. neg)   | O(N * M)            | Repeatedly relaxes all edges for N-1 rounds                   |
| **Queue-SPFA**   | Normal queue       | General weights (no neg cycle)| Avg O(M), Worst O(N*M) | Enqueues nodes when relaxed, faster in practice but can degrade |
| **0-1 BFS**      | Deque              | Only 0 or 1 weights           | O(N + M)            | Uses deque to maintain distance layers, processes 0-weight first |

---

### ❓ Why can queue-based SPFA still work correctly with general weights?

Because it re-enqueues nodes **only when their distances improve**.

Eventually, it finds the shortest path, but **order of processing can be inefficient** (worse than Dijkstra).

In worst cases (especially with many relaxations), it can approach **O(N * M)** time.

---

### ❌ Why not just use a normal queue for all shortest path problems?

- Normal queue processes nodes **FIFO** without priority.

- In weighted graphs (with weights > 1), you might process nodes with **higher distances before smaller distances**.

This leads to **more relaxations and inefficiency**.

✅ Dijkstra’s algorithm uses a **priority queue** to always pick the **minimum distance** node next, ensuring optimal processing order.

✅ 0-1 BFS uses a **deque** to approximate this priority behavior but **only for weights 0 and 1**.

---

### 🧪 What about using a queue for Dijkstra?

You can simulate Dijkstra with a normal queue, but:

- It loses the **priority ordering**, so it may process **longer paths before shorter ones**.

This causes **unnecessary reprocessing** and increases runtime.

Hence, **priority queue is preferred** for Dijkstra.

---

## 3. Code for 0-1 BFS (using deque)

```cpp
// Function: minimumEdgeReversal
// Input: edges - vector of edges [u, v]
//        n - number of nodes
//        src - source node
//        dst - destination node
// Output: minimum number of edge reversals to reach dst from src, -1 if impossible

int minimumEdgeReversal(vector<vector<int>> &edges, int n, int src, int dst) {
    // Create adjacency list with pairs: (neighbor, weight)
    // weight = 0 for original edge direction
    // weight = 1 for reversed edge direction

    // Initialize dist array with INT_MAX
    // Set dist[src] = 0

    // Use deque to process nodes based on edge weight (0 or 1)
    // For each edge with weight 0, push front (high priority)
    // For each edge with weight 1, push back (low priority)

    // While deque not empty:
    //   Pop front node
    //   For each neighbor:
    //     If dist can be relaxed:
    //       Update dist
    //       Push front or back depending on edge weight

    // Return dist[dst] or -1 if unreachable
}
```

