# Form Pyramid

## Problem Statement

Given an array `arr`, we need to form the **largest possible pyramid** by reducing elements if necessary.

A valid pyramid has heights that:

* Increase by `1` toward the peak.
* Decrease by `1` after the peak.
* Have a minimum height of `1` at both ends.

For example:

```text
    3
   2 2
  1 1 1
```

The sum of a pyramid with height `h` is:

```text
1 + 2 + 3 + ... + h + ... + 3 + 2 + 1 = h²
```

The goal is to calculate the **minimum number of elements that need to be removed** from the original array.

---

## Approach

We use **Dynamic Programming** from both directions.

### 1. Left Array

`left[i]` stores the maximum possible height at index `i` when considering elements from the left.

```cpp
left[i] = min(arr[i], left[i - 1] + 1);
```

This ensures that the height can increase by at most `1`.

---

### 2. Right Array

`right[i]` stores the maximum possible height at index `i` when considering elements from the right.

```cpp
right[i] = min(arr[i], right[i + 1] + 1);
```

This ensures that the height can decrease by at most `1` toward the right.

---

### 3. Find Maximum Pyramid Height

For every index `i`, the possible peak height is limited by:

```cpp
min({
    left[i],
    right[i],
    i + 1,
    n - i
});
```

Here:

* `left[i]` → limitation from the left
* `right[i]` → limitation from the right
* `i + 1` → number of positions available on the left
* `n - i` → number of positions available on the right

We take the maximum of these values as the pyramid's height.

---

### 4. Calculate Elements to Remove

The total number of elements/blocks initially present is:

```cpp
total = sum(arr)
```

A pyramid of height `h` contains:

```text
h²
```

blocks.

Therefore:

```cpp
answer = total - h²
```

---

## C++ Solution

```cpp
class Solution {
public:
    int formPyramid(vector<int>& arr) {
        int n = arr.size();

        vector<int> left(n), right(n);

        // Maximum possible height ending at i
        left[0] = 1;

        for (int i = 1; i < n; i++) {
            left[i] = min(arr[i], left[i - 1] + 1);
        }

        // Maximum possible height starting at i
        right[n - 1] = 1;

        for (int i = n - 2; i >= 0; i--) {
            right[i] = min(arr[i], right[i + 1] + 1);
        }

        // Total number of blocks
        long long total = 0;

        for (int x : arr) {
            total += x;
        }

        int maxHeight = 0;

        // Find maximum possible pyramid height
        for (int i = 0; i < n; i++) {
            int h = min({
                left[i],
                right[i],
                i + 1,
                n - i
            });

            maxHeight = max(maxHeight, h);
        }

        // Sum of pyramid:
        // 1 + 2 + ... + h + ... + 2 + 1 = h * h
        long long pyramidSum = 1LL * maxHeight * maxHeight;

        return (int)(total - pyramidSum);
    }
};
```

---

## Example

### Input

```text
arr = [1, 3, 5, 3, 1]
```

The maximum pyramid can be:

```text
    5
   3 3
  1 1 1
```

Maximum height:

```text
h = 3
```

Pyramid sum:

```text
h² = 3² = 9
```

Original sum:

```text
1 + 3 + 5 + 3 + 1 = 13
```

Elements removed:

```text
13 - 9 = 4
```

### Output

```text
4
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

We traverse the array a constant number of times.

### Space Complexity

```text
O(n)
```

We use two auxiliary arrays:

```cpp
left
right
```

---

## Key Concept

The main idea is to calculate the maximum valid height possible from **both directions**.

```text
Left constraint  → left[]
Right constraint → right[]
```

For every possible peak:

```text
height = min(left[i], right[i], i + 1, n - i)
```

Then:

```text
Maximum Pyramid Height → h
Pyramid Blocks         → h²
Answer                 → total - h²
```

---

## Topics

* Dynamic Programming
* Arrays
* Prefix / Suffix DP
* Greedy Constraints
* Mathematical Formula
* Array Traversal

---

## Complexity

| Metric | Complexity |
| ------ | ---------- |
| Time   | `O(n)`     |
| Space  | `O(n)`     |

---

## Author

**Suraj Kumar**

* C++ / DSA
* Competitive Programming
* IIT Patna CSE
