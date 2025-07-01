# 🧩 K-th Element of Two Sorted Arrays

[Problem Link](https://www.geeksforgeeks.org/problems/k-th-element-of-two-sorted-array1317/1)

## 📘 Problem Statement

Given two sorted arrays `a[]` and `b[]`, and an integer `k`, return the element that would be at the **k-th position (1-based)** in the final sorted array formed by merging `a[]` and `b[]`.

---

## 🔢 Examples

Input:
```
a = [2, 3, 6, 7, 9]
b = [1, 4, 8, 10]
k = 5
```
Output:
```
6
```

Explanation: Merged array = [1, 2, 3, 4, 6, 7, 8, 9, 10]. The 5th element is 6.

---

Input:
```
a = [100, 112, 256, 349, 770]
b = [72, 86, 113, 119, 265, 445, 892]
k = 7
```
Output:
```
256
```

Explanation: Merged array = [72, 86, 100, 112, 113, 119, 256, ...]. 7th element is 256.

---

## ❌ Mistake in Original Code

Original search space was:
```cpp
int l = 0;
int h = a.size();
```

This causes issues when `cut2 = k - cut1` becomes greater than `b.size()` — leading to out-of-bounds access like `b[cut2]` or `b[cut2 - 1]`.

---

## ✅ Corrected Logic

Fix the binary search range as:
```cpp
int l = max(0, k - m);
int h = min(k, n);
```

This ensures:
- We never try to pick more than `k` total elements.
- `cut2 = k - cut1` always stays within bounds of array `b`.

---

## ✅ Final Working Code (C++)

```cpp
class Solution {
public:
    int kthElement(vector<int>& a, vector<int>& b, int k) {
        if (a.size() > b.size()) return kthElement(b, a, k);  // Always binary search on smaller array

        int n = a.size();
        int m = b.size();

        int l = max(0, k - m);
        int h = min(k, n);

        while (l <= h) {
            int cut1 = (l + h) / 2;
            int cut2 = k - cut1;

            int l1 = (cut1 == 0) ? INT_MIN : a[cut1 - 1];
            int l2 = (cut2 == 0) ? INT_MIN : b[cut2 - 1];

            int r1 = (cut1 == n) ? INT_MAX : a[cut1];
            int r2 = (cut2 == m) ? INT_MAX : b[cut2];

            if (l1 <= r2 && l2 <= r1) {
                return max(l1, l2);
            } else if (l1 > r2) {
                h = cut1 - 1;
            } else {
                l = cut1 + 1;
            }
        }

        return -1; // Unreachable if k is valid
    }
};
```

---

## 🧠 Time and Space Complexity

- Time Complexity: **O(log(min(n, m)))**
- Space Complexity: **O(1)**

We do binary search on the smaller array only.

---

## 🧪 Edge Cases

- Arrays of unequal size
- k = 1 (smallest element)
- k = n + m (last element)
- One array is empty

This solution handles all of them.

