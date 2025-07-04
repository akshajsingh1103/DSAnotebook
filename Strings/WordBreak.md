# Word Break Problem – DP Approaches Explained

[Problem Link](https://leetcode.com/problems/word-break/description/)

This file explains two approaches to solving the Word Break problem using **dynamic programming**:

1. Top-Down (Recursion + Memoization)
2. Bottom-Up (Iterative Tabulation)

It also covers a common confusion around the direction of processing and how to determine what `dp[i]` really means.

---

## 🧩 Problem Statement

Given a string `s` and a dictionary of words `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

---

## ✅ Approach 1: Top-Down (Recursive DP with Memoization)

### 📌 Code

class Solution {
public:
    bool dfs(string &s, unordered_set<string>& dict, int index, vector<int> &dp){
        if (index >= s.size()) return true;
        if (dp[index] != -1) return dp[index];

        for (int i = index; i <= s.size(); i++) {
            string sub = s.substr(index, i - index);
            if (dict.count(sub)) {
                if (dfs(s, dict, i, dp)) {
                    return dp[index] = true;
                }
            }
        }

        return dp[index] = false;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        vector<int> dp(s.size(), -1);
        return dfs(s, dict, 0, dp);
    }
};

### 🧠 How It Works

- Starts from `index = 0`.
- Tries every possible substring `s[index..i-1]`.
- If the substring is in the dictionary and the remaining string (`s[i..]`) can be broken, then `dp[index] = true`.

### 🤔 Common Confusion

> "It feels like we are solving the problem from the end toward the start in recursion."

Yes — even though recursion **starts at index 0 and moves forward**, the memoized `dp[]` values get filled on the **way back** as recursive calls return. So `dp[8]` might be filled before `dp[4]`, then `dp[0]`. This gives a sense of **right-to-left filling**.

---

## ✅ Approach 2: Bottom-Up (Iterative DP with Optimization)

### 📌 Code

class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        int n = s.size();

        // Optimization: compute max word length
        int maxWordLen = 0;
        for (const string& word : wordDict) {
            maxWordLen = max(maxWordLen, (int)word.length());
        }

        vector<bool> dp(n + 1, false);
        dp[0] = true; // empty prefix is valid

        for (int i = 1; i <= n; ++i) {
            for (int j = max(0, i - maxWordLen); j < i; ++j) {
                if (dp[j] && dict.count(s.substr(j, i - j))) {
                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[n];
    }
};

### 🧠 How It Works

- `dp[i]` means the substring `s[0..i-1]` can be segmented.
- For each `i`, we check if there's a `j` such that:
  - `dp[j]` is true (so `s[0..j-1]` is breakable),
  - and `s[j..i-1]` is a valid word.

### 🧵 Confusion Clarified

> "Why do we check if `dp[j]` is true and `s[j..i-1]` is in the dict? Why not the other way?"

Because we are **building up** the answer. `dp[j]` tells us that everything **before** `j` is already valid. Now we check if adding one more word (`s[j..i-1]`) still keeps it valid.

We don't check `s[0..j-1]` and `dp[i-j]` because we're not breaking from the front — we're checking **can we end at `i`** with a valid word.

---

## 🔄 Key Differences

| Feature            | Top-Down                         | Bottom-Up                         |
|--------------------|----------------------------------|-----------------------------------|
| Style              | Recursive + Memoization          | Iterative + Tabulation            |
| Call direction     | Left to right                    | Left to right                     |
| `dp[]` fill order  | On return (deepest first)        | Linearly (0 to n)                 |
| Control flow       | DFS with caching                 | Loop with early exit              |
| Substring checked  | `s[index..i-1]`                  | `s[j..i-1]`                       |
| Typical confusion  | Looks like "right to left" fill  | Why check `dp[j]` not `dp[i-j]`   |

---

## 🧠 Summary

Both methods solve the same problem with the same logic underneath. The main difference is **how the state is computed**:
- In top-down, you let recursion guide the exploration and memoize results.
- In bottom-up, you iteratively build the answer using previously solved states.

If memory or stack depth is a concern, or you want better performance with short dictionary words, go with the bottom-up optimized version.

