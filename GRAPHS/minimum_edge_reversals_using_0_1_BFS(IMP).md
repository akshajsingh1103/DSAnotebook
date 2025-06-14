# 🚀 Problem: Minimum Edge Reversals


📚 [0-1 BFS Explanation and personal doubts](./"0_1_BFS_using_Deque(IMP).md")
[Problem Link GFG](https://www.geeksforgeeks.org/problems/minimum-edges/1)



Given a **directed graph** with `n` nodes and edges, we want to find the **minimum number of edge reversals** required to reach from source `src` to destination `dst`.

If it's impossible to reach `dst` from `src`, return -1.

---

## 🧠 Why Use 0-1 BFS?

This graph is special:  
Each edge can either be used **in its original direction (cost = 0)**,  
or **reversed (cost = 1)**.

This creates a weighted graph where edge weights are **only 0 or 1**.

👑 Perfect use case for **0-1 BFS**, which is optimized for such graphs.

---

## ⚙️ How 0-1 BFS Works

- Use a **deque (double-ended queue)** instead of a priority queue.
- For an edge of **weight 0**, push the neighbor to the **front** (high priority).
- For an edge of **weight 1**, push the neighbor to the **back** (low priority).
- This ensures all nodes with current `distance d` are processed before any with `distance d + 1`.

This deque behavior simulates a priority queue — but faster for 0/1 weights.

---
## 💻 Code

```cpp
// User function Template for C++

class Solution {
  public:
    int minimumEdgeReversal(vector<vector<int>> &edges, int n, int src, int dst) {
        // Adjacency list storing pairs: (neighbor, weight)
        // weight = 0 for original edge, 1 if reversed
        vector<vector<pair<int, int>>> adj(n + 1);
        
        for (auto edge : edges) {
            int u = edge[0];
            int v = edge[1];
            // Original direction (no reversal needed)
            adj[u].push_back({v, 0});
            // Reverse direction (reversal needed)
            adj[v].push_back({u, 1});
        }

        // Distance array to track min reversals to reach each node
        vector<int> dist(n + 1, INT_MAX);
        dist[src] = 0;

        // Deque for 0-1 BFS: (reversals, node)
        deque<pair<int, int>> dq;
        dq.push_front({0, src});

        while (!dq.empty()) {
            auto [reversalsSoFar, currentNode] = dq.front();
            dq.pop_front();

            // Explore all neighbors
            for (auto [neighbor, weight] : adj[currentNode]) {
                // If a better path (fewer reversals) is found
                if (dist[neighbor] > reversalsSoFar + weight) {
                    dist[neighbor] = reversalsSoFar + weight;

                    if (weight == 0) {
                        // Original edge: high priority → push to front
                        dq.push_front({reversalsSoFar, neighbor});
                    } else {
                        // Reversed edge: low priority → push to back
                        dq.push_back({reversalsSoFar + 1, neighbor});
                    }
                }
            }
        }

        // Return -1 if destination is unreachable
        return (dist[dst] == INT_MAX) ? -1 : dist[dst];
    }
};

```

---

## 🔍 Why Not Dijkstra?

Dijkstra also works (since edge weights are non-negative),  
but it uses a **priority queue**, which takes `O(log N)` time for insertion/removal.

⏱ Time Complexities:
| Algorithm     | Time Complexity     | Notes |
|---------------|----------------------|-------|
| **Dijkstra**  | `O((N + M) * log N)`  | Slower due to `log N` heap ops |
| **0-1 BFS**   | `O(N + M)`            | Linear time, faster for 0/1 weights |

Since 0-1 BFS is faster and fits this edge-weight pattern perfectly, we use it here.

---

## 🧾 Summary

- 0-1 BFS is ideal for graphs with only `0` and `1` edge weights.
- Reversing an edge adds a **cost of 1**, so we simulate the reversal with edge weights.
- Using a deque ensures efficient shortest path traversal.


