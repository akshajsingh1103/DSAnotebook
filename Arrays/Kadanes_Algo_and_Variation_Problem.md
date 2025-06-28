# Kadane's Algorithm (General, handles all-negative arrays)

---

## 1. General Kadane’s Algorithm (Handles negative numbers)

Kadane’s algorithm finds the maximum sum of any contiguous subarray in an array — even if all numbers are negative.

### Key difference from the simple version:

- **Do NOT reset current sum to zero when it becomes negative**.
- Instead, keep track of whether starting fresh from the current element is better.

### Code:

```cpp
int maxSubArray(vector<int>& nums) {
int currentSum = nums[0];
int maxSum = nums[0];
for (int i = 1; i < nums.size(); ++i) {
    currentSum = max(nums[i], currentSum + nums[i]);
    maxSum = max(maxSum, currentSum);
}
return maxSum;
}
```

---

## 2. Explanation

- At each position, we decide whether to:
  - Continue the current subarray by adding `nums[i]`, or
  - Start a new subarray from `nums[i]` (if `nums[i]` alone is better).
- This way, the algorithm works well even when all elements are negative.

---
## 3. Maximum Sum of Contiguous Subarray with Exactly Length k

---

### Problem

Find the maximum sum of any **contiguous subarray** of length exactly `k` in an integer array.

---

### Approach: Sliding Window

- Use a sliding window of fixed size `k`.
- Calculate the sum of the first `k` elements.
- Then move the window one step at a time:
  - Subtract the element leaving the window.
  - Add the new element entering the window.
- Keep track of the maximum sum encountered.

---

### Code
```cpp
int maxSubArraySumWithK(vector<int>& nums, int k) {
int n = nums.size();
if (n < k) return INT_MIN; // Not enough elements
int windowSum = 0;
// Sum of first k elements
for (int i = 0; i < k; ++i) {
    windowSum += nums[i];
}

int maxSum = windowSum;

// Slide the window through the array
for (int i = k; i < n; ++i) {
    windowSum += nums[i] - nums[i - k];
    maxSum = max(maxSum, windowSum);
}

return maxSum;
}
```

---

### Example

Input:

vector<int> nums = {2, 3, -1, 5, -2, 6, -1};
int k = 3;

Output:


Explanation: The subarray `[3, -1, 5]` sums to 7, but `[5, -2, 6]` sums to 9, `[6, -1]` is length 2 (ignored). The max sum for length 3 subarrays is 8 from `[3, -1, 5]` or `[5, -2, 6]` depending on elements.

---

### Complexity

- Time Complexity: **O(n)** — we traverse the array once.
- Space Complexity: **O(1)** — only a few variables used.

---
