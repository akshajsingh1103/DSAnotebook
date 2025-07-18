# Problem: Number of Subarrays With Sum (LeetCode 930)

[Problem Link](https://leetcode.com/problems/binary-subarrays-with-sum/description/)

See [Similar Problem](./NumberOfSubArraysWithKOddNumbers.md) for a related approach.

Given a binary array `nums` and an integer `goal`, return the number of non-empty subarrays with a sum equal to `goal`.

A subarray is a contiguous part of the array.

---

## 🔍 Pattern: Target Sum in Subarrays

This is a classic example of a **"target sum in subarray"** problem.  
For these types of problems, two common and efficient patterns are:

1. **Prefix Sum + Hash Map** – good for general cases (positive and negative numbers)
2. **Sliding Window (Two Pointers)** – works when array has **non-negative elements only**

---

### 🧠 General Tip

> If you're given an array of **non-negative numbers** and asked for the number of subarrays that add up to an exact target:
>
> ✅ Use the formula:
>
> ```
> number of subarrays with sum == goal
> = count of subarrays with sum ≤ goal
> - count of subarrays with sum ≤ (goal - 1)
> ```

This works efficiently in **O(n)** time using a **sliding window**.

---

## ✅ Solution Using Sliding Window (C++)

```cpp
class Solution {
public:
    // Helper: Count subarrays with sum ≤ goal
    int atMost(vector<int>& nums, int goal) {
        int i = 0, sum = 0, count = 0;
        for (int j = 0; j < nums.size(); j++) {
            sum += nums[j];
            while (i <= j && sum > goal) {
                sum -= nums[i++];
            }
            count += j - i + 1;
        }
        return count;
    }

    int numSubarraysWithSum(vector<int>& nums, int goal) {
        if (goal < 0) return 0; // edge case
        return atMost(nums, goal) - atMost(nums, goal - 1);
    }
};
```

---

## 📊 Time and Space Complexity

| Operation     | Complexity |
|---------------|------------|
| Time          | O(n)       |
| Space         | O(1)       |

- Only a few integer variables are used (no extra data structures).
- Fast and efficient for large arrays.

---

## ✅ Example

```cpp
Input: nums = [1,0,1,0,1], goal = 2
Output: 4

Explanation: Valid subarrays:
- [1,0,1]
- [0,1,0,1]
- [1,0,1]
- [1,0,1]
```

---

## 🧠 When to Use Sliding Window for Target Subarray Sum?

Use this approach when:

- The array contains **only non-negative numbers** (like 0s and 1s)
- You are asked for:
  - Number of subarrays with sum equal to `goal`
  - Or subarrays with sum ≤, ≥, etc.

For general arrays (with negatives), prefer prefix sum + hash map.

---
