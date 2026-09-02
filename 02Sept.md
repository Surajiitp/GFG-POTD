# Unoccupied Computers

## Problem Statement

A cafe has `n` computers. The customer events are represented by a string `s` of uppercase English letters.

Each distinct letter appears exactly twice:

- The first occurrence represents the customer's **arrival**.
- The second occurrence represents the customer's **departure**.

A customer is assigned a computer only if one is available at the time of arrival. Otherwise, the customer is rejected.

Return the number of customers who could not be assigned a computer.

---

## Example 1

### Input

```text
n = 3
s = "GACCBDDBAGEE"
Output
1
Explanation

There are 3 computers.

Customers are assigned computers whenever one is available. Customer D arrives when all computers are occupied, so D cannot get a computer.

Therefore, the answer is 1.

Example 2
Input
n = 1
s = "ABCBAC"
Output
2
Explanation

There is only 1 computer.

A arrives and gets the computer.
B arrives but no computer is available → rejected.
C arrives but no computer is available → rejected.
B leaves, but B never had a computer.
A leaves and the computer becomes free.
C leaves, but C never had a computer.

So, B and C could not get computers.

Answer = 2.

Approach

We simulate all customer events from left to right.

We maintain:

free → number of available computers.
seen[26] → whether the customer has appeared before.
hasComputer[26] → whether the customer actually received a computer.
ans → number of customers who were rejected.
Arrival

For the first occurrence of a character:

If a computer is available:
Assign the computer.
Decrease free.
Mark that customer as having a computer.
Otherwise:
Reject the customer.
Increase ans.
Departure

For the second occurrence:

If the customer had a computer:
Release the computer.
Increase free.
If the customer was rejected:
Do nothing.

The important point is that a rejected customer never occupied a computer, so their departure should not increase the number of free computers.

C++ Solution
class Solution {
public:
    int solve(int n, string s) {
        int free = n;
        int ans = 0;

        vector<bool> hasComputer(26, false);
        vector<bool> seen(26, false);

        for (char c : s) {
            int x = c - 'A';

            // Arrival
            if (!seen[x]) {
                seen[x] = true;

                if (free > 0) {
                    free--;
                    hasComputer[x] = true;
                }
                else {
                    ans++;
                }
            }

            // Departure
            else {
                if (hasComputer[x]) {
                    free++;
                    hasComputer[x] = false;
                }
            }
        }

        return ans;
    }
};
Complexity Analysis

Let |s| be the length of the string.

Time Complexity: O(|s|)
Space Complexity: O(26) = O(1)
Key Takeaway

The main idea is to distinguish between customers who got a computer and customers who were rejected.

Arrival + Computer Available → Occupy Computer
Arrival + No Computer        → Rejected
Departure + Had Computer     → Free Computer
Departure + Was Rejected     → Do Nothing

This simple simulation gives the correct answer efficiently.
