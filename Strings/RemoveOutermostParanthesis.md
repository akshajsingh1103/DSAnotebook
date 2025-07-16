# 🧠 LeetCode: Remove Outermost Parentheses

## 🔗 Problem Link
[Remove Outermost Parentheses – LeetCode](https://leetcode.com/problems/remove-outermost-parentheses/)

---

## 📄 Problem Description

A valid parentheses string is either:

- `""` (empty string),
- `"(" + A + ")"` (a valid string surrounded by a pair of `()`),
- or `"A + B"` (two valid strings concatenated).

Examples of valid strings:
- `""`
- `"()"`
- `"(())"`
- `"(()(()))"`

A **primitive** valid parentheses string is:
- Non-empty,
- Cannot be split into two non-empty valid strings.

---

### 🎯 Objective:
Given a valid parentheses string `s`, **remove the outermost parentheses** from **each primitive substring** in its decomposition.

### 📥 Example 1:
- Input: `"(()())(())"`
- Primitive decomposition: `"(()())"` + `"(())"`
- After removing outermost parens: `"()()" + "()" = "()()()"`

### 📥 Example 2:
- Input: `"(()())(())(()(()))"`
- Output: `"()()()()(())"`

### 📥 Example 3:
- Input: `"()()"`
- Output: `""` (each `"()"` is primitive; outermost removed = empty)

---

## 🧠 Key Concepts

### ✅ What is `depth`?
- A counter to track **how many open parentheses** are currently unmatched.
- Increases when you see `'('`.
- Decreases when you see `')'`.

### ✅ Why use `depth`?
To know:
- When we're at the **first `(`** of a primitive (`depth == 0`)
- When we're at the **last `)`** of a primitive (depth becomes 0 after decrement)

We **skip**:
- First `'('` of a primitive → when `depth == 0` before adding.
- Last `')'` of a primitive → when `depth == 0` **after** decrementing.

---

## 🤔 Doubts You Had (And Clarified)

### ❓ Q: Why skip when `depth == 0` after `)`?
👉 Because that means we just **closed** the first unmatched `(` of the primitive — i.e., we reached the **last `)`** of the primitive. So we skip it.

---

## 🔄 Step-by-Step Dry Run

For input: `"(()())"`

| Char | depth (before) | Action                          | depth (after) | Result  |
|------|----------------|----------------------------------|----------------|---------|
| `(`  | 0              | ❌ Skip (first of primitive)     | 1              |         |
| `(`  | 1              | ✅ Add                           | 2              | `'('`   |
| `)`  | 2              | ✅ Add                           | 1              | `'()'`  |
| `(`  | 1              | ✅ Add                           | 2              | `'()('` |
| `)`  | 2              | ✅ Add                           | 1              | `'()()'`|
| `)`  | 1              | ❌ Skip (last of primitive)      | 0              | `'()()'`|

---

## ✅ C++ Solution (Official depth method)

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string res;
        int depth = 0;

        for (char ch : s) {
            if (ch == '(') {
                if (depth > 0) res += ch; // Skip outermost (
                depth++;
            } else {
                depth--;
                if (depth > 0) res += ch; // Skip outermost )
            }
        }

        return res;
    }
};
```
---

## Time and Space Complexity
- ⏱ **Time Complexity**: O(n) – each character is processed once.
- 📦 **Space Complexity**: O(n) – in the worst case, we store almost all characters in the result.
