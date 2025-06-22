# Lowest Common Ancestor in a Binary Tree

**Link:** [GFG - Lowest Common Ancestor](https://www.geeksforgeeks.org/problems/lowest-common-ancestor-in-a-binary-tree/1)

---

## Problem Statement

Given a binary tree and two nodes `n1` and `n2`, find their Lowest Common Ancestor (LCA).

The LCA is defined as the lowest node in the tree that has both `n1` and `n2` as descendants (where a node can be a descendant of itself).

---

## Approach / Theory

- Use DFS to search for `n1` and `n2` in the tree.
- Recursively check left and right subtrees.
- If both left and right recursive calls return non-null pointers, the current node is the LCA.
- Otherwise, propagate the non-null result upwards.

---

## Why does the first common node have both children non-null?

- For a node to be the LCA, one node must be found in its left subtree and the other in its right subtree.
- When the recursive calls return, the first node where **both** left and right recursive calls are **non-null** indicates that `n1` and `n2` have been found in separate subtrees.
- Any node above that will have only one side returning non-null because both nodes have already been "covered" by the LCA.
- Nodes below the LCA will return null for one side or both sides until the target nodes are found.

---

## Code

```cpp
/* A binary tree node

struct Node
{
    int data;
    struct Node* left;
    struct Node* right;

    Node(int x){
        data = x;
        left = right = NULL;
    }
};
 */

class Solution {
  public:
    // Function to return the lowest common ancestor in a Binary Tree.
    Node* dfs(Node* temp, int n1, int n2){
        
        if(temp == nullptr) return nullptr;
        
        if(temp->data == n1){
            return temp ;
        }if(temp->data == n2){
            return temp ;
        }
        
        
        
        Node* l = dfs(temp->left, n1, n2);
        Node* r = dfs(temp->right, n1, n2);
        
        if(l!=nullptr && r!=nullptr){
            return temp;
        }else if(l!=nullptr && r == nullptr){
            return l;
        }else if(l==nullptr && r != nullptr){
            return  r;
        }else{
            return nullptr ;
        }
        
        
        
    }
    Node* lca(Node* root, int n1, int n2) {
        return dfs(root, n1, n2);
        
    }
};
```

---

## Explanation

- Start DFS from the root.
- If current node matches either `n1` or `n2`, return current node.
- Recur left and right.
- If both left and right return non-null, current node is LCA.
- Else, return whichever is non-null.
- If both are null, return null.

---

## Complexity

- Time: O(n), where n = number of nodes.
- Space: O(h), where h = height of the tree (recursion stack).

---

## Tags

- Tree
- DFS
- Recursion
- Lowest Common Ancestor


