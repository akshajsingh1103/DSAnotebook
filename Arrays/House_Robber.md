# 🏡 House Robber – DP Explanation and Notes

[Problem Link](https://leetcode.com/problems/house-robber/submissions/1586293457/)

---

## 📌 Problem Statement

> You are a professional robber planning to rob houses along a street.  
> Each house has a certain amount of money, but adjacent houses have security systems.  
> **You can't rob two adjacent houses.**

**Input:** An array `nums` where `nums[i]` is the money at house `i`  
**Output:** Maximum money you can rob without alerting the police

---

## 🧠 Recursive Approach (Top-Down)

We define:

`dp(i) = max money you can rob starting from house i`

### 🔁 Recurrence Relation

```cpp
dp(i) = max(
nums[i] + dp(i + 2), // Rob this house → skip next
dp(i + 1) // Skip this house
)
```

### 🧱 Base Case

- If `i >= n` → return 0 (no houses left)

---

## 🛠️ Memoization (Recursive with DP)

To avoid recomputation, we use a `dp[]` array.

**Code:**

```cpp
int rob(int i, vector<int>& nums, vector<int>& dp) {
    if (i >= nums.size()) return 0;
    if (dp[i] != -1) return dp[i];
    return dp[i] = max(nums[i] + rob(i + 2, nums, dp),rob(i + 1, nums, dp));
                                                        
}
```

### 🔎 What is `dp[i]` storing?

`dp[i]` stores the **maximum money you can rob starting from house i to the end**

---

## 🔄 Converting to Iterative DP (Bottom-Up)

### Step-by-step Plan:

1. Identify what values `dp[i]` depends on → here: `dp[i+1]` and `dp[i+2]`
2. Compute `dp[i]` for `i = n-1` to `0`
3. Base cases:
   - `dp[n] = 0`
   - `dp[n+1] = 0`

**Code:**

```cpp
int rob(vector<int>& nums) {
int n = nums.size();
vector<int> dp(n + 2, 0);// extra space for dp[i+2]

for (int i = n - 1; i >= 0; i--) {
    dp[i] = max(nums[i] + dp[i + 2], dp[i + 1]);
}

return dp[0];

} 
```


---

## 🤯 Space Optimization (Reducing to Two Variables)

### Observation:

At every point, `dp[i]` depends only on `dp[i+1]` and `dp[i+2]`

So we can store just:

- `dp1 = dp[i+1]`
- `dp2 = dp[i+2]`

And simulate the array using two variables.

**Optimized Code:**

```cpp
int rob(vector<int>& nums) {
    int dp1 = 0; // dp[i+1]
    int dp2 = 0; // dp[i+2]
    for (int i = nums.size() - 1; i >= 0; i--) {
        int curr = max(nums[i] + dp2, dp1);
        dp2 = dp1;
        dp1 = curr;
    }

    return dp1;

}

```


---

## 🧪 Dry Run: nums = [2, 7, 9, 3, 1]

| i | nums[i] | dp2 | dp1 | curr = max(nums[i]+dp2, dp1) |
|---|---------|-----|-----|-------------------------------|
| 4 | 1       | 0   | 0   | 1                             |
| 3 | 3       | 0   | 1   | 3                             |
| 2 | 9       | 1   | 3   | 10                            |
| 1 | 7       | 3   | 10  | 10                            |
| 0 | 2       | 10  | 10  | 12 ✅                         |

---

## ❓ Common Doubts Answered

### What is `dp[i]` storing?

> It stores the **max money you can rob starting at house `i` to the end**

---

### Why are we going backward?

> Because `dp[i]` depends on future values: `dp[i+1]` and `dp[i+2]`,  
> So we fill `dp[]` from back to front to ensure dependencies are computed first.

---

### How do we reduce space?

> Since we only need two future values at any time,  
> we can drop the full `dp[]` array and just maintain two variables.

---

## 🎯 How to Convert Any Recursive DP to Iterative

### Step-by-step:

1. Write the recursive function.
2. Identify base cases and dependencies (e.g., `i+1`, `i+2`).
3. Create a `dp[]` array.
4. Loop from the direction opposite to recursion (usually right to left).
5. Fill `dp[i]` using the same formula.
6. Optionally reduce space if only k previous states are needed.

---

## 💡 Template

**Recursive with Memo:**

```cpp
int dp(int i) {
    if (base_case) return ...;
    if (DP[i] != -1) return DP[i];
    return DP[i] = max(dp(i + 1), nums[i] + dp(i + 2));
}
```

---

**Iterative DP (Bottom-Up):**

```cpp
for (int i = n - 1; i >= 0; i--) {
    dp[i] = max(dp[i + 1], nums[i] + dp[i + 2]);
}
```


---

**Space Optimized Approach:**

```cpp
int dp1 = 0, dp2 = 0;
for (int i = n - 1; i >= 0; i--) {
    int curr = max(dp1, nums[i] + dp2);
    dp2 = dp1;
    dp1 = curr;
}
```

---

## 🧮 Time & Space Complexity

| Version               | Time Complexity | Space Complexity |
|-----------------------|-----------------|------------------|
| Recursion Only        | O(2^n)          | O(1)             |
| Recursion + Memo (DP) | O(n)            | O(n)             |
| Iterative Tabulation  | O(n)            | O(n)             |
| Space Optimized       | O(n)            | O(1)             |
