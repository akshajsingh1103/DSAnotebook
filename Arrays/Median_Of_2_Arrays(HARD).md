# Median of Two Sorted Arrays – LeetCode Hard

[Problem Link](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## 🧩 Problem Statement

You are given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively. Return the **median** of the two sorted arrays.

> The overall run time complexity should be `O(log(min(m, n)))`.

---

## 🚀 Binary Search Approach – Intuition

We don't merge the arrays. Instead, we find the **correct partition** between the two arrays such that:

- All elements in the left half are less than or equal to all elements in the right half.
- The number of elements in the left and right are equal (or off by one, if the total is odd).

This is done using **binary search on the smaller array**.

---

## ✂️ Partition Explanation

Let `cut1` be the index where we split `nums1` and `cut2` where we split `nums2`.

We define:
- `l1 = max of left part of nums1` → `nums1[cut1 - 1]` or `INT_MIN` if `cut1 == 0`
- `r1 = min of right part of nums1` → `nums1[cut1]` or `INT_MAX` if `cut1 == size`
- Similarly for `l2` and `r2` from `nums2`

### ✅ We want this condition:

```
l1 <= r2 && l2 <= r1
```

If this is true:
- The combined arrays are partitioned correctly.
- Median is calculated as:
  - Even total: `(max(l1, l2) + min(r1, r2)) / 2.0`
  - Odd total: `max(l1, l2)`

---

## 🧭 Where Is Binary Search Happening?

This problem is tricky because you're not searching for a **value**, you're searching for a **partition index** (`cut1`) in the smaller array (`nums1`).

### ✅ Here's the binary search logic:

1. Set `low = 0`, `high = nums1.size()`
2. While `low <= high`:
   - `cut1 = (low + high) / 2`
   - `cut2 = (total + 1)/2 - cut1` → so left part has half the elements

You check the partition using:
- `l1 = nums1[cut1 - 1]` or `INT_MIN` if `cut1 == 0`
- `r1 = nums1[cut1]` or `INT_MAX` if `cut1 == nums1.size()`
- Same for `l2` and `r2` from `nums2`

Then check:

```
If l1 <= r2 && l2 <= r1 → 🎯 correct partition
Else if l1 > r2 → move left → high = cut1 - 1
Else → move right → low = cut1 + 1
```

So yes — the binary search is over the **possible split positions** in `nums1`.

You’re zeroing in on the correct partition using standard binary search mechanics.

---

## 🔄 What If the Partition Is Invalid?

### Case 1: `l1 > r2`
→ This means `cut1` is too far to the right (nums1’s left side is too big).  
→ So, **move left**: `high = cut1 - 1`

### Case 2: `l2 > r1`
→ This means `cut1` is too far to the left (nums1’s left side is too small).  
→ So, **move right**: `low = cut1 + 1`

### ❗ What if *both* conditions are violated? (`l1 > r2` **and** `l2 > r1`)
- This is actually **impossible** in a valid sorted input.
- One of the two conditions will always guide binary search direction.
- If both fail, it suggests a bug in indexing or input assumptions.

---

## 💻 Code (To be filled)

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int s1 = nums1.size();
        int s2 = nums2.size();

        if(s1>s2) return findMedianSortedArrays(nums2, nums1);
        int l = 0;
        int h = s1;
        while(l<=h){
            int mid1 = (l+h)/2;
            int mid2 = (s1+s2+1)/2 - mid1 ;
            int l1 = (mid1==0)?INT_MIN:nums1[mid1-1];
            int l2 = (mid2==0)?INT_MIN:nums2[mid2-1];
            int r1 = (mid1>=s1)?INT_MAX:nums1[mid1];
            int r2 = (mid2>=s2)?INT_MAX:nums2[mid2];

            if(l1<=r2 && l2<=r1){
                if((s1+s2)%2==0){
                    return (max(l1,l2)+min(r1,r2))/2.0;
                }else{
                    return max(l1,l2);
                }
            }else if(r2<l1){
                h = mid1-1;
            }else{
                l=mid1+1 ;
            }
        }return 0.0 ;
    }
};
```

---

## 🕰️ Time and Space Complexity

- **Time Complexity**: `O(log(min(n, m)))`  
  Binary search is done on the smaller array only.

- **Space Complexity**: `O(1)`  
  Only a few variables are used; no extra space needed.

---

## 🧪 Sample Test Case

**Input:**

```
nums1 = [1]
nums2 = [1]
```

**Expected Output:**

```
1.0
```

---

## ✅ Summary

| Concept       | Explanation |
|---------------|-------------|
| Binary Search | Done only on the smaller array |
| Partitioning  | Splits arrays into left/right halves |
| Condition     | `l1 <= r2 && l2 <= r1` must hold |
| Adjustments   | Move low/high based on which side is too big |
| Median        | Average of mid elements or max of left half |
| Both failed?  | Should not happen if arrays are sorted correctly |

---

