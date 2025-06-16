# Level Order Traversal

[Problem Link](https://www.geeksforgeeks.org/create-a-mirror-tree-from-the-given-binary-tree/)

---
## Code

```cpp
treenode* mirrorTree(node* root)
{
    // Base Case
    if (root == NULL)
        return root;
    node* t = root->left;
    root->left = root->right;
    root->right = t;

    if (root->left)
        mirrorTree(root->left);
    if (root->right)
        mirrorTree(root->right);
  
  return root;
}
```
---
