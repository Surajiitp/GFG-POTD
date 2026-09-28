# 📊 Range GCD Queries

This repository contains an efficient C++ solution for the **Range GCD Queries** problem, solved using a **Segment Tree** data structure to handle both range Greatest Common Divisor (GCD) queries and point updates.

---

## 🚀 Problem Statement

Given an integer array `arr[]` and a 2D array `queries[][]` containing `q` queries, where each query is of one of two types:
1. **Type 1 (`[0, l, r]`)**: Return the **GCD** of all elements in the range `[l, r]` (both inclusive).
2. **Type 2 (`[1, index, value]`)**: Update `arr[index]` to the given `value`.

*Note: Uses 0-based indexing.*

---

## 💡 Example

### Input
* `arr[] = [2, 3, 4, 6, 8, 16]`
* `q = 3`
* `queries[][] = [[0, 0, 2], [1, 3, 8], [0, 2, 5]]`

### Output
* `[1, 4]`

### Explanation
* **Query `[0, 0, 2]`**: Find the GCD of the subarray `arr[0...2] = [2, 3, 4]`. The GCD is `1`.
* **Query `[1, 3, 8]`**: Update `arr[3]` from `6` to `8`. The array becomes `[2, 3, 4, 8, 8, 16]`.
* **Query `[0, 2, 5]`**: Find the GCD of the subarray `arr[2...5] = [4, 8, 8, 16]`. The GCD is `4`.

---

## 🛠️ Approach: Segment Tree

To handle frequent range queries and point updates efficiently without hitting Time Limit Exceeded (TLE) errors, we use a **Segment Tree**:
1. **Tree Construction (`build`)**: Recursively builds the tree where each node stores the GCD of its respective segment, taking $O(N)$ time.
2. **Point Updates (`updateTree`)**: Modifies the value at a specific index and bubbles up the updated GCD values through tree ancestors in $O(\log N)$ time.
3. **Range Queries (`queryTree`)**: Traverses the segments overlapping with the range `[l, r]` and computes the combined GCD of the matching parts in $O(\log N)$ time.

### Complexity Analysis
* **Time Complexity**: 
  * **Tree Construction**: $O(N)$
  * **Per Query / Update**: $O(\log N)$ time complexity per operation.
* **Space Complexity**: $O(N)$ to store the segment tree array (`4 * N`).

---

## 💻 C++ Solution Code

```cpp
#include <iostream>
#include <vector>
#include <numeric>

using namespace std;

class Solution {
    int n;
    vector<int> tree;

    void build(int node, int start, int end, vector<int>& arr) {
        if (start == end) {
            tree[node] = arr[start];
            return;
        }
        int mid = start + (end - start) / 2;
        build(2 * node, start, mid, arr);
        build(2 * node + 1, mid + 1, end, arr);
        tree[node] = std::gcd(tree[2 * node], tree[2 * node + 1]);
    }

    void updateTree(int node, int start, int end, int idx, int val) {
        if (start == end) {
            tree[node] = val;
            return;
        }
        int mid = start + (end - start) / 2;
        if (idx <= mid) {
            updateTree(2 * node, start, mid, idx, val);
        } else {
            updateTree(2 * node + 1, mid + 1, end, idx, val);
        }
        tree[node] = std::gcd(tree[2 * node], tree[2 * node + 1]);
    }

    int queryTree(int node, int start, int end, int l, int r) {
        if (r < start || end < l) {
            return 0; // Identity element for GCD
        }
        if (l <= start && end <= r) {
            return tree[node];
        }
        int mid = start + (end - start) / 2;
        int p1 = queryTree(2 * node, start, mid, l, r);
        int p2 = queryTree(2 * node + 1, mid + 1, end, l, r);
        if (p1 == 0) return p2;
        if (p2 == 0) return p1;
        return std::gcd(p1, p2);
    }

public:
    vector<int> processQueries(vector<int>& arr, vector<vector<int>>& queries) {
        n = arr.size();
        tree.assign(4 * n, 0);
        build(1, 0, n - 1, arr);
        
        vector<int> result;
        for (const auto& q : queries) {
            if (q[0] == 0) {
                // Type 1: Range GCD Query
                result.push_back(queryTree(1, 0, n - 1, q[1], q[2]));
            } else {
                // Type 2: Point Update
                arr[q[1]] = q[2];
                updateTree(1, 0, n - 1, q[1], q[2]);
            }
        }
        return result;
    }
};
