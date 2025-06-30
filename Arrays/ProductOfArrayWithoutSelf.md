# 🔄 Product of Array Except Self (Efficient Solution)

## 🧩 Problem Statement

[Problem Link](https://leetcode.com/problems/product-of-array-except-self/)

Given an array `nums` of `n` integers, return an array `res` such that:

```
res[i] = product of all elements in nums except nums[i]
```

Constraints:
- Must run in **O(n)** time
- Cannot use division
- Must use **O(1) extra space**

---

## ✅ Optimal Approach: Prefix & Suffix Products (Without Arrays)

### 🔧 Intuition

At index `i`, the answer is:
```
res[i] = product of all elements before i (prefix)
         ×
         product of all elements after i (suffix)
```

But instead of creating two full arrays (`prefix[]` and `suffix[]`), we use:
- One result array `res[]` (which we’re required to return)
- Two variables: `left` and `right` to simulate the prefix and suffix values on-the-fly

---

## 📌 Key Observations

### 1. Leftmost and Rightmost Multipliers Are 1

- For index `0` (first element), there is **no prefix**, so we set:
  ```
  res[0] = 1
  ```
- For index `n-1` (last element), there is **no suffix**, so:
  ```
  multiply res[n-1] by 1 in second pass
  ```

This is logically sound because the **product of zero elements is defined as 1**, which acts as a multiplicative identity:
```
∏(empty set) = 1
```

---

## 🛠️ Implementation Steps

### 🔹 First Pass (Left to Right)
- Build `res[i]` such that it holds **product of all elements before i**
```cpp
res[i] = left
left *= nums[i]
```

### 🔹 Second Pass (Right to Left)
- Multiply `res[i]` by the **product of all elements after i**
```cpp
res[i] *= right
right *= nums[i]
```

---

## 💻 Final Code (C++)

```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n, 1);

    int left = 1;
    for (int i = 0; i < n; i++) {
        res[i] = left;
        left *= nums[i];
    }

    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        res[i] *= right;
        right *= nums[i];
    }

    return res;
}
```

---

## 🧠 Why `res[]` Does Not Count Toward Space Complexity

Even though `res[]` has size `n`, it **does not count** toward space complexity because:
- It is the **required output array**
- Space complexity only considers **extra space used in addition to the output**

This is a **standard assumption** in algorithm problems (especially on LeetCode).

---

## ⏱️ Time and Space Complexity

| Metric            | Value         |
|-------------------|---------------|
| Time Complexity    | O(n)          |
| Space Complexity   | O(1) extra ✅ |
| Division Used?     | ❌ No         |
| Handles Zero?      | ✅ Yes        |

---

## ✅ Example

For:
```cpp
nums = [1, 2, 3, 4]
```

Pass 1: prefix products:
```
res = [1, 1, 2, 6]
```

Pass 2: suffix products:
```
res = [24, 12, 8, 6]
```

---

## 🧠 Final Notes

- This approach avoids division and avoids using full prefix/suffix arrays.
- Instead of computing all prefix/suffix values up front, we **accumulate them on the fly**.
- By using only two scalar variables (`left`, `right`), we maintain **O(1)** space.

---

**End of Explanation**
