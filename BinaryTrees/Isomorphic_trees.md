# Isomorphic Trees

**Problem Link:** [GFG - Check if Tree is Isomorphic](https://www.geeksforgeeks.org/problems/check-if-tree-is-isomorphic/1)

---

## Problem Statement

Given two binary trees, the task is to check if the two trees are isomorphic.

Two trees are called isomorphic if one of them can be obtained from the other by a series of flips (any number of nodes, where children are swapped).

---

## Approach / Theory

To solve this, we perform a recursive comparison between nodes of both trees. There are two possibilities at every step:
- We **don't swap** the left and right children and both subtrees match.
- We **swap** the left and right children and the subtrees still match.

If either of these configurations is valid for all nodes, the trees are isomorphic.

We also check that the current node data matches (`temp1->data == temp2->data`) because isomorphism requires identical values, just possibly flipped structure.

---

## First Attempt (Incorrect)

### Code  
`## Code`
```cpp
bool func(Node* temp1, Node* temp2){
        if(temp1 == nullptr && temp2==nullptr) return true ;
        if(temp1== nullptr && temp2!=nullptr || temp2==nullptr && temp1!=nullptr) return false ;
        
        
        
        bool leftMatchNormal = false, rightMatchNormal = false;
        bool leftMatchSwapped = false, rightMatchSwapped = false;

        if(temp1->left && temp2->left)
            leftMatchNormal = (temp1->left->data == temp2->left->data);
        else if(temp1->left == nullptr && temp2->left == nullptr)
            leftMatchNormal = true;

        if(temp1->right && temp2->right)
            rightMatchNormal = (temp1->right->data == temp2->right->data);
        else if(temp1->right == nullptr && temp2->right == nullptr)
            rightMatchNormal = true;

        if(temp1->left && temp2->right)
            leftMatchSwapped = (temp1->left->data == temp2->right->data);
        else if(temp1->left == nullptr && temp2->right == nullptr)
            leftMatchSwapped = true;

        if(temp1->right && temp2->left)
            rightMatchSwapped = (temp1->right->data == temp2->left->data);
        else if(temp1->right == nullptr && temp2->left == nullptr)
            rightMatchSwapped = true;

        if(leftMatchNormal && rightMatchNormal){
            if(!func(temp1->left, temp2->left)) return false;
            if(!func(temp1->right, temp2->right)) return false;
        } else if(leftMatchSwapped && rightMatchSwapped){
            if(!func(temp1->left, temp2->right)) return false;
            if(!func(temp1->right, temp2->left)) return false;
        } else {
            return false;
        }
        return true;
    }
    bool isIsomorphic(Node *root1, Node *root2) {
        return func(root1, root2);
    }
```

### Why It Failed

- **Did not compare the data of the current nodes** (`temp1->data != temp2->data` check was missing).
- Relied on comparing child node values directly (like `temp1->left->data == temp2->left->data`), which is risky:
  - Nodes with the same value might belong in different subtrees.
    
    ```
      Tree 1          Tree 2
        4               4
       / \             / \
      3   3           3   3
     /                     \
    1                       1

    ```

  - Trees like this would wrongly appear equal:

    ```
    Tree 1          Tree 2
      4               1
     / \             / \
    2   3           3   2
    ```

    Your function would say they’re the same due to matching child values, even if the structure is flipped, and it wouldn’t handle swapping properly unless it goes deeper.

---

## Final Solution (Correct)

### Code  
`## Code`
```cpp
bool func(Node* temp1, Node* temp2) {
    if (temp1 == nullptr && temp2 == nullptr) return true;
    if (temp1 == nullptr || temp2 == nullptr) return false;
    if (temp1->data != temp2->data) return false;

    // First try no swap
    if (func(temp1->left, temp2->left) && func(temp1->right, temp2->right))
        return true;

    // Then try swap
    if (func(temp1->left, temp2->right) && func(temp1->right, temp2->left))
        return true;

    return false;
}

```

### Explanation

- Base case checks if both nodes are null → return true.
- If only one is null → return false.
- If node data doesn't match → return false.
- Recursively check:
  - `swap` case: left with right and right with left.
  - `noswap` case: left with left and right with right.
- If **either** of these configurations holds at every node → trees are isomorphic.

#### Complexity:
- **Time:** O(min(N1, N2)), where N1 and N2 are the number of nodes in each tree.
- **Space:** O(h), where h is the height of the trees (due to recursion stack).

---

## Tags

- [x] Tree
- [x] Recursion
- [x] Binary Tree
- [x] DFS
- [x] Isomorphism


