# Largest Odd Squares with Limited 1s

## Problem

Given a binary matrix `mat[][]` of size `n × m` and an integer `k`, process a list of queries.

Each query contains coordinates `[i, j]`, representing the center of a square.

For every query, find the side length of the **largest odd-sized square** centered at `(i, j)` such that the square contains **at most `k` ones**.

If no valid odd-sized square exists, return `-1`.

---

## Approach

We use a **2D Prefix Sum** to efficiently calculate the number of `1`s inside any square.

### Step 1: Build Prefix Sum

Define:

```text
pref[i][j] = number of 1s in the rectangle
            from (0,0) to (i-1,j-1)
