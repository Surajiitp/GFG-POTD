# Adventure in a Maze

## Problem

Given an `n × n` grid where every cell contains one of the values `1`, `2`, or `3`.

The value of each cell determines the allowed movement:

- `1` → Move **Right** only
- `2` → Move **Down** only
- `3` → Move **Right or Down**

We start from the top-left cell `(0,0)` and need to reach the bottom-right cell `(n-1,n-1)`.

For every valid path:

- **Adventure** = sum of all cell values visited in that path.
- Find the **total number of distinct valid paths**.
- Find the **maximum possible Adventure** among all valid paths.

Return:

```text
[totalPaths, maxAdventure]


complete soln

class Solution {
public:
    vector<int> findWays(vector<vector<int>>& grid) {

        int n = grid.size();
        const long long MOD = 1000000007;

        vector<vector<long long>> ways(
            n, vector<long long>(n, 0)
        );

        vector<vector<int>> best(
            n, vector<int>(n, -1)
        );

        ways[0][0] = 1;
        best[0][0] = grid[0][0];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {

                if (ways[i][j] == 0)
                    continue;

                // Right
                if ((grid[i][j] == 1 || grid[i][j] == 3)
                    && j + 1 < n) {

                    ways[i][j + 1] =
                        (ways[i][j + 1] + ways[i][j]) % MOD;

                    best[i][j + 1] =
                        max(best[i][j + 1],
                            best[i][j] + grid[i][j + 1]);
                }

                // Down
                if ((grid[i][j] == 2 || grid[i][j] == 3)
                    && i + 1 < n) {

                    ways[i + 1][j] =
                        (ways[i + 1][j] + ways[i][j]) % MOD;

                    best[i + 1][j] =
                        max(best[i + 1][j],
                            best[i][j] + grid[i + 1][j]);
                }
            }
        }

        // No valid path
        if (ways[n - 1][n - 1] == 0)
            return {0, 0};

        return {
            (int)ways[n - 1][n - 1],
            best[n - 1][n - 1]
        };
    }
};


