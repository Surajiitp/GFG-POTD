# Your Social Network

**Difficulty:** Medium
**Platform:** GeeksforGeeks
**Language:** C++

## Problem Statement

Geek is creating a social networking site called **Geeksbook** with `n` users numbered from `1` to `n`.

Every user `i` (`2 ≤ i ≤ n`) has exactly one friend, and that friend always has a smaller user number than `i`. User `1` has no friend.

The friends are given in the array `arr[]`, where:

* `arr[0]` → friend of user `2`
* `arr[1]` → friend of user `3`
* ...
* `arr[i - 2]` → friend of user `i`

For every user `i`, find all users `j` that can be reached by repeatedly following the friend relationship.

For every reachable pair, return:

```text
[i, j, k]
```

where:

* `i` = starting user
* `j` = reachable user
* `k` = number of links followed to reach `j`

The result must be ordered by:

1. Starting user `i` in increasing order.
2. Reachable user `j` in increasing order.

## Example 1

### Input

```text
arr = [1, 2]
```

The relationships are:

```text
2 → 1
3 → 2 → 1
```

### Output

```text
[[2, 1, 1],
 [3, 1, 2],
 [3, 2, 1]]
```

### Explanation

* User `2` reaches user `1` in `1` link.
* User `3` reaches user `2` in `1` link.
* User `3` reaches user `1` in `2` links.

## Example 2

### Input

```text
arr = [1, 1]
```

Relationships:

```text
2 → 1
3 → 1
```

### Output

```text
[[2, 1, 1],
 [3, 1, 1]]
```

## Approach

For every user `i` from `2` to `n`:

1. Start from user `i`.
2. Follow the friend link using `arr[curr - 2]`.
3. Increase the distance by `1` after every link.
4. Add `[i, curr, distance]` to the answer.
5. Continue until user `1` is reached.
6. Sort the resulting pairs according to `i`, then `j`.

Since every friend has a smaller user number, following the links will always move toward user `1`, so there is no cycle.

## C++ Solution

```cpp
class Solution {
public:
    vector<vector<int>> socialNetwork(vector<int>& arr) {
        int n = arr.size() + 1;
        vector<vector<int>> ans;

        for (int i = 2; i <= n; i++) {
            int curr = i;
            int dist = 0;

            while (curr != 1) {
                curr = arr[curr - 2];
                dist++;

                ans.push_back({i, curr, dist});
            }
        }

        sort(ans.begin(), ans.end(), [](const vector<int>& a,
                                        const vector<int>& b) {
            if (a[0] != b[0])
                return a[0] < b[0];

            return a[1] < b[1];
        });

        return ans;
    }
};
```

## Complexity Analysis

Let `E` be the total number of reachable pairs.

### Time Complexity

```text
O(E + E log E)
```

The traversal takes `O(E)` and sorting takes `O(E log E)`.

In the worst case, `E = O(n²)`, so the overall complexity is:

```text
O(n² log n)
```

### Space Complexity

```text
O(E)
```

for storing the answer.

## Key Concept

This problem can be viewed as a **directed tree/forest-like structure** where every node has exactly one outgoing edge toward a smaller-numbered node.

The main technique is:

```text
Follow parent/friend links → count distance → store reachable pairs → sort
```

## Constraints

```text
2 ≤ arr.size() ≤ 500
1 ≤ arr[i] ≤ 500
```

## Tags

`Graph` `Traversal` `Simulation` `Sorting` `Linked Structure` `C++` `GeeksforGeeks`
