# Maximum Frequency with K Increments

## Problem

Given an integer array `arr[]`. In one operation, you can choose an index and increase the value at that index by `1`.

You are given an integer `k`, representing the maximum number of operations.

Find the **maximum possible frequency of any element** after performing at most `k` increment operations.

### Examples

#### Example 1

```text
Input:  arr[] = [2, 2, 4], k = 4
Output: 3

Explanation: Apply two increments to each of the first two elements to make the array [4, 4, 4]. Therefore, the maximum frequency is 3.

Example 2
Input:  arr[] = [7, 7, 7, 7], k = 5
Output: 4

Explanation: The frequency of 7 is already 4, so no operations are needed.

Approach
Sorting + Sliding Window
Sort the array.
Maintain a sliding window [left, right].
Consider arr[right] as the target value.
Calculate the number of operations required to make every element in the current window equal to arr[right].

The required operations are:

(window size × arr[right]) - current_sum

where:

window size = right - left + 1
If the required operations exceed k, move left forward.
Keep track of the largest valid window.

Since the array is sorted, arr[right] is the largest element in the current window, so all other elements can be increased to it.

C++ Solution
class Solution {
public:
    int maxFrequency(vector<int>& arr, int k) {
        sort(arr.begin(), arr.end());

        long long left = 0;
        long long current_sum = 0;
        int max_freq = 0;

        for (int right = 0; right < arr.size(); ++right) {
            current_sum += arr[right];

            while ((long long)(right - left + 1) * arr[right]
                   - current_sum > k) {
                current_sum -= arr[left];
                left++;
            }

            max_freq = max(max_freq,
                           (int)(right - left + 1));
        }

        return max_freq;
    }
};
Dry Run

For:

arr = [2, 2, 4]
k = 4

The array is already sorted.

Window:

[2, 2, 4]

Current sum:

2 + 2 + 4 = 8

Required operations:

3 × 4 - 8
= 12 - 8
= 4

Since:

4 <= k

we can make all elements equal to 4.

[2, 2, 4] → [4, 4, 4]

Therefore:

Maximum Frequency = 3
Complexity
Sorting: O(n log n)
Sliding Window: O(n)
Total Time: O(n log n)
Space: O(1) extra space
Key Concept

Sorting + Sliding Window + Running Sum

The main formula is:

Operations Required =
(window size × target) - sum

If the required operations are greater than k, shrink the window.

while (required_operations > k)
    left++;

This allows us to find the maximum number of elements that can be made equal using at most k increments.
