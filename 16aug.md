# Minimum Product of a Non-Empty Subset

## 📝 Problem Statement

Given an integer array arr[], find the minimum possible product that can be obtained by multiplying the elements of any non-empty subset of the array.

Examples:

Input: arr[] = [1, 2, 3]
Output: 1
Explanation: The possible subset products are 1, 2, 3, 2, 3, 6, and 6. The minimum product is 1, obtained by selecting the subset [1].
Input: arr[] = [4, -2, 5]
Output: -40
Explanation: The minimum product is -40, obtained by selecting the subset [4, -2, 5].

Constraints:

1 ≤ arr.size() ≤ 10
-10 ≤ arr[i] ≤ 10


class Solution {
public:
    int minProd(vector<int>& arr) {
        int n = arr.size();

        if (n == 1)
            return arr[0];

        int negCount = 0;
        int zeroCount = 0;

        int maxNeg = INT_MIN;
        int minPos = INT_MAX;

        int prod = 1;

        for (int x : arr) {

            if (x == 0) {
                zeroCount++;
                continue;
            }

            if (x < 0) {
                negCount++;
                maxNeg = max(maxNeg, x);
            }
            else {
                minPos = min(minPos, x);
            }

            prod *= x;
        }

        // All elements are zero
        if (zeroCount == n)
            return 0;

        // No negative elements
        if (negCount == 0) {
            if (zeroCount > 0)
                return 0;

            return minPos;
        }

        // Odd number of negative elements
        if (negCount % 2 == 1)
            return prod;

        // Even number of negative elements
        // If zero exists, choosing [0] gives minimum product
        if (zeroCount > 0)
            return 0;

        // Remove the negative number closest to zero
        prod /= maxNeg;

        return prod;
    }
};
