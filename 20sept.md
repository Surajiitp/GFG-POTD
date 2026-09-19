# Largest Subsquare with X Boundary

## 📌 Problem Statement

Given a square matrix `mat[][]` of size `n × n`, where each cell contains either `'X'` or `'O'`, find the size of the **largest square submatrix** whose **boundary is completely surrounded by `'X'`**.

The cells inside the square can contain either `'X'` or `'O'`.

### Example

```text
Input:
X X X O
X O X X
X X X X
O X X X

Output:
3
```

The largest valid square has side length `3`.

---

## 💡 Approach

The main challenge is checking whether the four sides of a square contain only `'X'`.

Instead of checking every cell of every possible square repeatedly, we precompute two DP tables:

### 1. `right[i][j]`

`right[i][j]` stores the number of consecutive `'X'` cells starting from `(i, j)` and going **towards the right**.

For example:

```text
X X X O
```

The values become:

```text
3 2 1 0
```

---

### 2. `down[i][j]`

`down[i][j]` stores the number of consecutive `'X'` cells starting from `(i, j)` and going **downwards**.

Example:

```text
X
X
X
O
```

The values become:

```text
3
2
1
0
```

Both tables are calculated from **bottom-right to top-left**.

---

## 🔍 Checking a Square

Suppose `(i, j)` is the top-left corner and the side length is `len`.

The four boundaries must contain only `'X'`:

### Top boundary

```cpp
right[i][j] >= len
```

### Left boundary

```cpp
down[i][j] >= len
```

We already know these two conditions because:

```cpp
len = min(right[i][j], down[i][j]);
```

Now we only need to verify the other two boundaries.

### Bottom boundary

Bottom-left corner:

```cpp
bottom = i + len - 1
```

Check:

```cpp
right[bottom][j] >= len
```

### Right boundary

Bottom-right corner:

```cpp
lastCol = j + len - 1
```

Check:

```cpp
down[i][lastCol] >= len
```

If all four boundaries contain `'X'`, the square is valid.

---

## 🚀 Algorithm

1. Create two DP arrays:

   * `right`
   * `down`

2. Traverse the matrix from bottom-right to top-left.

3. For every `'X'` cell:

   * Calculate consecutive `'X'` cells towards the right.
   * Calculate consecutive `'X'` cells downwards.

4. For every cell `(i, j)`:

   * Start with the largest possible side:

     ```cpp
     len = min(right[i][j], down[i][j]);
     ```
   * Check whether the bottom and right boundaries are also valid.
   * If not, decrease `len`.

5. Keep updating `ans` with the largest valid square.

6. Return `ans`.

---

## ⏱️ Complexity

Let the matrix size be `n × m`.

### Time Complexity

```text
O(n × m × min(n, m))
```

In the worst case, for every cell we may check multiple possible side lengths.

### Space Complexity

```text
O(n × m)
```

Two DP matrices `right` and `down` are used.

---

## 💻 C++ Solution

```cpp
class Solution {
public:
    int largestSubsquare(vector<vector<char>>& mat) {
        int n = mat.size();
        int m = mat[0].size();

        vector<vector<int>> right(n, vector<int>(m, 0));
        vector<vector<int>> down(n, vector<int>(m, 0));

        // Precompute consecutive X's towards right and down
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {

                if (mat[i][j] == 'X') {

                    right[i][j] = 1;
                    down[i][j] = 1;

                    if (j + 1 < m)
                        right[i][j] += right[i][j + 1];

                    if (i + 1 < n)
                        down[i][j] += down[i + 1][j];
                }
            }
        }

        int ans = 0;

        // Try every cell as top-left corner
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                // Maximum possible side length
                int len = min(right[i][j], down[i][j]);

                while (len > ans) {

                    int bottom = i + len - 1;
                    int lastCol = j + len - 1;

                    // Check bottom and right boundaries
                    if (bottom < n && lastCol < m &&
                        right[bottom][j] >= len &&
                        down[i][lastCol] >= len) {

                        ans = len;
                        break;
                    }

                    len--;
                }
            }
        }

        return ans;
    }
};
```

---

## 🧠 Key Insight

The important idea is to **avoid checking every boundary cell individually**.

Instead, we precompute:

```text
right[i][j] → consecutive X's to the right
down[i][j]  → consecutive X's downward
```

Then a square can be validated in **O(1)** time.

For a square of side `len`:

```text
       →→→→→
       X X X X
       X     X
       X     X
       X X X X
       →→→→→

Top    : right[i][j] >= len
Left   : down[i][j] >= len
Bottom : right[bottom][j] >= len
Right  : down[i][lastCol] >= len
```

This converts repeated boundary traversal into simple DP lookups.

---

## 🔑 Pattern Used

* Dynamic Programming
* Matrix Traversal
* Prefix/Suffix Counting
* Boundary Checking

---

## 📚 Problem Type

**GeeksforGeeks — Largest Subsquare with X Boundary**

Difficulty: **Medium**

---

## 🏷️ Tags

```text
#DynamicProgramming
#Matrix
#DP
#GeeksForGeeks
#CPP
#MatrixDP
#CompetitiveProgramming
```
