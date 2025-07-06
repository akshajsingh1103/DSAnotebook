# 🔁 `nextPermutation` Algorithm — C++ Implementation

[Problem Link](https://leetcode.com/problems/next-permutation/description/)

## 📌 Problem Statement

Given an array of integers `nums`, rearrange it into the lexicographically next greater permutation of numbers.  
If such arrangement is not possible, rearrange it as the **lowest possible order** (i.e., sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

---

## ✅ C++ Code

```cpp
class Solution {
    public :
    void nextPermutation(vector<int>& nums) {

        int ind1=-1;
        int ind2=-1;
        // step 1 find breaking point 
        for(int i=nums.size()-2;i>=0;i--){
            if(nums[i]<nums[i+1]){
                ind1=i;
                break;
            }
        }
        // if there is no breaking  point 
        if(ind1==-1){
            reverse(nums.begin(),nums.end());
        }
        
        else{
            // step 2 find next greater element and swap with ind2
            for(int i=nums.size()-1;i>=0;i--){
                if(nums[i]>nums[ind1]){
                    ind2=i;
                    break;
                }
            }

            swap(nums[ind1],nums[ind2]);
            // step 3 reverse the rest right half
            reverse(nums.begin()+ind1+1, nums.end());
        }
    }
   
    
};
```

---

## 🔍 Explanation

- **Step 1**: Find the rightmost element (`ind1`) where `nums[ind1] < nums[ind1 + 1]`. This identifies the pivot point.
- **Step 2**: Find the next larger element (`ind2`) to the right of `ind1` and swap them.
- **Step 3**: Reverse the subarray after `ind1` to get the next smallest lexicographic order.

---

## 🧪 Example

**Input**: `[1, 2, 3]`  
**Output**: `[1, 3, 2]`  
**Explanation**: The next permutation of `[1, 2, 3]` is `[1, 3, 2]`.

**Input**: `[3, 2, 1]`  
**Output**: `[1, 2, 3]`  
**Explanation**: This is the last permutation, so we reverse to get the first.

---

## 🧠 Time & Space Complexity

- **Time Complexity**: `O(n)` — linear scan from the end + reverse
- **Space Complexity**: `O(1)` — in-place swaps and reverse

---
