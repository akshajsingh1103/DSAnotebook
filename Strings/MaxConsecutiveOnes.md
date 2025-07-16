# 📘 Sliding Window Solution: Max Consecutive Ones with K Flips

[Problem Link](https://leetcode.com/problems/max-consecutive-ones-iii/)

## 🧩 Problem Statement

Given a binary array `nums` and an integer `k`, return the **maximum number of consecutive 1's** in the array if you can flip at most `k` 0's.

---

## 🧪 Examples

### Example 1:
```
Input:  nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2  
Output: 6  
Explanation: Flip two 0s to get [1,1,1,0,0,1,1,1,1,1,1]
```

### Example 2:
```
Input:  nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3  
Output: 10  
```

---

## ✅ Approach: Sliding Window

We maintain a window (`left` to `right`) that always contains **at most `k` zeros**.  
When the number of zeros exceeds `k`, we shrink the window from the left.

### Key Idea:
- Use two pointers (`left`, `right`) to create a window.
- Track how many zeros are inside the window using `zeroCount`.
- If `zeroCount > k`, move the `left` pointer to reduce the window size.
- Update the max length of the window when valid.

---

## 🔄 Final Optimized C++ Code

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int left = 0, right = 0, zeroCount = 0, maxLen = 0;

        for (; right < nums.size(); ++right) {
            if (nums[right] == 0) {
                zeroCount++;
            }

            // Shrink window from the left if more than k zeros
            while (zeroCount > k) {
                if (nums[left] == 0) {
                    zeroCount--;
                }
                left++;
            }

            // Update max window size
            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

---

## ⚙️ Time & Space Complexity

- **Time Complexity:** O(n)  
  Every element is visited at most twice (once by `right`, once by `left`).

- **Space Complexity:** O(1)  
  Only a few integer variables are used.

---

## 🆚 Comparison: Original vs Improved Version

### 🔸 Original Code (Your Version):
```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int l = 0 , r = 0, c = 0, maxLen = 0;
        while (r < nums.size()) {
            if (nums[r] == 0) {
                if (c >= k) {
                    while (nums[l] != 0 && l < r) l++;
                    l++;
                    c--;
                }
                c++;
            }
            maxLen = max(maxLen , r - l + 1);
            r++;
        }
        return maxLen;
    }
};
```

### 🔸 Issues with Original Version:
- Contains an **inner loop**: `while (nums[l] != 0 && l < r)` which may make it slightly less efficient.
- Slightly more complex logic to shrink the window.
- Harder to maintain and debug in edge cases.

### ✅ Benefits of Improved Version:
- **Simpler** logic using `zeroCount` and clean `while (zeroCount > k)` loop.
- Avoids unnecessary nested conditions.
- **More readable**, predictable, and efficient.

---

## 📝 Summary

- Use sliding window when you need to find a subarray with constraints.
- Always track the count of the "violating" element — here, zeros.
- Shrink the window only when the constraint is violated (more than `k` zeros).
- Maintain the length of the largest valid window as the answer.

---
