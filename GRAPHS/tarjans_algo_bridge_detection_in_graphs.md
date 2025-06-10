# 🌉 Bridge Detection in Graph using Tarjan's Algorithm

---

## 🔗 Problem Link

[Problem Link](https://www.geeksforgeeks.org/problems/bridge-edge-in-graph/1)

---

## ❓ Problem Statement

Given an undirected graph with `V` vertices and a list of edges, determine whether a specific edge `(c, d)` is a **bridge** in the graph.  
A **bridge** is an edge whose removal increases the number of connected components in the graph.

---

## ✅ Approach: DFS + Tarjan’s Algorithm

- Use **Tarjan’s Algorithm** to compute discovery time and lowest reachable vertex (`low[]`) during DFS traversal.
- An edge `(u, v)` is a **bridge** if `low[v] > disc[u]`.
- Track discovery time and low-link values.
- Only one DFS traversal is required.

---

## 📦 Code

```cpp
class Solution {
  public:
    bool dfs(int node, int parent, vector<vector<int>>& adj, vector<int>& vis, vector<int>& low, int& timer, vector<int>& disc, int c, int d) {
        vis[node] = true;
        disc[node] = low[node] = timer++;
        
        for (auto nbr : adj[node]) {
            if (nbr == parent) continue;
            if (!vis[nbr]) {
                if (dfs(nbr, node, adj, vis, low, timer, disc, c, d)) 
                    return true;
                
                low[node] = min(low[node], low[nbr]);
                
                // Check if (node, nbr) is a bridge
                if (low[nbr] > disc[node]) {
                    if ((node == c && nbr == d) || (node == d && nbr == c)) {
                        return true;
                    }
                }
            } else {
                low[node] = min(low[node], disc[nbr]);
            }
        }
        return false;
    }

    bool isBridge(int V, vector<vector<int>> &edges, int c, int d) {
        vector<vector<int>> adj(V); // ✅ Fixed adjacency list
        for (auto& it : edges) {
            adj[it[0]].push_back(it[1]);
            adj[it[1]].push_back(it[0]);
        }

        vector<int> vis(V, 0), disc(V, -1), low(V, -1);
        int timer = 0;

        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                if (dfs(i, -1, adj, vis, low, timer, disc, c, d)) 
                    return true;
            }
        }
        return false;
    }
};

```


---

## ⏱️ Time Complexity

- **DFS Traversal:** Each vertex and edge is visited once.

\[
\text{Time Complexity} = O(V + E)
\]

---

## 💾 Space Complexity

- `adj`: Adjacency list → `O(V + E)`
- `disc`, `low`, `vis`: Each is `O(V)`
- Stack call (DFS): Up to `O(V)` in the worst case

\[
\text{Space Complexity} = O(V + E)
\]

---

## ⚠️ Common Mistake

❌ Initializing the adjacency list as a matrix:

```cpp
// Wrong ❌
vector<vector<int>> adj(V, vector<int>(V, 0));
adj[0] = [0, 0, 0, ..., 0]  // 0 to V-1
adj[1] = [0, 0, 0, ..., 0]
adj[it[0]].push_back(it[1]); //this causes
adj[0] = [0, 0, 0, 0, 2]  // Invalid — 0s are already there



 // Right ✅
vector<vector<int>> adj(V); 
adj[0] = {1, 2}
adj[1] = {0, 3}

```
