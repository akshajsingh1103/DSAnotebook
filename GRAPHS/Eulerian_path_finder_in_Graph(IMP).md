# Eulerian Path Finder in a Graph

[Problem Link](https://www.geeksforgeeks.org/dsa/paths-travel-nodes-using-edgeseven-bridges-konigsberg/)

---

## Theory

This problem is a generalization of the famous **Seven Bridges of Königsberg** problem, which led to the foundation of **Graph Theory** by **Leonhard Euler** in 1735.

### 🧩 Problem Goal
Determine whether an **Eulerian path** exists in a given graph — that is, a path that **uses each edge exactly once**, possibly revisiting vertices.

---

## 🔄 Eulerian Path vs 🔂 Hamiltonian Path

| Feature                    | Eulerian Path                            | Hamiltonian Path                          |
|----------------------------|------------------------------------------|--------------------------------------------|
| Focus                      | Traverses **every edge** once            | Visits **every vertex** once               |
| Revisits Vertices?         | ✅ Yes                                    | ❌ No                                       |
| Edge Usage                 | Each edge **exactly once**               | May skip some edges                        |
| Vertex Usage               | May visit vertices multiple times        | Each vertex **exactly once**               |
| Complexity                 | Can be solved in **linear time**         | **NP-Complete**                            |
| Conditions Known?          | ✅ Yes                                    | ❌ No known general necessary/sufficient condition |

---

## 🔍 Eulerian Path Conditions

### Undirected Graph

- A graph has an **Eulerian path** if and only if:
  - It is **connected**, and
  - It has **0 or 2 vertices with odd degree**.

- A graph has an **Eulerian circuit** (start and end at the same node) if:
  - It is **connected**, and
  - **All vertices have even degree**.

### Directed Graph

- A graph has an **Eulerian path** if and only if:
  - It is **strongly connected** (or all non-zero degree vertices are in a single strongly connected component), and
  - Exactly **one vertex** has `(out-degree - in-degree) = 1` (start node),
  - Exactly **one vertex** has `(in-degree - out-degree) = 1` (end node),
  - All other vertices have equal in-degree and out-degree.

- A graph has an **Eulerian circuit** if:
  - All vertices have equal in-degree and out-degree,
  - And the graph is strongly connected.

---

## ❗ Hamiltonian Path Notes

- A **Hamiltonian path** visits **each vertex exactly once**.
- There is **no known efficient algorithm** to determine its existence for arbitrary graphs.
- This is a well-known **NP-complete problem** — meaning it's believed that no polynomial-time solution exists for the general case.

---

## ✅ Code
```cpp
// Check whether an Eulerian Path exists in the undirected graph
// Also find a valid starting node (one of the odd-degree nodes if there are two)
bool hasEulerianPath(vector<vector<int>>& adj, int& start, int n) {
    int odd = 0;

    // Count how many vertices have odd degree
    for (int i = 0; i < n; i++) {
        int deg = 0;
        for (int j = 0; j < n; j++) {
            deg += adj[i][j];  // Sum of row = degree for undirected adjacency matrix
        }

        // If degree is odd, increment counter
        if (deg % 2 != 0) {
            odd++;
            start = i;  // Choose one of the odd-degree vertices as starting point
        }
    }

    // Eulerian Path exists only if there are 0 or 2 odd-degree vertices
    return (odd == 0 || odd == 2);
}

// Perform DFS to build the Eulerian path
// This uses Hierholzer's algorithm to ensure every edge is visited exactly once
void dfs(int u, vector<vector<int>>& adj, vector<int>& path, int n) {
    // Traverse all adjacent vertices
    for (int v = 0; v < n; v++) {
        // While there's an edge between u and v
        while (adj[u][v] > 0) {
            adj[u][v]--;  // Remove the edge u-v
            adj[v][u]--;  // Remove the edge v-u (undirected)
            dfs(v, adj, path, n);  // Recurse on the neighbor
        }
    }

    // After exploring all edges from u, add it to the path
    path.push_back(u);
}

// Main function to find the Eulerian path
vector<int> findEulerianPath(vector<vector<int>> adj) {
    int n = adj.size();  // Number of nodes
    int start = 0;       // Starting node for DFS

    // Step 1: Check if Eulerian path exists and get starting node
    if (!hasEulerianPath(adj, start, n)) {
        return {-1};  // No Eulerian path exists
    }

    // Step 2: Count the total number of edges in the graph (once)
    int edgeCount = 0;
    for (int i = 0; i < n; i++)
        for (int j = i; j < n; j++)
            edgeCount += adj[i][j];

    // Step 3: Perform DFS traversal to build the Eulerian path
    vector<int> path;
    dfs(start, adj, path, n);

    // Step 4: Verify that all edges were used (path should have edgeCount + 1 vertices)
    if ((int)path.size() != edgeCount + 1) {
        return {-1};  // Not all edges used → invalid path
    }

    // Step 5: Convert result to 1-based indexing if needed
    for (int& x : path) {
        x += 1;
    }

    // The path was built in reverse order during DFS → reverse it
    reverse(path.begin(), path.end());

    return path;
}

```

---

## Time and Space Complexity

### Time Complexity:
- `hasEulerianPath`: O(n²)
- Counting edges: O(n²)
- DFS traversal: O(n²) (since you use an adjacency matrix)
- ✅ **Total Time Complexity**:  
  \[{O(n^2)}\]

### Space Complexity:
- Adjacency matrix: O(n²)
- Path storage: O(m)
- DFS call stack: O(m)
- ✅ **Total Space Complexity**:  
  \[{O(n^2)}\]
  *(Dominated by adjacency matrix. Can be reduced to O(n + m) with adjacency list.)*

