Minimum Cost Selection

Problem

Given an n x 3 matrix mat[][], where each row represents the costs of three available choices at a shop, select exactly one choice from every row such that the same choice is not selected in two adjacent rows.

Return the minimum total cost required.

Example 1

Input:

mat = [[1, 50, 50],
       [50, 50, 50],
       [1, 50, 50]]

Output:

52

Explanation:

Choose:

Row 1 → Choice 1 = 1

Row 2 → Choice 2 = 50

Row 3 → Choice 1 = 1

Total cost:

1 + 50 + 1 = 52

Example 2

Input:

mat = [[1, 4, 1],
       [3, 2, 2],
       [3, 2, 3]]

Output:

5

Explanation:

Choose:

Row 1 → Choice 1 = 1

Row 2 → Choice 2 = 2

Row 3 → Choice 3 = 2

Total cost:

1 + 2 + 2 = 5

Approach

This problem can be solved efficiently using Dynamic Programming.

For each row, maintain the minimum cost of reaching that row when choosing each of the three choices.

Let:

dp0 = minimum cost up to the current row if choice 0 is selected.

dp1 = minimum cost up to the current row if choice 1 is selected.

dp2 = minimum cost up to the current row if choice 2 is selected.

Transition

If we select choice 0 in the current row, the previous row cannot have choice 0.

Therefore:

new0 = mat[i][0] + min(dp1, dp2)

Similarly:

new1 = mat[i][1] + min(dp0, dp2)
new2 = mat[i][2] + min(dp0, dp1)

After calculating the new values, update dp0, dp1, and dp2.

At the end, the answer is the minimum of the three possibilities:

min(dp0, dp1, dp2)

Algorithm

Initialize the DP values using the first row:

dp0 = mat[0][0]
dp1 = mat[0][1]
dp2 = mat[0][2]

Traverse the remaining rows.

For each row, calculate:

new0 = mat[i][0] + min(dp1, dp2)
new1 = mat[i][1] + min(dp0, dp2)
new2 = mat[i][2] + min(dp0, dp1)

Update:

dp0 = new0
dp1 = new1
dp2 = new2

Return:

min(dp0, dp1, dp2)

C++ Solution

class Solution {
public:
    int minCost(vector<vector<int>>& mat) {
        int n = mat.size();

        int dp0 = mat[0][0];
        int dp1 = mat[0][1];
        int dp2 = mat[0][2];

        for (int i = 1; i < n; i++) {
            int new0 = mat[i][0] + min(dp1, dp2);
            int new1 = mat[i][1] + min(dp0, dp2);
            int new2 = mat[i][2] + min(dp0, dp1);

            dp0 = new0;
            dp1 = new1;
            dp2 = new2;
        }

        return min({dp0, dp1, dp2});
    }
};

Complexity Analysis

Time Complexity

O(n)

Each row is processed once, and only a constant number of operations are performed.

Space Complexity

O(1)

Only three DP variables are maintained, regardless of the number of rows.

Key Idea

The important observation is:

When choosing a particular option in the current row, we only need the minimum cost from the previous row among the other two options.

This allows us to solve the problem in O(n) time and O(1) extra space.
