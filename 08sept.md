Word in Grid - All Occurrences

Difficulty: Medium
Accuracy: 22.88%
Points: 4

📝 Problem Statement

Given a 2D grid mat[][] of size n × m consisting of characters and a string word, find all starting positions where the word occurs in the grid.

The word can be formed by moving in any of the 8 directions:

Up
Down
Left
Right
4 Diagonal directions

The movement must remain in a straight line, meaning the direction cannot change while forming the word.

Each cell can be used at most once for an occurrence.

Return all unique starting coordinates in lexicographically smallest order.

💡 Approach

We check every cell of the grid as a possible starting position.

Steps
Traverse every cell (i, j).
If mat[i][j] is not equal to the first character of word, skip it.
Try all 8 possible directions from that cell.
Move continuously in the selected direction.
Check whether every character of word matches.
If the complete word is found, add {i, j} to the answer.
Break after finding one valid direction for that starting cell to avoid duplicate coordinates.

Since we traverse the grid from top-left to bottom-right, the coordinates are naturally generated in lexicographically sorted order.

🧭 8 Directions
(-1,-1)  (-1,0)  (-1,1)

( 0,-1)    X     ( 0,1)

( 1,-1)  ( 1,0)  ( 1,1)

These represent all possible straight-line directions.

💻 C++ Solution
class Solution {
public:
    vector<vector<int>> searchWord(vector<vector<char>> grid, string word) {
        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> ans;

        // 8 possible directions
        int dx[] = {-1, -1, -1, 0, 0, 1, 1, 1};
        int dy[] = {-1, 0, 1, -1, 1, -1, 0, 1};

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                // First character must match
                if (grid[i][j] != word[0])
                    continue;

                // Try all 8 directions
                for (int d = 0; d < 8; d++) {

                    int x = i;
                    int y = j;
                    int k = 0;

                    while (k < word.size()) {

                        // Check boundaries and character
                        if (x < 0 || x >= n || y < 0 || y >= m ||
                            grid[x][y] != word[k]) {
                            break;
                        }

                        x += dx[d];
                        y += dy[d];
                        k++;
                    }

                    // Complete word found
                    if (k == word.size()) {
                        ans.push_back({i, j});
                        break;
                    }
                }
            }
        }

        return ans;
    }
};
🔍 Example
Input
mat = {
    {a, b, a, b},
    {a, b, e, b},
    {e, b, e, b}
}

word = "abe"
Valid occurrences
(0,0) → a
        ↓
        b
        ↘
          e
(0,2) → a
        ↓
        b
        ↙
          e
(1,0) → a → b → e
Output
{{0,0}, {0,2}, {1,0}}
⏱️ Complexity Analysis

Let:

n = number of rows
m = number of columns
L = length of the word

For every cell, we check 8 directions and at most L characters.

Time Complexity
O(n × m × 8 × L)

Since 8 is constant:

O(n × m × L)
Space Complexity
O(1)

excluding the space required for the output.

⭐ Key Takeaways
Check every cell as a potential starting point.
There are exactly 8 possible directions.
Once a direction is selected, keep moving in the same direction.
No visited array is required because the word is searched in a straight line.
Traverse row-wise and column-wise to naturally maintain lexicographical order.
Stop checking directions once the word is successfully found from a starting cell.
copy paste with approach and soln
