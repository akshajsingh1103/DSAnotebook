# Traveling Salesman Problem (TSP) — DFS vs DP with Bitmasking
[Problem Link](https://www.geeksforgeeks.org/problems/travelling-salesman-problem2732/1)
---

## Problem Summary

Given a cost matrix `cost` where `cost[i][j]` is the cost to travel from city `i` to city `j`, find the minimum cost to visit every city exactly once and return to the starting city (city 0).

---

## Original DFS Solution (Leads to TLE)

```cpp
class Solution {
public:
    int dfs(vector<vector<int>> &adj, vector<vector<int>>& cost, vector<int>& vis, int node, int total, int &totalnodes, int c, int &minCost, int src) {
        if (total == totalnodes) {
            for (auto it : adj[node]) {
                if (it == src) {
                    c += cost[node][src];
                    minCost = min(c, minCost);
                }
            }
            return minCost;
        }
        
        for (auto it : adj[node]) {
            if (!vis[it]) {
                vis[it] = true;
                minCost = min(minCost, cost[node][it] + dfs(adj, cost, vis, it, total + 1, totalnodes, c + cost[node][it], minCost, src));
                vis[it] = false;
            }
        }
        return minCost;
    }

    int tsp(vector<vector<int>>& cost) {
        int n = cost.size();
        vector<vector<int>> adj(n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                adj[i].push_back(j);
            }
        }
        vector<int> vis(n, 0);
        vis[0] = true;
        int mincst = INT_MAX;
        
        // Floyd-Warshall is removed due to redundancy (see explanation below)
        return dfs(adj, cost, vis, 0, 1, n, 0, mincst, 0);
    }
};
```
## Mistakes & Lessons Learned

- **TLE (Time Limit Exceeded) Issue:**  
  The pure DFS explores all permutations of cities — which is factorial in complexity (O(n!)) — and becomes infeasible for larger numbers of cities.

- **Floyd-Warshall Redundancy:**  
  Initially, Floyd-Warshall was used to compute shortest paths between all city pairs. However, in TSP, you must visit each city exactly once, so shortcuts created through intermediate nodes violate this rule and mislead the DFS by artificially lowering path costs. Hence, Floyd-Warshall preprocessing is **redundant** in this context.

- **DP Subproblem Reuse Misunderstanding:**  
  Contrary to some beliefs, many subproblems repeat in TSP due to different routes leading to the same state `(city, visitedMask)`. Using memoization (DP) helps avoid recalculating these states multiple times, drastically reducing computation from factorial to exponential with a manageable base (O(n² * 2^n)).

---

## Bitmask DP Solution (Correct & Efficient)

```cpp
class Solution {
public:
    // dp[city][mask] = minimum cost to complete visiting all cities starting from `city` having visited cities in `mask`
    int solveTSP(vector<vector<int>>& dp, int city, int mask, vector<vector<int>>& cost, int n) {
        // Base case: all cities visited, return cost to return to start city (0)
        if (mask == (1 << n) - 1) {
            return cost[city][0];
        }
        
        if (dp[city][mask] != -1) {
            return dp[city][mask];  // Return cached answer
        }
        
        int ans = INT_MAX;
        
        // Try all unvisited cities as next steps
        for (int next = 0; next < n; next++) {
            // Check if city `next` is unvisited using bitwise AND '&'
            if ((mask & (1 << next)) == 0) {
                int newMask = mask | (1 << next);  // Mark `next` as visited using bitwise OR '|'
                int newCost = cost[city][next] + solveTSP(dp, next, newMask, cost, n);
                ans = min(ans, newCost);
            }
        }
        
        return dp[city][mask] = ans;  // Memoize and return
    }

    int tsp(vector<vector<int>>& cost) {
        int n = cost.size();
        vector<vector<int>> dp(n, vector<int>(1 << n, -1));
        return solveTSP(dp, 0, 1, cost, n);
    }
};
```
## Explanation of Bitmask Operations

- `mask & (1 << next)`  
  Checks if the `next` city is already visited in the current bitmask.

  - If the result is `0`, city `next` is **not visited** yet.
  - If non-zero, city `next` is **already visited**.

- `mask | (1 << next)`  
  Marks city `next` as visited by setting the corresponding bit in the bitmask.

---

## Time and Space Complexity

| Approach   | Time Complexity         | Space Complexity               |
|------------|-------------------------|-------------------------------|
| DFS        | O(n!) — factorial growth| O(n) for recursion stack      |
| Bitmask DP | O(n² * 2^n)             | O(n * 2^n) for DP memoization |

