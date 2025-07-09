# 🧠 Coin Change 2 – Full Explanation (Recursive → Tabulation → Space Optimization)

[Problem Link](https://leetcode.com/problems/coin-change-ii/description/)
This document explains how to solve the **Coin Change 2** problem from basic recursion to fully space-optimized dynamic programming. It includes:

- ✅ Recursive approach
- ✅ 2D Tabulation
- ✅ 1D Optimization (with two arrays)
- ✅ Explanation of tricky transitions and base cases
- ✅ Answers to common beginner doubts (like mine 😅)

---

## 🧩 Problem Statement

> You are given an integer `amount` and an array `coins[]`.  
> Return the number of combinations that make up the amount.  
> You may use each coin **unlimited times**.

---

## 🪜 Step 1: Recursive Solution

The brute-force recursive approach:

```
int countWays(int index, int amount, vector<int>& coins) {
    if (index == coins.size()) return amount == 0 ? 1 : 0;

    int take = 0;
    if (amount >= coins[index])
        take = countWays(index, amount - coins[index], coins);

    int skip = countWays(index + 1, amount, coins);

    return take + skip;
}
```

### ⚠️ Problems:
- Exponential time (TLE for large input)
- Same subproblems calculated repeatedly

---

## 💡 Step 2: Bottom-Up DP (2D Tabulation)

We define:
- `dp[i][j]` = number of ways to make amount `j` using coins from `i` onward

```
vector<vector<unsigned long long>> dp(n + 1, vector<unsigned long long>(amount + 1, 0));

for (int i = 0; i <= n; i++)
    dp[i][0] = 1; // Base case: one way to make amount 0

for (int i = n - 1; i >= 0; i--) {
    for (int j = 0; j <= amount; j++) {
        unsigned long long take = (j >= coins[i]) ? dp[i][j - coins[i]] : 0;
        unsigned long long skip = dp[i + 1][j];
        dp[i][j] = take + skip;
    }
}

return static_cast<int>(dp[0][amount]);
```

---

## 🧠 Key Transition: Why `dp[i][j] = take + skip`

This means:
- Take `coins[i]` → stay on same row: `dp[i][j - coins[i]]`
- Skip `coins[i]` → move to next row: `dp[i + 1][j]`

---

## 🔁 Step 3: Space Optimization Using Two 1D Arrays

Since we only use the current row and the one below it, we can use two 1D arrays:

- `prev[j]` represents `dp[i + 1][j]`
- `curr[j]` represents `dp[i][j]`

```
vector<unsigned long long> prev(amount + 1, 0), curr(amount + 1, 0);
prev[0] = 1;

for (int i = n - 1; i >= 0; i--) {
    curr[0] = 1; // Base case: one way to make 0 amount

    for (int j = 1; j <= amount; j++) {
        unsigned long long take = (j >= coins[i]) ? curr[j - coins[i]] : 0;
        unsigned long long skip = prev[j];
        curr[j] = take + skip;
    }

    prev = curr; // Move current row to prev for next iteration
}
```

---

## 🤯 Common Doubts I Had (and Maybe You Do Too)

### ❓ Why is `dp[i][0] = 1` in 2D version?

Because there's always **one way to make amount 0** — take no coins.

### ❓ Why do we set `prev[0] = 1` and also `curr[0] = 1`?

- `prev[0] = 1` is for the **last row** — initializing base case
- `curr[0] = 1` is needed **every iteration** because we rebuild `curr` from scratch each time — and amount 0 always has 1 way

### ❓ Why is `curr[0] = 1` *inside* the loop?

If you leave it outside, it gets **overwritten** in the next round.  
You need to **reset** it for every new `curr[]`, just like you'd reset a new DP row in 2D.

---

## 🎯 Final Optimization: One 1D Array

Once you're comfy with `prev/curr`, you can reuse just one array:

```
vector<unsigned long long> dp(amount + 1, 0);
dp[0] = 1;

for (int coin : coins) {
    for (int j = coin; j <= amount; j++) {
        dp[j] += dp[j - coin];
    }
}
```

💡 **Why it works**:
- Inner loop moves left to right, so we don't overwrite values we still need.
- Each `dp[j]` accumulates combinations including the current coin

---

## ✅ Final Notes

- Recursive → memo → tabulation → 2D → 1D → final 1D
- Transitions make more sense if you understand how DP table gets filled
- It’s okay if this takes time — space optimization is a brain-stretch!

---

## 🧪 Example: `coins = [1, 2], amount = 4`

Combinations:
- 1+1+1+1
- 1+1+2
- 2+2

So the answer is `3` — and that’s what all versions of this algorithm compute.

---

Happy DP-ing! 💪
