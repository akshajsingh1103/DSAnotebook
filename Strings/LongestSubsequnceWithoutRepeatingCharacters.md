# Longest Substring Without Repeating Characters

[Problem Link](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

This document walks through three versions of solving the classic problem:

> **Problem:** Given a string `s`, return the length of the longest substring without repeating characters.

---

## ❌ 1. Original Attempt (Incorrect Logic)
```cpp
// Problematic: Resets the entire window on duplicates
class Solution {
public:
int lengthOfLongestSubstring(string s) {
  if(s.length() == 0) return 0;
  if(s.length() == 1) return 1;
  set<char> freq;
  int i = 0, j = 1;
      int t = 1;
      freq.insert(s[0]);
  
      while(j < s.size()) {
          if(freq.count(s[j])) {
              freq.clear();
              t = max(t, j - i);
              i = j;
              freq.insert(s[i]);
              j++;
          } else {
              freq.insert(s[j]);
              j++;
          }
      }
  
      return t;
  }
};
```
---

## ✅ 2. Sliding Window with Set (Correct & Clean)

```cpp
// Sliding window using a set to track unique characters
class Solution {
public:
int lengthOfLongestSubstring(string s) {
  unordered_set<char> freq;
  int i = 0, j = 0, maxLen = 0;
    while (j < s.size()) {
        if (!freq.count(s[j])) {
            freq.insert(s[j]);
            maxLen = max(maxLen, j - i + 1);
            j++;
        } else {
            freq.erase(s[i]);
            i++;
        }
    }

    return maxLen;
}
};
```


### ✅ Why This Works

- It only removes **one character at a time** from the start of the window.
- Ensures the substring always contains **only unique characters**.
- Efficient: **O(n)** time, where `n` is the string length.

---

## 🚀 3. Fully Optimized Sliding Window with Map

```cpp
// Most efficient: uses map to remember last seen positions
class Solution {
public:
int lengthOfLongestSubstring(string s) {
unordered_map<char, int> lastSeen;
int maxLen = 0, start = 0;
    for (int end = 0; end < s.length(); ++end) {
        char current = s[end];

        if (lastSeen.count(current) && lastSeen[current] >= start) {
            start = lastSeen[current] + 1;
        }

        lastSeen[current] = end;
        maxLen = max(maxLen, end - start + 1);
    }

    return maxLen;
}
};
```

### ✅ Why This Is Most Efficient

- Instead of removing characters one-by-one like the set version, it **jumps the start pointer** past the last duplicate.
- This avoids unnecessary work and allows us to keep scanning quickly.

---

## 🤔 Common Doubts Answered

### Q: Why do we check `lastSeen[current] >= start`?
To ensure the duplicate character we're reacting to is **inside the current window**.

Without this check, we might move `start` **backwards**, which breaks the sliding window logic.

### Q: Why not just clear everything on a duplicate like in version 1?
Because doing so discards useful characters. For example, in `"abcabcbb"`, when we hit the second `'a'`, we can still keep `"bc"` instead of resetting.

### Q: What's the time complexity of the final solution?
- **Time:** O(n), where `n` is the length of the string.
- **Space:** O(m), where `m` is the size of the character set (e.g., 26 for lowercase, 128 for ASCII).

---

## ✅ Final Recommendation

Use the **map-based approach** (version 3) for best performance, especially on large strings. It's clean, handles edge cases properly, and is efficient.

---

## ❤️ Credits

Thanks for the insightful questions that led to breaking this down step-by-step. Understanding where and *why* an algorithm fails is the best way to master it.
