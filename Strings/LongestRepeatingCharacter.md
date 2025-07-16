# 🔠 Longest Repeating Character Replacement (Sliding Window)

[Problem Link](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

## 🧩 Problem Statement

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation **at most `k` times**.

Return the **length of the longest substring** containing the same letter you can get after performing the above operations.

---

## 📥 Examples

### Example 1:
```
Input: s = "ABAB", k = 2  
Output: 4  
Explanation: Replace two 'A's with 'B' or two 'B's with 'A' to get "AAAA" or "BBBB".
```

### Example 2:
```
Input: s = "AABABBA", k = 1  
Output: 4  
Explanation: Replace one 'A' with 'B' to get "AABBBBA", longest valid substring is "BBBB".
```

---

## 💡 Approach: Sliding Window with Frequency Tracking

We maintain a **sliding window** and track the **frequency of characters** in that window.  
Our goal is to keep the number of replacements needed within `k`.

### ✅ Key Insight:

To form a window with all the same character, we only need to track:

```
window length - max frequency character count <= k
```

If the above is true, we can expand the window.  
If not, we shrink it from the left.

---

## 🚀 Optimized C++ Code (Using Fixed Array)

```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        int freq[26] = {0};  // Array for 26 uppercase letters
        int left = 0, maxFreq = 0, maxLen = 0;

        for (int right = 0; right < s.size(); ++right) {
            freq[s[right] - 'A']++;
            maxFreq = max(maxFreq, freq[s[right] - 'A']);

            int windowLen = right - left + 1;

            if (windowLen - maxFreq > k) {
                freq[s[left] - 'A']--;
                left++;
            }

            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

---

## ⚙️ Time and Space Complexity

- **Time Complexity:** `O(n)`  
  Each character is processed at most twice (by `right` and `left`).

- **Space Complexity:** `O(1)`  
  Fixed-size array of 26 elements.

---

## 🔁 Why Use an Array Instead of `std::vector`?

| Feature           | `int freq[26]`             | `vector<int> freq(26)`      |
|-------------------|----------------------------|------------------------------|
| Allocation        | Stack (faster)             | Heap (slightly slower)       |
| Bounds Checking   | No                         | Yes                          |
| Performance       | Slightly better            | Slightly worse               |
| Code Simplicity   | Shorter and lighter        | Slightly more verbose        |

---

## ✅ Summary

- Use sliding window with frequency tracking to solve efficiently in `O(n)`.
- Track `maxFreq` to avoid checking each letter individually.
- Use a **fixed-size array** (`int freq[26]`) for speed and simplicity.
