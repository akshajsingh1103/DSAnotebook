# 🔤 Subsequence vs Permutation — Generate All (C++)

This file covers the difference between **subsequences** and **permutations** of a string, how to generate both using backtracking, and the time/space complexities.

---

## 🧠 Subsequence vs Permutation — What’s the Difference?

| Feature              | Subsequence                             | Permutation                      |
|----------------------|------------------------------------------|----------------------------------|
| Use all characters?  | ❌ No                                    | ✅ Yes                           |
| Keep original order? | ✅ Yes                                   | ❌ No                            |
| Allows skipping?     | ✅ Yes                                   | ❌ No                            |
| Allows reordering?   | ❌ No                                    | ✅ Yes                           |
| Empty string valid?  | ✅ Yes                                   | ❌ No                            |
| Total combinations   | `2^n`                                    | `n!` (if characters are unique)  |

---

## 📌 1. Print All Subsequences of a String

A **subsequence** is a subset of characters with preserved **relative order**.

### ✅ Example

For string `"abc"`:

All subsequences:  
`["", "a", "b", "c", "ab", "ac", "bc", "abc"]`

### 💻 C++ Code

```cpp
class Solution {
public:
    vector<string> allSubsequences(string s) {
        vector<string> result;
        string curr = "";
        backtrack(0, s, curr, result);
        return result;
    }

    void backtrack(int i, string& s, string& curr, vector<string>& result) {
        if (i == s.size()) {
            result.push_back(curr);
            return;
        }

        // Include current character
        curr.push_back(s[i]);
        backtrack(i + 1, s, curr, result);

        // Exclude current character (backtrack)
        curr.pop_back();
        backtrack(i + 1, s, curr, result);
    }
};
```

### ⏱ Time and Space Complexity

- **Time:** `O(2^n × n)` — 2^n subsequences, each up to n length
- **Space:** `O(2^n × n)` — storing all subsequences

---

## 📌 2. Print All Permutations of a String

A **permutation** uses **all characters** and reorders them in all possible ways.

### ✅ Example

For string `"abc"`:

All permutations:  
`["abc", "acb", "bac", "bca", "cab", "cba"]`

### 💻 C++ Code

```cpp
class Solution {
public:
    vector<string> allPermutations(string s) {
        vector<string> result;
        permute(0, s, result);
        return result;
    }

    void permute(int index, string& s, vector<string>& result) {
        if (index == s.size()) {
            result.push_back(s);
            return;
        }

        for (int i = index; i < s.size(); ++i) {
            swap(s[i], s[index]);
            permute(index + 1, s, result);
            swap(s[i], s[index]); // backtrack
        }
    }
};
```

### ⏱ Time and Space Complexity

- **Time:** `O(n!)` — all permutations of n characters
- **Space:** `O(n!) × n` — storing all permutations (each of length n)

---

## 📝 Summary Table

| Operation     | Uses All Chars | Allows Reorder | Total Outputs | Time Complexity | Space Complexity |
|---------------|----------------|----------------|----------------|------------------|-------------------|
| Subsequences  | ❌             | ❌             | `2^n`          | `O(2^n × n)`     | `O(2^n × n)`       |
| Permutations  | ✅             | ✅             | `n!`           | `O(n!)`          | `O(n! × n)`        |

---

## 💡 Notes

- **Subsequences** form the **power set** of characters in order
- **Permutations** rearrange all characters in every possible way
- Both can be solved with **backtracking**
- Don’t confuse **substrings** (continuous) with subsequences

---
