# Hamiltonian Path and Cycle
[Problem Link](https://www.geeksforgeeks.org/problems/hamiltonian-path2522/1)

**Problem Type:** Graphs – Backtracking (DFS)

## Problem Summary

Given an undirected graph with `n` vertices and `m` edges, determine if there exists:
- A **Hamiltonian Path** – a path that visits each vertex exactly once.
- A **Hamiltonian Cycle** – a cycle that visits each vertex exactly once and returns to the starting vertex.

This solution uses backtracking (DFS) to explore all paths, marking nodes as visited. A minor modification in the base case allows the same code to be used for both Hamiltonian Path and Cycle detection.

## Key Points

- **Hamiltonian Path**: Visit all nodes exactly once.
- **Hamiltonian Cycle**: Hamiltonian Path + edge from last node back to starting node.
- We try DFS from every node as a potential start.
- The same DFS logic is used; just modify the base case to check if a cycle exists.

## Time and Space Complexity

- **Time Complexity:** O(n!), due to the number of permutations of nodes.
- **Space Complexity:** O(n + m) for adjacency list + O(n) for visited array + recursion stack.

## C++ Implementation

```cpp
class Solution {
public:
    // DFS to explore all paths
    bool dfs(vector<vector<int>> &adj, vector<int>& vis, int node, int total, int &totalnodes, int start) {
        // Base case: All nodes visited
        if (total == totalnodes) {
            // Uncomment below for Hamiltonian Cycle check
            /*
            for (auto it : adj[node]) {
                if (it == start) return true; // forms a cycle
            }
            return false; // path only
            */
            return true; // Hamiltonian Path
        }

        for (auto it : adj[node]) {
            if (!vis[it]) {
                vis[it] = true;
                if (dfs(adj, vis, it, total + 1, totalnodes, start)) return true;
                vis[it] = false; // backtrack
            }
        }
        return false;
    }

    bool check(int n, int m, vector<vector<int>> edges) {
        vector<vector<int>> adj(n + 1);
        for (auto it : edges) {
            adj[it[0]].push_back(it[1]);
            adj[it[1]].push_back(it[0]);
        }

        vector<int> vis(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            vis[i] = true;
            if (dfs(adj, vis, i, 1, n, i)) return true;
            vis[i] = false;
        }
        return false;
    }
};

