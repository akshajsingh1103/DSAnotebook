# 🌳 Counting Spanning Trees using the Matrix-Tree Theorem

This document outlines the steps to calculate the **number of spanning trees** in an undirected graph using the **Matrix-Tree Theorem**.

---

## 📚 Theory


## 🌐 Kirchhoff’s Theorem (Matrix-Tree Theorem)

**Kirchhoff’s Theorem** provides a powerful way to compute the **number of spanning trees** in a connected undirected graph using **linear algebra**.

---

### 🧱 Step-by-Step Explanation

1. **Construct the Laplacian Matrix `L`:**
   - For a graph with `n` nodes:
     - `L[i][i]` = degree of node `i`
     - `L[i][j]` = `-1` if there is an edge between node `i` and node `j`
     - `L[i][j]` = `0` otherwise

2. **Remove any one row and one column from `L`:**
   - This produces a cofactor matrix `L'` of size `(n-1) x (n-1)`

3. **Calculate the determinant of `L'`:**
   - The value of this determinant is equal to the **total number of spanning trees** of the graph.


### 🧠 Special Case: Cayley’s Formula

For a **complete graph** of `n` nodes (`Kₙ`), every node is connected to every other node.

- The number of spanning trees is given by:  
  \[
  \{Number of Trees} = n^{n-2}
  \]

This is known as **Cayley’s Formula**, which is a special case of Kirchhoff’s theorem.

---

## Algorithm

### STEP 1: Create Adjacency Matrix
Construct the **adjacency matrix** `A` of the given undirected graph.  
- Each cell `A[i][j]` is `1` if there is an edge between node `i` and node `j`, and `0` otherwise.

### STEP 2: Replace Diagonal with Degree of Nodes
Transform the matrix into a **Laplacian matrix** `L`.  
- For each diagonal element `L[i][i]`, replace it with the **degree** of node `i` (i.e., the number of edges connected to node `i`).

### STEP 3: Replace Non-Diagonal 1s with -1
In the Laplacian matrix `L`:
- Replace all non-diagonal `1`s with `-1`.  
This step gives the **correct Laplacian matrix** used for the Matrix-Tree Theorem.

### STEP 4: Calculate Cofactor of Any Element
From the Laplacian matrix `L`, form a **cofactor matrix** by:
- Deleting any one row and its corresponding column.
- Calculating the **determinant** of the resulting matrix.

### STEP 5: Total Number of Spanning Trees
The determinant obtained in the previous step is the **total number of spanning trees** for the given graph.

---

## 🧪 Code Implementation

```cpp
// User function Template for C++

class Solution {
  public:

    // Function to get the minor of a matrix after removing specified row and column
    vector<vector<int>> findMinor(vector<vector<int>> & matrix, int row, int col) {
        int n = matrix.size();
        vector<vector<int>> ans(n - 1, vector<int>(n - 1, 0));
        int r = 0;

        for (int i = 0; i < n; i++) {
            if (i == row) continue;  // Skip the selected row
            int c = 0;
            for (int j = 0; j < n; j++) {
                if (j == col) continue;  // Skip the selected column

                // Fill the minor matrix
                ans[r][c] = matrix[i][j];
                c++;
            }
            r++;
        }

        return ans;
    }

    // Recursive function to compute determinant of a matrix
    int determinant(vector<vector<int>>& matrix) {
        int n = matrix.size();

        // Base Case 1: Single element
        if (n == 1) return matrix[0][0];

        // Base Case 2: 2x2 matrix
        if (n == 2) {
            return matrix[0][0]*matrix[1][1] - matrix[0][1]*matrix[1][0];
        }

        int det = 0;
        int sign = 1;

        // Laplace expansion across the first row
        for (int i = 0; i < n; i++) {
            vector<vector<int>> minor = findMinor(matrix, 0, i);
            det += sign * matrix[0][i] * determinant(minor);
            sign *= -1;  // Alternate signs
        }

        return det;
    }

    // Main function to count spanning trees using Matrix-Tree Theorem
    int countSpanningTrees(vector<vector<int>>& graph, int n, int m) {

        // Step 1-3: Construct Laplacian matrix
        vector<vector<int>> lm(n, vector<int>(n, 0));
        for (auto& edge : graph) {
            int u = edge[0];
            int v = edge[1];

            // Decrease off-diagonal elements for adjacency (-1)
            lm[u][v] = -1;
            lm[v][u] = -1;

            // Increase diagonal elements with degree
            lm[u][u]++;
            lm[v][v]++;
        }

        // Step 4: Remove one row and column (e.g., row 0 and col 0) to compute cofactor
        auto reduced = findMinor(lm, 0, 0);

        // Step 5: Determinant of cofactor = number of spanning trees
        return determinant(reduced);
    }
};

```
---

## 🔁 Time Complexity Analysis

At each level:
- Number of recursive calls: factorial growth → `n!`
- Cost per call (minor construction): `O(n²)`

**Total Time Complexity:**
T(n) = O(n! * n²)

---

## 💾 Space Complexity

- Space for Laplacian matrix and minor matrices: `O(n²)`
- Recursive call stack depth: `O(n)`

**Total Space Complexity:**
O(n²)
---