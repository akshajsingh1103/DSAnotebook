# Majority Element II – Extended Boyer-Moore Voting Algorithm

[Problem Link](https://leetcode.com/problems/majority-element-ii/submissions/1678444936/)

## 📌 Problem Statement

Find all elements in an array that appear **more than ⌊n/3⌋ times**.

---

## 🧠 Boyer-Moore Voting Algorithm (Short Theory)

The Boyer-Moore Voting Algorithm is used to find majority element(s) in an array using a **voting system** that cancels out non-majority elements.

- For **more than ⌊n/2⌋**, we track **1 candidate**
- For **more than ⌊n/3⌋**, we track **2 candidates**
- For **more than ⌊n/k⌋**, we track **k - 1 candidates**

---

## 🔄 General Steps for ⌊n/k⌋ Majority Problem

1. **If current number matches a candidate**:  
   ➤ Increment its count

2. **Else if there’s a free candidate slot (count == 0)**:  
   ➤ Assign current number as that candidate and set count to 1

3. **Else**:  
   ➤ Decrement **all counts** by 1 (simulate canceling out votes)

---

## 🧮 Time & Space Complexity

For the specific case of **⌊n/3⌋**:

- ✅ **Time Complexity**: `O(n)`  
  (Two passes: one to find candidates, one to verify)

- ✅ **Space Complexity**: `O(1)`  
  (We use only a fixed number of counters and candidates — no extra arrays)

For the **general ⌊n/k⌋** case:

- ✅ **Time Complexity**: `O(n * k)` (worst case — during final verification)
- ✅ **Space Complexity**: `O(k)` (we maintain up to `k - 1` candidates and their counts)

---

## ❓ My Doubts Explained

### 1. **Why at most 2 elements > n/3?**
If more than 2 elements appeared more than ⌊n/3⌋ times, their total count would exceed `n`, which isn't possible.

### 2. **Why are we using `else-if` in the logic?**
It ensures only **one action runs per number** to avoid incorrect processing:
- Match → increment
- Free slot → assign
- No match and no slot → cancel both counts

### 3. **Why don’t we reduce count if a number is not equal to just one candidate?**
Because we only cancel out when the current number **doesn’t match either** candidate — that's the moment we simulate a "voting loss".

### 4. **Why do we need a second pass?**
The first pass only finds **potential** candidates.  
The second pass actually **verifies** if they appear more than ⌊n/3⌋ times.

---

## ✍️ Add Your Code Here

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int count1 = 0 ;
        int count2 = 0 ;
        int candidate1=0;
        int candidate2=0;

        for(auto it : nums){
            if(it==candidate1){
                count1++;
            }else if(it==candidate2){
                count2++;
            }else if(count1==0){
                candidate1 = it ;
                count1++;
            }else if(count2==0){
                candidate2=it;
                count2++;
            }else{
                count1--;
                count2--;
            }
        }vector<int>result;
        count1=0;
        count2=0;
        for(auto it:nums){
            if(it == candidate1) count1++;
            else if(it==candidate2) count2++;
        }
        if(count1>nums.size()/3) result.push_back(candidate1);
        if(count2>nums.size()/3) result.push_back(candidate2);
        return result ;
    }
};

```
For a generalized case n/k, heres the gfg article to refer to 
[Article for n/k more general case](https://www.geeksforgeeks.org/dsa/given-an-array-of-of-size-n-finds-all-the-elements-that-appear-more-than-nk-times/)
