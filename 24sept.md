# Maximum Stack Height of Discs

## Problem Statement

Given two arrays `r[]` and `h[]` of size `n`, where:

* `r[i]` represents the **radius** of the `i-th` disc.
* `h[i]` represents the **height** of the `i-th` disc.

A disc can be placed **above** another disc only if both:

* Its radius is **strictly smaller**.
* Its height is **strictly smaller**.

Find the **maximum possible total height** of a stack that can be formed.

---

## Approach

This problem can be solved using:

* Sorting
* Coordinate Compression
* Fenwick Tree (Binary Indexed Tree)
* Dynamic Programming

### Step 1: Store the Discs

Store each disc as:

```cpp
{radius, height}
```

Then sort all discs by radius.

---

### Step 2: Coordinate Compression

The Fenwick Tree works efficiently with indices, so compress all height values into their sorted unique positions.

For example:

```text
Heights = [10, 5, 20, 10]

Compressed = [5, 10, 20]
```

Each height is converted into its corresponding index.

---

### Step 3: Fenwick Tree

The Fenwick Tree stores the **maximum stack height** achievable for different height values.

For a disc with height `h`, we need the best stack whose height is **strictly smaller than `h`**.

Therefore:

```cpp
query(pos - 1)
```

is used.

Then:

```cpp
current = best + height;
```

---

### Step 4: Handle Equal Radii

The condition requires the radius to be **strictly smaller**.

Therefore, discs having the same radius cannot be placed on top of each other.

We first calculate the DP values for all discs having the same radius and only then update the Fenwick Tree.

This prevents one disc with radius `r` from using another disc with the same radius.

---

## C++ Solution

```cpp
class Solution {
public:
    int maxStackHeight(vector<int>& r, vector<int>& h) {
        int n = r.size();

        vector<pair<int, int>> discs;

        for (int i = 0; i < n; i++) {
            discs.push_back({r[i], h[i]});
        }

        // Sort by radius
        sort(discs.begin(), discs.end());

        // Coordinate compression of heights
        vector<int> vals;

        for (auto &d : discs) {
            vals.push_back(d.second);
        }

        sort(vals.begin(), vals.end());

        vals.erase(
            unique(vals.begin(), vals.end()),
            vals.end()
        );

        int m = vals.size();

        // Fenwick Tree
        vector<int> bit(m + 1, 0);

        auto query = [&](int idx) {
            int ans = 0;

            while (idx > 0) {
                ans = max(ans, bit[idx]);
                idx -= idx & -idx;
            }

            return ans;
        };

        auto update = [&](int idx, int value) {
            while (idx <= m) {
                bit[idx] = max(bit[idx], value);
                idx += idx & -idx;
            }
        };

        int ans = 0;
        int i = 0;

        while (i < n) {
            int j = i;

            vector<pair<int, int>> updates;

            // Process discs having the same radius together
            while (j < n &&
                   discs[j].first == discs[i].first) {

                int height = discs[j].second;

                int pos = lower_bound(
                    vals.begin(),
                    vals.end(),
                    height
                ) - vals.begin() + 1;

                // Find maximum stack with strictly smaller height
                int best = query(pos - 1);

                int current = best + height;

                ans = max(ans, current);

                updates.push_back({pos, current});

                j++;
            }

            // Update after processing the entire radius group
            for (auto &[pos, value] : updates) {
                update(pos, value);
            }

            i = j;
        }

        return ans;
    }
};
```

---

## Why Do We Use `query(pos - 1)`?

The disc placed below must have a **strictly smaller height**.

Suppose the current height is:

```text
10
```

We cannot use another disc with height `10`.

After coordinate compression, if `10` has position `pos`, then:

```cpp
query(pos - 1)
```

considers only heights smaller than `10`.

---

## Why Are Same-Radius Discs Processed Together?

Suppose we have:

```text
Radius: 5, Height: 4
Radius: 5, Height: 3
```

They cannot be stacked because their radii are equal.

If we immediately update the Fenwick Tree after processing the first disc, the second disc could incorrectly use it.

Therefore, we use two phases:

```text
1. Calculate DP for all discs with the same radius.
2. Update Fenwick Tree after the group is completely processed.
```

This guarantees the radius condition remains **strictly smaller**.

---

## Complexity

Let `n` be the number of discs.

### Sorting

```text
O(n log n)
```

### Coordinate Compression

```text
O(n log n)
```

### Fenwick Tree Operations

Each query and update takes:

```text
O(log n)
```

For all discs:

```text
O(n log n)
```

### Overall

```text
Time Complexity:  O(n log n)
Space Complexity: O(n)
```

---

## Key Concept

The problem can be viewed as finding a **maximum-weight increasing subsequence** where both radius and height must increase strictly.

The Fenwick Tree allows us to efficiently find:

```text
Maximum stack height with smaller height
```

while processing discs in increasing radius order.

---

## Tags

`Dynamic Programming` `Fenwick Tree` `BIT` `Coordinate Compression` `Sorting` `LIS` `Greedy` `GFG` `C++`
