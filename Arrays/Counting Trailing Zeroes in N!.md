# Counting Trailing Zeroes in Factorials (`n!`)

[Problem Link](https://leetcode.com/problems/factorial-trailing-zeroes/submissions/1683082133/)

## Problem Statement

Given an integer `n`, find the number of trailing zeroes in `n!` (the factorial of `n`).

- `n! = n × (n-1) × (n-2) × ... × 3 × 2 × 1`
- Trailing zeroes are the number of zeros at the end of the number.

---

## Key Insight

Trailing zeroes come from factors of **10**.

Since:

10 = 2 × 5


and factorials have **more 2s than 5s**, the count of trailing zeroes is determined by the number of **5s** in the factorial's prime factorization.

---

## Why Count 5s?

Every multiple of 5 contributes at least one factor of 5.  
However, some numbers contribute **more than one 5**, e.g., 25, 125, 625, etc.

### Example: `n = 25`

- Multiples of 5 up to 25:  
  5, 10, 15, 20, 25 → **5 numbers**  
  Each contributes at least one 5 → count = 25 ÷ 5 = 5

- But 25 = 5 × 5, contributing **two 5s**.  
  The previous count only accounted for one factor of 5 for 25.

- So count multiples of 25 too:  
  25 ÷ 25 = 1 (additional 5)

- Total factors of 5:  
  5 (from multiples of 5) + 1 (from multiples of 25) = **6**

---

## General Approach

Count how many numbers up to `n` are divisible by powers of 5:

```cpp
int trailingZeroes(int n) {
    int count = 0;
    for (int i = 5; i <= n; i *= 5) {
        count += n / i;
    }
    return count;
}
```


- `n / 5` counts factors from multiples of 5  
- `n / 25` counts extra factors from multiples of 25  
- `n / 125` counts extra factors from multiples of 125  
- And so on...

Stop when `i > n`.

---

## Example Walkthrough: `n = 100`

- 100 ÷ 5 = 20  
- 100 ÷ 25 = 4  
- 100 ÷ 125 = 0 (stop here)

Total trailing zeroes = 20 + 4 = **24**

---

## Why Not Count 2s?

Because factorials have **plenty of 2s** (every even number contributes at least one 2).  
The bottleneck for creating trailing zeros is the number of 5s.

---

## Summary

- Count trailing zeroes by counting factors of 5 in `n!`
- Use powers of 5 to count multiple factors from numbers like 25, 125, etc.
- The algorithm runs in O(log₅ n) time.

---

## Complete C++ Code Example

```cpp
#include <iostream>
using namespace std;

int trailingZeroes(int n) {
    int count = 0;
    for (int i = 5; i <= n; i *= 5) {
        count += n / i
```

