# Shortest Safe Route in Grid

## Problem

Given a `n x m` matrix `mat[][]` containing `0` and `1`:

* `0` represents a landmine.
* `1` represents a safe cell.
* A cell is considered **unsafe** if it contains a landmine or is directly adjacent to a landmine.
* We can move only in four directions:

  * Up
  * Down
  * Left
  * Right

The task is to find the minimum number of steps required to travel from **any safe cell in the leftmost column** to **any safe cell in the rightmost column**.

If no such path exists, return `-1`.

---

## Example

### Input

```text
mat[][] =
[
    [1, 0, 1, 1, 1],
    [1, 1, 1, 1, 1],
    [1, 1, 1, 1, 1],
    [1, 1, 1, 0, 1],
    [1, 1, 1, 1, 0]
]
```

### Output

```text
6
```

### Explanation

We first mark all landmines and their directly adjacent cells as unsafe.

Then, starting from every safe cell in the first column, we perform BFS to find the shortest route to the last column.

---

## Approach

The solution is divided into two main steps.

### 1. Mark Unsafe Cells

For every cell containing `0`:

* Mark the cell itself as unsafe.
* Mark its four adjacent cells as unsafe if they are inside the matrix.

This ensures that BFS never enters a cell that is dangerous.

### 2. Apply BFS

Since every movement from one cell to another has the same cost (`1` step), **Breadth First Search (BFS)** is the ideal choice.

Instead of starting BFS from a single cell, we start from **all safe cells in the first column**.

For each cell:

1. Remove it from the queue.
2. If it belongs to the last column, return its distance.
3. Explore its four neighboring cells.
4. Add a neighbor to the queue only if:

   * It is inside the matrix.
   * It is safe.
   * It has not been visited.

If BFS finishes without reaching the last column, return `-1`.

---

## Algorithm

```text
1. Create an unsafe matrix of size n x m.

2. Traverse the entire matrix:
   - If mat[i][j] == 0:
       Mark (i, j) unsafe.
       Mark all valid adjacent cells unsafe.

3. Create a queue for BFS and a visited matrix.

4. Add every safe cell from the first column to the queue
   with distance = 1.

5. While the queue is not empty:
   - Take the front cell.
   - If it belongs to the last column, return its distance.
   - Explore all four directions.
   - For every valid, safe and unvisited cell:
       Mark it visited.
       Add it to the queue with distance + 1.

6. If no path reaches the last column, return -1.
```

---

## C++ Solution

```cpp
class Solution {
public:
    int shortestPath(vector<vector<int>>& mat) {
        int n = mat.size();
        int m = mat[0].size();

        vector<vector<bool>> unsafe(
            n, vector<bool>(m, false)
        );

        int dr[] = {-1, 1, 0, 0};
        int dc[] = {0, 0, -1, 1};

        // Mark landmines and adjacent cells as unsafe
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                if (mat[i][j] == 0) {
                    unsafe[i][j] = true;

                    for (int k = 0; k < 4; k++) {
                        int nr = i + dr[k];
                        int nc = j + dc[k];

                        if (nr >= 0 && nr < n &&
                            nc >= 0 && nc < m) {
                            unsafe[nr][nc] = true;
                        }
                    }
                }
            }
        }

        queue<pair<pair<int, int>, int>> q;

        vector<vector<bool>> visited(
            n, vector<bool>(m, false)
        );

        // Start BFS from all safe cells in first column
        for (int i = 0; i < n; i++) {
            if (!unsafe[i][0]) {
                q.push({{i, 0}, 1});
                visited[i][0] = true;
            }
        }

        // BFS
        while (!q.empty()) {

            auto current = q.front();
            q.pop();

            int r = current.first.first;
            int c = current.first.second;
            int dist = current.second;

            // Reached the last column
            if (c == m - 1) {
                return dist;
            }

            for (int k = 0; k < 4; k++) {

                int nr = r + dr[k];
                int nc = c + dc[k];

                if (nr >= 0 && nr < n &&
                    nc >= 0 && nc < m &&
                    !unsafe[nr][nc] &&
                    !visited[nr][nc]) {

                    visited[nr][nc] = true;

                    q.push({
                        {nr, nc},
                        dist + 1
                    });
                }
            }
        }

        return -1;
    }
};
```

---

## Complexity Analysis

Let `n` be the number of rows and `m` be the number of columns.

### Time Complexity

```text
O(n × m)
```

Each cell is processed a constant number of times during unsafe-cell marking and BFS.

### Space Complexity

```text
O(n × m)
```

We use:

* `unsafe` matrix
* `visited` matrix
* BFS queue

---

## Key Concepts

* Breadth First Search (BFS)
* Multi-Source BFS
* Matrix Traversal
* Grid Traversal
* Shortest Path
* Visited Array

---

## Important Insight

The most important observation is that **BFS should start from all safe cells in the first column simultaneously**.

Because every move has equal cost, BFS guarantees that the first time we reach the last column, we have found the shortest possible safe route.

---

## Edge Cases

* If every cell in the first column is unsafe → return `-1`.
* If every cell in the last column is unsafe → return `-1`.
* If the matrix contains only one column → the answer is `1` if any cell in that column is safe, otherwise `-1`.
* If no safe route connects the two columns → return `-1`.

---

## Difficulty

**Medium**

## Platform

**GeeksforGeeks**

## Topic

**Graph / BFS / Matrix**
