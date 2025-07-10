# 🔄 Minimum Bracket Reversals – Full Explanation, Code, and Doubt Clarifications

[Article Link GFG](https://www.geeksforgeeks.org/dsa/minimum-number-of-bracket-reversals-needed-to-make-an-expression-balanced/)

---

## 📘 Problem Statement

You are given a string `s` consisting only of `{` and `}`. In one move, you can **reverse** any bracket:
- Change `{` to `}`
- Change `}` to `{`

Your task is to return the **minimum number of reversals** needed to make the string **balanced**. If it’s **not possible**, return `-1`.

---

## ❗ Key Constraint

- The length of the string **must be even** to be balanced.
  - Odd-length strings → always impossible → return `-1`.

---

## 🔁 Stack-Based Approach (O(n) Time & Space)

### 🧠 Idea

Use a **stack** to remove matched `{}` pairs. The remaining unmatched brackets will always be in the form:

```
}}}}...{{{{      // Some unmatched '}' followed by some unmatched '{'
```

We then compute how many **reversals** are needed to balance the remaining brackets.

### 🧮 Reversal Formula

If:
- `m = number of unmatched '}'`
- `n = number of unmatched '{'`

Then the number of reversals is:
```
ceil(m / 2) + ceil(n / 2)
```

Or in optimized integer math:
```
(m + n) / 2 + (n % 2)
```

### ✅ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int countMinReversals(string expr)
{
    int len = expr.length();

    // Odd-length strings can't be balanced
    if (len % 2 != 0)
        return -1;

    stack<char> s;
    for (int i = 0; i < len; i++) {
        if (expr[i] == '}' && !s.empty() && s.top() == '{') {
            s.pop(); // balanced pair
        } else {
            s.push(expr[i]);
        }
    }

    // Now stack has only unmatched brackets
    int red_len = s.size();

    // Count how many opening brackets are in the stack
    int n = 0;
    while (!s.empty() && s.top() == '{') {
        s.pop();
        n++;
    }

    // red_len = m + n, so:
    return (red_len / 2 + n % 2);
}
```

---

## 🔥 Optimized Counter-Based Approach (O(n) Time, O(1) Space)

### 🧠 Idea

We don’t need a stack — we can just **count** unmatched braces while traversing.

- `left_brace`: counts unmatched `'{'`
- `right_brace`: counts unmatched `'}'` that couldn’t match a `'{'`

### ✅ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int countMinReversals(string expr)
{
    int len = expr.length();
    if (len % 2 != 0) return -1;

    int left_brace = 0, right_brace = 0;

    for (int i = 0; i < len; i++) {
        if (expr[i] == '{') {
            left_brace++;
        } else {
            if (left_brace == 0) {
                right_brace++;
            } else {
                left_brace--;
            }
        }
    }

    return ceil(left_brace / 2.0) + ceil(right_brace / 2.0);
}
```

---

## ❓ Doubts You Had — Fully Explained

---

### ❓ **“Why doesn’t just counting number of `{` and `}` work?”**

✅ Because ordering matters.  
Example:  
```text
"}{"  → 1 '{' and 1 '}' (same count) but NOT balanced.
```

Balanced means:
- At every point in the string, you **must not have more `}` than `{`**.

---

### ❓ **“Why use `ceil()`? Why not just divide by 2?”**

Because if you have an **odd number** of unmatched braces, one will always be left out and need an extra reversal.

Example:
- `{{{` → needs 2 reversals (`ceil(3/2) = 2`)
- `}}}` → also needs 2

---

### ❓ **“Why `n % 2` in formula? Why not `m % 2`?”**

You can use **either** — they’re **always equal** because `m + n` is even.

The formula:
```cpp
(m + n) / 2 + (n % 2)
```
works because:
- If both `m` and `n` are odd → 1 extra reversal is needed
- Since `m % 2 == n % 2` when total is even, using `n % 2` is enough

It’s written as `n % 2` just for **convenience**, since we count `n` directly.

---

## 🧪 Example Walkthrough

Input: `"}{{}}{{{"`

**After removing matches:**
- 3 unmatched `'{'`
- 1 unmatched `'}'`

Reversals:
```
ceil(3 / 2) + ceil(1 / 2) = 2 + 1 = 3
```

✅ Answer: `3`

---

## 🧠 Complexity

| Approach      | Time Complexity | Space Complexity |
|---------------|-----------------|------------------|
| Stack-based   | O(n)            | O(n)             |
| Counter-based | O(n)            | O(1)             |

---

## ✅ Conclusion

- Use **stack** for clearer logic, or
- Use **counters** for optimal space
- Formula is:
  ```
  ceil(left / 2) + ceil(right / 2)
  ```

---

