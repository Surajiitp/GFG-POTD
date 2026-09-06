Sum of Pairwise ANDs

Problem Statement

Given an array "arr[]" of integers, calculate the sum of the bitwise AND of all pairs of elements such that the first index is less than the second index.

In other words, find:

(arr[0] & arr[1]) +
(arr[0] & arr[2]) +
...
(arr[n-2] & arr[n-1])

where "i < j".

---

Examples

Example 1

Input:

arr = [5, 10, 15]

Output:

15

Explanation:

5 & 10  = 0
5 & 15  = 5
10 & 15 = 10

Therefore:

0 + 5 + 10 = 15

---

Example 2

Input:

arr = [10, 20, 30, 40]

Output:

46

Explanation:

10 & 20 = 0
10 & 30 = 10
10 & 40 = 8
20 & 30 = 20
20 & 40 = 0
30 & 40 = 8

Therefore:

0 + 10 + 8 + 20 + 0 + 8 = 46

---

Approach

A direct approach would check every pair, resulting in "O(n²)" complexity, which is too slow for "n = 10⁵".

Instead, we process the numbers bit by bit.

Key Observation

For a particular bit:

- If "cnt" numbers have that bit set,
- Then that bit will be present in the AND of every pair among those "cnt" numbers.
- Number of such pairs is:

cnt * (cnt - 1) / 2

- The value contributed by this bit is:

(cnt * (cnt - 1) / 2) * (2^bit)

We repeat this for all 31 bits.

---

Example

Consider:

arr = [5, 10, 15]

Binary representation:

5  = 0101
10 = 1010
15 = 1111

For each bit, count how many numbers contain that bit.

For example, for the "1"'s bit:

5  -> 1
10 -> 0
15 -> 1

There are "2" numbers with this bit set.

Number of pairs:

2 * 1 / 2 = 1

Contribution:

1 * 1 = 1

Doing this for every bit gives the final answer "15".

---

C++ Solution

class Solution {
public:
    long long pairAndSum(vector<int>& arr) {
        long long ans = 0;
        int n = arr.size();

        // Check every bit
        for (int bit = 0; bit < 31; bit++) {
            long long cnt = 0;

            // Count numbers having this bit set
            for (int x : arr) {
                if (x & (1 << bit)) {
                    cnt++;
                }
            }

            // Number of pairs having this bit set
            long long pairs = cnt * (cnt - 1) / 2;

            // Add contribution of this bit
            ans += pairs * (1LL << bit);
        }

        return ans;
    }
};

---

Complexity Analysis

Let "n" be the size of the array.

There are at most "31" relevant bits because:

arr[i] <= 10^8

For every bit, we scan the entire array.

Time Complexity

O(31 × n)

Since "31" is constant:

O(n)

Space Complexity

O(1)

---

Why This Works

The bitwise AND of two numbers contains a particular bit only when both numbers have that bit set.

So instead of calculating AND for every pair individually, we count how many pairs contribute to each bit and add their total contribution.

This changes the solution from:

O(n²)

to:

O(31n) ≈ O(n)

making it efficient for large arrays.

---

Constraints

1 ≤ arr.size() ≤ 10^5
1 ≤ arr[i] ≤ 10^8

---

Tags

"Bit Manipulation" "Bitwise AND" "Counting" "Arrays" "Optimization"
