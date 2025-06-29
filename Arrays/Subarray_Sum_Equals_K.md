# Subarray Sum Equals K — Explanation and Guide

[Problem Link](https://leetcode.com/problems/subarray-sum-equals-k/)

## 🧠 Problem Statement

Given an integer array `nums` and an integer `k`, return the total number of **contiguous subarrays** that sum to `k`.

> Subarrays must be **contiguous**, not just any combination of elements.

---

## ❌ Brute-force Approach (for contrast)

A simple approach would be to check **all possible subarrays** (nested loops over all start and end indices) and compute the sum of each.

This would work, but is inefficient:

- Time complexity: **O(n²)** (or **O(n³)** if you recalculate sums each time)
- Not acceptable for large arrays

This leads us to the optimized approach below.

---

## ✅ Optimized Approach: Prefix Sum + Hash Map

### ✨ Key Insight

If the cumulative sum from the start to index `j` is `prefixSum`,  
and there exists an earlier prefix sum `prefixSum - k`,  
then the subarray between that earlier point and index `j` must sum to `k`.

Mathematically:

```
prefixSum_j - prefixSum_i = k ⟹ subarray(i+1...j) sums to k
```

Rewriting:

```
prefixSum_i = prefixSum_j - k
```

This allows us to avoid checking all subarrays — we just track previous prefix sums in a hash map.

---

## 📚 How It Works Step-by-Step

1. Initialize:
   - `prefixSum = 0`
   - `count = 0`
   - `prefixMap = {0: 1}` — meaning a sum of 0 exists once before we start

2. Loop through the array:
   - Add current number to `prefixSum`
   - Check if `prefixSum - k` exists in the map
     - If it does, add its count to `count`
   - Record or increment `prefixSum` in the map

---

## 🔁 Dry Run Example

For input:

```
nums = [1, 1, 1], k = 2
```

Step-by-step:

| Index | Num | Prefix Sum | Needed (prefixSum - k) | Exists in Map? | Count | Map State                  |
|-------|-----|-------------|------------------------|----------------|--------|-----------------------------|
| 0     | 1   | 1           | -1                     | ❌             | 0      | {0:1, 1:1}                  |
| 1     | 1   | 2           | 0                      | ✅             | 1      | {0:1, 1:1, 2:1}             |
| 2     | 1   | 3           | 1                      | ✅             | 2      | {0:1, 1:1, 2:1, 3:1}        |

✅ Final count = 2  
(Valid subarrays: [1,1] from 0–1 and [1,1] from 1–2)

---

## 💬 Thought Questions

- Why do we initialize the map with `{0: 1}`?
- Why does `prefixSum - k` being in the map imply there's a subarray ending at the current index that sums to `k`?
- What would happen if the array contains negative numbers?
- How does this differ from a brute-force nested loop approach?

---

## 🚧 Code Placeholder (You add it here)

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> prefix;
        prefix[0]=1 ;

        int cnt = 0 ;
        int pref=0 ;
        for(auto it : nums){
            pref+=it ;

            if(prefix.find(pref-k)!=prefix.end()){
                cnt+=prefix[pref-k];
            }
            prefix[pref]++;
        }
        return cnt ;
    }
};
```

---

## ⏱️ Time and Space Complexity

| Aspect        | Complexity |
|---------------|------------|
| Time          | O(n)       |
| Space         | O(n)       |
