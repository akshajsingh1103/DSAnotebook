# Minimum Distance Between Two Nodes in a Binary Tree

**Link:** [GFG - Minimum Distance Between Two Nodes](https://www.geeksforgeeks.org/problems/min-distance-between-two-given-nodes-of-a-binary-tree/1)

---

## Problem Statement

Given a binary tree and two nodes `a` and `b`, find the minimum distance (number of edges) between the two nodes.

---

## Approach / Theory

- First, find the Lowest Common Ancestor (LCA) of nodes `a` and `b`.  
  (See [Lowest Common Ancestor](./lowest_common_ancestor.md) for details)

- Then, find the distance from the LCA to node `a` and from the LCA to node `b`.

- The minimum distance between `a` and `b` is the sum of these two distances.

---

## Code

```cpp
/* A binary tree node
struct Node
{
    int data;
    Node* left, * right;
}; */

class Solution {
  public:
    /* Should return minimum distance between a and b
    in a tree with given root*/
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
    int getDist(Node* root, int k){
        if(root == nullptr) return -1 ;
        if(root->data == k) return 0 ;
        
        int l = getDist(root->left, k);
        if(l!= -1) return l+1 ;
        int r = getDist(root->right, k);
        if(r!=-1 )return r+1 ;
        
        return -1 ;
        
    }
    int findDist(Node* root, int a, int b) {
        Node* lca = dfs(root, a, b);
        // cout << lca->data << endl ;
        int d1 = getDist(lca ,a)  ;
        int d2  = getDist(lca, b) ;
        // cout << "d1 is " << d1 <<" and d2 is " << d2 <<endl ;
        return (d1+d2>=0)?d1+d2:-1 ;
        
    }
};
```

---

## Explanation

- Use a DFS to find the LCA node.
- Use another DFS function to calculate the distance from LCA to each target node.
- Add those distances to get the final answer.

---

## Complexity

- Time: O(n), as each DFS traversal takes O(n).
- Space: O(h), h is the height of the tree, due to recursion stack.

---

## Related Problems

- [Lowest Common Ancestor in a Binary Tree](./lowest_common_ancestor.md)

---

## Tags

- Tree
- DFS
- Lowest Common Ancestor
- Distance Calculation


