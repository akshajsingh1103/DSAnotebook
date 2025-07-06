# 🧩 `minInsertions` Parentheses Problem — Analysis & Comparison
[Problem Link](https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/description/)
## 📌 Problem Statement

Given a string `s` containing only `'('` and `')'`, we need to make the string valid by inserting the **minimum number of characters**. A string is valid when:

- Every `'('` is **matched with exactly two `')'`**
- The two `')'` must be **consecutive** and **after** the `'('`

---

## 🔁 Your Original Code (Right-to-Left Logic)

```cpp
class Solution {
public:
int minInsertions(string s) {
int insertions = 0;
int needClose = 0;

    for(int i = s.length() - 1; i >= 0; i--) {
        if(s[i] == ')') {
            needClose++;
        } else { // s[i] == '('
            if (needClose >= 2) {
                needClose -= 2;
            } else {
                insertions += (2 - needClose);
                needClose = 0;
            }
        }
    }

    insertions += (needClose / 2) + (needClose % 2) * 2;
    return insertions;
}
```

---

## ✅ Correct Code (Left-to-Right Logic)

```cpp
class Solution {
public:
int minInsertions(string s) {
int res = 0; // total insertions
int need = 0; // number of ')' needed

    for (int i = 0; i < s.length(); ++i) {
        if (s[i] == '(') {
            need += 2;
            if (need % 2 != 0) {
                res++;    // insert one ')'
                need--;   // now we need an even number
            }
        } else { // s[i] == ')'
            need--;
            if (need < 0) {
                res++;   // insert one '('
                need = 1; // this new '(' needs one more ')'
            }
        }
    }

    return res + need;
}
};
```


---

## ❗ Why Your Method Breaks

### 🔍 Key Problem:
Your method counts total `')'` from the end, but **does not verify** if `')'` characters actually appear in **pairs**. This matters because a single `')'` is invalid — we need exactly two `')'` **after** each `'('`.

---

## ⚔️ Comparison: Right-to-Left vs Left-to-Right

| Feature | Your Method (Right to Left) | Correct Method (Left to Right) |
|--------|-----------------------------|-------------------------------|
| Direction | Right to Left | Left to Right |
| Handles `"))"` grouping? | ❌ No | ✅ Yes |
| Checks adjacency of `')'`? | ❌ No | ✅ Yes |
| Tracks incomplete `')'`? | ❌ No | ✅ Yes |
| Edge case-safe? | ❌ No (fails cases like `"())"`) | ✅ Yes |
| Logic clarity | Medium | Clear and intuitive |
| Insertion logic | Assumes `)` balance via count | Handles real-time pairing and imbalance |

---
## 💡 Final Takeaway

Your method tries to balance `')'` count, but misses **valid grouping** of `"))"` and positional order. The left-to-right method correctly tracks:

- Open `'('` waiting for two `')'`
- Whether `')'` are in valid pairs
- When insertions are needed for unmatched pieces

Stick to left-to-right and always validate if `')'` are in pairs!
---
