# Comparing Two Approaches to Count Substrings Containing 'a', 'b', and 'c'

[Problem Link](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/)

This document compares two C++ implementations to solve the problem:

> Given a string `s` with only characters `'a'`, `'b'`, and `'c'`, count the number of substrings containing at least one `'a'`, one `'b'`, and one `'c'`.

---

## First Attempt (Slower, O(n²) Time Complexity)

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int a=0,b=0,c=0 ;
        int count = 0;
        int j= 0 ;
        while(j<s.size()){

            if(s[j]=='a') a++;
            else if(s[j]=='b') b++;
            else c++;
            
            if(a>=1 && b>=1 && c>=1){
                int k = 0 ;
                int a1=a;
                int b1=b;
                int c1=c ;
                while(a1>=1 && b1>=1 && c1>=1){
                    if(s[k]=='a') a1--;
                    else if(s[k]=='b') b1--;
                    else c1--;
                    k++;
                }
                count+=k ;
            }j++;

        }return count ;
    }
};
```

### Explanation:

- The code increments character counts while moving the right pointer `j`.
- When the window `[0..j]` contains all three characters, it tries to find how many valid substrings start at the left by shrinking from the beginning.
- This is done by copying the counts into `a1`, `b1`, `c1` and shrinking `k` until any count drops below 1.
- It adds `k` to `count` because all substrings starting at index 0 up to `k-1` are valid.
- **Problem:** The inner while loop restarts shrinking from the beginning on every step of `j`. This leads to repeated work and thus **O(n²)** time complexity.
- On large strings, this approach will time out (TLE).

---

## Second Attempt (Optimized Sliding Window, O(n) Time Complexity)

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int a=0,b=0,c=0 ;
        int count = 0;
        int j= 0 ;
        int k = 0;
        while(j<s.size()){

            if(s[j]=='a') a++;
            else if(s[j]=='b') b++;
            else c++;
            
            // if(a>=1 && b>=1 && c>=1){
                while(a>=1 && b>=1 && c>=1){
                    if(s[k]=='a') a--;
                    else if(s[k]=='b') b--;
                    else c--;
                    k++;
                    count+=s.size()-j ;
                }
                
            j++;

        }return count ;
    }
};
```

### Explanation:

- We maintain a sliding window `[k..j]`.
- For every new character at `j`, update counts.
- Then shrink from left (`k`) while the window remains valid (contains `'a'`, `'b'`, `'c'`).
- Every time the window is valid, we add `n - j` to the count because:
  - All substrings starting from `k` and ending anywhere between `j` and the end of the string (`n-1`) contain all three chars.
- This approach **never restarts shrinking from the beginning**.
- Each character is visited at most twice (once when expanding `j`, once when shrinking `k`).
- **Time complexity is O(n), efficient even for large inputs.**

---

## Comparison of Time Complexities and Behavior

| Aspect                    | First Attempt         | Second Attempt        |
|---------------------------|----------------------|----------------------|
| Time Complexity           | O(n²) (nested loops) | O(n) (two-pointer)   |
| Shrinking Strategy        | From start every time | Incremental sliding  |
| Counting Approach         | Counts after full shrink with copies | Counts while shrinking |
| Handles Large Inputs      | No (TLE)             | Yes                  |
| Code Efficiency           | Less efficient       | More efficient       |

---
