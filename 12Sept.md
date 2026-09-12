
📌 GFG POTD — Max Product Subsequence of Size K

🧩 Problem

Given an array arr[] of integers and an integer k, find a subsequence of size exactly k whose product is maximum among all possible subsequences.

Return the maximum product that can be obtained.

Example 1

Input:
arr[] = [1, 2, 0, 3], k = 2

Output:
6

Explanation:
The subsequence {2, 3} gives the maximum product:
2 × 3 = 6

Example 2

Input:
arr[] = [1, 2, -1, -3, -6, 4], k = 4

Output:
144

Explanation:
The subsequence {2, -3, -6, 4} gives:
2 × (-3) × (-6) × 4 = 144

💡 Approach — Dynamic Programming

The main difficulty is the presence of negative numbers.

For a product:

A positive product can become negative after multiplying by a negative number.

A negative product can become positive after multiplying by a negative number.

Therefore, keeping only the maximum product is not enough.

We maintain two DP tables:

mx[i][j] → maximum product obtainable by choosing j elements from the first i elements.

mn[i][j] → minimum product obtainable by choosing j elements from the first i elements.

DP Transitions

For every element x = arr[i-1]:

1. Don't take the current element

mx[i][j] = mx[i-1][j]
mn[i][j] = mn[i-1][j]

2. Take the current element

If a valid product of j-1 elements exists:

value = previous_product × x

Update both maximum and minimum:

mx[i][j] = max(mx[i][j], value)
mn[i][j] = min(mn[i][j], value)

We consider both mx[i-1][j-1] and mn[i-1][j-1] because multiplying a negative number can turn the minimum product into the maximum product.

🔑 Base Case

Choosing 0 elements gives product 1:

mx[0][0] = mn[0][0] = 1;

All other states are initially invalid.

🧠 Why Do We Need Both Maximum and Minimum?

Consider:

Current product = -10
Current element = -5

Then:

(-10) × (-5) = 50

So a minimum negative product can produce the final maximum answer.

That is why the DP stores both:

maximum product
minimum product

💻 C++ Solution

class Solution {
public:
    long long maxProduct(vector<int>& arr, int k) {
        int n = arr.size();

        const long long INF = 4e18;

        vector<vector<long long>> mx(
            n + 1, vector<long long>(k + 1, -INF)
        );

        vector<vector<long long>> mn(
            n + 1, vector<long long>(k + 1, INF)
        );

        mx[0][0] = mn[0][0] = 1;

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= min(i, k); j++) {

                // Don't take current element
                mx[i][j] = mx[i - 1][j];
                mn[i][j] = mn[i - 1][j];

                // Take current element
                if (j > 0) {
                    long long x = arr[i - 1];

                    // From maximum product
                    if (mx[i - 1][j - 1] != -INF) {
                        long long val = mx[i - 1][j - 1] * x;

                        mx[i][j] = max(mx[i][j], val);
                        mn[i][j] = min(mn[i][j], val);
                    }

                    // From minimum product
                    if (mn[i - 1][j - 1] != INF) {
                        long long val = mn[i - 1][j - 1] * x;

                        mx[i][j] = max(mx[i][j], val);
                        mn[i][j] = min(mn[i][j], val);
                    }
                }
            }
        }

        return mx[n][k];
    }
};

📊 Complexity Analysis

Let:

n = arr.size()

k = required subsequence size

Time Complexity

O(n × k)

Each DP state is processed once.

Space Complexity

O(n × k)

Two DP tables mx and mn are maintained.

🚀 Key Takeaways

This is a 2D Dynamic Programming problem.

Since the array contains negative numbers, tracking only the maximum is incorrect.

Maintain both:

Maximum product

Minimum product

For every element, there are two choices:

Take it

Skip it

The subsequence must contain exactly k elements.

The DP handles positive, negative, and zero values.

🏷️ Tags

Dynamic Programming Subsequence Array Maximum Product Minimum Product GFG POTD C++

📅 GFG POTD

Problem: Max Product Subsequence of Size K
Difficulty: Medium
Language: C++
Approach: Dynamic Programming
