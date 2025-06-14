# 💸 Minimize Cash Flow Between Friends
[Problem Link](https://www.geeksforgeeks.org/problems/minimize-cash-flow/1)

---

## 🎯 Problem Overview

Given a matrix of transactions between `N` friends where `transaction[i][j]` represents the amount person `i` pays to person `j`, the goal is to **minimize the total number of transactions** needed to settle all debts among friends.

---

## 🧠 Why Use a Net Array?

Instead of trying to cancel individual transactions or traverse the original graph:

- We **compress** each person’s total incoming and outgoing amounts into a **net balance**:
  - A **positive** net means the person is owed money.
  - A **negative** net means the person owes money.
- This approach eliminates cycles, redundant paths, and simplifies the problem to just settling net debts.

👉 The net array is a **lossless summary** of the full transaction graph and avoids reprocessing known relationships.

---

## ⚙️ Why Greedy Pairing Works

After computing the net balances, we want to minimize the number of actual transactions required. Here's why greedy pairing is optimal:

- Always match the **person who owes the most** with the **person who is owed the most**.
- Settle the **minimum** of their balances in one transaction.
- Update both balances and repeat until all are settled.


### ✅ Why This Is Optimal

This greedy method doesn’t just *feel* right — it has strong grounding:

- Each transaction reduces the **total absolute debt** by the **maximum possible amount**.
- This guarantees the fastest convergence toward a fully settled state.
- It is equivalent to **minimizing the number of edges in the final settlement graph**.

This greedy approach has been shown to result in the **minimum number of cash flows** in such debt-settlement problems.

---

## 🧪 Code Placeholder

```cpp
// User function Template for C++
class Solution {
  public:
    vector<vector<int>> minCashFlow(vector<vector<int>> &transaction, int n) {
        // Max-heaps to store people who need to receive (creditors) and who need to pay (debtors)
        priority_queue<pair<int, int>> creditors, debtors;

        // Step 1: Calculate net balance for each person
        for (int person = 0; person < n; person++) {
            int netAmount = 0;
            for (int other = 0; other < n; other++) {
                netAmount -= transaction[person][other]; // money paid to others
                netAmount += transaction[other][person]; // money received from others
            }

            // If person is owed money (creditor)
            if (netAmount > 0) {
                creditors.push({netAmount, person});
            }
            // If person owes money (debtor)
            else if (netAmount < 0) {
                debtors.push({-netAmount, person}); // store as positive for easier comparison
            }
        }

        // Step 2: Initialize the result matrix with all zeros
        vector<vector<int>> result(n, vector<int>(n, 0));

        // Step 3: Greedily settle debts between highest debtor and highest creditor
        while (!debtors.empty()) {
            auto debtor = debtors.top(); debtors.pop();
            auto creditor = creditors.top(); creditors.pop();

            int amountToSettle = min(debtor.first, creditor.first);

            // Record the transaction: debtor pays creditor
            result[debtor.second][creditor.second] = amountToSettle;

            // Update remaining balances and push back if not yet fully settled
            if (debtor.first > amountToSettle) {
                debtors.push({debtor.first - amountToSettle, debtor.second});
            }
            if (creditor.first > amountToSettle) {
                creditors.push({creditor.first - amountToSettle, creditor.second});
            }
        }

        return result;
    }
};

```

---

## ⏱️ Time and Space Complexity

- **Time Complexity:** `O(N^2 + K log N)`
  - `O(N^2)` to compute net balances from the transaction matrix.
  - Heap operations for `K` settlements, each costing `log N`.

- **Space Complexity:** `O(N^2)` for the result matrix, and `O(N)` for heaps and the net array.

---