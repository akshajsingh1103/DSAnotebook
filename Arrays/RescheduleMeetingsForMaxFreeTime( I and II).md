# 🧠 Reschedule Meetings for Maximum Free Time — Two Variants Explained

[Problem Link I](https://leetcode.com/problems/reschedule-meetings-for-maximum-free-time-i/description/?envType=daily-question&envId=2025-07-09)
[Problem Link II](https://leetcode.com/problems/reschedule-meetings-for-maximum-free-time-ii/description/?envType=daily-question&envId=2025-07-10)

---

## 📘 Variant 1: Sliding Window — No Rescheduling

**Problem:**  
You are given an event duration and a list of non-overlapping meetings.  
Find the **maximum total free time from any `k` consecutive free gaps** between them.

---

### 💡 Approach

1. Compute the **gaps between meetings**:
   - Before the first meeting
   - Between each consecutive meeting
   - After the last meeting

2. Use a **sliding window of size `k+1`** to find the **maximum sum** of consecutive gaps.

---

### ✅ Code

```
class Solution {
public:
int maxFreeTime(int eventTime, int k, vector<int>& startTime, vector<int>& endTime) {
vector<int> duration;
if (startTime[0] != 0) duration.push_back(startTime[0]);
    for (int i = 1; i < startTime.size(); i++) {
        duration.push_back(startTime[i] - endTime[i - 1]);
    }

    if (endTime.back() != eventTime) duration.push_back(eventTime - endTime.back());

    int s = 0, maxDur = INT_MIN, i = 0, j = 0;
    while (j < duration.size()) {
        if (j - i >= k + 1) {
            s -= duration[i];
            i++;
        } else {
            s += duration[j];
            maxDur = max(maxDur, s);
            j++;
        }
    }
    return maxDur;
}
};
```

---

## 📘 Variant 2: Reschedule One Meeting to Maximize Free Time

**Problem:**  
You may reschedule **at most one meeting** (same duration) anywhere within the timeline, as long as it doesn’t overlap with others.  
Return the **maximum continuous free time** that can be achieved.

---

### 💡 Core Idea

1. Calculate the `n+1` **free time gaps**:
   - Before the first meeting
   - Between each meeting
   - After the last meeting

2. For each meeting:
   - Try **removing** it and **rescheduling** it to any other gap (left or right)
   - This allows the **two neighboring gaps** of the removed meeting to be **merged**

3. Use:
   - A **prefix max (largestLeft)** to track biggest free gap seen so far
   - A **suffix max (largestRight)** to track largest free gap coming later

---

### 🧠 Gap Calculation

gaps[0] = startTime[0]
gaps[1] = startTime[1] - endTime[0]
...
gaps[n] = eventTime - endTime[n-1]


---

### 🔄 Prefix and Suffix Max

largestRight[i] = max(largestRight[i+1], gaps[i+1]) // suffix max
largestLeft = max(largestLeft, gaps[i]) // prefix max during loop

---

### ✅ Final Optimized Code (Renamed and Cleaned)
```
class Solution {
public:
int maxFreeTime(int T, vector<int>& S, vector<int>& E) {
int n = S.size();
if (n == 0) return T;
    vector<int> free(n + 1);
    free[0] = S[0];
    for (int i = 1; i < n; ++i)
        free[i] = S[i] - E[i - 1];
    free[n] = T - E[n - 1];

    vector<int> right(n + 1);
    for (int i = n - 1; i >= 0; --i)
        right[i] = max(right[i + 1], free[i + 1]);

    int best = 0, left = 0;
    for (int i = 0; i < n; ++i) {
        int len = E[i] - S[i];
        if (left >= len || right[i + 1] >= len)
            best = max(best, free[i] + free[i + 1] + len);
        best = max(best, free[i] + free[i + 1]);
        left = max(left, free[i]);
    }

    return best;
}
};
```

## ❓ Doubts & Clarifications — Reschedule One Meeting Problem II

### 1. Who fills the `largestRight` array?

- `largestRight` is filled **in a right-to-left loop**, where each position `i` stores the maximum gap from `i+1` to the end.
- This helps quickly check the largest free gap available to the **right** of any meeting.
- The code:
  
for (int i = n - 1; i >= 0; --i)
largestRight[i] = max(largestRight[i + 1], gaps[i + 1]);


---

### 2. How is `largestLeft` being calculated and updated?

- `largestLeft` is a single variable updated during the left-to-right loop.
- It tracks the largest gap seen so far **to the left** of the current meeting.
- After processing the gaps before the `i`-th meeting, `largestLeft` is updated as:
- 
largestLeft = max(largestLeft, gaps[i]);

- This allows checking if the meeting can be rescheduled into any previously seen left gap.

---

### 3. What does `gaps` array represent exactly?

- `gaps` stores all free times between meetings:
- `gaps[0]` is free time before the first meeting (`startTime[0]`).
- For `i` in [1..n-1], `gaps[i]` is the free time between meeting `i-1` and `i` (`startTime[i] - endTime[i-1]`).
- `gaps[n]` is free time after the last meeting (`eventTime - endTime[n-1]`).

---

### 4. How is the maximum free time computed when rescheduling one meeting?

- When a meeting is removed, the two gaps on either side merge.
- The maximum free time can be either:
- The merged gap plus the removed meeting’s duration (if it fits somewhere else), OR
- Just the sum of the two adjacent gaps if rescheduling is not possible.
- This is checked using `largestLeft` and `largestRight` to ensure the meeting duration fits in some other gap.

---

### 6. Why is this approach efficient? What about time complexity?

- Calculations of gaps, prefix max (`largestLeft`), and suffix max (`largestRight`) arrays take **O(n)** time.
- The single pass loop to check possible rescheduling also runs in **O(n)**.
- No nested loops or exhaustive checks, so the overall complexity remains linear.

---

### 7. What if there are no meetings? 

- If `n == 0`, the entire event duration is free and returned immediately.

---

## 🕒 Time and Space Complexity

- **Time Complexity:** O(n)
- **Space Complexity:** O(n)

Efficient single-pass operations using precomputed prefix and suffix max arrays.

---



