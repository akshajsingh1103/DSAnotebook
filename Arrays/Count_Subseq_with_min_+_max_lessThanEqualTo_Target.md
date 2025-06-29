# Number of Subsequences That Satisfy the Given Sum Condition

[Problem Link](https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/description/?envType=daily-question&envId=2025-06-29)

## 🧠 Problem Summary

Given an array `nums` and an integer `target`, return the number of **non-empty subsequences** such that:

```
min(subsequence) + max(subsequence) <= target
```

Since the answer could be large, return it modulo `1e9 + 7`.

---

## ✅ Optimized Approach: Two Pointers + Precomputed Powers

### 🔑 Key Observations

- We only care about the **minimum and maximum** of a subsequence.
- After sorting the array:
  - `nums[l]` will be the minimum
  - `nums[r]` will be the maximum
- If `nums[l] + nums[r] <= target`, then **all subsequences** formed by choosing any elements between indices `l` and `r` (inclusive) that include `nums[l]` as the minimum and have max ≤ `nums[r]` are valid.
- The number of such subsequences is `2^(r - l)`.

---

## 💡 Important Clarification About `r - l`

I was confused initially thinking that the exponent should be `r - l - 1` because:

- The minimum element `nums[l]` **must be included** in the subsequence (so it is mandatory, not optional).
- The maximum element `nums[r]` **may or may not be included** (it's optional).
- All the elements **between `l` and `r` (exclusive)** can be either included or excluded.

Therefore:

- We fix the minimum `nums[l]` (mandatory),
- For the remaining `r - l` elements (including the max at index `r`), **each element can either be included or not**, so `2^(r - l)` possible subsets.

This explains why the exponent is `r - l` and **not** `r - l - 1`.

---

## 🧮 Precomputing Powers of 2

We precompute powers of 2 modulo `MOD` up to the size of the array:

```cpp
powers[i] = (powers[i-1] * 2LL) % MOD;
```

- `2LL` means the literal `2` as a **long long** integer (64-bit) to avoid integer overflow during multiplication.
- This ensures multiplication is done safely before taking modulo.

---

## 🔁 Step-by-Step Algorithm

1. Sort the array `nums`.
2. Precompute powers of 2 modulo `MOD`.
3. Initialize two pointers:
   - `l = 0` (left/start)
   - `r = n - 1` (right/end)
4. Initialize `count = 0`.
5. While `l <= r`:
   - If `nums[l] + nums[r] <= target`:
     - Add `powers[r - l]` to `count`
     - Move `l` forward (`l++`)
   - Else:
     - Move `r` backward (`r--`)

---

## 🧩 Why do we do modulo again on count?

Because the count can get very large, we take modulo at every addition step to keep the number within integer limits and satisfy the problem constraints.

```cpp
count = (count + powers[r - l]) % MOD;
```

This ensures `count` never exceeds `MOD`.

---

## 📊 Dry Run Example

Given:

```
nums = [3, 5, 6, 7], target = 9
```

Sorted:

```
[3, 5, 6, 7]
```

| l | r | nums[l] | nums[r] | Sum    | Valid? | Add to Count (2^(r - l)) | New Count |
|---|---|---------|---------|--------|--------|---------------------------|-----------|
| 0 | 3 | 3       | 7       | 10     | ❌     | -                         | 0         |
| 0 | 2 | 3       | 6       | 9      | ✅     | 2^(2) = 4                 | 4         |
| 1 | 2 | 5       | 6       | 11     | ❌     | -                         | 4         |
| 1 | 1 | 5       | 5       | 10     | ❌     | -                         | 4         |

Final count = 4

---

## 🚧 Code Placeholder

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7 ;
    int numSubseq(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<int>powers(nums.size(),1);
        for(int i = 1 ; i<nums.size(); i++){
            powers[i]=(powers[i-1]*2LL)%MOD;
        }
        int l = 0 ;
        int r = nums.size()-1 ;
        int cnt = 0;
        while(l<=r){
            if(nums[l]+nums[r]<= target){
                cnt= (cnt+powers[r-l])%MOD;
                l+=1 ;
            }else{
                r--;
            }
        }return cnt ;
    }
};
```

---

## ⏱️ Time and Space Complexity

| Operation               | Complexity |
|-------------------------|------------|
| Sorting the array       | O(n log n) |
| Two-pointer traversal   | O(n)       |
| Precomputing powers     | O(n)       |
| Total Time              | O(n log n) |
| Space for powers array  | O(n)       |

---


