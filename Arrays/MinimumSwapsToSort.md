# 🧠 Minimum Swaps to Sort – Explanation

## 📌 Problem Statement

Given an array of **distinct integers**, find the **minimum number of swaps** required to sort the array in strictly increasing order.

---

## ✅ What You Need To Do

- You are allowed to **swap any two elements**.
- The goal is to **sort the array** using the **least number of swaps**.

---

## 💡 Key Idea – Cycle Detection

Sorting the array efficiently comes down to **detecting cycles** in the array.

---

### 🔄 What Is a Cycle?

If a value is not in its correct place, and the correct value at that position is also misplaced, then we can follow a chain of wrong positions until we **return to the starting point**.  
That’s a **cycle** of misplacements.

For a cycle of length `k`, we always need exactly `k - 1` swaps to fix it.

---

### 🧠 Why Cycles Give the Minimum?

- One swap can **move two elements** closer to their correct positions.
- But to fully fix a cycle of `k` elements, you need to correctly position `k - 1` of them.
- The last one falls into place automatically.
- This is the **mathematical minimum** for such cycles.

---

## ⚙️ Step-by-Step Algorithm

1. Pair each element with its original index.
2. Sort the array based on the element values.
3. Use a visited array to track which elements are already placed correctly.
4. For each unvisited index:
   - Trace the cycle starting from that index.
   - Count the cycle size.
   - Add `cycle_size - 1` to the total swap count.
5. Return the total number of swaps.

---

## 🔁 Example Walkthrough

### Input:

```
[2, 8, 5, 4]
```

### After pairing with indices and sorting:

```
Original Index → Sorted Position Mapping:
(2, 0) → 0 ✅  
(8, 1) → 3  
(5, 2) → 2 ✅  
(4, 3) → 1
```

### Cycle Detected:

1 → 3 → 1 is a cycle of length 2 → `2 - 1 = 1` swap

✅ Final answer: **1 swap**

---

## 🔐 Why Is This the Minimum?

- You can’t fix a cycle of misplaced values in fewer than `k - 1` swaps.
- This is because swapping fewer times leaves at least one element out of place.
- So, **no other method can sort the array in fewer swaps**.

---

## 🧑‍💻 Code

```cpp
int minSwapsToSort(vector<int>& arr) {
    int n = arr.size();

    // Step 1: Pair the elements with their indices
    vector<pair<int, int>> arrPos(n);
    for (int i = 0; i < n; i++) {
        arrPos[i] = {arr[i], i};
    }

    // Step 2: Sort the array by value
    sort(arrPos.begin(), arrPos.end());

    // Step 3: Track visited elements
    vector<bool> visited(n, false);

    int swaps = 0;

    for (int i = 0; i < n; i++) {
        // If already visited or already in the right place
        if (visited[i] || arrPos[i].second == i) {
            continue;
        }

        // Start cycle
        int cycle_size = 0;
        int j = i;

        while (!visited[j]) {
            visited[j] = true;

            // Move to the index of the element that should be here
            j = arrPos[j].second;
            cycle_size++;
        }

        // If cycle_size is k, we need (k - 1) swaps
        if (cycle_size > 0) {
            swaps += (cycle_size - 1);
        }
    }

    return swaps;
}
```

---

## 🕒 Time and Space Complexity

- **Time Complexity:** O(n log n) — due to sorting
- **Space Complexity:** O(n) — for visited array and index mapping

---

## 🧪 Test Cases to Try

```text
Input: [2, 8, 5, 4]
Output: 1

Input: [10, 19, 6, 3, 5]
Output: 2

Input: [1, 3, 4, 5, 6]
Output: 0

Input: [4, 3, 1, 2]
Output: 3
```

---

## 🏁 Final Notes

- The **cycle detection** method guarantees minimum swaps.
- It’s fast, efficient, and works with any array of distinct elements.
