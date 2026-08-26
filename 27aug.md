Maximum Area of a Rectangle After Column Permutations

Problem

Given a binary matrix mat containing only 0s and 1s, you are allowed to rearrange the columns in any order.

Find the maximum possible area of a rectangle containing only 1s after rearranging the columns.

Example

Input:
[
  [0, 0, 1, 0],
  [0, 1, 1, 1],
  [1, 1, 1, 1]
]

Output:
6

Approach

The key idea is to process the matrix row by row.

1. Calculate consecutive heights

For every column, maintain the number of consecutive 1s ending at the current row.

If mat[i][j] == 1:

height[j]++;

Otherwise:

height[j] = 0;

For example, if the current heights are:

[3, 1, 2, 3]

we can rearrange the columns because column swaps are allowed.

2. Sort the heights

Sort the heights in descending order:

[3, 3, 2, 1]

If we use the first k columns, the height of the rectangle is the k-th height and the width is k.

So for every position j:

area = height[j] * (j + 1);

Take the maximum area over all rows.

C++ Solution

class Solution {
public:
    int maxArea(vector<vector<int>>& mat) {
        int n = mat.size();
        int m = mat[0].size();

        vector<int> height(m, 0);
        int ans = 0;

        for (int i = 0; i < n; i++) {

            // Calculate consecutive 1s in each column
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 1)
                    height[j]++;
                else
                    height[j] = 0;
            }

            // Columns can be swapped, so sort heights
            vector<int> h = height;
            sort(h.rbegin(), h.rend());

            // Try every possible width
            for (int j = 0; j < m; j++) {
                int width = j + 1;
                ans = max(ans, h[j] * width);
            }
        }

        return ans;
    }
};

Why Does Sorting Work?

Suppose the heights are:

[4, 3, 3, 1]

After sorting:

[4, 3, 3, 1]

For width 3, the smallest height among those three columns is 3.

Therefore:

Area = 3 × 3 = 9

Because columns can be freely swapped, we can always place the largest heights next to each other. Sorting lets us evaluate the best possible arrangement directly.

Complexity

Let:

n = number of rows

m = number of columns

For every row:

Updating heights takes O(m)

Sorting takes O(m log m)

Checking all widths takes O(m)

Therefore:

Time Complexity:  O(n × m log m)
Space Complexity: O(m)

Key Takeaway

This problem can be solved using:

Prefix/consecutive height calculation

Sorting

Greedy area calculation

The important observation is that column permutations allow us to sort the column heights for every row.
