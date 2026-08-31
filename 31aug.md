# Minimum Cost for n Characters

## Problem Statement

Given four integers `n`, `i`, `d`, and `c`:

* `i` = cost of inserting one character.
* `d` = cost of deleting the last character.
* `c` = cost of copying the entire current string and pasting it immediately, which doubles its length.

Initially, the screen is empty.

Find the **minimum cost required to obtain exactly `n` characters** on the screen.

---

## Examples

### Example 1

```text
Input:
n = 9
i = 1
d = 2
c = 1

Output:
5
```

### Explanation

One optimal sequence is:

```text
Insert       → 1
Insert       → 2
Copy-Paste   → 4
Copy-Paste   → 8
Insert       → 9
```

Total cost:

```text
1 + 1 + 1 + 1 + 1 = 5
```

---

### Example 2

```text
Input:
n = 9
i = 10
d = 1
c = 1

Output:
17
```

Since insertion is expensive, we can use copy-paste operations and delete extra characters.

One possible sequence:

```text
Insert       → 1
Copy-Paste   → 2
Copy-Paste   → 4
Delete       → 3
Copy-Paste   → 6
Delete       → 5
Copy-Paste   → 10
Delete       → 9
```

Total cost:

```text
10 + 1 + 1 + 1 + 1 + 1 + 1 + 1 = 17
```

---

## Approach

We use **Dynamic Programming**.

Let:

```text
dp[x] = minimum cost required to obtain exactly x characters
```

### Base Cases

For zero characters:

```text
dp[0] = 0
```

To create one character, we must insert it:

```text
dp[1] = i
```

---

## Transition

For every length `x` from `2` to `n`, there are two main possibilities.

### 1. Insert One Character

We can obtain `x` characters by first obtaining `x - 1` characters and inserting one more:

```text
dp[x] = dp[x - 1] + i
```

---

### 2. Copy-Paste

Copy-paste doubles the current string.

#### Case 1: `x` is Even

If `x` is even, we can first create `x / 2` characters and then copy-paste:

```text
x/2 → x
```

Cost:

```text
dp[x / 2] + c
```

So:

```text
dp[x] = min(dp[x], dp[x / 2] + c)
```

---

#### Case 2: `x` is Odd

For an odd `x`, directly reaching `x` through doubling is impossible.

Instead, we can create `(x + 1) / 2` characters, copy-paste to get `x + 1`, and then delete one character:

```text
(x + 1) / 2
        ↓
    Copy-Paste
        ↓
      x + 1
        ↓
      Delete
        ↓
        x
```

Cost:

```text
dp[(x + 1) / 2] + c + d
```

Therefore:

```text
dp[x] = min(dp[x], dp[(x + 1) / 2] + c + d)
```

---

## Algorithm

1. Create a DP array of size `n + 2`.
2. Set `dp[0] = 0`.
3. Set `dp[1] = i`.
4. For every `x` from `2` to `n`:

   * Consider inserting one character.
   * If `x` is even, consider copy-pasting from `x / 2`.
   * If `x` is odd, consider copy-pasting from `(x + 1) / 2` and deleting one character.
5. Return `dp[n]`.

---

## C++ Solution

```cpp
class Solution {
public:
    int minCost(int n, int i, int d, int c) {

        vector<long long> dp(n + 2, 0);

        dp[0] = 0;
        dp[1] = i;

        for (int x = 2; x <= n; x++) {

            // Insert one character
            dp[x] = dp[x - 1] + i;

            if (x % 2 == 0) {

                // Copy-paste from x/2 to get x
                dp[x] = min(dp[x],
                            dp[x / 2] + c);

            } else {

                // Copy-paste from (x+1)/2 to get x+1,
                // then delete one character
                dp[x] = min(dp[x],
                            dp[(x + 1) / 2] + c + d);
            }
        }

        return dp[n];
    }
};
```

---

## Dry Run

Consider:

```text
n = 9
i = 1
d = 2
c = 1
```

The DP values are:

| Characters | Minimum Cost |
| ---------- | ------------ |
| 0          | 0            |
| 1          | 1            |
| 2          | 2            |
| 3          | 4            |
| 4          | 3            |
| 5          | 5            |
| 6          | 4            |
| 7          | 6            |
| 8          | 4            |
| 9          | 5            |

Therefore:

```text
dp[9] = 5
```

---

## Why Does the Odd Case Work?

Suppose we want `5` characters.

We can create `3` characters first:

```text
3
```

Copy-paste doubles it:

```text
3 → 6
```

Then delete one:

```text
6 → 5
```

So the cost is:

```text
dp[3] + c + d
```

This is why for odd `x` we check:

```cpp
dp[(x + 1) / 2] + c + d
```

---

## Complexity

### Time Complexity

```text
O(n)
```

We calculate each DP state exactly once.

### Space Complexity

```text
O(n)
```

We store the minimum cost for every number of characters from `0` to `n`.

---

## Key Takeaway

The important observation is that for every target length `x`, we only need to consider:

1. **Insert** one character from `x - 1`.
2. **Copy-paste** from `x / 2` when `x` is even.
3. **Copy-paste + delete** when `x` is odd.

This gives a simple and efficient **Dynamic Programming** solution that works within:

```text
1 ≤ n ≤ 10^6
1 ≤ i, d, c ≤ 100
```

---

## Tags

`Dynamic Programming` `DP` `Greedy-like Transition` `Optimization` `C++` `Minimum Cost`
