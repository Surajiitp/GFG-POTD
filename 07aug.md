# Friends Pairing Problem

## Problem Statement

Given **n** friends, each friend can either remain single or be paired up with another friend. Each friend can be paired only once.

Find the total number of ways in which the friends can remain single or be paired up.

> **Note:** Since the answer can be very large, return it modulo **10⁹ + 7**.

---

## Examples

### Example 1

**Input**
```text
n = 3
```

**Output**
```text
4
```

**Explanation**

The possible arrangements are:

- {1}, {2}, {3}
- {1}, {2,3}
- {2}, {1,3}
- {3}, {1,2}

---

### Example 2

**Input**
```text
n = 2
```

**Output**
```text
2
```

**Explanation**

Possible arrangements are:

- {1}, {2}
- {1,2}

---

## Approach

Use **Dynamic Programming**.

Let `dp[i]` represent the number of ways to arrange `i` friends.

For the `i-th` friend, there are two possibilities:

1. **Remain Single**
   - Remaining friends = `i - 1`
   - Ways = `dp[i - 1]`

2. **Pair Up**
   - Choose one friend from the remaining `i - 1` friends.
   - Remaining arrangements = `dp[i - 2]`
   - Ways = `(i - 1) × dp[i - 2]`

Therefore,

```
dp[i] = dp[i-1] + (i-1) × dp[i-2]
```

---

## Algorithm

1. Handle base cases:
   - `dp[1] = 1`
   - `dp[2] = 2`
2. Iterate from `3` to `n`.
3. Apply the recurrence relation.
4. Return the answer modulo **10⁹ + 7**.

---

## C++ Solution

```cpp
class Solution {
public:
    int countFriendsPairings(int n) {
        const int MOD = 1000000007;

        if (n <= 2)
            return n;

        long long prev2 = 1;
        long long prev1 = 2;

        for (int i = 3; i <= n; i++) {
            long long curr = (prev1 + (i - 1LL) * prev2) % MOD;
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
};
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| **Time Complexity** | **O(n)** |
| **Space Complexity** | **O(1)** |

---

## Key Concepts

- Dynamic Programming
- Recurrence Relation
- Space Optimization
- Modular Arithmetic

---

⭐ If you found this solution helpful, please consider giving this repository a **Star**!
