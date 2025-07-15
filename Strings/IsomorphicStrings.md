# 🧠 Character Indexing in Arrays 

[Problem Link](https://leetcode.com/problems/isomorphic-strings/description/)

This guide explains how and why characters can be used as array indexes in C++, especially in problems like **"Isomorphic Strings"**, where we replace hash maps with fixed-size arrays for better performance.

---

## 🔍 The Question

> **How are we using characters as indexes in an array? Aren’t arrays indexed by integers?**

Yes, arrays in C++ *are* indexed by integers — but **characters are just small integers under the hood**.

---

## 🧬 Characters in C++

In C++:

- A `char` is just an 8-bit integer (range: 0–255).
- It holds the ASCII code of the character.

```cpp
char ch = 'e';   // ch = 101 in ASCII
int index = ch;  // index = 101
```

So this is perfectly legal and common:

```cpp
int arr[256];
arr['e'] = 42;   // Same as arr[101] = 42
```

---

## ✅ Why This Works in Practice

### Example: Isomorphic Strings (Optimized)

```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        int mapS[256] = {0};
        int mapT[256] = {0};

        for (int i = 0; i < s.length(); i++) {
            if (mapS[s[i]] != mapT[t[i]]) return false;

            mapS[s[i]] = i + 1;
            mapT[t[i]] = i + 1;
        }

        return true;
    }
};
```

### What’s happening?

- `s[i]` is a `char`. So is `t[i]`.
- When we do `mapS[s[i]]`, C++ treats `s[i]` as an `int` (its ASCII value).
- `mapS['e']` → `mapS[101]`
- That’s just indexing the 101st element in a 256-element array — valid!

---

## 🔢 ASCII Mapping Table (Samples)

| Character | ASCII Value |
|-----------|-------------|
| `'a'`     | 97          |
| `'z'`     | 122         |
| `'A'`     | 65          |
| `'0'`     | 48          |

---

## 🧪 Mini Demo

```cpp
#include <iostream>
using namespace std;

int main() {
    int map[256] = {0};
    char ch = 'A';

    map[ch] = 123;
    
    cout << "map['A'] = " << map['A'] << endl; // 123
    cout << "map[65] = " << map[65] << endl;   // Also 123

    return 0;
}
```

### Output:
```
map['A'] = 123
map[65] = 123
```

---

## 💡 Why Use Arrays Instead of Maps?

| Feature       | `unordered_map<char, int>` | `int[256]`         |
|---------------|----------------------------|--------------------|
| Lookup time   | O(1) average (but slower)  | True O(1) constant |
| Space         | Variable                   | Fixed (256 ints)   |
| Performance   | Good                       | **Faster**         |
| Clarity       | High                       | Medium             |

✅ Use arrays when:
- You’re working with **ASCII characters** only.
- You care about **speed and memory predictability**.

---

## 🧠 Summary

- `char` in C++ is just an integer type.
- So `arr['e']` is just `arr[101]`.
- Arrays indexed by characters work great — especially for problems like **isomorphic strings**, **anagrams**, **frequency maps**, etc.
- Just make sure the array is large enough (usually 256 for ASCII).

---
