# 🛫 Cheapest Flights Within K Stops - Key Takeaways
[problem link](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

---

## ❌ My Initial Mistakes

- ✅ Used `BFS`, but:
  - ❌ Didn't guard the queue with a `dist[neighbour] > curr_cost + weight` condition.
  - ❌ Assumed using a `priority_queue` (Dijkstra) would be better without understanding step constraints.
- ❌ Used a 1D `dist[]` array even with `priority_queue`, which led to:
  - Overwriting valid lower-stop paths with higher-stop cheaper paths (invalid under `k` constraint).
- ❌ Thought tracking just `cost` per node would be enough — ignored the effect of `stops`.

---

## ✅ What Works (with BFS)

- Use **`queue` (BFS)** instead of `priority_queue`.
- Track:
  - `node`
  - `cost so far`
  - `stops taken so far`
- BFS guarantees:
  - Nodes are processed in **order of increasing stops**.
  - First visit to a node will always be via **least steps** possible.

---

## 🧠 Why This Is NOT Dijkstra

| BFS                              | Dijkstra (priority_queue)                |
|----------------------------------|------------------------------------------|
| Processes by **number of stops** | Processes by **lowest cost**             |
| Can use `dist[node]`             | ❌ Needs `dist[node][stops]`             |
| First visit = fewest stops       | First visit = cheapest cost (not always valid) |
| ✅ Good for bounded `k` stops    | ❌ Risky unless cost + steps tracked     |

---

## 🧠 Key Insight

> In BFS, even if a **cheaper path** to a node is found later with **more stops**, it's okay to skip it —  
> because a more "step-efficient" path has already been processed (and BFS guarantees that came first).

---

## ✅ Best Practices

### ✅ Do
- Use BFS when constrained by steps/stops (`k`).
- Use `dist[node]` **only if** you're sure fewer-stop versions get processed first.
- Track stops in the queue state.
- Use a 2D `dist[node][stops]` **if** using a priority queue.

### ❌ Don't
- Don't use 1D `dist[]` with Dijkstra-style priority queue — it will overwrite valid shorter-stop paths.
- Don’t push a node into the queue without checking if you’ve found a better or valid way to get there.

---

## 📈 Time & Space Complexity

### Time Complexity:
- `O(E)` in the best BFS implementation
- Up to `O(n * k)` if using 2D `dist[node][stops]` and all paths are explored

### Space Complexity:
- BFS with 1D `dist[]`: `O(n)`
- With 2D `dist[node][stops]`: `O(n * k)`
- Queue space: also up to `O(n * k)` in worst case

---

## 💻 Code

## Code

```cpp
class Solution {
public:
    struct cmp{
        bool operator()(pair<int,pair<int, int>>& a,pair<int,pair<int, int>>& b ){
            return a.first > b.first ;
        }
    };

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {

        vector<vector<pair<int, int>>> adj(n);

        for(auto it: flights){
            adj[it[0]].push_back({it[1], it[2]}) ;
        }

        vector<int> dist(n+1, INT_MAX) ;

        queue<pair<int, pair<int, int>>> pq ;

        dist[src] = 0 ;
        pq.push({0,{src,0}});

        

        while(!pq.empty()){

            auto curr = pq.front();
            pq.pop();
        
            for(auto it : adj[curr.second.first]){

                if(curr.second.second<=k){

                    if(it.first == dst){
                        dist[it.first] = min(dist[it.first], curr.first + it.second); 
                    }else{
                        if(dist[it.first]>curr.first+it.second){
                            dist[it.first]= curr.first+it.second;
                            pq.push({curr.first+it.second,{it.first, curr.second.second + 1}});
                        }
                    }
                }
            }
        }return (dist[dst] == INT_MAX)?-1:dist[dst] ;
    }
};
```
