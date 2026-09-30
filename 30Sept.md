# Ways to Reach Origin

## Problem Statement

Geek is standing at a point `(x, y)` on a 2D grid and wants to reach the origin `(0, 0)`.

From any point `(x, y)`, Geek can move in only two directions:

- **Left:** `(x, y) -> (x - 1, y)`
- **Down:** `(x, y) -> (x, y - 1)`

Find the total number of distinct paths for Geek to reach `(0, 0)` from `(x, y)`.

Since the answer can be very large, return it modulo:

```text
10^9 + 7
Examples
Example 1

Input:

x = 3, y = 0

Output:

1

Explanation:

Since y = 0, Geek can only move left:

(3,0) -> (2,0) -> (1,0) -> (0,0)

So there is only 1 possible path.

Example 2

Input:

x = 3, y = 6

Output:

84

Explanation:

To reach (0,0) from (3,6), Geek needs:

3 left moves
6 down moves

Total moves:

3 + 6 = 9

The number of distinct arrangements of these moves is:

9C3 = 84

Therefore, the answer is:

84
Approach

We can solve this problem using Dynamic Programming.

Let:

dp[i][j]

represent the number of ways to reach the origin (0,0) from point (i,j).

From (i,j), Geek has two possible moves:

Move left to (i-1,j)
Move down to (i,j-1)

Therefore:

dp[i][j] = dp[i-1][j] + dp[i][j-1]
Base Cases

If i = 0, there is only one way to reach the origin:

(0,j) -> (0,j-1) -> ... -> (0,0)

So:

dp[0][j] = 1

Similarly, if j = 0:

dp[i][0] = 1
Algorithm
Create a DP table of size (x+1) × (y+1).
Initialize the first row with 1.
Initialize the first column with 1.

For every remaining cell:

dp[i][j] = dp[i-1][j] + dp[i][j-1]
Take modulo 10^9 + 7 at every step.
Return dp[x][y].
C++ Solution
class Solution {
public:
    int ways(int x, int y) {
        const int MOD = 1e9 + 7;

        vector<vector<int>> dp(
            x + 1,
            vector<int>(y + 1, 0)
        );

        // Base case: x = 0
        for (int i = 0; i <= x; i++) {
            dp[i][0] = 1;
        }

        // Base case: y = 0
        for (int j = 0; j <= y; j++) {
            dp[0][j] = 1;
        }

        // Fill DP table
        for (int i = 1; i <= x; i++) {
            for (int j = 1; j <= y; j++) {
                dp[i][j] =
                    (dp[i - 1][j] + dp[i][j - 1]) % MOD;
            }
        }

        return dp[x][y];
    }
};
Dry Run

For:

x = 3
y = 2

The DP table becomes:

       j
       0   1   2
     +---+---+---+
 i=0 | 1 | 1 | 1 |
     +---+---+---+
 i=1 | 1 | 2 | 3 |
     +---+---+---+
 i=2 | 1 | 3 | 6 |
     +---+---+---+
 i=3 | 1 | 4 | 10|
     +---+---+---+

Therefore:

dp[3][2] = 10

So there are 10 distinct paths.

Complexity Analysis

Let x and y be the coordinates.

Time Complexity
O(x × y)

We visit every cell of the DP table once.

Space Complexity
O(x × y)

We use a 2D DP table.

Mathematical Interpretation

The problem can also be solved using combinations.

To reach (0,0) from (x,y), we need:

x left moves
y down moves

Total moves:

x + y

Therefore, the number of possible paths is:

(x + y) C x

or equivalently:

(x + y) C y

For example:

x = 3, y = 6

Ways = 9C3
     = 84

The DP solution is simpler and avoids directly calculating large factorials.

Key Concept

This problem is a classic example of:

Dynamic Programming
Grid DP
Counting Paths
Pascal's Triangle
Combinations

The main recurrence is:

dp[i][j] = dp[i-1][j] + dp[i][j-1]
Tags

Dynamic Programming Grid DP Counting Math Combinatorics C++
