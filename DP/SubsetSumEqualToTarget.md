# Subset Sum Equal to Target – From Recursion to Tabulation


## 💭 Problem Statement

Given an array of positive integers `arr` and a target sum `k`, determine whether a subset exists such that the sum of the subset equals `k`.

---

## 🤔 What Is a Subset?

A subset or subsequence of an array is any combination of elements (not necessarily contiguous) that preserves the **original order**.

For example, for `[2, 3, 1]`, valid subsets are:

- `{2}`, `{3}`, `{1}`
- `{2, 3}`, `{2, 1}`, `{3, 1}`, `{2, 3, 1}`

But `{3, 2}` is **not valid** because the order is reversed.

---

## 🧩 Recursive Solution

We use a recursive function:

```cpp
bool solve(int index, int sum, vector<int>& arr, int target) {
    if (index == arr.size()) {
        return sum == target;
    }

    // Include current element
    if (solve(index + 1, sum + arr[index], arr, target)) return true;

    // Exclude current element
    if (solve(index + 1, sum, arr, target)) return true;

    return false;
}
```

This explores all subsets, choosing to include or exclude each element.

---

## 🧠 Base Case

```cpp
if (index == arr.size()) return sum == target;
```

Means:
- We've used all elements
- Check if the accumulated sum matches the target

---

## 🛠 Transition to Tabulation

### 🎯 Goal:
Convert the top-down recursion into a bottom-up **DP table**.

---

## 📦 What dp[i][sum] Means

It answers:

> Can we reach the `target` starting from index `i` with current sum `sum`?

---

## 🧱 Table Dimensions

- `i` → goes from `0` to `n`
- `sum` → goes from `0` to `target`

So we declare:

```cpp
vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));
```

---

## ✅ Base Case in Tabulation

Only one valid terminal state:

```cpp
dp[n][target] = true;
```

Meaning:
- We’ve reached the end of the array
- If sum is **already** equal to the target, then it's a valid path

All other `dp[n][sum ≠ target]` stay `false`.

---

## 🔁 Building the Table – Transition Logic

```cpp
for (int i = n - 1; i >= 0; i--) {
    for (int sum = 0; sum <= target; sum++) {
        // Option 1: don't take arr[i]
        bool notTake = dp[i + 1][sum];

        // Option 2: take arr[i] if sum + arr[i] ≤ target
        bool take = false;
        if (sum + arr[i] <= target)
            take = dp[i + 1][sum + arr[i]];

        dp[i][sum] = take || notTake;
    }
}
```

---

## ❓ Why OR?

`dp[i][sum] = true` if:
- Either taking the current element
- Or skipping it
…leads to a valid solution later.

The table builds **backward**, asking:
> "From this index and sum, can I reach a known successful state?"

---

## 🧠 Important Realization

- You initialize `dp[n][target] = true`
- Every other `dp[i][sum]` is derived by checking whether you can reach this target sum **from current sum**
- So you're tracing back the path to that one valid end condition

---

## 🧪 Example

For `arr = [2, 3, 1]`, `target = 4`, you'll eventually compute:

- `dp[2][3] = true` because taking `1` leads to `sum = 4`
- So `dp[1][3]` can also become true if needed

---

## ✅ Final Answer

Return:

```cpp
return dp[0][0];
```

Why? Because in recursion, we started with `index = 0` and `sum = 0`.

---

## 💡 Summary

| Concept | Meaning |
|--------|---------|
| `dp[i][sum]` | Can we reach `target` from index `i` starting with current sum = `sum`? |
| Base Case | `dp[n][target] = true` |
| Loop Order | Reverse on `i` (from `n-1` to `0`) |
| Transition | Take or not take `arr[i]` |
| Final Answer | `dp[0][0]` |

---

## Partiion with equal subset sum 

In ths you just have to create two disjoint subsets of array that are subsequences 

###Algo
Just have to check if the sum of the whole array is even 
IF NOT, then false
IF IT IS EVEN, then check if the target of SUM/2 is possible in that array if yes then True else False

Heres the [Problem Link](https://leetcode.com/problems/partition-equal-subset-sum/description/)
