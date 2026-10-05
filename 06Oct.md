# Longest Increasing Path in Matrix

**Platform:** GeeksforGeeks
**Difficulty:** Hard
**Points:** 8
**Accuracy:** 44.5%

## Problem Statement

Given an `n × m` matrix, find the length of the **longest increasing path**.

A valid path must satisfy:

* Values must be **strictly increasing**.
* A cell cannot be visited more than once.
* Movement is allowed only in four directions:

  * Up
  * Down
  * Left
  * Right
* Diagonal movement is not allowed.
* Movement outside the matrix is not allowed.

### Example 1

```text
Input:
n = 3, m = 3
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

Output:
5
```

One longest path is:

```text
1 → 2 → 3 → 6 → 9
```

### Example 2

```text
Input:
n = 3, m = 3
matrix = [
    [3, 4, 5],
    [6, 2, 6],
    [2, 2, 1]
]

Output:
4
```

One longest path is:

```text
3 → 4 → 5 → 6
```

---

## Approach

We can model the matrix as a **Directed Acyclic Graph (DAG)**.

For every pair of adjacent cells:

```text
mat[neighbor] > mat[current]
```

we create a directed edge:

```text
current → neighbor
```

Because every edge goes from a smaller value to a larger value, a cycle is impossible.

Therefore, we can find the longest path using **Topological Sorting with Kahn's Algorithm**.

### Key Idea

1. Calculate the **indegree** of every cell.
2. A cell with `indegree = 0` has no smaller neighboring cell pointing to it, so it can be a starting point.
3. Put all such cells into a queue.
4. Process the queue **level by level**.
5. Every BFS level represents one length of the increasing path.
6. For each cell, visit its larger neighbors and decrease their indegree.
7. When a neighbor's indegree becomes `0`, add it to the queue.
8. The number of BFS levels is the answer.

---

## Why BFS Levels Give the Answer

Suppose the increasing path is:

```text
1 → 2 → 3 → 6 → 9
```

The cells are processed in levels:

```text
Level 1: 1
Level 2: 2
Level 3: 3
Level 4: 6
Level 5: 9
```

Therefore:

```text
Longest Path = 5
```

---

## Algorithm

```text
1. Create an indegree matrix.
2. For every cell:
      Check its four neighboring cells.
      If neighbor value > current value:
          Increase neighbor's indegree.
3. Push every cell with indegree = 0 into the queue.
4. Initialize ans = 0.
5. While queue is not empty:
      Let size = current queue size.
      Increment ans.

      Process exactly 'size' cells:
          For every larger neighboring cell:
              Decrease its indegree.
              If indegree becomes 0:
                  Push it into the queue.
6. Return ans.
```

---

## C++ Solution

```cpp
class Solution {
public:
    int longIncPath(vector<vector<int>>& mat, int n, int m) {

        vector<vector<int>> indegree(n, vector<int>(m, 0));

        int dr[] = {-1, 1, 0, 0};
        int dc[] = {0, 0, -1, 1};

        // Calculate indegree
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                for (int k = 0; k < 4; k++) {

                    int ni = i + dr[k];
                    int nj = j + dc[k];

                    if (ni >= 0 && ni < n &&
                        nj >= 0 && nj < m &&
                        mat[ni][nj] > mat[i][j]) {

                        indegree[ni][nj]++;
                    }
                }
            }
        }

        queue<pair<int, int>> q;

        // Starting cells
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                if (indegree[i][j] == 0) {
                    q.push({i, j});
                }
            }
        }

        int ans = 0;

        // BFS level by level
        while (!q.empty()) {

            int size = q.size();
            ans++;

            while (size--) {

                auto [i, j] = q.front();
                q.pop();

                for (int k = 0; k < 4; k++) {

                    int ni = i + dr[k];
                    int nj = j + dc[k];

                    if (ni >= 0 && ni < n &&
                        nj >= 0 && nj < m &&
                        mat[ni][nj] > mat[i][j]) {

                        indegree[ni][nj]--;

                        if (indegree[ni][nj] == 0) {
                            q.push({ni, nj});
                        }
                    }
                }
            }
        }

        return ans;
    }
};
```

---

## Complexity Analysis

Let:

```text
N = n × m
```

Each cell has at most 4 neighbors.

### Time Complexity

```text
O(n × m)
```

Each cell and each of its four neighbors is processed a constant number of times.

### Space Complexity

```text
O(n × m)
```

We use:

* `indegree` matrix → `O(n × m)`
* Queue → `O(n × m)` in the worst case.

---

## Important Insight

The main trick is to convert the matrix into a **DAG**.

```text
Smaller value
     |
     ↓
Larger value
     |
     ↓
Even larger value
```

Since values are strictly increasing, cycles cannot exist.

So the problem becomes:

> **Find the longest path in a DAG using Topological Sort.**

The BFS level count directly gives the length of the longest increasing path.

---

## Concepts Used

* Matrix Traversal
* Graph Representation
* Directed Acyclic Graph (DAG)
* Indegree
* Topological Sort
* Kahn's Algorithm
* BFS
* Queue
* Dynamic Graph Processing

---

## Edge Cases

### Single Cell

```text
[[5]]
```

Answer:

```text
1
```

### All Values Equal

```text
[
 [2, 2],
 [2, 2]
]
```

No increasing movement is possible.

Answer:

```text
1
```

### Strictly Increasing Matrix

Every valid increasing connection can be followed, allowing a long path depending on the matrix structure.

---

## Takeaway

This problem is a great example of combining **matrix traversal + graph theory**.

The most important observation is:

> **Every increasing adjacent relationship can be treated as a directed edge from the smaller cell to the larger cell.**

Then **Kahn's Topological Sort + BFS levels** gives the longest increasing path efficiently in `O(n × m)` time.
