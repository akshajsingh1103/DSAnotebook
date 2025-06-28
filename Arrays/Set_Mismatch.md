# 🧠 Understanding the "Set Mismatch" Problem

[Problem Link](https://leetcode.com/problems/set-mismatch/)

## 📝 Problem Statement

Given an array `nums` of size `n` containing numbers from `1` to `n`:

- One number appears **twice** (duplicate),
- One number is **missing**.

Return a vector `{duplicate, missing}`.

---

## 🧩 Core Insight

Each number `x` should ideally appear **once** and map to **index `x - 1`**.

### 💡 Key Observations:

- As we iterate through the array:
  - For a value `val`, mark the index `val - 1` as **visited** by **negating** the value at that index.
  - If we try to mark an index that is **already negative**, it means we've **visited it before** — this gives us the **duplicate**.
- After the first pass, any index `i` with a **positive value** means the number `i + 1` was **never visited**, hence **missing**.

---

## ✅ Algorithm Steps

1. Traverse the array.
2. Use the value of each element to map to an index.
3. If that index is already negative → it's the **duplicate**.
4. After traversal, check which index is still positive → it's the **missing number**.

---

## ⏱️ Time and Space Complexity

- **Time Complexity:** O(n) — Single pass for marking and one pass to find the missing number.
- **Space Complexity:** O(1) — No extra space used (modifying array in-place).

---

## 💻 Code


```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int dupl, missing ;
        for (int i = 0; i < nums.size(); ++i) {
            int index = abs(nums[i]) - 1;
            if (nums[index] < 0) {
                dupl = index + 1;
            } else {
                nums[index] = -nums[index];
            }
        }
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] > 0) {
                missing = i + 1;
                break;
            }
        }
        return {dupl,missing};
    }
};
```

