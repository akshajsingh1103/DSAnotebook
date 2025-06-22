# Detect Duplicate Subtrees

**Link:** [GFG - Detect Duplicate Subtrees](https://www.geeksforgeeks.org/problems/duplicate-subtree-in-binary-tree/1)

---

## Problem Statement

Given a binary tree, find whether there exists a **duplicate subtree of size two or more**. A duplicate subtree means two or more identical subtrees present in the tree.

---

## Approach / Theory

To detect duplicate subtrees, we need to serialize every subtree and check how many times it occurs.

- A subtree is represented by a string like: `nodeValue,leftSubtree,rightSubtree`.
- If the same serialized string appears more than once, and it's not just a single leaf node, it's considered a duplicate.

---

## First Attempt (Naive)

### Code  
`## Code`
```cpp
/*The structure of the Binary Tree Node  is
struct Node
{
  char data;
  struct Node* left;
  struct Node* right;
};*/

class Solution {
  public:
    /*This function returns true if the tree contains
    a duplicate subtree of size 2 or more else returns false*/
    string dfs(Node* temp, unordered_map<string, int>&m){
        if(temp == nullptr) return "#";
        if(temp->right == nullptr && temp->left == nullptr) return to_string(temp->data);
        
        
        string c = "";
        c+=to_string(temp->data);
        string l = dfs(temp->left, m);
        string r = dfs(temp->right, m);
        
        c+= ","+ l + "," + r ;
        
        m[c]++;
        return c ;
        
    }
    int dupSub(Node *root) {
        unordered_map<string, int> m;
        string k = dfs(root, m);
        for(auto it : m){
            if(it.second>=2) return 1;
        }
        return 0;
    }
};
```

### Explanation

- Perform DFS on each node and build a **string representation** of every subtree.
- Store all serialized strings in a hash map.
- If any string (representing a subtree) appears more than once, return `true`.

### Why It Failed

- The **string concatenation cost** is what kills this version:
    - For each node, you build a string like: `"10,20,#,#,30,#,#"`
    - These strings can get long for deeper trees.
- At depth `d`, the string length can be `O(d)`
    - Leaf → O(1)
    - Parent of leaf → O(2)
    - ...
    - Root → O(n)
- Total cost becomes:  
  \[
  O(1 + 2 + 3 + \dots + n) = O(n^2)
  \]
- Hence, this version is **inefficient for large trees** due to **string copying overhead**.

---

## Final Solution (Optimized with Subtree Hashing)

### Code  
`## Code`
```cpp
class Solution {
  public:
    unordered_map<string, int> strToId;
    unordered_map<int, int> freq;
    int idCounter = 1;
    bool found = false;

    int dfs(Node* root) {
        if (!root) return 0;

        int left = dfs(root->left);
        int right = dfs(root->right);

        string key = to_string(root->data) + "," + to_string(left) + "," + to_string(right);

        int id;
        if (strToId.find(key) != strToId.end()) {
            id = strToId[key];
        } else {
            id = idCounter++;
            strToId[key] = id;
        }

        // Only count non-leaf subtrees
        if (root->left || root->right) {
            freq[id]++;
            if (freq[id] == 2) {
                found = true;
            }
        }

        return id;
    }

    int dupSub(Node *root) {
        strToId.clear();
        freq.clear();
        idCounter = 1;
        found = false;
        dfs(root);
        return found ? 1 : 0;
    }
};
```

### Explanation

- Instead of storing full subtree strings, we **hash them into integer IDs**.
- For each node, generate a **tuple-like key**:  
  `node.val + "," + left_id + "," + right_id`
- If this key is already seen, use the existing ID; otherwise, assign a new one.
- Count occurrences using the integer IDs.
- **Avoids large string concatenations** — now each subtree lookup is `O(1)`.

#### Improvements Over Naive:
- **String concatenation → replaced with hashing**
- **Time complexity:** O(n), since each subtree is serialized once and uses constant-time map lookups.
- **Space complexity:** O(n), for maps storing subtree IDs and frequencies.

---


