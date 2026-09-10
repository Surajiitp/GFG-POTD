Count Pairs with Given GCD and LCM

📝 Problem Statement

Given two integers x and y representing the GCD and LCM of
two unknown positive integers a and b, count the number of valid
ordered pairs (a, b) satisfying:

GCD(a, b) = x
LCM(a, b) = y

The pairs (a, b) and (b, a) are counted as distinct when a != b.

💡 Examples

Example 1

Input:

x = 2, y = 12

Output:

4

Explanation:

The valid pairs are:

(2, 12)
(4, 6)
(6, 4)
(12, 2)

All these pairs have GCD 2 and LCM 12.

Example 2

Input:

x = 6, y = 4

Output:

0

Explanation:

LCM must always be a multiple of GCD. Since:

4 % 6 != 0

no valid pair exists.

🧠 Key Observation

For two positive integers a and b:

a × b = GCD(a, b) × LCM(a, b)

Given:

GCD(a, b) = x
LCM(a, b) = y

Let:

a = x × p
b = x × q

Then p and q must be coprime:

gcd(p, q) = 1

Also:

p × q = y / x

So the problem becomes:

Count the factor pairs (p, q) of y / x whose GCD is 1.

🚀 Approach

If y % x != 0, return 0.

Calculate:

n = y / x

Iterate over all divisors a of n up to sqrt(n).

For every divisor:

b = n / a

If:

gcd(a, b) == 1

then the pair is valid.

If a == b, add 1; otherwise add 2, because (a,b) and (b,a)
are distinct.

💻 C++ Solution

class Solution {
public:
    int pairCount(int x, int y) {

        if (y % x != 0)
            return 0;

        int n = y / x;
        int ans = 0;

        for (int a = 1; a * a <= n; a++) {

            if (n % a == 0) {
                int b = n / a;

                if (__gcd(a, b) == 1) {

                    if (a == b)
                        ans++;
                    else
                        ans += 2;
                }
            }
        }

        return ans;
    }
};

🔍 Dry Run

For:

x = 2
y = 12

First:

n = y / x
  = 12 / 2
  = 6

Factor pairs of 6:

(1, 6)
(2, 3)
(3, 2)
(6, 1)

All have GCD 1, so all are valid.

Corresponding original pairs:

(2 × 1, 2 × 6) = (2, 12)
(2 × 2, 2 × 3) = (4, 6)
(2 × 3, 2 × 2) = (6, 4)
(2 × 6, 2 × 1) = (12, 2)

Therefore:

Answer = 4

⏱️ Complexity Analysis

Let:

n = y / x

Time Complexity

O(√n × log n)

We check divisors up to √n, and gcd takes logarithmic time.

Space Complexity

O(1)

Only constant extra space is used.

📌 Important Concepts

GCD

LCM

Number Theory

Divisors

Factor Pairs

Coprime Numbers

Euclidean Algorithm

Ordered Pairs

⭐ Key Takeaway

The main trick is to divide both numbers by their common GCD.

Instead of directly finding (a, b), calculate:

n = y / x

and find coprime factor pairs:

p × q = n
gcd(p, q) = 1

This gives an efficient O(√n log n) solution.
