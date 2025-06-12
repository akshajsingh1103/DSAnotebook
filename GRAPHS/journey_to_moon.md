# 🌙 Journey to the Moon

Determine the number of valid astronaut pairs that can be formed from different countries. A classic graph problem involving connected components.

---

## 🔗 Problem Link

<!-- Paste the HackerRank problem URL here -->
[https://www.hackerrank.com/challenges/journey-to-the-moon/problem]

---

## 💡 Problem Summary

You're given `n` astronauts and a list of astronaut pairs who belong to the same country. Your task is to compute the number of ways to choose two astronauts such that they belong to different countries.

This problem maps naturally to a graph problem, where each astronaut is a node, and pairs form edges. The size of each connected component corresponds to the size of one country.

---

## ❌ Mistakes I Made

- **Used `int` to store large result values**
  - Even though all internal calculations used `long long`, I mistakenly assigned the return value of the function to an `int` in `main()`. This silently overflowed the output.

- **Mixed `int` and `long long` types**
  - In the final pair calculation loop, I used `int size` which caused truncation when multiplying large component sizes. Should have used `long long`.

- **Left `cout` debug prints inside submission**
  - This caused unexpected output and failed test cases on HackerRank.

- **Initially used an inefficient O(n²) nested loop**
  - For calculating total valid pairs, which was later optimized to O(n) using a prefix sum approach.

---

## 💻 Code

```cpp
void dfs(int node, vector<vector<int>>& adj, vector<int>& vis, long long &count) {
    vis[node] = true;
    for (int neighbor : adj[node]) {
        if (!vis[neighbor]) {
            ++count;
            dfs(neighbor, adj, vis, count);
        }
    }
}

long long journeyToMoon(int n, vector<vector<int>> astronaut) {
    vector<vector<int>> adj(n);
    vector<int> vis(n, 0);

    for (const auto& pair : astronaut) {
        adj[pair[0]].push_back(pair[1]);
        adj[pair[1]].push_back(pair[0]);
    }

    vector<long long> components;
    for (int i = 0; i < n; ++i) {
        if (!vis[i]) {
            long long count = 1;
            dfs(i, adj, vis, count);
            components.push_back(count);
        }
    }

    long long total = accumulate(components.begin(), components.end(), 0LL);
    long long result = 0;
    for (long long size : components) {
        total -= size;
        result += size * total;
    }

    return result;
}

```

---

## 🧠 Time & Space Complexity

### Time Complexity:

| Step                                  | Time Complexity |
|---------------------------------------|-----------------|
| Building adjacency list               | O(P)            |
| DFS traversal to find components      | O(N + P)        |
| Counting valid inter-country pairs    | O(C)            |
| **Total**                             | **O(N + P)**    |

Where:
- `N` = total number of astronauts
- `P` = number of same-country pairs
- `C` = number of connected components (≤ N)

---

### Space Complexity:

| Structure               | Space Complexity |
|------------------------|------------------|
| Adjacency List         | O(N + P)         |
| Visited array          | O(N)             |
| Component sizes list   | O(C)             |
| **Total**              | **O(N + P)**     |

---

## 🧪 Test Case Example

### Input:

100000 2
1 2
3 4


### Output:
4999949998


### Explanation:
- Component Sizes: [2, 2, 1, 1, ..., 1] → 99,996 isolated nodes
- Total possible astronaut pairs: `n * (n - 1) / 2 = 4999950000`
- Subtract intra-country pairs: `1 (from 1-2) + 1 (from 3-4) = 2`
- Final valid inter-country pairs: `4999950000 - 2 = 4999949998`

---

## ✅ Final Notes

- Be extremely cautious with **integer limits** when dealing with large numbers.
- Always check the default return types in provided templates (especially on platforms like HackerRank).
- Consider using `long long` consistently in graph problems involving large combinations.
- For optimal performance, avoid nested loops when you can achieve the same with prefix sums or math tricks.
