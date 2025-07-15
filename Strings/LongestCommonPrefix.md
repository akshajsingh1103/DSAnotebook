# 💻 Longest Common Prefix: 

[Problem Link](https://leetcode.com/problems/longest-common-prefix/description/)

This document summarizes the process of debugging and solving the **"Longest Common Prefix"** problem. It starts with an initial incorrect attempt and follows the conversation through various logical doubts to arrive at a correct and efficient solution.

---

## 🎯 The Problem

Given a vector of strings, find the longest common prefix string amongst them.  
If there is no common prefix, return an empty string `""`.

### Example

**Input:**  
```cpp
["flower", "flow", "flight"]
```

**Output:**  
```cpp
"fl"
```

---

## 🧱 Initial Attempt & The Core Flaw

### ❌ Initial Code Attempt

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";

        string prefix = strs[0];
        int prefLen = INT_MAX;
        for (int i = 1; i < strs.size(); i++) {
            int j = 1;
            while (j <= strs[0].size()) {
                // ❗ This condition is flawed
                if (strs[i].find(prefix.substr(0, j)) == 0) break;
                j++;
            }
            prefLen = min(prefLen, j);
        }

        return prefix.substr(0, prefLen);
    }
};
```

### 🔍 The Core Flaw

The line:
```cpp
if (strs[i].find(prefix.substr(0, j)) == 0) break;
```
was intended to confirm that the substring is a prefix. However:

- It **breaks on the first success**, not the first failure.
- As a result, it doesn't fully test longer prefixes that *could* be common.
- It falsely concludes a shorter prefix like `"f"` is the longest, even when `"fl"` exists.

---

## ❓ Doubt #1: Why Isn't It Working?

The code checks if the current string starts with the current prefix substring. But it **breaks too early** — on the first match — instead of continuing to verify longer prefixes.

---

## 🧠 Doubt #2: What Does `find()` Actually Do?

`std::string::find()` returns the **starting index** of a substring.

- `find(...) == 0`: The substring is a **prefix**.
- `find(...) > 0`: The substring exists, but is **not a prefix**.
- `find(...) == npos`: The substring doesn't exist at all.

✅ So this check is valid:
```cpp
if (strs[i].find(prefix) != 0)
```
It correctly checks if `prefix` is **not** at the start of `strs[i]`.

---

## 💡 The Breakthrough Idea

> “What if we do it the other way? Start from the full first word and shrink until it matches?”

This flipped the logic from **building a prefix** to **shrinking it** until it becomes valid — a much more natural and correct approach.

---

## 🧪 Second Attempt (Improved, but Still Flawed)

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";

        string prefix = strs[0];
        int prefLen = INT_MAX;
        for (int i = 1; i < strs.size(); i++) {
            int j = 1;
            bool up = false;
            while (j <= strs[0].size()) {
                if (strs[i].find(prefix.substr(0, j)) != 0) {
                    break;
                }
                j++;
                up = true;
            }
            if (up) prefLen = min(prefLen, j - 1);
        }

        return prefix.substr(0, prefLen);
    }
};
```

### 🚨 Remaining Issues

- `prefLen = INT_MAX`: Dangerous if the loop is skipped (e.g., only one string).
- Confusing logic with `up` flag and `j - 1`.
- Still substr-heavy, and not very efficient.

---

## ✅ Final Correct Solutions

### ✅ **Method 1: Shortening from the Back (Efficient & Clean)**

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";

        string prefix = strs[0];

        for (int i = 1; i < strs.size(); i++) {
            while (strs[i].find(prefix) != 0) {
                prefix = prefix.substr(0, prefix.length() - 1);
                if (prefix.empty()) return "";
            }
        }

        return prefix;
    }
};
```

🔹 **Time Complexity:** O(N * M), where N = number of strings, M = length of longest string  
🔹 **Space Complexity:** O(1)

---

### ✅ **Method 2: Character-by-Character (Your Original Idea, Fixed)**

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";

        int prefixLen = strs[0].length();

        for (int i = 1; i < strs.size(); i++) {
            int j = 0;
            while (j < prefixLen && j < strs[i].length() && strs[0][j] == strs[i][j]) {
                j++;
            }
            prefixLen = j;
        }

        return strs[0].substr(0, prefixLen);
    }
};
```

🔹 **Advantage:** No need for `find()` or substring creation  
🔹 **Faster:** Uses raw character comparison — more optimal for large datasets

---

## 🧠 Lessons Learned

- Be very careful with **loop-breaking conditions**. Breaking too early can hide bugs.
- Understand **what functions like `find()` return** — not just if they return *something*.
- When debugging string logic, try flipping the approach: **build up** or **shrink down**.

---

## ✅ Summary

| Attempt    | Approach                             | Result                       |
|------------|--------------------------------------|------------------------------|
| First      | Build prefix using `find()`          | ❌ Incorrect due to early break |
| Second     | Improved `find()` usage with flags   | ⚠️ Better but fragile         |
| Final #1   | Shrink the prefix using `find()`     | ✅ Clean & Correct            |
| Final #2   | Char-by-char matching                | ✅ Optimal & Efficient        |

---
