# Remove Duplicate Letters

[Problem Link](https://leetcode.com/problems/remove-duplicate-letters/submissions/1684212854/)

Given a string `s`, remove duplicate letters so that every letter appears **once and only once**. You must make sure your result is the **smallest in lexicographical order** among all possible results.

---

### 🧠 Problem Summary

- You are given a string `s` of lowercase letters.
- Remove duplicate letters so that:
  - Each letter appears **once**.
  - The result is **lexicographically smallest** among all valid results.

---

### ✅ Approach: Greedy + Stack

We use a greedy algorithm combined with a stack-like structure (a string) to build the result:

1. **Frequency Count**: Count how many times each character appears.
2. **Used Tracker**: Keep track of which characters are already in the result.
3. **Greedy Construction**:
   - For each character:
     - If it's already in the result, skip it.
     - Otherwise, while the current character is smaller than the last character in the result and the last character still appears later, pop it from the result.
     - Add the current character to the result.

This ensures:
- Each character appears only once.
- The lexicographical order is maintained.

---

### 🧪 Example

#### Input:  
`s = "cbacdcbc"`

#### Output:  
`"acdb"`

#### Why?  
- We remove duplicates.
- "acdb" is the smallest possible string in lexicographical order that contains all unique letters from the original string.

---

### 🧾 Code (C++)

```cpp
class Solution {
public:
    string removeDuplicateLetters(string s) {
        vector<int> freq(26, 0);     // Frequency of each character
        vector<bool> used(26, false); // To track what's already in result
        string result = "";

        // Count frequency of each character
        for (char c : s) {
            freq[c - 'a']++;
        }

        for (char c : s) {
            freq[c - 'a']--; // We're now seeing one less of this char

            if (used[c - 'a']) continue; // Already in result? Skip it

            // While: last char in result > current char, and can appear again later
            while (!result.empty() && 
                   c < result.back() && 
                   freq[result.back() - 'a'] > 0) {
                used[result.back() - 'a'] = false; // Mark as not used
                result.pop_back(); // Remove it from result
            }

            result += c;
            used[c - 'a'] = true;
        }

        return result;
    }
};
```

---

### ⏱️ Complexity

Time: O(n) — Each character is pushed and popped at most once.
Space: O(1) — Uses fixed-size arrays for frequency and tracking (since alphabet size is constant at 26).

---

