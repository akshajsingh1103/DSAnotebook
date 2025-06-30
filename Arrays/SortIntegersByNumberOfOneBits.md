# 🔢 Sort Integers by the Number of 1 Bits

[Problem Link](https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits/)

## 🧠 Problem Statement

Given an array of integers `arr`, sort the array in ascending order by the **number of 1s in their binary representation**.

If two numbers have the same number of 1s, sort them in ascending numerical order.

---

## 📌 Example 1:

**Input:**  
arr = [0,1,2,3,4,5,6,7,8]

**Output:**  
[0,1,2,4,8,3,5,6,7]

### Explanation:
- Bit count for each:
  - 0: 0
  - 1,2,4,8: 1
  - 3,5,6: 2
  - 7: 3
- Sorted accordingly.

---

## 📌 Example 2:

**Input:**  
arr = [1024,512,256,128,64,32,16,8,4,2,1]

**Output:**  
[1,2,4,8,16,32,64,128,256,512,1024]

### Explanation:
- All have exactly one bit set.
- Sorted by value.

---

## 💡 Intuition

- We need to sort each number by how many 1s it has in its binary form.
- If two numbers have the same bit count, we break the tie using the number itself.
- We can use **Brian Kernighan’s Algorithm** to count 1s efficiently.
- Store pairs of `(bitCount, number)` to sort.
- Use a **custom comparator**.

---

## 🔍 Brian Kernighan’s Algorithm

### What is it?

An efficient algorithm to count the number of 1s (set bits) in a number’s binary representation.

### Code:
```cpp
static int countBits(int num){
        int c = 0;
        if(num==0) return 0;
        while(num){
            num&=(num-1);
            c++;
        }

        return c ;
    }
```
---
### Key Insight:

`n & (n - 1)` removes the **lowest set bit** from `n`.

- This works because subtracting 1 from a number flips the **rightmost 1** to 0 and everything after it to 1.
- ANDing it with `n` removes that 1.
- Each operation reduces `n`'s number of set bits by one.
- So, the number of operations = number of 1s.

#### 🧠 Fun Fact:

> For any **consecutive numbers** `n` and `n - 1`,  
> `n & (n - 1)` removes **one set bit** from `n`.

---


## ⏱️ Time & Space Complexity

| Metric             | Value                  |
|--------------------|------------------------|
| Time Complexity     | O(n log n) (due to sorting) |
| Space Complexity    | O(n) (for storing pairs)     |
| Bit Count per Number | O(k), where k = number of 1s |

---

## 🧾 Final C++ Code

```cpp
class Solution {
public:
    static int countBits(int num){
        int c = 0;
        if(num==0) return 0;
        while(num){
            num&=(num-1);
            c++;
        }

        return c ;
    }
    struct cmp{
        bool operator()(int a, int b){
            int c1 = countBits(a);
            int c2 = countBits(b);
            if(c1==c2){
                return a < b;
            }return c1<c2;
        }
    };
    vector<int> sortByBits(vector<int>& arr) {

        sort(arr.begin(), arr.end(), cmp());
        return arr; 

    }
};
```