# 🔁 Longest Palindromic Substring (Leetcode #5)

[Problem Link](https://leetcode.com/problems/longest-palindromic-substring/description/)

### 📝 Problem Statement

Given a string `s`, return the **longest palindromic substring** in `s`.

---

### 🧪 Examples

**Input:** `s = "babad"`  
**Output:** `"bab"` (or `"aba"`)

**Input:** `s = "cbbd"`  
**Output:** `"bb"`

---

### 🚫 Initial (Incorrect) Attempt

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        if(s.size()==1) return 1;
        int maxCount = INT_MIN;
        for(int i = 0 ; i < s.size(); i++){
            int k = i - 1;
            int l = i + 1;
            int count = 0;
            while(k >= 0 && l < s.size()){
                if(s[k] == s[j]) count += 2;
                else break;
            }
            maxCount = max(maxCount, count + 1);
        }
        return maxCount;
    }
};
```

### ❌ Issues in Above Code

- ❌ Returning `int` instead of the required `string`
- ❌ Typo: used `s[j]` which is undefined; should be `s[l]`
- ❌ Missing updates to `k--` and `l++` inside the while loop
- ❌ Not handling **even-length** palindromes
- ❌ Only tracking count, not actual substring
- ❌ Base case `s.size() == 1` returned `1` (int), not `"s"` (string)

---

### ✅ Final Correct Approach — Expand Around Center

We expand around all possible palindrome centers.

There are `2n - 1` centers in a string of length `n`:
- `n` single characters (odd-length centers)
- `n - 1` between-character gaps (even-length centers)

At each center, we expand outward while the characters match.

---

### 💻 Final C++ Code

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.size() == 1) return s;

        int start = 0, maxLen = 1;

        for (int i = 0; i < s.size(); i++) {
            // Odd-length palindrome
            expand(s, i, i, start, maxLen);
            // Even-length palindrome
            expand(s, i, i + 1, start, maxLen);
        }

        return s.substr(start, maxLen);
    }

    void expand(const string& s, int left, int right, int& start, int& maxLen) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            int len = right - left + 1;
            if (len > maxLen) {
                maxLen = len;
                start = left;
            }
            left--;
            right++;
        }
    }
};
```

---

### ⏱ Time & Space Complexity

#### Time Complexity: `O(n²)`
- There are `2n - 1` possible centers.
- For each center, the while-loop expands up to `O(n)` in worst case.
- So total time = `O(n × n) = O(n²)`

#### Space Complexity: `O(1)`
- No extra memory is used except integer counters and string slicing.
- Result is returned using `s.substr()`.

---

### ✅ Summary

| Component                | Value                  |
|--------------------------|------------------------|
| Time Complexity          | `O(n²)`                |
| Space Complexity         | `O(1)`                 |
| Handles Odd/Even Lengths | ✅ Yes                 |
| Method Used              | Expand Around Center   |
| Optimized From           | Incorrect 2-pointer    |

---

### 🧠 Takeaways

- Palindromes are best found by **expanding from the middle**, not checking all substrings.
- Avoid using `.substr()` to check each substring — it's inefficient.
- Make sure you’re tracking both **length** and **starting index** to extract the correct substring.
