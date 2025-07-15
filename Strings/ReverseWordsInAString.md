# 📝 Reverse Words in a String — Full Notes (Markdown)

[Problem Link](https://leetcode.com/problems/reverse-words-in-a-string/)

## 🚩 Problem Statement

Given a string `s`, reverse the **order of the words**.

- A word is a **sequence of non-space characters**.
- Words are separated by **one or more spaces**.
- Return a string with:
  - Words in **reverse order**
  - Exactly **one space** between them
  - **No** leading or trailing spaces

---

### 💡 Examples

**Input:**  
`s = "the sky is blue"`  
**Output:**  
`"blue is sky the"`

**Input:**  
`s = "  hello world  "`  
**Output:**  
`"world hello"`

**Input:**  
`s = "a good   example"`  
**Output:**  
`"example good a"`

---

## 💭 Intuition

To reverse the words with **O(1) extra space**, we follow these steps:

1. **Remove Extra Spaces In-Place**
   - Eliminate leading/trailing spaces
   - Convert multiple spaces between words into a single space

2. **Reverse the Entire String**

3. **Reverse Each Word in Place**
   - This restores the correct order of characters inside each reversed word

---

## 🧪 Time and Space Complexity

- **Time:** O(n)
- **Space:** O(1) extra (in-place on mutable string)

---

## ✅ Final C++-Style Code (in Markdown)

#include <string>
#include <algorithm>

void removeExtraSpaces(std::string& s) {
int n = s.size();
int i = 0, j = 0;

cpp
Copy
Edit
// Skip leading spaces
while (j < n && s[j] == ' ') j++;

bool spaceSeen = false;
while (j < n) {
    if (s[j] != ' ') {
        if (spaceSeen) {
            s[i++] = ' ';
            spaceSeen = false;
        }
        s[i++] = s[j++];
    } else {
        spaceSeen = true;
        j++;
    }
}

// Remove trailing space if any
s.resize(i > 0 && s[i - 1] == ' ' ? i - 1 : i);
}

void reverseWords(std::string& s) {
// Step 1: Clean up spaces
removeExtraSpaces(s);

pgsql
Copy
Edit
// Step 2: Reverse the whole string
std::reverse(s.begin(), s.end());

// Step 3: Reverse each word
int n = s.size();
int start = 0;

for (int end = 0; end <= n; ++end) {
    if (end == n || s[end] == ' ') {
        std::reverse(s.begin() + start, s.begin() + end);
        start = end + 1;
    }
}
}

yaml
Copy
Edit

---

## ❓ Doubts You Asked (and Clarified)

### Q1: **Is `std::string` mutable in C++?**
> ✅ Yes. You can modify characters, append, delete, and insert. `std::string` is mutable unless declared `const`.

---

### Q2: **How does space removal work in-place?**

We use two pointers (`i` for writing, `j` for reading).  
Logic:
- Skip leading spaces
- Insert **at most one** space between words
- Never insert a trailing space
- If a trailing space sneaks in, `s.resize(i - 1)` trims it

---

### Q3: **What if we have `"  spacehello  "`? Won’t we end up with trailing spaces?**

> ❌ No. Here's why:
- We only insert a space **before a word**
- If there's no word after the last space, no space is added
- Worst case: a **single trailing space**, which gets trimmed by `resize(i - 1)`

---

### Q4: **Can two spaces remain at the end?**

> ❌ Impossible with this logic.

- The code ensures **only one space** is added between words
- After the last word, `spaceSeen == true`, but **no word follows**, so **no space is written**
- If a trailing space gets written by mistake, it's trimmed at the end

---

### Q5: **Is the loop with `reverse()` inside O(n²)?**

> ❌ No.

Each character:
- Is reversed **once** in the full string
- Is reversed **once** in its own word
- No character is reversed more than twice

So total time remains **O(n)**

---

## ✅ Summary

- Clean spaces in-place
- Reverse the entire string
- Reverse each word individually
- Done in **O(n) time** and **O(1) extra space**
---
