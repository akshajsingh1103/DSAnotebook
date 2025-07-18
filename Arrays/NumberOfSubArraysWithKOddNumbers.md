# Number of Subarrays with Exactly K Odd Numbers

[Problem Link](https://leetcode.com/problems/count-number-of-nice-subarrays/description/)

See [Similar Problem](./BinarySubstringsWithSums.md) for a related approach.

## Problem Statement

Given an integer array `nums` and an integer `k`, return the number of contiguous subarrays that contain **exactly** `k` odd numbers.

---

## Intuition

- Counting subarrays with exactly `k` odd numbers directly is hard.
- Instead, count subarrays with **at most** `k` odd numbers.
- Use the formula:

count_exact_k = count_at_most_k - count_at_most_(k - 1)

---

## Approach: Sliding Window

- Use two pointers `l` and `r` to maintain a window.
- Expand `r` pointer and decrement `k` for every odd number found.
- Shrink `l` pointer while `k < 0` (more than allowed odd numbers).
- Add `(r - l + 1)` to result at each step — all subarrays ending at `r` with at most `k` odds.

---

## Code Implementation

```cpp
class Solution {
public:
int atMost(vector<int>& nums, int k) {
if (k < 0) return 0; // edge case
    int l = 0, res = 0;
    for (int r = 0; r < nums.size(); r++) {
        if ((nums[r] & 1) == 1) k--;  // odd check via bitwise AND
        while (k < 0) {
            if ((nums[l] & 1) == 1) k++;
            l++;
        }
        res += r - l + 1;
    }
    return res;
}

int numberOfSubarrays(vector<int>& nums, int k) {
    if (k == 0) return atMost(nums, 0);
    return atMost(nums, k) - atMost(nums, k - 1);
}
};

```


---

## Explanation

- `atMost(nums, k)` counts subarrays with at most `k` odd numbers.
- `numberOfSubarrays(nums, k)` subtracts `atMost(k-1)` from `atMost(k)` to get exactly `k`.
- Handles `k == 0` gracefully by returning `atMost(nums, 0)`.

---

## Complexity

- Time Complexity: **O(n)** — single pass sliding window.
- Space Complexity: **O(1)** — constant space.

---

## Summary

- Efficient sliding window approach with two-pointer technique.
- Optimal O(n) time and O(1) space.
- Clear handling of edge cases (`k == 0`).
- Bitwise odd check `(num & 1)` for small optimization.

---
