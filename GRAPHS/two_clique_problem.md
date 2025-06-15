# 🧩 Two Cliques Problem – Detailed Explanation

---

## 📝 Problem Statement

Given a simple **undirected** graph \( G \), determine if it is possible to divide its vertices into **two disjoint cliques**.

> A **clique** is a subset of nodes where **every pair of nodes is connected** by an edge.

---

## 🧠 Key Insight: Use the Complement Graph

Let:
- \( G \) be the original graph.
- \( G' \) be the **complement graph** where:
  - An edge exists in \( G' \) **iff** it does **not exist** in \( G \).

---

## 🤔 What's the Connection?

We want to split \( G \) into two cliques \( C_1 \) and \( C_2 \), where:

- Every node inside \( C_1 \) is connected to every other node in \( C_1 \).
- Same for \( C_2 \).

This means:
- There are **no missing edges** inside \( C_1 \) or \( C_2 \).

That implies:
- In the **complement graph \( G' \)**, there should be **no edges inside** \( C_1 \) or \( C_2 \).

But that’s exactly the definition of a **bipartite graph**!

> A graph is **bipartite** if its vertex set can be split into two disjoint sets \( A \) and \( B \), such that **no edge exists between any two vertices in the same set**.

Therefore:

> ✅ **G has two cliques ⟺ G' is bipartite**

---

## 💡 Summary

| Graph Type        | Method Applies? | Reason |
|-------------------|------------------|--------|
| Undirected Graph  | ✅ Yes            | Complement trick works |
| Directed Graph    | ❌ No             | Directionality breaks symmetry |

In a **directed graph**, the idea of a clique is asymmetric (edges must exist in both directions), so the complement-bipartite trick does **not** work.

---

## 💻 Code Placeholder


Reverse the edges of the given graph and use this method to find if its bipartite or not (can be colored with 2 colors or not)

```cpp
class Solution {
  public:
    bool dfs(int node, vector<vector<int>>&adj, vector<int>&color){
        // if(total == adj.size()) return true ;
        
        for(auto it :adj[node]){
            if(color[it]==-1){
                color[it] = 1- color[node] ;
                if(!dfs(it, adj, color)){
                    return false ;
                }
            }else if(color[it]==color[node]) return false ;
        }return true ;
    }
    bool isBipartite(int V, vector<vector<int>> &edges) {
        // Code here
        vector<vector<int>> adj(V);
        vector<int> color(V, -1) ;
        for(auto it: edges){
            adj[it[0]].push_back(it[1]);
            adj[it[1]].push_back(it[0]);
        }
        color[0]=0 ;
        // for(int i = 0 ; i<V ; i++){
            // if(color[i]==-1){
        if(dfs(0, adj, color)) return true ;
            // }
            // 
        // }
        return false ;
    }
};
```
---

## ⏱ Time Complexity

- Constructing the complement graph: \( O(n^2) \)
- Checking bipartiteness via BFS/DFS: \( O(n + m') \), where \( m' \) is the number of edges in the complement graph.

---

## 📌 Conclusion

To solve the **two cliques problem**:

1. Build the **complement graph** of the original undirected graph.
2. Check if it is **bipartite**.
3. If yes, the original graph can be divided into two cliques.


