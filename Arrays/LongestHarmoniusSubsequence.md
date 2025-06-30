# Longest Harmonious Subsequence (LHS) – Explanation and Optimized Code

[Problem Link](https://leetcode.com/problems/longest-harmonious-subsequence/?envType=daily-question&envId=2025-06-30)


## 🚀 Problem Statement

Given an integer array `nums`, return the length of its longest harmonious subsequence.  
A harmonious subsequence is one where the **difference between the maximum and minimum number is exactly 1**.

---

## 🧠 Initial Intuition

I initially thought of solving the problem by:

1. **Sorting the array.**
2. Using **two nested loops**:  
   - Fixing the starting index `i`,  
   - Scanning from the end to find an index `j` where `nums[j] - nums[i] == 1`.
3. If found, updating the length as `j - i + 1`.

// Initial core logic:
sort(nums.begin(), nums.end());

for (i = 0; i < nums.size(); i++) {
    for (j = nums.size() - 1; j > i; j--) {
        if (nums[j] - nums[i] == 1) {
            update result;
            break;
        }
    }
}

---

## 🧨 Problems with This Approach

- ❌ **Inefficient**: Time complexity is **O(n²)** due to the nested loops.
- ❌ **Redundant Checks**: Many comparisons are unnecessary. We're repeatedly comparing the same values or skipping already checked combinations.
- ❌ **Incorrect Intuition**: It focuses too much on positions and sorting, rather than just the values and their frequencies.

---

## 💡 Realization & Correct Intuition

After revisiting the problem, I realized:

> If the difference between the max and min elements in a subsequence is exactly 1, then the **only valid elements** in that subsequence must be:
> 
> - The **smaller element** (say `x`)
> - The **larger element** (`x + 1`)
> - And their **duplicates**.

That means the only thing that matters is:
- How many times `x` appears.
- How many times `x+1` appears.

So we don't need to care about **indexes** or **positions**, just **frequencies**.

---

## ✅ Optimized Approach

1. Use a **hash map** to count the frequency of each number.
2. For every key `k` in the map:
   - Check if `k + 1` exists.
   - If it does, the length of the harmonious subsequence formed by `k` and `k + 1` is `freq[k] + freq[k+1]`.

---

## ⏱ Time and Space Complexity

- **Time Complexity**: `O(n)`
  - One pass to build the frequency map.
  - One pass to check for harmonious subsequences.
- **Space Complexity**: `O(n)`
  - Due to the hash map storing frequencies of each unique number.

---

## 💻 Code

(Add your code here)

```cpp
class Solution {
public:
    int findLHS(vector<int>& nums) {
        unordered_map<int, int> m ;
        int maxlength = 0 ;

        for(auto it: nums){
            m[it]++;
        }

        for(auto it : m){
            if(m.count(it.first+1)){
                maxlength=max(maxlength, it.second + m[it.first+1]);
            }
        }return maxlength;
    }
};
```