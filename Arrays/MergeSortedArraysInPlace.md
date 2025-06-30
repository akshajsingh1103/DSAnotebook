# 🔄 Merge Sorted Arrays (In-Place)

## 🧠 Problem Statement

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

`nums1` has a length of `m + n`, where the first `m` elements denote the valid elements, and the rest are `0`s to accommodate elements from `nums2`.

Your task is to **merge `nums2` into `nums1`** as one sorted array, in-place.

---

## 📌 Example

**Input:**  
nums1 = [1,2,3,0,0,0], m = 3  
nums2 = [2,5,6], n = 3

**Output:**  
[1,2,2,3,5,6]

---

## 💡 Intuition

- Since both arrays are already sorted, we can use a **two-pointer technique** starting from the **back** of both arrays.
- Place the larger of the two elements at the end of `nums1` (which has extra space).
- Decrease pointers as we place elements.
- If elements remain in `nums2` after one list is exhausted, copy them to the front of `nums1`.

---

## ⚠️ Edge Cases Handled

1. If `n == 0`, just return — nothing to merge.
2. If `m == 0`, simply assign `nums2` to `nums1`.

---

## ⏱️ Time & Space Complexity

| Metric              | Value           |
|---------------------|------------------|
| Time Complexity     | O(m + n)         |
| Space Complexity    | O(1) (in-place)  |

---

## 🧾 Final C++ Code

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        if(n==0) return ;
        if(m==0){
            nums1=nums2 ;
            return ;
        }
        int l= m-1 ;
        int r = n-1 ;
        int k = m+n-1 ;
        while(l>=0 && r >=0){
            if(nums1[l]>nums2[r]){
                nums1[k]=nums1[l];
                k--;
                l--;
            }else{
                nums1[k] = nums2[r];
                k--;
                r--;
            }
        }while(r>=0){
            nums1[k]=nums2[r];
            k--;
            r--;
        }
        return ;
    }
};
```
---

## 🤔 Why Can't We Use the `merge()` In-Place Logic in Merge Sort?

It feels like we **should** be able to use the same trick:  
just **pad zeros** at the end of the left half so we can merge from the back like in the LeetCode problem.  
But here's exactly why it doesn't work that easily 👇

---

### 🧱 1. Merge Sort Merges *Inside* One Array

In merge sort:
- You're sorting and merging parts of **one array**
- The left and right halves are **right next to each other**, like this:

arr = [1, 5, 9, | 2, 4, 6]
↑ ↑
left right


There is **no space** between them to store the merged result like `nums1` has in the LeetCode problem.

---

### 🧹 2. “Padding Zeros” Would Mean Shifting Everything

To pad zeros into the left part, you'd have to:
- Shift all the right-side elements forward to make space
- OR copy everything to a new array

That’s **extra work** that actually **adds time**, not saves it.

---

### 😵 3. Merge Sort Recursion Needs Clean Boundaries

- You can't just "pad zeros" at random during recursion — each merge call handles a part of the array.
- You’d have to **manage where padding goes**, and that makes recursion messy and unsafe.

---

### ✅ Why `merge()` Works But Merge Sort Can’t Use It

| Situation          | Works? | Why |
|--------------------|--------|-----|
| LeetCode merge()   | ✅ Yes | One array has extra space |
| Merge Sort merging | ❌ No  | No space, subarrays touch |

---

### 💡 Final Takeaway

You **can't use** the `merge()` in-place trick from LeetCode in merge sort because:
- Merge sort doesn’t give you extra space between halves
- You’d have to shift elements = more work = **slower**
- It’s better to just use a **temp array** and keep merge sort clean and fast

---
