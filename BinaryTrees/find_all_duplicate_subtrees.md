# Find All Duplicate Subtrees

**Link:** [GFG / LeetCode - Find Duplicate Subtrees](https://www.geeksforgeeks.org/problems/duplicate-subtrees/1)

---

## Problem Statement

Given the root of a binary tree, return all duplicate subtrees.  
For each kind of duplicate, only one node (the root of the duplicate subtree) should be returned.

A duplicate subtree is defined as a subtree that appears **more than once** in the tree — i.e., two subtrees with the same structure and node values.

---

## Approach / Theory

This problem is almost identical to the one where we **only check whether a duplicate exists**.

🔗 **See reference:** [Detect Duplicate Subtrees](./detect_duplicate_subtrees.md)

### Key difference:
- Instead of returning `1` or `0`, we **return the root nodes of all such duplicate subtrees**.
- The mechanism for detecting duplicates is the same — serialize each subtree uniquely, and track frequency.

---

## Final Solution (Optimized with Subtree Hashing + Node Collection)

### Code  
`## Code`

```cpp
class Solution {
  public:

    unordered_map<string, int> strToId;
    unordered_map<int, int> freq;
    int idCounter = 1;
    vector<Node*>ans ;

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

        freq[id]++;
        if (freq[id] == 2) {
            ans.push_back(root);
        }

        return id;
    }

    vector<Node*> printAllDups(Node* root) {
        strToId.clear();
        ans.clear();
        freq.clear();
        idCounter = 1;
        dfs(root);
        return ans ;
        
    }
};
```

### Explanation

- Serialize each subtree with a key like:  
  `node.val,left_id,right_id`
- Store each subtree key in a map → assign unique IDs
- Track how often each subtree ID appears using a frequency map.
- If frequency hits `2`, add the current node to the result list `ans`
  - (Only on `== 2`, to avoid duplicates in the result list)

### Complexity

- **Time:** O(n), each node visited once, and string operations are constant via hashing
- **Space:** O(n), for maps and result list

---

## Relation to Similar Problems

This is an **extension** of:

📁 [Detect Duplicate Subtrees](./detect_duplicate_subtrees.md)

Both use the same technique: subtree serialization + hashing. The only difference is the output.

---


