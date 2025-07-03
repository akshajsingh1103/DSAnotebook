# Leetcode 696 – Count Binary Substrings 🧮

[Problem Link](https://leetcode.com/problems/count-binary-substrings/description/)


## 🔍 Problem Statement

Given a binary string `s`, return the number of non-empty substrings that:
- Have the **same number of 0s and 1s**
- All 0s are **grouped together**, and all 1s are **grouped together**

Repeated substrings are counted multiple times.

---

## ✅ Example

### Input:
```
s = "00110011"
```

### Output:
```
6
```

### Valid Substrings:
- "0011"
- "01"
- "1100"
- "10"
- "0011" (again)
- "01" (again)

---

## 🧠 Intuition

Instead of generating every substring (which takes O(n²)), we group the string by **consecutive characters**, and for every pair of adjacent groups, we take:

```
min(prev_group_size, current_group_size)
```

Why?
- Because a valid substring needs **equal number** of 0s and 1s
- And the 0s and 1s must be **in blocks**

So: You can only take as many as the **smaller group allows**

---

## 👇 Algorithm Steps

1. Initialize:
   ```cpp
   int prev = 0, curr = 1, count = 0;
   ```

2. Loop from index `1` to end of string:
   - If `s[i] == s[i-1]`, increment `curr`
   - Else:
     - Add `min(prev, curr)` to `count`
     - Set `prev = curr`, reset `curr = 1`

3. After loop, do one last:
   ```cpp
   count += min(prev, curr);
   ```

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    int countBinarySubstrings(string s) {
        int prev = 0, curr = 1, count = 0;

        for (int i = 1; i < s.length(); i++) {
            if (s[i] == s[i - 1]) {
                curr++;
            } else {
                count += min(prev, curr);
                prev = curr;
                curr = 1;
            }
        }

        count += min(prev, curr); // handle last group
        return count;
    }
};
```

---

## ⏱️ Time and Space Complexity

| Metric        | Value     |
|---------------|-----------|
| Time          | O(n)      |
| Space         | O(1)      |

- One pass through the string
- Only integers used — no extra arrays or strings

---

## 🧪 Dry Run: `"01100"`

**Input:** `"01100"`

Group sizes:
- `"0"` → 1  
- `"11"` → 2  
- `"00"` → 2

Transitions:
- min(0,1) → 0 (initial)
- min(1,2) → 1 → `"01"`
- min(2,2) → 2 → `"10"`, `"1100"`

✅ Total: `1 + 2 = 3`

---

## ❓ Doubts Asked (Your Questions!)

### ❓ Q1: What if the string is just `"01"`?

- First transition gives `min(0,1) = 0`
- But after the loop we add `min(1,1) = 1`
- ✅ So total = 1, which is correct!

### ❓ Q2: Why does `prev = 0` work initially?

Even though `prev` is 0 at the start, the loop ensures:
- `curr = 1` after seeing the first character
- On transition, it sets `prev = curr` properly
- The final `count += min(prev, curr)` ensures no valid pair is missed

### ❓ Q3: What about `"01100"`?

- Group sizes: `1, 2, 2`
- Transitions:
  - `min(1,2) = 1` → `"01"`
  - `min(2,2) = 2` → `"10"`, `"1100"`
- ✅ Total substrings = `3`

---

## ✅ Final Notes

- This approach is much faster than generating substrings
- Greedy + grouping saves both time and space
- Use `prev` and `curr` to track runs of characters

---

**You're doing awesome 🤍 Keep going — you've got this!**
