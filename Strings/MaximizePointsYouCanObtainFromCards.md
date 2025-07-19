# 🃏 Leetcode 1423 — Maximum Points You Can Obtain from Cards

## 🔗 Problem Link

[Leetcode 1423](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/)

---

## 📌 Problem Summary

You're given an array `cardPoints`, where each element represents the points of a card in a row. You can pick **exactly `k` cards**, either from the **start** or **end** of the array. Return the **maximum possible score** you can get.

---

## 🧠 Approach 1: Prefix + Suffix Sum (Original Approach)

We calculate:
- `pref[i]`: sum of the first `i` cards
- `suff[i]`: sum of the last `i` cards

Then try all combinations of `i` cards from the start and `k-i` from the end, and track the maximum score.

### ✅ Code

```cpp
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int n = cardPoints.size();
        vector<int> pref(k + 1, 0);  // prefix sum array
        vector<int> suff(k + 1, 0);  // suffix sum array

        for (int i = 1; i <= k; i++) {
            pref[i] = pref[i - 1] + cardPoints[i - 1];
            suff[i] = suff[i - 1] + cardPoints[n - i];
        }

        int maxScore = INT_MIN;
        for (int i = 0; i <= k; i++) {
            maxScore = max(maxScore, pref[i] + suff[k - i]);
        }

        return maxScore;
    }
};
```

### ⏱️ Time Complexity
- `O(k)` — compute prefix and suffix arrays, then iterate `k+1` times

### 🧠 Space Complexity
- `O(k)` — extra space for `pref` and `suff` arrays

---

## 🧠 Approach 2: Space-Optimized Sliding Window

Take the first `k` cards from the front, then gradually replace them one by one from the back, keeping track of the running total and updating the max score.

### ✅ Code

```cpp
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int n = cardPoints.size();
        int total = 0;

        // Take first k cards from the front
        for (int i = 0; i < k; i++) {
            total += cardPoints[i];
        }

        int maxScore = total;

        // Replace cards from front with back one by one
        for (int i = 1; i <= k; i++) {
            total = total - cardPoints[k - i] + cardPoints[n - i];
            maxScore = max(maxScore, total);
        }

        return maxScore;
    }
};
```

### ⏱️ Time Complexity
- `O(k)` — one pass through the first `k` cards, then `k` replacements

### 🧠 Space Complexity
- `O(1)` — only a few variables used, no extra arrays

---

## 🔚 Conclusion

| Approach              | Time Complexity | Space Complexity | Notes                          |
|-----------------------|------------------|------------------|--------------------------------|
| Prefix + Suffix Sum   | O(k)             | O(k)             | Easy to write, uses more space |
| Sliding Window        | O(k)             | O(1)             | Most optimal, best approach    |

---
