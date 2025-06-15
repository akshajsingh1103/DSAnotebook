# Vertex Cover Problem

---

## Theory & Approaches 🎯

The **Vertex Cover** problem asks:  
> *"What is the smallest set of vertices such that every edge in the graph touches at least one of those vertices?"*

This problem is **NP-hard**, meaning finding the *optimal* (smallest) vertex cover efficiently for large graphs is basically impossible (unless P=NP).  

---

### 1️⃣ Approximate & Greedy Solutions

Since optimal is hard, we use *approximation* or *heuristic* methods to get "good enough" answers fast!

- **Greedy Degree-Based Heuristic** 🔥  
  The idea:  
  - Look at vertex degrees (how many edges each node has).  
  - Pick vertices with the highest degree first (they cover the most edges!).  
  - Remove all edges touched by those chosen vertices.  
  - Repeat until no edges remain.  
   
  **Pros:** Simple and often fast.  
  **Cons:** No guarantee how close it is to the optimal solution.

- **2-Approximation Algorithm 🎯**  
  A more clever approach:  
  - Pick *any* remaining edge `(u, v)`.  
  - Add **both** `u` and `v` to the vertex cover set.  
  - Remove all edges connected to `u` or `v`.  
  - Repeat until no edges remain.  

  This method guarantees that the size of the cover is at **most twice** the size of the optimal vertex cover — which is pretty neat! 🎉

---

### Greedy Algorithm (Degree-Based Heuristic)

##code  
```cpp
class Solution {
  public:
    int vertexCover(int n, vector<pair<int, int>> &edges) {
        
        vector<int> deg(n + 1, 0);
        vector<vector<int>> adj(n + 1);
        vector<bool> visited(n + 1, false);

        for (auto& it : edges) {
            adj[it.first].push_back(it.second);
            adj[it.second].push_back(it.first);
            deg[it.first]++;
            deg[it.second]++;
        }

        priority_queue<pair<int, int>> pq;

        // Initially push all nodes with degree > 0
        for (int i = 1; i <= n; ++i) {
            if (deg[i] > 0) {
                pq.push({deg[i], i});
            }
        }

        vector<int> result;

        while (!pq.empty()) {
            
            auto n = pq.top();
            pq.pop();

            if (visited[n.second] || deg[n.second] < n.first) continue;

            visited[n.second] = true;
            result.push_back(n.second);

            for (int v : adj[n.second]) {
                
                if (!visited[v]) {
                    deg[v]--;
                    if(deg[v]>0){
                        pq.push({deg[v], v});
                    }
                }
            }
            deg[n.second] = 0;
        }

        return result.size();
    }
};
```

---

### 2-Approximation Algorithm

##code  
```cpp
void Graph::printVertexCover()
{
    bool visited[V];
    for (int i = 0; i < V; i++)
        visited[i] = false;

    for (int u = 0; u < V; u++)
    {
        if (visited[u] == false)
        {
            for (auto v : adj[u])
            {
                if (!visited[v])
                {
                    visited[u] = true;
                    visited[v] = true;
                    break;
                }
            }
        }
    }
}

```

---

### Complexity Overview ⏳

| Algorithm               | Time Complexity                            | Space Complexity        |
|------------------------|--------------------------------------------|-------------------------|
| Greedy Degree-Based     | O(M log N) (due to priority queue operations) | O(N + M)               |
| 2-Approximation         | O(N + M) (simple graph traversal)          | O(N + M)                |

---

## 2️⃣ Brute Force / Exact Solution using Bitmasking 🔍

For small graphs, the **only guaranteed way** to find the optimal vertex cover is to check *every possible subset* of vertices. This is called **brute force**, and it’s where bitmasking shines.

---

### What’s Bitmasking? 🤔

- Every subset of vertices can be represented as a binary number (bitmask), where:
  - The *i-th* bit represents whether the *i-th* vertex is included (`1`) or not (`0`).  
- For `N` vertices, there are `2^N` subsets (from `0` to `2^N - 1`).  
- We iterate through all these subsets (bitmasks), checking if that subset covers all edges.  
- The smallest such subset that covers all edges is our **minimum vertex cover**.

---

### Why `1 << N` ? 🧮

- The operator `<<` is a bitwise left shift.  
- `1 << N` means `1` shifted left by `N` positions — which equals `2^N`.  
- So, when we say `for mask in [0 .. (1 << N) - 1]`, we're iterating over all subsets of an N-element set!

---

### How it works in practice?

1. Loop over `mask` from `0` to `(1 << N) - 1`  
2. For each edge `(u, v)`, check if **at least one** of `u` or `v` is in the subset (mask)  
3. If all edges are covered, track the size of the subset and update the answer if smaller  
4. Output the minimum subset size found

---

##code  
```cpp
class Solution {
  public:
    int vertexCover(int n, vector<pair<int, int>> &edges) {
        int mincover = n + 1;  // Start with worst case: all vertices

        // Iterate over all possible subsets using bitmasking
        for (int mask = 0; mask < (1 << n); mask++) {

            unordered_set<int> coverset;

            // Build the current subset of vertices based on the bitmask
            for (int i = 0; i < n; i++) {
                if (mask & (1 << i)) {  // If the i-th bit is set
                    coverset.insert(i);
                }
            }

            bool allcovered = true;

            // Check if every edge is covered by at least one vertex in the subset
            for (auto it : edges) {
                it.first--;  // Convert 1-based to 0-based indexing
                it.second--;

                // ❗️Both u and v are NOT in the cover ⇒ this edge is uncovered
                if (coverset.count(it.first) == 0 && coverset.count(it.second) == 0) {
                    allcovered = false;
                    break;
                }
            }

            // If this subset covers all edges, update the minimum cover size
            if (allcovered) {
                mincover = min(mincover, (int)coverset.size()); // size() returns size_t (unsigned), so cast to int
            }
        }

        return mincover;
    }
};

```

## 💡 Common Bitmasking Mistakes & Clarifications

---

### 🔸 Why use `&&` instead of `||` when checking edge coverage?

You may be tempted to write:

    if (coverset.count(u) == 0 || coverset.count(v) == 0)

But that's **incorrect** ❌.

What we actually want is:

    if (coverset.count(u) == 0 && coverset.count(v) == 0)

**Why?**

- We're checking if an edge `(u, v)` is **not covered** by any vertex in the current subset.
- An edge is considered **uncovered** only if **both** `u` and `v` are missing from the set.
- If even **one** of them is in the set, the edge is **covered** ✅.
- So we use `&&` — only fail when both are absent.

---

### 🔸 Why cast `coverset.size()` to `(int)`?

In this line:

    mincover = min(mincover, (int)coverset.size())

You may wonder: isn't `.size()` already a number?

Yes, but it's a `size_t` (an **unsigned** type), whereas `mincover` is an `int`.

To avoid **type mismatch** warnings or bugs when comparing signed vs unsigned types, we do:

    (int)coverset.size()

---

### 🔸 Why subtract `1` from `it.first` and `it.second`?

Most competitive programming inputs are **1-based** — that is, vertices are numbered from 1 to N.

But bitmasking and arrays in C++ are **0-based**.

So before working with bit positions or accessing vectors, we do:

    it.first--;
    it.second--;

to convert them into 0-based indexing.

---

### 🔸 What does `(1 << n)` mean?

This is a **bitwise left shift**.  
`1 << n` means "1 shifted left by n bits" — which gives you `2ⁿ`.

For example:
- `1 << 0` = 1
- `1 << 3` = 8
- `1 << 5` = 32

This is used to loop through **all subsets** of a set with `n` elements:

    for (int mask = 0; mask < (1 << n); mask++)

Each value of `mask` from 0 to `2ⁿ - 1` represents a possible subset.

---

---

### Complexity of Bitmasking 🐢

| Method                 | Time Complexity              | Space Complexity          |
|------------------------|------------------------------|--------------------------|
| Brute Force (Bitmask)  | O(2^N * M) (check all subsets × all edges) | O(N + M)                  |

---

**Summary:**  
- Greedy & approximation methods are fast but might miss the smallest vertex cover.  
- Bitmask brute force is exact but only works for tiny graphs (N ≤ ~20).  
- Bitmasking elegantly leverages binary numbers to represent subsets and checks all possibilities systematically.

---


