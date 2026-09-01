# Count Palindromic Strings with Constraints

## Problem Statement

Given two integers `n` and `k`, consider an alphabet consisting of the first `k` lowercase English letters.

Find the number of palindromic strings whose length is less than or equal to `n`, such that:

- Every character belongs to the given alphabet.
- No character appears more than twice in the string.
- Return the answer modulo `10^9 + 7`.

### Example 1

```text
Input:
n = 3
k = 2

Output:
6

Possible strings:

"a"
"b"
"aa"
"bb"
"aba"
"bab"
Example 2
Input:
n = 4
k = 3

Output:
18

Possible strings:

"a", "b", "c"
"aa", "bb", "cc"
"aba", "aca", "bab", "bcb", "cac", "cbc"
"abba", "acca", "baab", "bccb", "caac", "cbbc"
Approach

The main observation is that a palindrome is completely determined by its first half and, for an odd-length palindrome, its middle character.

Since no character can appear more than twice, every character used in the first half must be distinct.

Let:

P(k, i) = k × (k - 1) × ... × (k - i + 1)

This is the number of ways to choose and arrange i distinct characters from k characters.

Even Length

For a palindrome of length:

2 * i

the first i characters determine the entire palindrome.

Example:

abc | cba

Number of possibilities:

P(k, i)
Odd Length

For a palindrome of length:

2 * i - 1

we have i - 1 mirrored pairs and one center character.

Example:

abcba

The number of possibilities is also:

P(k, i)

Therefore:

Length 2i - 1 → P(k, i)
Length 2i     → P(k, i)

So for every i, the same permutation value can be added for both lengths.

Algorithm

Initialize:

ans = 0;
ways = 1;

Iterate over i while:

2 * i <= n

Update:

ways = ways * (k - i + 1) % MOD;

Now ways represents:

P(k, i)
Add it twice:
once for length 2i - 1
once for length 2i
If n is odd, calculate the additional odd-length case and add it once.
Return the answer modulo 10^9 + 7.
C++ Solution
class Solution {
public:
    static const long long MOD = 1000000007;

    int palindromicStrings(int n, int k) {

        long long ans = 0;
        long long ways = 1;

        // P(k, i)
        for (int i = 1; 2 * i <= n; i++) {

            ways = ways * (k - i + 1) % MOD;

            // ways represents:
            // Length 2*i - 1
            // Length 2*i
            ans = (ans + 2 * ways) % MOD;
        }

        // Extra odd length when n is odd
        if (n % 2 == 1) {

            int i = n / 2 + 1;

            ways = ways * (k - i + 1) % MOD;

            ans = (ans + ways) % MOD;
        }

        return ans;
    }
};
Dry Run
Example 1
n = 3
k = 2

For:

i = 1

We get:

P(2, 1) = 2

This represents:

Length 1 → 2
Length 2 → 2

So:

ans = 4

Since n is odd, we need one more length:

P(2, 2) = 2 × 1 = 2

Therefore:

ans = 4 + 2
    = 6

Output:

6
Example 2
n = 4
k = 3

For:

i = 1
P(3, 1) = 3

Lengths:

Length 1 → 3
Length 2 → 3

Contribution:

6

For:

i = 2
P(3, 2) = 3 × 2 = 6

Lengths:

Length 3 → 6
Length 4 → 6

Contribution:

12

Total:

6 + 12 = 18

Output:

18
Why Does the Formula Work?

Consider a palindrome:

a b c d c b a

The first half is:

a b c

Once these characters are selected and arranged, the second half is fixed by symmetry:

a b c | d | c b a

Because each character can appear at most twice, the characters in the first half must be distinct.

For an odd-length palindrome, the center character is also distinct from the characters in the first half.

Therefore, the number of choices is represented by the permutation:

P(k, i)

This gives the same count for the corresponding odd and even lengths.

Complexity Analysis

Let n be the maximum allowed length.

Time Complexity
O(n)

We iterate through the possible half-lengths only once.

Space Complexity
O(1)

Only a few variables are used.

Key Concepts
Palindrome
Permutations
Combinatorics
Mathematical Counting
Modular Arithmetic
Constant Space Optimization
Constraints
1 <= k <= 26
1 <= n <= 52
n <= 2 * k
Important Formula

The number of ways to arrange i distinct characters from k characters is:

P(k, i) = k × (k - 1) × ... × (k - i + 1)

For every i:

Length 2i - 1 → P(k, i)
Length 2i     → P(k, i)

If n is odd, one additional odd-length term is added.

Final Complexity
Time:  O(n)
Space: O(1)
Tags

Math Combinatorics Permutation Palindrome Modular Arithmetic
