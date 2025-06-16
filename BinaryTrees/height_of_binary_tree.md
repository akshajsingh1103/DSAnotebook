# Height Of Binary Tree

[Problem Link](https://www.geeksforgeeks.org/problems/height-of-binary-tree/1)

---
## Code

```cpp
class Solution {
  public:
    int func(Node * temp){
        if(temp == nullptr) return 0 ;
        
        int right = func(temp->right);
        int left = func(temp->left);
        
        return max(left, right) + 1 ;
    }
    int height(Node* node) {
        return func(node) - 1 ;
        
    }
};

```
---

