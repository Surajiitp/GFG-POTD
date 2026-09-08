Max Digit Sum Number in 1 to N

Problem Statement

Given a number n, find a number in the range from 1 to n such that
its digit sum is maximum.

If multiple numbers have the same maximum digit sum, return the
largest number among them.

Examples

Example 1

Input: n = 48
Output: 48

Explanation:

Digit sum of 48 = 4 + 8 = 12

Digit sum of 39 = 3 + 9 = 12

Both have the maximum digit sum, but 48 > 39, so the answer is 48.

Example 2

Input: n = 90
Output: 89

Explanation:

Digit sum of 90 = 9

Digit sum of 89 = 17

Therefore, 89 has the maximum digit sum.

Approach

The important observation is that for a number n, a strong candidate
for the maximum digit sum can be obtained by:

Considering n itself.

Decreasing one non-zero digit of n by 1.

Replacing every digit after that position with 9.

Calculating the digit sum of each candidate.

Keeping the number with the maximum digit sum.

If digit sums are equal, keeping the larger number.

Why does this work?

Suppose we decrease a digit by 1. This loses only 1 from that digit,
while changing all digits to its right to 9.

For example:

n = 5234

Decrease 5:
4234 -> 4999

Decrease 2:
5134 -> 5199

Decrease 3:
5224 -> 5229

Decrease 4:
5233

The optimal number must be either n itself or one of these candidates.

Algorithm

1. Convert n into a string.
2. Calculate the digit sum of n.
3. Set answer = n.
4. For every digit:
      a. Skip it if it is 0.
      b. Decrease the digit by 1.
      c. Set all digits after it to 9.
      d. Convert the candidate to an integer.
      e. Calculate its digit sum.
      f. Update the answer if:
           - its digit sum is larger, or
           - its digit sum is equal and the number is larger.
5. Return answer.

C++ Solution

class Solution {
public:
    int findMax(int n) {
        string s = to_string(n);

        int ans = n;
        int maxSum = 0;

        // Calculate digit sum of n
        for (char c : s) {
            maxSum += c - '0';
        }

        // Try decreasing each digit by 1
        // and making all digits after it 9
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '0')
                continue;

            string t = s;

            // Decrease current digit
            t[i]--;

            // Make all following digits 9
            for (int j = i + 1; j < t.size(); j++) {
                t[j] = '9';
            }

            int num = stoi(t);

            // Calculate digit sum
            int sum = 0;
            for (char c : t) {
                sum += c - '0';
            }

            // Update answer
            if (sum > maxSum || (sum == maxSum && num > ans)) {
                maxSum = sum;
                ans = num;
            }
        }

        return ans;
    }
};

Complexity Analysis

Let d be the number of digits in n.

Time Complexity: O(d²)

Space Complexity: O(d)

Since n <= 10^9, the number of digits is very small, so this approach
is easily fast enough.

Important GFG Function Name

For GeeksforGeeks, the required function name is:

int findMax(int n)

Do not rename it to maxDigitSum, because the GFG driver calls
findMax(n).

Constraints

1 ≤ n ≤ 10^9

Key Takeaway

To maximize digit sum while staying within n, try numbers formed by
decreasing one digit and replacing all following digits with 9,
along with n itself.
