# 🧮 Count and Say (Leetcode #38)

[Problem Link](https://leetcode.com/problems/count-and-say/)

### Problem Statement

The **Count and Say** sequence is defined as follows:

- `countAndSay(1) = "1"`
- For each `n > 1`, `countAndSay(n)` is the **run-length encoding (RLE)** of `countAndSay(n - 1)`

**Run-length encoding (RLE)** means compressing consecutive identical characters into a count followed by the character.

For example:
- `"3322251"` → `"23321511"`  
  - Two 3s → "23"  
  - Three 2s → "32"  
  - One 5 → "15"  
  - One 1 → "11"

---

### Examples

**Input:** `n = 4`  
**Output:** `"1211"`  

**Explanation:**
```
countAndSay(1) = "1"
countAndSay(2) = "11"     (one 1)
countAndSay(3) = "21"     (two 1s)
countAndSay(4) = "1211"   (one 2, one 1)
```

---

### Approach

We build the result **iteratively** starting from `"1"`.  
At each step, we read the previous string and apply **run-length encoding**.

1. Start with `res = "1"` (base case)  
2. Loop from `2` to `n`  
3. For each group of same digits in `res`, count them and append `count + digit` to `curr`  
4. Assign `res = curr` for the next iteration

---

### Time and Space Complexity

- **Time Complexity:** `O(n * m)`  
  `n` is the number of iterations (from 1 to `n`)  
  `m` is the average length of intermediate strings  
  The string length grows roughly 30–40% per iteration

- **Space Complexity:** `O(m)`  
  At any point we only store two strings: current and previous

---

### Code (C++)

```
class Solution {
public:
    string countAndSay(int n) {
        string res = "1";
        for (int i = 2; i <= n; i++) {
            string curr;
            curr.reserve(res.size() * 2);  // optional optimization

            int j = 0;
            while (j < res.size()) {
                char c = res[j];
                int cnt = 1;

                while (j + 1 < res.size() && res[j + 1] == c) {
                    cnt++;
                    j++;
                }

                curr += to_string(cnt) + c;
                j++;
            }

            res = curr;
        }
        return res;
    }
};
```

---

### Summary

| Property          | Value                    |
|------------------|--------------------------|
| Input constraint | `1 <= n <= 30`           |
| Type             | Iterative String Building|
| Technique        | Run-length Encoding (RLE)|
| Time Complexity  | `O(n * m)`               |
| Space Complexity | `O(m)`                   |
| Optimized With   | `string.reserve()`       |
