# Kth Ancestor in a Binary Tree

**Link:** [GFG - Kth Ancestor in a Tree](https://www.geeksforgeeks.org/problems/kth-ancestor-in-a-tree/1)

---

## Problem Statement

Given a binary tree and a node value, find its **k-th ancestor** in the tree.  
If there is no such ancestor, return -1.

---

## Theory & Approach

- Perform a DFS to locate the target node.
- While returning back (unwinding recursion), decrease `k`.
- When `k` becomes zero, the current node is the k-th ancestor.
- Make sure to avoid decrementing `k` before the target node is actually found.

---

## Early Mistakes

- Checked `k == 0` **before** recursion, missing the target node completely.
- Decremented `k` at every level, even before finding the node, causing premature results.
- Didn’t handle return values correctly from left/right recursive calls.

---

## Corrected Code

## Code
```cpp
class Solution {
  public:
    Node* func(Node* temp, int target, int &k) {
        if (temp == nullptr) return nullptr;
        
        if (temp->data == target) {
            return temp;
        }
        
        Node* l = func(temp->left, target, k);
        Node* r = func(temp->right, target, k);
        
        if (l == nullptr && r == nullptr) return nullptr;
        
        if(k==0) return l?l:r ;
        
        if(k>0){
            k--;
            return temp ;
        }

    }

    int kthAncestor(Node *root, int k, int node) {
        Node* t = func(root, node, k);
        if(k==0 && t){
            return t->data ;
        }return -1 ;
        
    }
};

```

---

## Explanation

- Search left and right subtree recursively.
- When target node is found, return it.
- On returning back, decrement `k`.
- When `k` is zero, current node is the k-th ancestor.
- Return the found ancestor node upwards.

---

## Complexity

- Time: O(n), where n = number of nodes (DFS traversal).
- Space: O(h), h = height of the tree (recursion stack).

---

## Tags

- Tree
- DFS
- Recursion
- Ancestor Tracking


