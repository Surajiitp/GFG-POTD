# Zigzag Sequence — Dynamic Programming

## 📌 Problem Statement

Given an `n × n` matrix `mat`, select **exactly one element from each row** such that two consecutive selected elements **cannot be from the same column**.

Find the **maximum possible sum** of the selected elements.

### Example

```text
mat =
[
  [5, 2, 3],
  [1, 9, 4],
  [7, 6, 8]
]
```

We need to choose one element from every row, but the column chosen in consecutive rows must be different.

---

## 💡 Approach

We use **Dynamic Programming + Maximum and Second Maximum**.

### DP Definition

```cpp
dp[j] = maximum sum possible after processing the current row
        when column j is selected
```

Initially:

```cpp
dp = mat[0];
```

because we can choose any element from the first row.

### Transition

For every new row `i`, if we select column `j`:

```text
newdp[j] = mat[i][j] + best previous value
```

But the previous selected column **cannot be `j`**.

So we need the maximum value in the previous `dp` excluding column `j`.

Instead of checking every column for every `j`, we maintain:

* `maxi1` → largest value in `dp`
* `maxi2` → second largest value
* `idx1` → column index of `maxi1`

Then:

```cpp
if (j == idx1)
    newdp[j] = mat[i][j] + maxi2;
else
    newdp[j] = mat[i][j] + maxi1;
```

This reduces the transition from `O(n²)` per row to `O(n)`.

---

## 🔍 Why Maximum and Second Maximum?

Suppose:

```text
dp = [10, 20, 15]
```

The maximum is:

```text
maxi1 = 20
idx1 = 1
```

If we want to select column `1` again, we cannot use `20` because it belongs to the same column.

Therefore, we use:

```text
maxi2 = 15
```

For other columns, we can safely use `20`.

---

## 🧠 Algorithm

1. Initialize `dp` with the first row.
2. For every remaining row:

   * Find the largest and second-largest values in `dp`.
   * Store the index of the largest value.
   * Calculate `newdp`:

     * If current column is the index of maximum → use second maximum.
     * Otherwise → use maximum.
3. Replace `dp` with `newdp`.
4. Return the maximum value in the final `dp`.

---

## 💻 C++ Solution

```cpp
class Solution {
public:
    int zigzagSequence(vector<vector<int>>& mat) {
        int n = mat.size();

        // dp[j] = maximum sum ending at column j
        vector<int> dp = mat[0];

        for (int i = 1; i < n; i++) {

            // Find largest and second largest in previous dp
            int maxi1 = -1, maxi2 = -1;
            int idx1 = -1;

            for (int j = 0; j < n; j++) {
                if (dp[j] > maxi1) {
                    maxi2 = maxi1;
                    maxi1 = dp[j];
                    idx1 = j;
                }
                else if (dp[j] > maxi2) {
                    maxi2 = dp[j];
                }
            }

            vector<int> newdp(n);

            for (int j = 0; j < n; j++) {

                // Same column cannot be selected consecutively
                if (j == idx1)
                    newdp[j] = mat[i][j] + maxi2;
                else
                    newdp[j] = mat[i][j] + maxi1;
            }

            dp = newdp;
        }

        return *max_element(dp.begin(), dp.end());
    }
};
```

---

## ⏱️ Complexity

For each row, we scan the array a constant number of times.

### Time Complexity

```text
O(n²)
```

### Space Complexity

```text
O(n)
```

We only store the DP values for the current and previous row.

---

## 🚀 Key Takeaway

The important optimization is:

> **To find the best previous value excluding the current column, maintain the largest and second-largest DP values.**

This avoids checking all previous columns for every current column.

**Pattern:**
`Dynamic Programming + Maximum/Second Maximum`
