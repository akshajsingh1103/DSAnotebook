# Level Order Traversal

[Problem Link](https://www.geeksforgeeks.org/problems/level-order-traversal/1)

---
## Code

```cpp
class Solution {
  public:
    vector<vector<int>> levelOrder(Node *root) {
        vector<vector<int>> res;
        queue<Node*>q ;
        q.push(root);
        q.push(nullptr);
        vector<int> blank;
        res.push_back(blank);
        
        while(!q.empty()){
            Node *temp = q.front();
            q.pop();
            
            if(temp == nullptr){
                vector<int> blank;
                res.push_back(blank);
                if(!q.empty()){
                    q.push(nullptr);
                }
            }else{
                res.back().push_back(temp->data);
                if(temp->left){
                    q.push(temp->left);
                }if(temp->right){
                    q.push(temp->right);
                }
            }
        }return res ;
        
    }
};

```
---