# Chinese Postman Problem (CPP) — Detailed Explanation and Approach

[Problem Link](https://www.geeksforgeeks.org/problems/chinese-postman/1)

---

## 📌 Problem Statement

Given a **connected, undirected weighted graph** with `n` nodes and edges, find the **minimum cost** to traverse **all edges at least once** and return to the starting point.  
This is also called the **Route Inspection Problem** or **Chinese Postman Problem**.

---

## 🔑 Key Insight: Degrees and Eulerian Paths

- If the graph is **Eulerian** (i.e., **all nodes have even degree**),  
  → The answer is simply the **sum of all edge weights** because you can traverse every edge exactly once in a cycle.

- Otherwise, some nodes have **odd degree**, and an Eulerian circuit is **not directly possible**.

> **Why are odd degree nodes a problem?**  
> Every time you enter a node via one edge, you must leave via another to maintain a cycle.  
> Odd degree means there is a mismatch — the node is either missing an edge in or out.

---

## ⚙️ Fixing Odd Degree Nodes by Pairing

To make the graph Eulerian, we must **add edges** (possibly duplicates of existing edges) to make **all degrees even**.

- Important fact:  
  **The number of odd degree nodes is always even.**

- We must **pair these odd nodes** and add paths between each pair to balance their degrees.

---

## 💡 Why Pair Odd Nodes?

Each additional path added between two odd nodes increases their degrees by 1, making **both even**.

Thus, the problem reduces to:

> **Find the minimum sum of shortest paths to pair all odd nodes.**

---

## 🔢 How Many Ways to Pair Odd Nodes?

Suppose there are `k` odd nodes.

The number of ways to pair them up into disjoint pairs (perfect matchings) is:

\[
(k-1) \times (k-3) \times (k-5) \times \ldots \times 1
\]

**Example:** For \( k=4 \), the number of pairings is:  
- Choose partner for node 1: 3 choices  
- For the remaining two nodes: 1 choice  
- **Total:** \( 3 \times 1 = 3 \) pairings

---

## 🚀 Using Floyd-Warshall for Shortest Paths

- Floyd-Warshall computes shortest distances between **all pairs of nodes**.
- This allows connecting any odd node to another through the shortest path, which **may pass through even-degree nodes**.
- Hence, the added path **doesn't need to be a direct edge**, but the minimal cost path between them.

---

## 🧩 Bitmask + DP for Minimum Pairing Cost

We solve the minimal pairing cost using:

- **Bitmask:** Represents which odd nodes have been paired (`1`) or not (`0`).
- **Recursive DP:**  
  - Find the first unpaired odd node `i`.  
  - Pair it with all other unpaired nodes `j > i`.  
  - Recurse with updated mask and sum the pairing cost.  
  - Memoize results to avoid recomputation.

This approach **efficiently explores all perfect matchings**.

---

## 📊 Time and Space Complexity

- Let \( k \) = number of odd nodes (always even).  
- Number of DP states = \( 2^k \).  
- For each state, up to \( O(k) \) pairings checked, so overall:  
  \[
  O(k^2 \times 2^k)
  \]

- Usually, \( k \) is small compared to \( n \), so this is feasible.

- **Space complexity:** \( O(2^k) \) for memoization.

---

## 📝 Summary of the Approach

1. Calculate degrees of nodes and sum of all edges.
2. Identify all odd degree nodes.
3. Use Floyd-Warshall to get shortest paths between every pair.
4. Use bitmask DP to find the minimum cost to pair all odd nodes.
5. Return **sum of edges + minimum pairing cost**.

---

## 💻 Code Placeholder

```cpp
// Recursive function to find the minimum cost to pair all odd degree nodes
int mincost(int mask, vector<vector<int>>& dist, int k, vector<int>& odd, vector<int>& dp) {
    // Base case: if all odd nodes are paired (mask bits all set), cost is 0
    if (mask == (1 << k) - 1) return 0;

    // If result for this mask is already computed, return it to avoid recomputation
    if (dp[mask] != -1) return dp[mask];

    int ans = INT_MAX;
    int i;
    
    // Find the first odd node that is not paired yet (bit is 0)
    for (i = 0; i < k; i++) {
        if (!(mask & (1 << i))) break;
    }

    // Try pairing this unpaired node `i` with any other unpaired node `j > i`
    for (int j = i + 1; j < k; j++) {
        if (!(mask & (1 << j))) {
            // Create new mask that marks nodes i and j as paired
            int newmask = mask | (1 << i) | (1 << j);

            // If no path exists between odd[i] and odd[j], skip this pairing
            if (dist[odd[i]][odd[j]] == 1e9) continue;

            // Calculate cost: shortest path between these two nodes + cost of pairing rest
            int cost = dist[odd[i]][odd[j]] + mincost(newmask, dist, k, odd, dp);

            // Choose the minimum pairing cost
            ans = min(ans, cost);
        }
    }

    // Memoize and return the result for this mask
    return dp[mask] = ans;
}

// Main function to solve the Chinese Postman Problem
int chinesePostmanProblem(vector<vector<int>>& e, int n) {
    vector<int> degree(n + 1, 0);  // Track degree of each node
    vector<vector<int>> dist(n + 1, vector<int>(n + 1, 1e9));  // Distance matrix initialized to "infinity"
    int sum = 0;  // Sum of all edges weights

    // Build degree array and initialize direct distances from edges
    for (auto& it : e) {
        int u = it[0], v = it[1], w = it[2];
        degree[u]++;
        degree[v]++;
        sum += w;
        dist[u][v] = min(dist[u][v], w);
        dist[v][u] = min(dist[v][u], w);
    }

    // Distance from a node to itself is zero
    for (int i = 1; i <= n; i++) dist[i][i] = 0;

    // Floyd-Warshall to compute shortest distances between all pairs of nodes
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                if (dist[i][k] != 1e9 && dist[k][j] != 1e9)  // Avoid integer overflow
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);

    // Collect all nodes with odd degree
    vector<int> odd;
    for (int i = 1; i <= n; i++) {
        if (degree[i] % 2 != 0)
            odd.push_back(i);
    }

    // If no odd degree nodes, graph is Eulerian: just return sum of all edges
    if (odd.empty()) return sum;

    // Use DP with bitmasking to find minimal pairing cost of odd nodes
    vector<int> dp((1 << odd.size()), -1);

    // Return total cost = original edges sum + minimal extra cost to pair odd nodes
    return sum + mincost(0, dist, odd.size(), odd, dp);
}

```

