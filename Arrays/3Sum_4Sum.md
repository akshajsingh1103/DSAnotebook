# 🧩 3Sum / 4Sum Problem – Solution + Doubts

[3Sum Link](https://leetcode.com/problems/3sum/)
[4Sum Link](https://leetcode.com/problems/4sum/)

---

## ❓ Doubt: Why `j > i + 1` in 4Sum?

When writing the nested loop for 4Sum, we often use:

```
if (j > i + 1 && nums[j] == nums[j - 1]) continue;
```

### ❓ Why not use `j > 0`?

- Because in 4Sum, `j` starts from `i + 1`, **not** from 0.
- The **first valid occurrence** of a number for `nums[j]` **should be considered**.
- You only want to **skip duplicates** *after* that first one.

### ✅ So `j > i + 1` ensures:

- The first value at `j` for each `i` is accepted.
- Only subsequent duplicates are skipped (to avoid repeated quadruplets).

### 🔍 Example

Given:

```
nums = [-2, -2, -1, 0, 0, 1, 2, 2]
```

Fix `i = 0` → `nums[i] = -2`.

- `j = 1`: `nums[j] == nums[j - 1] == -2`  
  But this is the **first** use of `-2` as the second number → ✅ **don't skip**
- `j = 2`: Now `nums[j] == nums[j - 1]` again → ❌ **skip**

Hence:

```
if (j > i + 1 && nums[j] == nums[j - 1]) continue;
```

is the correct form.

---

## 🧠 Solution Strategy for 3Sum and 4Sum

### ✅ Core Idea (Both Problems)

1. **Sort the array**
2. Use **nested loops** to fix elements
3. Use **two pointers** to find remaining values
4. **Skip duplicates** to avoid repeating results
5. Return all unique triplets / quadruplets

---

## 📈 Time and Space Complexity

### 3Sum

- **Time:** O(n²)
  - One loop over `i`
  - Two pointers for each `i`
- **Space:** O(1) (excluding result)

### 4Sum

- **Time:** O(n³)
  - Two nested loops (`i`, `j`)
  - Two-pointer for each pair
- **Space:** O(1) (excluding result)

---

## 💻 3Sum Code

<!-- Paste your 3Sum solution below -->

```cpp

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        int n = nums.size();
    
    for (int i = 0; i < n - 2; ++i) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        
        int j = i + 1, k = n - 1;
        
        while (j < k) {
            int sum = nums[i] + nums[j] + nums[k];
            
            if (sum == 0) {
                result.push_back({nums[i], nums[j], nums[k]});

                while (j < k && nums[j] == nums[j + 1]) ++j;
                while (j < k && nums[k] == nums[k - 1]) --k;
                ++j;
                --k;
            } 
            else if (sum < 0) {
                ++j; 
            } 
            else {
                --k;
            }
        }
    }
    
    return result;
    }
};
```

---

## 💻 4Sum Code

<!-- Paste your 4Sum solution below -->

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> result;
        if(nums.size()<4) return result ;
        for(int i = 0 ; i<nums.size()-3; i++){
            if(i>0 && nums[i]==nums[i-1]) continue ;
            for(int j = i+1 ; j<nums.size()-2; j++){
                if(j>i+1 && nums[j]==nums[j-1]) continue ;
                int k = j+1 ;
                int l = nums.size()-1 ;
                while(k<l){
                    long long s = (long long)nums[i] + nums[j] + nums[k] + nums[l];

                    if(s==target){
                        result.push_back({nums[i], nums[j], nums[k], nums[l]});
                        while(k<l && nums[k]==nums[k+1]) k++;
                        while(k<l && nums[l]==nums[l-1]) l--;
                        k++;
                        l--;
                    }else if(s<target){
                        k++;
                    }else{
                        l--;
                    }
                }
            }
        }return result;
    }
};
```

---

