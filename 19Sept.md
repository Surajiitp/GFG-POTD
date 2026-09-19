# Minimum Cost to Make Two Strings Identical

## Problem Statement

Given two strings `s1` and `s2`, and two integers `costS1` and `costS2`:

* `costS1` = cost of deleting one character from `s1`
* `costS2` = cost of deleting one character from `s2`

You can delete any number of characters from either string.

The order of the remaining characters must be preserved.

Find the **minimum cost required to make `s1` and `s2` identical**.

---

## Approach

The key observation is that the characters which remain in both strings must form a **Common Subsequence**.

We use **Dynamic Programming** to find the maximum saving we can get by keeping common characters.

### Why does this work?

Initially, if we delete every character:

```text
Cost = n × costS1 + m × costS2
```

where:

* `n = s1.length()`
* `m = s2.length()`

Suppose a character occurs in both strings and we keep it.

Normally, we would delete:

```text
costS1 + costS2
```

for those two characters.

By keeping them, we save exactly:

```text
costS1 + costS2
```

Therefore, we need to find the **Longest Common Subsequence (LCS)**.

The final answer is:

```text
Total Cost - Maximum Saving
```

---

## Dynamic Programming

Let:

```text
dp[j]
```

represent the maximum saving possible for the current prefix of `s1` and the first `j` characters of `s2`.

For every pair of characters:

### If characters are equal

```cpp
if (s1[i - 1] == s2[j - 1])
```

We keep this character in the common subsequence:

```cpp
dp[j] = prev + costS1 + costS2;
```

### If characters are different

We skip one of the characters:

```cpp
dp[j] = max(dp[j], dp[j - 1]);
```

Here:

* `dp[j]` before update represents the previous row.
* `dp[j - 1]` represents the current row.
* `prev` stores the diagonal value `dp[i-1][j-1]`.

---

## C++ Solution

```cpp
class Solution {
public:
    int findMinCost(string &s1, string &s2, int costS1, int costS2) {
        
        int n = s1.size();
        int m = s2.size();

        vector<long long> dp(m + 1, 0);

        for (int i = 1; i <= n; i++) {
            long long prev = 0;

            for (int j = 1; j <= m; j++) {
                long long temp = dp[j];

                if (s1[i - 1] == s2[j - 1]) {
                    dp[j] = prev + costS1 + costS2;
                }
                else {
                    dp[j] = max(dp[j], dp[j - 1]);
                }

                prev = temp;
            }
        }

        long long totalCost =
            1LL * n * costS1 +
            1LL * m * costS2;

        return totalCost - dp[m];
    }
};
```

---

## Example

### Input

```text
s1 = "abcd"
s2 = "ac"
costS1 = 2
costS2 = 3
```

The common subsequence is:

```text
"ac"
```

Initial deletion cost:

```text
4 × 2 + 2 × 3
= 8 + 6
= 14
```

Keeping `"ac"` saves:

```text
2 × (2 + 3)
= 10
```

Therefore:

```text
Minimum Cost = 14 - 10
             = 4
```

We delete:

```text
'b' and 'd' from s1
```

Both strings become:

```text
"ac"
```

---

## Complexity

### Time Complexity

```text
O(n × m)
```

where:

* `n = s1.length()`
* `m = s2.length()`

### Space Complexity

```text
O(m)
```

We use a 1D DP array instead of a full `n × m` table.

---

## Key Concept

This problem is essentially a **Weighted Longest Common Subsequence** problem.

Instead of maximizing the number of common characters, we maximize:

```text
LCS length × (costS1 + costS2)
```

Since every matched character gives the same saving, the problem can be solved using standard LCS DP with weighted transitions.

---

## Topics

* Dynamic Programming
* Longest Common Subsequence
* String
* Space Optimization
* 1D DP
* Weighted LCS

---

## Pattern

```text
Minimum Deletion Cost
        ↓
Keep Common Characters
        ↓
Find Common Subsequence
        ↓
Weighted LCS
        ↓
Total Cost - Maximum Saving
```
