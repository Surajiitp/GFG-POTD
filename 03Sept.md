 Max Adjacent Diffs Sum with 1 Replacements

## Problem Statement

Given an integer array `arr[]`, you are allowed to replace any number of elements with `1`.

After making any number of modifications, find the maximum possible sum of absolute differences between consecutive elements.

For an array:

`arr = [a1, a2, a3, ..., an]`

the required sum is:

`|a2-a1| + |a3-a2| + ... + |an-a(n-1)|`

---

## Examples

### Example 1

**Input:**

```text
arr = [3, 2, 1, 4, 5]

Output:

8

Explanation:

Modify the array as:

[3, 1, 1, 4, 1]

Then:

|1 - 3| + |1 - 1| + |4 - 1| + |1 - 4|
= 2 + 0 + 3 + 3
= 8

Therefore, the maximum sum is 8.

Example 2

Input:

arr = [1, 5]

Output:

4

No modification is needed:

|5 - 1| = 4
Approach

This problem can be solved using Dynamic Programming with two states.

For every element, we have two choices:

Keep the element unchanged.
Replace the element with 1.

We only need to remember the best answer for the previous element.

So we maintain two variables:

keep

keep represents the maximum sum when the previous element is kept unchanged.

replace

replace represents the maximum sum when the previous element is replaced with 1.

DP Transition

For the current element arr[i]:

Case 1: Keep arr[i]

The previous element can either be:

Original arr[i-1]
Replaced with 1

Therefore:

newKeep = max(
    keep + |arr[i] - arr[i-1]|,
    replace + |arr[i] - 1|
)
Case 2: Replace arr[i] with 1

Again, the previous element can either be original or replaced:

newReplace = max(
    keep + |arr[i-1] - 1|,
    replace + |1 - 1|
)

Since:

|1 - 1| = 0

we get:

newReplace = max(
    keep + |arr[i-1] - 1|,
    replace
)

After calculating the new states, update:

keep = newKeep
replace = newReplace
C++ Solution
class Solution {
public:
    long long maxDiffSum(vector<int>& arr) {
        int n = arr.size();

        if (n <= 1)
            return 0;

        long long keep = 0;
        long long replace = 0;

        for (int i = 1; i < n; i++) {

            long long newKeep = max(
                keep + abs(arr[i] - arr[i - 1]),
                replace + abs(arr[i] - 1)
            );

            long long newReplace = max(
                keep + abs(arr[i - 1] - 1),
                replace
            );

            keep = newKeep;
            replace = newReplace;
        }

        return max(keep, replace);
    }
};
Dry Run

Consider:

arr = [3, 2, 1, 4, 5]

At each position, we consider both possibilities:

Keep the current element
        OR
Replace the current element with 1

One optimal modified array is:

[3, 1, 1, 4, 1]

Its sum is:

|1 - 3| = 2
|1 - 1| = 0
|4 - 1| = 3
|1 - 4| = 3

Total:

2 + 0 + 3 + 3 = 8
Why Two States Are Enough?

The contribution of the next difference depends only on the value chosen for the previous element.

The previous element has only two possible values:

Original value
      OR
1

Therefore, we don't need to store the entire DP array.

We only keep:

keep
replace

This reduces the space complexity to O(1).

Complexity Analysis

Let n be the size of the array.

Time Complexity
O(n)

We process every element exactly once.

Space Complexity
O(1)

Only two DP variables are used.

Key Takeaway

This is a two-state Dynamic Programming problem.

For every element:

        arr[i]
       /      \
    Keep     Replace
              with 1

By maintaining the best answer for both choices, we get:

Time  : O(n)
Space : O(1)

This is an optimized solution suitable for:

n <= 10^5
