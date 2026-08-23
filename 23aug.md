Geek in a Maze

Problem Statement

Given a maze mat[][] of size n × m, where each cell is either:

'.' representing an empty cell

'#' representing an obstacle

Find the number of distinct empty cells that Geek can visit starting from the cell (r, c).

Geek can:

Move up, down, left, or right to an adjacent non-obstacle cell.

Make at most u upward moves on any path.

Make at most d downward moves on any path.

Make unlimited left and right moves.

If the starting cell is an obstacle, return 0.

There can be multiple paths from the starting cell.

Examples

Example 1

Input:

r = 1, c = 0
u = 1, d = 1

mat = [
    ['.', '.', '.'],
    ['.', '#', '.'],
    ['#', '.', '.']
]

Output:

5

Explanation:

One possible path is:

(1,0) -> (0,0) -> (0,1) -> (0,2) -> (1,2)

The cells (1,1) and (2,0) are obstacles, so Geek can visit 5 distinct empty cells.

Example 2

Input:

r = 2, c = 1
u = 2, d = 2

mat = [
    ['.', '.', '.'],
    ['.', '#', '.'],
    ['.', '.', '.']
]

Output:

8

Explanation:

Geek can visit all 8 empty cells.

One possible path is:

(2,1) -> (2,2) -> (1,2) -> (0,2)
      -> (0,1) -> (0,0) -> (1,0) -> (2,0)

Example 3

Input:

r = 2, c = 1
u = 1, d = 0

mat = [
    ['.', '.', '.'],
    ['.', '#', '.'],
    ['.', '.', '.']
]

Output:

5

Explanation:

Since no downward moves are allowed, Geek can reach the cells using paths such as:

(2,1) -> (2,0) -> (1,0)

and

(2,1) -> (2,2) -> (1,2)

Therefore, 5 distinct cells are reachable.

Constraints

1 ≤ n, m ≤ 10^6

0 ≤ r, c < 10^6

0 ≤ u, d ≤ 10^6

mat[i][j] is either '.' or '#'

The starting position (r, c) is inside the matrix.

Approach

This problem can be solved using BFS (Breadth-First Search).

A simple visited[row][col] array is not enough because the same cell may be reached using different numbers of upward and downward moves.

The key observation is:

downUsed - upUsed = currentRow - startingRow

Therefore:

downUsed = upUsed + (currentRow - startingRow)

So we only need to store the minimum number of upward moves required to reach every cell.

We maintain:

upUsed[x][y]

where upUsed[x][y] is the minimum number of upward moves needed to reach cell (x, y).

BFS State

For the current cell (x, y):

int currUp = upUsed[x][y];

The number of downward moves already used can be calculated as:

int currDown = currUp + (x - r);

Then we try all four directions.

1. Move Up

Moving up increases the number of upward moves by 1.

currUp + 1

We can move up only if:

currUp + 1 <= u

and the new value is smaller than the previously stored value.

2. Move Down

Moving down does not change upUsed.

The new number of downward moves is:

currDown + 1

We allow the move only if:

currDown + 1 <= d

3. Move Left

Left movement does not affect either upUsed or downUsed.

4. Move Right

Right movement also does not affect either upUsed or downUsed.

For every move, we also make sure that:

The next cell is inside the matrix.

The next cell is not an obstacle.

We improve the stored upUsed value when necessary.

Why Do We Store Minimum Upward Moves?

Suppose the same cell can be reached by two different paths.

If one path reaches it using fewer upward moves, that path is always at least as useful because:

It uses less of the limited upward-move budget.

The corresponding downward count is determined by the row difference.

It can potentially allow further movement that another path cannot.

Therefore, for every cell, keeping only the minimum upUsed value is sufficient.

Algorithm

Get the dimensions n and m.

If mat[r][c] == '#', return 0.

Create upUsed[n][m] and initialize every value to INT_MAX.

Set:

upUsed[r][c] = 0;

Push (r, c) into a queue.

Perform BFS.

For every popped cell:

Calculate currUp.

Calculate currDown.

Try moving up, down, left, and right.

Update a cell only if the new path requires fewer upward moves.

Count all cells where:

upUsed[i][j] != INT_MAX

Return the count.

C++ Solution

class Solution {
public:

    int numberOfCells(int r, int c, int u, int d,
                      vector<vector<char>> &mat) {

        int n = mat.size();
        int m = mat[0].size();

        // Starting cell is an obstacle
        if (mat[r][c] == '#')
            return 0;

        // Minimum upward moves required to reach each cell
        vector<vector<int>> upUsed(
            n, vector<int>(m, INT_MAX)
        );

        queue<pair<int, int>> q;

        // Starting cell
        upUsed[r][c] = 0;
        q.push({r, c});

        while (!q.empty()) {

            auto [x, y] = q.front();
            q.pop();

            int currUp = upUsed[x][y];

            // Number of downward moves used so far
            int currDown = currUp + (x - r);

            // Move Up
            if (x - 1 >= 0 &&
                mat[x - 1][y] == '.' &&
                currUp + 1 <= u &&
                currUp + 1 < upUsed[x - 1][y]) {

                upUsed[x - 1][y] = currUp + 1;
                q.push({x - 1, y});
            }

            // Move Down
            if (x + 1 < n &&
                mat[x + 1][y] == '.' &&
                currDown + 1 <= d &&
                currUp < upUsed[x + 1][y]) {

                upUsed[x + 1][y] = currUp;
                q.push({x + 1, y});
            }

            // Move Left
            if (y - 1 >= 0 &&
                mat[x][y - 1] == '.' &&
                currUp < upUsed[x][y - 1]) {

                upUsed[x][y - 1] = currUp;
                q.push({x, y - 1});
            }

            // Move Right
            if (y + 1 < m &&
                mat[x][y + 1] == '.' &&
                currUp < upUsed[x][y + 1]) {

                upUsed[x][y + 1] = currUp;
                q.push({x, y + 1});
            }
        }

        // Count all reachable cells
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                if (upUsed[i][j] != INT_MAX)
                    ans++;
            }
        }

        return ans;
    }
};

Complexity Analysis

Let:

N = n × m

Each cell is processed when we find a better value for upUsed.

Time Complexity

O(n × m)

Space Complexity

O(n × m)

Important Optimization

A priority queue approach may maintain a state such as:

(upUsed, downUsed, row, col)

This creates extra heap operations and can lead to Time Limit Exceeded on large test cases.

The optimized approach stores only:

upUsed[row][col]

because:

downUsed = upUsed + (row - startingRow)

This removes the need to maintain both movement counts in the BFS state.

Key Learning

The main idea of this problem is state optimization.

Instead of storing:

upUsed
downUsed
row
col

we only store:

upUsed[row][col]

because the downward moves can be derived mathematically from the current row.

This technique is useful in grid and graph problems where one part of the state can be derived from another.
