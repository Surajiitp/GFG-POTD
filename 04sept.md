# 🐦 Bird and Max Fruit Gathering

## Problem Statement

Given an array `arr[]` representing the fruit values of trees arranged in a **circle** and an integer `m`, find the maximum total fruits the bird can collect by visiting **at most `m` trees**.

The bird:

- Can start from any tree.
- Can move only to a neighboring tree.
- The first and last trees are also neighbors.
- Collects the fruit value of every tree it visits.

---

## Approach

Since the trees are arranged in a circle, the bird can visit a **continuous segment** of at most `m` trees.

So, the problem becomes finding the maximum sum of a subarray of length `m` in a circular array.

### Steps

1. If `m == n`, the bird can visit every tree, so return the total sum.
2. Calculate the sum of the first `m` elements.
3. Use a **sliding window** to move the window one position at a time.
4. When the window reaches the end of the array, use modulo `% n` to wrap around to the beginning.
5. Keep track of the maximum window sum.

For example:

```text
arr = [7, 2, 1, 3, 4]
m = 2

Possible circular windows include:

7 + 2 = 9
2 + 1 = 3
1 + 3 = 4
3 + 4 = 7
4 + 7 = 11

Maximum = 11

C++ Solution
class Solution {
public:
    long long maxFruits(vector<int>& arr, int m) {
        int n = arr.size();

        long long windowSum = 0;

        // First window
        for (int i = 0; i < m; i++) {
            windowSum += arr[i];
        }

        long long ans = windowSum;

        // Sliding window over the circular array
        for (int i = m; i < n + m - 1; i++) {
            windowSum -= arr[(i - m) % n];
            windowSum += arr[i % n];

            ans = max(ans, windowSum);
        }

        return ans;
    }
};
Example 1
Input
arr = [2, 1, 3, 5, 0, 1, 4]
m = 3

The best segment is:

1 + 3 + 5 = 9
Output
9
Example 2
Input
arr = [1, 6, 2, 5, 3, 4]
m = 2

Best segments:

6 + 2 = 8
5 + 3 = 8
Output
8
Example 3
Input
arr = [7, 2, 1, 3, 4]
m = 2

Because the array is circular:

4 + 7 = 11
Output
11
Complexity Analysis

Let n = arr.size().

Time Complexity
O(n)

We traverse the array only once using a sliding window.

Space Complexity
O(1)

No extra array is required.

Key Points
Trees form a circular array.
The bird always visits consecutive trees.
Use a sliding window of size m.
Use % n to handle circular movement.
Use long long for the sum because n and arr[i] can be large.
Tags

#Array #SlidingWindow #CircularArray #TwoPointers #POTD #GFG
