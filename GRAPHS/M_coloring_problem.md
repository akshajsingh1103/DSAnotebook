# 🎨 Graph Coloring Problem – Time Complexity Deep Dive

---

## 📌 Problem Statement

Given an undirected graph with `V` vertices and `E` edges, and an integer `m`, determine if it's possible to color the vertices using **at most** `m` colors such that **no two adjacent vertices share the same color**.

---

## ❌ Method 1: Pure Brute Force with Global Validity Check

### 🔧 Approach

- Try all possible combinations of `M^V` colorings.
- For each complete assignment, call `goodcolor()` to verify if adjacent nodes have different colors.
- No pruning is applied during recursion.

### 📦 Code

```cpp
#include <bits/stdc++.h>
using namespace std;

bool goodcolor(vector<int> adj[], vector<int> col){
    for(int i = 0; i < col.size(); i++) {
        for(auto it : adj[i]) {
            if(i != it && col[i] == col[it]) return false;
        }
    }
    return true;
}

bool genratecolor(int i, vector<int> col, int m, vector<int> adj[]) {
    if(i >= col.size()) {
        return goodcolor(adj, col);
    }

    for(int j = 0; j < m; j++) {
        col[i] = j;
        if(genratecolor(i + 1, col, m, adj)) return true;
        col[i] = -1;
    }

    return false;
}

bool graphColoring(int v, vector<vector<int>> &edges, int m) {
    vector<int> adj[v];
    for (auto it : edges) {
        adj[it[0]].push_back(it[1]);
        adj[it[1]].push_back(it[0]); 
    }

    vector<int> color(v, -1); 
    return genratecolor(0, color, m, adj);
}
```
### ⏱️ Time Complexity

- Total color assignments: `M^V`
- For each assignment, check all vertex pairs → `O(V^2)` in dense graphs

\[
\text{Total Time} = O(M^V \cdot (V+E))
\]

### 💾 Space Complexity

\[
O(V + E) \quad \text{(adjacency list + color array + recursion)}
\]

---

## ✅ Method 2: Optimized Backtracking with Pruning

### 🔧 Approach

- Use recursive backtracking.
- For each vertex, try each of the `m` colors **only if it's safe** (`issafe()`).
- Reduces the number of branches using early pruning.

### 📦 Code

```cpp
#include <bits/stdc++.h>
using namespace std;

bool issafe(int vertex, int col, vector<int> adj[], vector<int> &color) {
    for (auto it : adj[vertex]) {
        if (color[it] != -1 && col == color[it]) return false;
    }
    return true;
}

bool cancolor(int vertex, int m, vector<int> adj[], vector<int> &color) {
    if (vertex == color.size()) return true;

    for (int i = 0; i < m; i++) {
        if (issafe(vertex, i, adj, color)) {
            color[vertex] = i;
            if (cancolor(vertex + 1, m, adj, color)) return true;
            color[vertex] = -1;
        }
    }

    return false;
}

bool graphColoring(int v, vector<vector<int>> &edges, int m) {
    vector<int> adj[v];
    for (auto it : edges) {
        adj[it[0]].push_back(it[1]);
        adj[it[1]].push_back(it[0]);
    }

    vector<int> color(v, -1);
    return cancolor(0, m, adj, color);
}
```
### ⏱️ Time Complexity

#### 🔁 Recursive Tree Analysis

At each of the `V` vertices:

- Try up to `M` colors
- The recursion tree has `M` branches per level, depth = `V`

Total recursive calls:

\[
M^0 + M^1 + M^2 + \dots + M^{V-1} = \sum_{i=0}^{V-1} M^i = \frac{M^V - 1}{M - 1} \in O(M^V)
\]

#### 🧠 Work per Call

Each `issafe()` call checks at most `V` neighbors:

\[
\text{Total Time} = O(M^V \cdot V)
\]

---

### 💾 Space Complexity

\[
O(V + E) \quad \text{(adjacency list + color array + recursion)}
\]

---

### ⚖️ Summary Table

| Metric               | Brute Force (`goodcolor()`)  | Optimized Backtracking (`issafe()`) |
|----------------------|-------------------------------|--------------------------------------|
| Time Complexity       | \( O(M^V \cdot V^2) \)         | \( O(M^V \cdot V) \)                  |
| Space Complexity      | \( O(V + E) \)                 | \( O(V + E) \)                        |
| Pruning              | ❌ No                          | ✅ Yes                                |
| Validity Check Cost   | \( O(V^2) \)                   | \( O(V) \)                            |
| Practical Use         | 🔴 Very Inefficient            | 🟢 Much Better                        |

---

