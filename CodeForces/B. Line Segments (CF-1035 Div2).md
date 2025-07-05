# Codeforces Round 1035 (Div. 2) - Problem B

[Problem Link](https://codeforces.com/contest/2119/problem/B)
---

## 🧩 Problem Summary

You start at a point **P = (px, py)** and need to reach a target point **Q = (qx, qy)**.  
You're given **n step lengths**: `a₁, a₂, ..., aₙ`. Each step must be taken **in any direction**.

Your goal is to take all the steps and **end exactly at point Q**.

---

## 🎯 Target Distance

The straight-line distance between start and end points is:

\[
D = \sqrt{(px - qx)^2 + (py - qy)^2}
\]

This is the **exact distance** you must land from the starting point after using all your steps.

---

## 🛤️ Direction Is Free, But Lengths Are Fixed

You can take each step in **any direction**, but:

- You **must** take all of them.
- You **cannot** change their lengths.

The question becomes:

> Can you take the steps in such a way that you land **exactly D units away** from where you started?

---

## 📏 Two Key Concepts: rmin and rmax

### 🟢 rmax

This is easy: if you walk in a **perfectly straight line**, what's the farthest you can get?

\[
r_{max} = a_1 + a_2 + \dots + a_n
\]

You get this when you walk all steps in the **same direction**.

---

### 🔴 rmin

This is trickier: what's the **closest** you can land from your starting point?

Here’s the idea:

- Imagine the **longest step** pulls you in one direction.
- Then you try to **cancel it out** by walking the rest of the steps in the **opposite direction**.
- You’re basically playing **tug-of-war** with vectors.

If the sum of the other steps isn’t enough to pull you all the way back, you get dragged.

Mathematically:

\[
r_{min} = \max(0,\ 2 \cdot \text{max}(a_i) - \sum a_i)
\]

This is the **smallest possible distance** you can end up from the start after all steps.

---

## 🤔 Why That Formula?

Let’s say your biggest step is 8, and the rest add up to 2.

- Walk 8 units forward.
- Try to walk 2 units back.
- Net distance = 8 - 2 = 6

So:

\[
r_{min} = 8 - 2 = 6 = 2 \cdot 8 - 10
\]

If the other steps are strong enough to pull you back or balance out the big step, great.  
If not, you're **forced** to be some distance away — and that's `rmin`.

---

## ✅ The Final Check

You're trying to land **exactly** `D` units away.

But you're **limited** to the range `[rmin, rmax]`.

So you must check:

\[
r_{min}^2 \le D^2 \le r_{max}^2
\]

We square everything to avoid square roots and floating-point errors.

---

## 🧪 Example

### Input:
px = 0, py = 0
qx = 3, qy = 0
a = [1, 1, 1]


- D = 3
- rmax = 3
- rmin = max(0, 2 * 1 - 3) = 0

→ `0^2 <= 3^2 <= 3^2` → ✅ Yes

---

## 🔥 Counter Example

### Input:

px = 0, py = 0
qx = 2, qy = 0
a = [8, 1, 1]


- D = 2  
- rmax = 10  
- rmin = max(0, 2×8 - 10) = 6

→ `6^2 <= 2^2` → ❌ No

Even if you wiggle and spin like crazy, the **big step pulls too far**, and the others can't pull you back.

---

## 🐛 Common Doubt: "Can't I Just Wiggle and Curve Around?"

Nope — and here's why:

- You’re allowed to walk in **any path**, but the **final distance** from start must be **exactly D**.
- If your steps force you to be **at least 6 units away**, then you can’t reach something only 2 units away — **no matter how curvy you walk**.

Wiggling affects **how** you get there, not **where** you can land in terms of net distance.

---

## 🧠 Final Summary

- `D` is the exact distance you must land from start.
- `rmin` is the closest possible distance you can achieve using all steps.
- `rmax` is the farthest possible distance using all steps.
- You can only reach the target if:

\[
r_{min}^2 \le D^2 \le r_{max}^2
\]

---
## Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
 
int main() {
    int t; cin >> t;
    while (t--) {
        int n; cin >> n;
        long long px, py, qx, qy;
        cin >> px >> py >> qx >> qy;
        vector<long long> a(n);
        long long sum = 0, mx = 0;
        for (int i = 0; i < n; i++) {
            cin >> a[i];
            sum += a[i];
            if (a[i] > mx) mx = a[i];
        }
 
        long long rmin = 0;
        if (2 * mx > sum) rmin = 2 * mx - sum;
        long long rmax = sum;
 
        long long dx = px - qx;
        long long dy = py - qy;
        long double dist_sq = (long double)dx * dx + (long double)dy * dy;
 
        if (dist_sq >= (long double)rmin * rmin && dist_sq <= (long double)rmax * rmax)
            cout << "Yes\n";
        else
            cout << "No\n";
    }
}

```

