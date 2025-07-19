# 🔗 Leetcode 128 — Longest Consecutive Sequence

[Problem Link](https://leetcode.com/problems/longest-consecutive-sequence/description/)

## 📌 Problem Summary

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

---

## ✅ Optimal Approach: Using `unordered_set`

We use a `set` to allow constant-time lookups and check for the **start of a sequence** only. From there, we count the streak.

### 🔍 Key Idea:

- For each number, check if `num - 1` exists.
  - If it doesn't, `num` is the **start** of a sequence.
- Count how long the sequence continues using `num + 1`, `num + 2`, ...

---

## 💡 Code (Cleaned Version)

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;

        unordered_set<int> s(nums.begin(), nums.end());
        int maxStreak = 0;

        for (int num : s) {
            if (s.find(num - 1) == s.end()) {  // start of a sequence
                int currentNum = num;
                int currentStreak = 1;

                while (s.find(currentNum + 1) != s.end()) {
                    currentNum++;
                    currentStreak++;
                }

                maxStreak = max(maxStreak, currentStreak);
            }
        }

        return maxStreak;
    }
};
```

---

## ⏱️ Time & Space Complexity

| Aspect         | Complexity |
|----------------|------------|
| Time           | `O(n)`     |
| Space          | `O(n)`     |

---

## ✅ Conclusion

- Your current solution using `unordered_set` is already **optimal**.
- Avoid using arrays for lookup unless the number range is **guaranteed small** and positive.

---
