# 🌟 Longest Path in a DAG

## 🔗 Problem Link
[Link](https://www.geeksforgeeks.org/problems/longest-path-in-a-directed-acyclic-graph/1)

---

## 🧠 Overview

Topological Sort isn't just for scheduling — it's also a powerful tool for finding **shortest** and **longest paths** in a **Directed Acyclic Graph (DAG)**.  
By processing nodes in **topological order**, we can efficiently relax edges and compute distances from a source node in **linear time**, making it ideal for such problems.

---

## ⏱️ Time & Space Complexity

- **Time Complexity:** `O(V + E)`  
- **Space Complexity:** `O(V + E)` (for adjacency list and other data structures)

---

## 📌 Code

```cpp
// User function Template for C++

class Solution {
  public:
    vector<int> maximumDistance(vector<vector<int>> edges, int v, int e, int src) {
        // Step 1: Initialize in-degree array and adjacency list
        vector<int> indeg(v, 0);
        vector<vector<pair<int, int>>> adj(v);

        // Build the graph and in-degree array
        for (auto it : edges) {
            adj[it[0]].push_back({it[1], it[2]}); // directed edge with weight
            indeg[it[1]]++; // increase in-degree of destination node
        }

        // Step 2: Perform topological sort using Kahn's Algorithm
        queue<int> q;
        for (int i = 0; i < v; i++) {
            if (indeg[i] == 0) {
                q.push(i); // push all nodes with 0 in-degree
            }
        }

        vector<int> topo;
        while (!q.empty()) {
            int t = q.front();
            q.pop();
            topo.push_back(t);

            // Reduce in-degree of neighbors and add to queue if it becomes 0
            for (auto it : adj[t]) {
                indeg[it.first]--;
                if (indeg[it.first] == 0) {
                    q.push(it.first);
                }
            }
        }

        // Step 3: Initialize distance array
        vector<int> dist(v, INT_MIN);
        dist[src] = 0; // distance to source is 0

        // Step 4: Relax edges in topological order
        for (auto k : topo) {
            for (auto it : adj[k]) {
                // Relax the edge if current path gives a longer distance
                if (dist[k] != INT_MIN && dist[k] + it.second > dist[it.first]) {
                    dist[it.first] = dist[k] + it.second;
                }
            }
        }

        // Final result: longest distances from source to all other nodes
        return dist;
    }
};

```
#code
