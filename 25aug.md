Minimum Moves to Sort Permutation

Problem

Given an array arr[] containing integers from 1 to n exactly once,
sort the array in ascending order.

In one operation, you can pick any element and move it either to the
beginning or to the end of the array.

Return the minimum number of operations required to sort the array.

Examples

Example 1

Input:
arr = [2, 1, 3]

Output:
1

Explanation:
Move 1 to the beginning:
[1, 2, 3]

Example 2

Input:
arr = [4, 3, 1, 2]

Output:
2

Explanation:
Move 3 to the end:
[4, 1, 2, 3]

Then move 4 to the end:
[1, 2, 3, 4]

Approach

The key observation is that the elements which we do not move must
already appear in the correct relative order.

Since the final sorted permutation is:

1, 2, 3, ..., n

the elements that remain untouched must form a consecutive sequence of
values:

x, x+1, x+2, ..., y

and their positions in the original array must also be increasing.

Step 1: Store Positions

Create a pos array where:

pos[x] = index of x in arr

For example:

arr = [4, 3, 1, 2]

pos[1] = 2
pos[2] = 3
pos[3] = 1
pos[4] = 0

Step 2: Find the Longest Valid Consecutive Sequence

For every consecutive pair x and x + 1, check:

pos[x] < pos[x + 1]

If this is true, both elements can remain in their relative positions.

Otherwise, the current consecutive sequence breaks.

We find the longest such sequence.

Step 3: Calculate the Answer

If longest elements can stay where they are, all remaining elements
must be moved.

Therefore:

answer = n - longest

Dry Run

For:

arr = [4, 3, 1, 2]

Positions:

1 -> 2
2 -> 3
3 -> 1
4 -> 0

Now check consecutive values:

pos[1] < pos[2]
2 < 3  -> YES

pos[2] < pos[3]
3 < 1  -> NO

pos[3] < pos[4]
1 < 0  -> NO

The longest valid sequence is:

1, 2

Its length is 2.

So:

answer = 4 - 2
       = 2

C++ Solution

class Solution {
public:
    int minMoves(vector<int>& arr) {
        int n = arr.size();

        vector<int> pos(n + 1);

        for (int i = 0; i < n; i++) {
            pos[arr[i]] = i;
        }

        int longest = 1;
        int current = 1;

        for (int x = 1; x < n; x++) {
            if (pos[x] < pos[x + 1]) {
                current++;
            } else {
                current = 1;
            }

            longest = max(longest, current);
        }

        return n - longest;
    }
};

Complexity

Time Complexity: O(n)

Space Complexity: O(n)

Key Idea

The important observation is:

Keep the longest consecutive sequence x, x+1, x+2, ... whose
positions are already increasing. Move all other elements to the
beginning or end.

Therefore:

Minimum Moves = n - Longest Valid Consecutive Sequence

Constraints

1 <= n <= 10^5
1 <= arr[i] <= n
