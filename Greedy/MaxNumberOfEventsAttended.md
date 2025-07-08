# 🗓️ Max Events Attended Problem – Explanation & Optimization

[Problem Link](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/description/?envType=daily-question&envId=2025-07-07)

## ❗ The Problem

You're given a list of events, each with a start and end day. You can attend **only one event per day**, and for each event, you can choose **any day** in its interval to attend.

Your goal is to attend as **many events as possible**.

---

## 🐢 Why the Original Code Was Too Slow (TLE)

```cpp
sort(events.begin(), events.end(), cmp());
for(auto e:events){
    for(int i = e[0]; i<=e[1]; i++){
        if(!used[i]){
            used[i]=true ;
            c++;
            break;
        }
    }
}
```

### ❌ Problem:

For each event, you're checking **every single day** from `start` to `end` to find an unused one.

- Worst-case time complexity: **O(N * D)**  
  (N = number of events, D = number of days)
- This becomes very slow if many events span large day ranges.

---

## ⚡ Optimized Method: Greedy + Union-Find (Disjoint Set)

We want to **quickly find the next available day** for an event using Disjoint Set Union (DSU).

### ✅ Why It’s Better

- Instead of linearly checking every day between `start` and `end`,
- DSU allows you to **jump directly to the next available day**.
- After using a day, we **union** it with the next day, so next time we skip over used days automatically.

---

### 🔧 How It Works

1. **Sort** events by their end day (greedy: try to schedule early-ending events first).
2. For each day, store a `parent[i]` = i, meaning it is available.
3. Use **`find(i)`** to get the earliest available day starting from `i`.
4. When a day is used, **`unite(i, i+1)`** so future lookups skip it.

---

### 🧠 DSU Functions

```cpp
int find(int x) {
    if (parent[x] != x)
        parent[x] = find(parent[x]); // path compression
    return parent[x];
}

void unite(int x, int y) {
    parent[x] = y;
}
```

---

## 🧩 C++ Code (Paste Here)

```cpp
// Paste the optimized DSU code here
class Solution {
public:
    vector<int> parent;

    int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]); // path compression
        return parent[x];
    }

    void unite(int x, int y) {
        parent[x] = y;
    }

    int maxEvents(vector<vector<int>>& events) {
        sort(events.begin(), events.end(), [](auto& a, auto& b) {
            return a[1] < b[1]; // sort by end day
        });

        int max_day = 0;
        for (auto& e : events)
            max_day = max(max_day, e[1]);

        parent.resize(max_day + 2); // include max_day + 1
        for (int i = 0; i <= max_day + 1; ++i)
            parent[i] = i;

        int count = 0;
        for (auto& e : events) {
            int available_day = find(e[0]);
            if (available_day <= e[1]) {
                count++;
                unite(available_day, available_day + 1);
            }
        }
        return count;
    }
};
```
---
