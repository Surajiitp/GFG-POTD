# Values with Equal Array Remainders

## Problem

Given an integer array `arr[]`, count the number of positive integers `k` such that **all elements of the array leave the same remainder when divided by `k`**.

If there are infinitely many such values of `k`, return `-1`.

### Example

```text
Input:  arr[] = [38, 6, 34]
Output: 3
```

Valid values of `k` are:

```text
k = 1, 2, 4
```

---

## Approach

The key observation is:

If all elements have the same remainder when divided by `k`, then for any two elements:

```text
arr[i] % k = arr[j] % k
```

Therefore,

```text
(arr[i] - arr[j]) % k = 0
```

So, `k` must divide the **difference between every pair of elements**.

Instead of checking every pair, we can fix the first element and calculate:

```text
|arr[i] - arr[0]|
```

The required `k` must divide all these differences.

Therefore, `k` must be a **divisor of the GCD of all differences**.

### Step 1: Check if all elements are equal

If every element is equal, then for **any positive integer `k`**, all elements have the same remainder.

There are infinitely many such `k`, so return:

```text
-1
```

### Step 2: Find GCD of differences

Calculate:

```text
g = gcd(|arr[1] - arr[0]|,
       |arr[2] - arr[0]|,
       ...
       |arr[n-1] - arr[0]|)
```

### Step 3: Count divisors of `g`

Every positive divisor of `g` is a valid value of `k`.

We count divisors efficiently in `O(√g)` time.

---

## Why Does This Work?

Suppose:

```text
arr[i] % k = r
```

for every element.

Then:

```text
arr[i] = q1 * k + r
arr[0] = q2 * k + r
```

Subtracting:

```text
arr[i] - arr[0] = (q1 - q2) * k
```

Hence:

```text
k divides (arr[i] - arr[0])
```

for every `i`.

Therefore, `k` must divide their GCD.

Conversely, if `k` divides the GCD of all differences, then:

```text
arr[i] - arr[0]
```

is divisible by `k`, which means:

```text
arr[i] % k = arr[0] % k
```

for every element.

So **all positive divisors of the GCD are exactly the valid values of `k`**.

---

## Algorithm

1. Check whether all elements are equal.

   * If yes, return `-1`.
2. Initialize `g = 0`.
3. For every element from index `1`:

   * Calculate `abs(arr[i] - arr[0])`.
   * Update:

     ```cpp
     g = gcd(g, difference);
     ```
4. Count all divisors of `g`.
5. Return the divisor count.

---

## C++ Solution

```cpp
class Solution {
public:
    int sameMod(vector<int>& arr) {

        int n = arr.size();

        // If all elements are equal,
        // every positive k is valid.
        bool same = true;

        for (int i = 1; i < n; i++) {
            if (arr[i] != arr[0]) {
                same = false;
                break;
            }
        }

        if (same)
            return -1;

        // GCD of all differences
        int g = 0;

        for (int i = 1; i < n; i++) {
            g = gcd(g, abs(arr[i] - arr[0]));
        }

        // Count divisors of GCD
        int ans = 0;

        for (int i = 1; i * i <= g; i++) {

            if (g % i == 0) {
                ans++;

                // Count the paired divisor
                if (i != g / i)
                    ans++;
            }
        }

        return ans;
    }
};
```

---

## Dry Run

For:

```text
arr = [38, 6, 34]
```

Differences from the first element:

```text
|6 - 38|  = 32
|34 - 38| = 4
```

GCD:

```text
gcd(32, 4) = 4
```

Divisors of `4`:

```text
1, 2, 4
```

Therefore:

```text
Answer = 3
```

---

## Another Example

```text
arr = [3, 2]
```

Difference:

```text
|2 - 3| = 1
```

So:

```text
GCD = 1
```

The only divisor is:

```text
1
```

Therefore:

```text
Answer = 1
```

---

## Complexity

Let `n` be the size of the array and `g` be the GCD of all differences.

### Time Complexity

```text
O(n + √g)
```

* `O(n)` to calculate the GCD.
* `O(√g)` to count divisors.

Since `arr[i] ≤ 10^5`, this is easily fast enough.

### Space Complexity

```text
O(1)
```

Only a few integer variables are used.

---

## Key Takeaway

The important mathematical observation is:

> **All elements have the same remainder modulo `k` if and only if `k` divides the GCD of all pairwise differences.**

So the problem reduces to:

```text
Find GCD of differences → Count its divisors
```

If all array elements are equal, there are infinitely many valid `k`, so return `-1`.
