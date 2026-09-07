Minimum Elements Outside Subsequences

Problem

Given an array arr[] of size n, partition its elements into:

One strictly increasing subsequence

One strictly decreasing subsequence

Each element can belong to at most one subsequence, and some elements
may remain unused.

The goal is to find the minimum number of elements that cannot be
included in either subsequence.

Example 1

Input

arr[] = [7, 8, 1, 2, 4, 6, 3, 5, 2, 1, 8, 7]

Output

2

Explanation

One possible partition is:

Increasing subsequence: 1 2 4 5 8
Decreasing subsequence: 7 6 3 2 1

So, 10 elements are selected and 2 elements remain unused.

Therefore:

Answer = 12 - 10 = 2

Example 2

Input

arr[] = [1, 4, 2, 3, 3, 2, 4]

Output

0

Explanation

We can use:

Increasing subsequence: 1 2 3 4
Decreasing subsequence: 4 3 2

All elements are included, so the number of unused elements is 0.

Approach

Instead of directly minimizing the unused elements, we maximize the
number of elements that can be selected.

We maintain a DP table:

dp[inc][dec]

where:

inc = last value selected in the increasing subsequence.

dec = last value selected in the decreasing subsequence.

dp[inc][dec] = maximum number of elements selected so far.

For every element x, there are three choices:

1. Skip x

We simply keep the current state unchanged.

2. Put x in the increasing subsequence

This is possible when:

x > inc

or when the increasing subsequence is empty.

Then:

dp[x][dec] = max(dp[x][dec], dp[inc][dec] + 1)

3. Put x in the decreasing subsequence

This is possible when:

x < dec

or when the decreasing subsequence is empty.

Then:

dp[inc][x] = max(dp[inc][x], dp[inc][dec] + 1)

Finally, let maximumSelected be the maximum number of elements
included in the two subsequences.

The answer is:

n - maximumSelected

C++ Solution

class Solution {
public:
    int minCount(vector<int>& arr) {
        int n = arr.size();

        // dp[inc][dec] = maximum elements selected so far
        // inc = last element of increasing subsequence
        // dec = last element of decreasing subsequence

        vector<vector<int>> dp(101, vector<int>(101, -1));

        dp[0][0] = 0;

        for (int x : arr) {
            vector<vector<int>> ndp = dp;

            for (int inc = 0; inc <= 100; inc++) {
                for (int dec = 0; dec <= 100; dec++) {

                    if (dp[inc][dec] == -1)
                        continue;

                    // Put x in increasing subsequence
                    if (inc == 0 || x > inc) {
                        ndp[x][dec] = max(
                            ndp[x][dec],
                            dp[inc][dec] + 1
                        );
                    }

                    // Put x in decreasing subsequence
                    if (dec == 0 || x < dec) {
                        ndp[inc][x] = max(
                            ndp[inc][x],
                            dp[inc][dec] + 1
                        );
                    }
                }
            }

            dp = ndp;
        }

        int maximumSelected = 0;

        for (int inc = 0; inc <= 100; inc++) {
            for (int dec = 0; dec <= 100; dec++) {
                maximumSelected = max(
                    maximumSelected,
                    dp[inc][dec]
                );
            }
        }

        return n - maximumSelected;
    }
};

Dry Run

For:

arr = [1, 4, 2, 3, 3, 2, 4]

We can form:

Increasing: 1 → 2 → 3 → 4
Decreasing: 4 → 3 → 2

Hence all 7 elements are selected.

maximumSelected = 7
n = 7

answer = n - maximumSelected
       = 7 - 7
       = 0

Why This Works

At every position, the DP considers all valid possibilities for
assigning the current element:

skip it,

add it to the increasing subsequence,

add it to the decreasing subsequence.

Because the DP stores the best number of selected elements for every
pair of last values, no valid combination is missed.

Therefore, maximizing selected elements gives the minimum number of
unused elements.

Complexity

Since:

arr[i] <= 100

there are only 101 × 101 possible (inc, dec) states.

For each of the n elements:

Time Complexity: O(n × 101²)
Space Complexity: O(101²)

With n <= 100, this easily fits within the constraints.

Key Takeaway

Minimize unused elements = Maximize selected elements.

Use DP with the last element of the increasing and decreasing
subsequences as the state.

Important Conditions

x > inc   // strictly increasing
x < dec   // strictly decreasing

Equal values cannot be placed consecutively in either subsequence
because both subsequences must be strict.
