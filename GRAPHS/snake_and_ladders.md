# Snakes and Ladders (Leetcode 909)

[Problem Link](https://leetcode.com/problems/snakes-and-ladders/)

---

## Problem Summary

Given an `n x n` board representing a snakes and ladders game, numbered in a zigzag manner from bottom-left to top-right, find the minimum number of dice rolls to reach the last square (`n²`). Ladders and snakes can jump you to other squares, modifying your moves.

---

## Mistakes & Lessons Learned

1. **Incorrect coordinate calculation using `row % 2` instead of `quot % 2`:**  
   The zigzag numbering depends on the row's index from the **bottom**, not the top. Using `row % 2` caused wrong column calculations.


2. **Marking visited cells too late:**  
   Marking nodes as visited **after dequeuing** instead of **upon enqueueing** led to multiple enqueue operations on the same cell, causing TLE (Time Limit Exceeded).

3. **Attempting DFS approach:**  
   Tried DFS which explores all possible paths. DFS here would be **exponential in the worst case** (`O(6^(n²))`), since from each cell you can roll 1-6 steps, and this is impractical for larger boards.

---

## Why not DFS?

- DFS explores all possible dice rolls recursively and tries all paths.
- The branching factor is up to 6 at each square.
- For an `n x n` board (with `n²` cells), the complexity is roughly **`O(6^{n²})`** in the worst case — clearly infeasible.
- DFS also cannot guarantee the **minimum dice rolls** to reach the end because it may get stuck going deep in non-optimal paths.

---

## BFS Approach & Complexity

- BFS guarantees the **shortest path** in an unweighted graph — here, each dice roll represents one step.
- Model each square as a node in a graph.
- From each node, edges exist to up to 6 next nodes (dice rolls).
- Take ladders/snakes into account by jumping directly to destination squares.
- Keep track of visited nodes to avoid cycles and repeated visits.

### Time Complexity:

- **`O(n²)`**, because each square is visited **at most once**.
- Each node checks up to 6 neighbors → **`O(6 * n²) = O(n²)`**

### Space Complexity:

- **`O(n²)`** for the visited array and queue.

---

## C++ Implementation

```cpp
class Solution {
public:
    pair<int, int> getCoords(int s, int n){
        int quot = (s-1)/n ;
        int rem = (s-1)%n;
        int row = n-1 - quot ;
        // int col = (row%2==0)? n-1-rem : rem;
        int col = (quot % 2 == 0) ? rem : n - 1 - rem;
        return {row,col};
    }
    int snakesAndLadders(vector<vector<int>>& board) {
        int n = board.size();
        queue<pair<int, int>> q ;
        q.push({1, 0});
        vector<int> vis(n*n +1, 0);
        vis[1] = 1 ;
        while(!q.empty()){
            auto it = q.front();
            q.pop();
            if(it.first == n*n) return it.second ;
            // if(vis[it.first]) continue ;
            // if(vis[newcell]) continue ;  here it causes tle cause we keep on enqueuing same nodes again and again and its the same as bruteforce dfs
            for(int i = 1 ; i<=6 ; i++){
                int newcell = it.first + i ;
                if( newcell > n*n) break ;
                auto itnew = getCoords(newcell, n);
                int row =  itnew.first;
                int col = itnew.second;
                if(board[row][col] != -1){
                    newcell = board[row][col] ;
                }
                
                if(vis[newcell]) continue ;
                vis[newcell] = true ;
                q.push({newcell, it.second+1});
            }
        }return -1 ;

    }
};




