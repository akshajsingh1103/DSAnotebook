# Reverse Level Order Traversal

[Problem Link](https://www.geeksforgeeks.org/problems/reverse-level-order-traversal/1)

---
## Code

```cpp
class Solution {
  public:
    vector<int> reverseLevelOrder(Node *root) {
        // code here
        vector<int> res;
        queue<Node*>q ;
        q.push(root);
        vector<int> blank;
        while(!q.empty()){
            Node *temp = q.front();
            q.pop();
            res.push_back(temp->data);
            if(temp->right){
                q.push(temp->right);
            }if(temp->left){
                q.push(temp->left);
            }
        }int start = 0;
        int end = res.size()-1 ;
        while(start<=end){
            swap(res[start], res[end]);
            start++;
            end--;
        }
        
        return res ;
    }
};

```
---
