Count Subsequences Divisible by n

📝 Problem Statement

Given a numeric string s containing only digits and an integer n, count the non-empty subsequences of s whose numeric value is divisible by n.

Since the answer can be very large, return it modulo:

1e9 + 7

Example 1

Input:
s = "1234"
n = 4

Output:
4

The subsequences divisible by 4 are:

4, 12, 24, 124

Example 2

Input:
s = "330"
n = 6

Output:
4

The divisible subsequences are:

30, 30, 330, 0

💡 Approach

The main challenge is that a string of length up to 10^6 can have up to 2^n subsequences, so we cannot generate them explicitly.

Instead, we use Dynamic Programming based on remainders.

DP Definition

Let:

dp[r] = number of subsequences formed so far
        whose value has remainder r when divided by n

We only need the remainder, not the complete numeric value.

Processing a Digit

Suppose the current digit is d.

For every existing remainder r:

newRemainder = (r * 10 + d) % n

Every subsequence represented by dp[r] can be extended by the current digit.

So:

next[newRemainder] += dp[r]

We can also start a new subsequence using only the current digit:

next[d % n] += 1

We use a separate next array so that the current digit is used exactly once in each transition.

At the end:

dp[0]

contains the number of non-empty subsequences divisible by n.

🔍 Example Walkthrough

Consider:

s = "1234"
n = 4

For every subsequence, we track only its remainder modulo 4.

When a digit is appended:

remainder = (oldRemainder * 10 + digit) % 4

After processing all digits, the subsequences having remainder 0 are:

4
12
24
124

Therefore:

Answer = 4

💻 C++ Solution

class Solution {
public:
    int countSubsequences(string& s, int n) {
        const int MOD = 1e9 + 7;

        // dp[r] = number of subsequences with remainder r
        vector<long long> dp(n, 0);

        for (char c : s) {
            int digit = c - '0';

            vector<long long> next = dp;

            // Start a new subsequence with the current digit
            int rem = digit % n;
            next[rem] = (next[rem] + 1) % MOD;

            // Append current digit to all existing subsequences
            for (int r = 0; r < n; r++) {
                if (dp[r] == 0)
                    continue;

                int newRem = (r * 10LL + digit) % n;

                next[newRem] =
                    (next[newRem] + dp[r]) % MOD;
            }

            dp.swap(next);
        }

        return dp[0];
    }
};

⏱️ Complexity Analysis

For every digit, we iterate over all n possible remainders.

Time Complexity

O(|s| * n)

Given the constraint:

|s| * n <= 10^6

this is efficient enough.

Space Complexity

O(n)

We maintain two arrays of size n.

🎯 Key Insight

We don't need to store the actual value of a subsequence.

For divisibility by n, only its remainder modulo n matters.

When a digit d is appended:

new remainder = (old remainder × 10 + d) % n

This allows us to count an exponential number of subsequences in polynomial time.

⚠️ Important Detail

The empty subsequence is not counted.

That is why we only create new subsequences from the current digit:

next[digit % n]++;

and never initialize:

dp[0] = 1;

because that would represent the empty subsequence.

📌 Pattern Used

This problem is a classic example of:

Dynamic Programming

Subsequence DP

Modular Arithmetic

Remainder State Compression

State Transition

dp[r]
   ↓ append digit d
(r × 10 + d) % n

🔗 Problem Details

Problem: Count Subsequences Divisible by n
Difficulty: Medium
Expected Time: O(|s| × n)
Expected Space: O(n)
