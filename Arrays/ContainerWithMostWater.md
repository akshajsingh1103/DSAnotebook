# Container With Most Water - Optimized Two Pointer Approach

[Problem Link](https://leetcode.com/problems/container-with-most-water/)

## Problem Statement

Given `n` non-negative integers `height[0], height[1], ..., height[n-1]`, where each represents a point at coordinate `(i, height[i])`, `n` vertical lines are drawn. Find two lines, which together with the x-axis form a container, such that the container contains the most water.

## Code

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0, right = height.size()-1;
        int area = 0 ;
        while(left<right){
            int width = right -left ;
            area=max(area,width*min(height[left],height[right]));
            if(height[left]>=height[right]) right--;
            else left++ ;
        }return area ;
    }
};
```

## Approach: Two Pointers

### Intuition

The key idea is to use two pointers (`left` and `right`) starting at the two ends of the array and move them toward each other. The area is determined by the width between the two lines and the height of the shorter line.

### Why This Works

- **Width** starts at the maximum possible value (`right - left`) and gets smaller as we move the pointers inward.
- **Height** is limited by the shorter of the two lines (since water can't go above the shorter one).
- At each step, we calculate the area and update the max if it's larger.

### Why Move the Shorter Line?

This is the key optimization.

- If we move the taller line, we are *guaranteed* not to find a taller minimum height in the next step.
- But if we move the shorter line, there’s a *chance* that the new line is taller, which might increase the area (despite the width getting smaller).

So we always move the pointer pointing to the **shorter line** to potentially increase the height and find a better area.

### Time Complexity

- **O(n)** — We traverse the array once, with each pointer moving at most `n` steps.

### Space Complexity

- **O(1)** — No extra space is used.

## Summary

This two-pointer approach smartly narrows down the search space by eliminating line pairs that can't possibly contribute to the max area, thus making it optimal for this problem.

---

💡 This technique—shrinking the search window from both ends while making greedy decisions—is a powerful pattern to solve problems involving arrays and max/min conditions.

