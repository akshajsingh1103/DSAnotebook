# 🧩 Binary Tree Diameter — Debug and Fix Notes

[Problem Link](https://www.geeksforgeeks.org/problems/diameter-of-binary-tree/1)

## 📌 Problem Statement

Given the root of a binary tree, return the **diameter of the tree** — the number of **nodes** on the longest path between any two nodes.

---

## ❌ Original Attempt (Inefficient Approach)


### code
```cpp
//1) wrong approach just adding up the height of ther right and left wont account for the max diameter because It only checks the diameter passing through the root — but that’s not guaranteed to be the max.
             1
            /
           2
          / \
        3    5
       /      \
      4        6

// HERE ACTUAL diameter max is 4 but its not thru the root node so this approach is wrong

int func(Node * temp){
        if(temp == nullptr) return 0 ;
        
        int right = func(temp->right);
        int left = func(temp->left);
        
        return max(left, right) + 1 ;
    }
    int diameter(Node* root) {
        int right = func(root->right);
        int left = func(root->left);
        return right+left ;

---
//Now the inefficient approach is this

int d(Node* temp){
        if(temp== nullptr) return 0;
        int l = d(temp->left);
        int r = d(temp->right);
        int both = func(temp->left) + func(temp->right);
        return max(l,max(r,both));
    }
    int func(Node * temp){
        if(temp == nullptr) return 0 ;
        
        int right = func(temp->right);
        int left = func(temp->left);
        
        return max(left, right) + 1 ;
    }
    int diameter(Node* root) {
        
        return d(root) ;
        
    }
    // why this is wrong is stated below
        
```
---

### 🔍 Explanation of the Original Logic

The original approach defines two functions:

- `func()` — Computes the **height** of a node recursively.
- `diameter()` — Computes:
  - The **diameter of the left** and **right** subtrees recursively.
  - `func(left) + func(right)` to account for the **path through the current node**.

The logic is correct in theory — take the maximum of:
- Left subtree diameter
- Right subtree diameter
- Diameter through current node

#### 🚨 The Issue:
- `func()` is **repeatedly called** on the same subtrees.
- Results in **redundant height calculations**.

### 📉 Time and Space Complexity (Original Code)

- **Time Complexity**: O(N²) in the worst case (e.g., skewed trees)
- **Space Complexity**: O(H) (due to recursion stack; H = height of the tree)

---

## ✅ Optimized Approach (One-Pass DFS)

### code 
```cpp
int maxd=0;
    int func(Node * temp){
        if(temp == nullptr) return 0 ;
        
        int right = func(temp->right);
        int left = func(temp->left);
        maxd= max(maxd, left+right);
        return max(left, right) + 1 ;
    }
    int diameter(Node* root) {
        func(root);
        return maxd  ;
        
    }

```
---

### ⚙️ Explanation of the Optimized Logic

Use a **single DFS traversal** that:

- Calculates the **height** of each subtree.
- While doing so, **updates a reference variable** tracking the maximum diameter.

At each node:

int left_height = dfs(node->left);  
int right_height = dfs(node->right);  
diameter = max(diameter, left_height + right_height);

This avoids repeated height calculations.

### 📈 Time and Space Complexity (Optimized Code)

- **Time Complexity**: O(N) — Each node is visited **exactly once**.
- **Space Complexity**: O(H) — Due to recursion stack (H is tree height)

---

## ✅ Summary

| Version       | Time Complexity | Space Complexity | Notes                                |
|---------------|------------------|------------------|----------------------------------------|
| Original      | O(N²)            | O(H)             | Redundant height computations          |
| Optimized DFS | O(N)             | O(H)             | Single pass using post-order traversal |

