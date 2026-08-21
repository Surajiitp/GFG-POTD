# Number of Turns in Binary Tree

## Problem

Given the root of a binary tree and the values of two nodes `p` and `q`, find the number of **turns** required to travel from node `p` to node `q`.

A **turn** occurs whenever the direction of movement changes:

* Left → Right
* Right → Left

If the path between `p` and `q` contains **no turns**, return `-1`.

> All node values are distinct.

---

## Example

### Input

```text
root[] = [1, 2, 3, 4, 5, 6, 7, 8, N, N, N, 9, 10]
p = 5
q = 10
```

The path is:

```text
5 → 2 → 1 → 3 → 6 → 10
```

Directions along this path are:

```text
5 → 2    : Up
2 → 1    : Up
1 → 3    : Right
3 → 6    : Left
6 → 10   : Right
```

The direction changes at:

```text
2, 1, 3, 6
```

Therefore:

```text
Output = 4
```

---

### Example 2

```text
p = 1
q = 4
```

Path:

```text
1 → 2 → 4
```

There is no change between left and right directions.

Therefore:

```text
Output = -1
```

---

# Approach

The main idea is to store the path from the root to each target node.

For every node in the path:

```text
0 = move to left child
1 = move to right child
```

For example:

```text
       1
      /
     2
      \
       5
```

The path from `1` to `5` is:

```text
Left → Right
```

So we store:

```text
[0, 1]
```

Since:

```text
0 != 1
```

there is one turn.

---

## Step 1: Find Path to `p`

We perform DFS from the root.

Whenever we move:

```text
left  → push 0
right → push 1
```

If the target is found, we keep the path.

If the target is not found in that subtree, we backtrack by removing the last direction.

---

## Step 2: Find Path to `q`

We repeat the same process for `q`.

For example:

```text
pathP = [0, 1]
pathQ = [1, 0, 0]
```

---

## Step 3: Find the LCA Position

Both paths start from the root.

We compare them until they become different.

```text
pathP = [0, 1, ...]
pathQ = [0, 1, ...]
          ↑
        common
```

The first different position represents the point where the paths split.

This effectively identifies the path segments after the **Lowest Common Ancestor (LCA)**.

---

## Step 4: Count Turns in Each Segment

Suppose after removing the common path we get:

```text
a = [0, 1, 1]
b = [1, 0]
```

For `a`:

```text
0 → 1 = turn
1 → 1 = no turn
```

So:

```text
countTurns(a) = 1
```

For `b`:

```text
1 → 0 = turn
```

So:

```text
countTurns(b) = 1
```

---

## Step 5: Check the Turn at LCA

There can also be a turn **at the LCA**.

If:

```text
a[0] != b[0]
```

then one path leaves the LCA through the left child and the other through the right child.

Therefore, we add one more turn.

```cpp
if (!a.empty() && !b.empty() && a[0] != b[0])
    ans++;
```

---

## Important Edge Case

If the complete path contains no turns:

```text
ans == 0
```

the problem requires:

```text
-1
```

So:

```cpp
return ans == 0 ? -1 : ans;
```

---

# Algorithm

1. Find the root-to-`p` path.
2. Find the root-to-`q` path.
3. Store:

   * `0` for a left move.
   * `1` for a right move.
4. Find the common prefix of both paths.
5. Remove the common prefix.
6. Count direction changes in the remaining `p` path.
7. Count direction changes in the remaining `q` path.
8. If the first directions of both remaining paths are different, add one turn for the LCA.
9. If the total number of turns is `0`, return `-1`.
10. Otherwise, return the total number of turns.

---

# C++ Solution

```cpp
class Solution {
public:

    // Find path from root to target
    // 0 = left, 1 = right
    bool findPath(Node* root, int target, vector<int>& path) {

        if (!root)
            return false;

        if (root->data == target)
            return true;

        // Try left subtree
        path.push_back(0);

        if (findPath(root->left, target, path))
            return true;

        path.pop_back();

        // Try right subtree
        path.push_back(1);

        if (findPath(root->right, target, path))
            return true;

        path.pop_back();

        return false;
    }


    // Count changes:
    // Left -> Right
    // Right -> Left
    int countTurns(vector<int>& path) {

        int turns = 0;

        for (int i = 1; i < path.size(); i++) {

            if (path[i] != path[i - 1])
                turns++;
        }

        return turns;
    }


    int numberOfTurns(Node* root, int p, int q) {

        vector<int> pathP, pathQ;

        // Find paths from root to p and q
        findPath(root, p, pathP);
        findPath(root, q, pathQ);

        int i = 0;

        // Find common path up to LCA
        while (i < pathP.size() &&
               i < pathQ.size() &&
               pathP[i] == pathQ[i]) {

            i++;
        }

        // Remove common path
        vector<int> a(pathP.begin() + i, pathP.end());
        vector<int> b(pathQ.begin() + i, pathQ.end());

        int ans = 0;

        // Count turns on p side
        ans += countTurns(a);

        // Count turns on q side
        ans += countTurns(b);

        // Count turn at LCA
        if (!a.empty() &&
            !b.empty() &&
            a[0] != b[0]) {

            ans++;
        }

        // No turn => -1
        return ans == 0 ? -1 : ans;
    }
};
```

# Dry Run

Consider:

```text
       1
      / \
     2   3
    / \  / \
   4  5 6   7
         \
          10
```

For:

```text
p = 5
q = 10
```

Root-to-`p`:

```text
1 → 2 → 5
```

Directions:

```text
[0, 1]
```

Root-to-`q`:

```text
1 → 3 → 6 → 10
```

Directions:

```text
[1, 0, 1]
```

Common prefix:

```text
[]
```

because the first directions are already different.

### Path `p`

```text
[0, 1]

0 → 1 = 1 turn
```

### Path `q`

```text
[1, 0, 1]

1 → 0 = 1 turn
0 → 1 = 1 turn
```

### Turn at LCA

```text
0 != 1
```

Therefore:

```text
+1
```

Total:

```text
1 + 2 + 1 = 4
```

Hence:

```text
Answer = 4
```

---

# Complexity Analysis

Let `n` be the number of nodes.

### Time Complexity

Finding the two paths:

```text
O(n) + O(n) = O(n)
```

Comparing the paths and counting turns:

```text
O(n)
```

Overall:

```text
O(n)
```

### Space Complexity

The two root-to-node paths can contain up to `O(n)` nodes.

Therefore:

```text
O(n)
```

Additional recursion stack can also be:

```text
O(n)
```

in the worst case of a skewed tree.

---

# Key Idea

The important observation is:

> **A turn depends only on consecutive left/right directions along the path.**

By representing:

```text
Left  = 0
Right = 1
```

a turn is simply:

```text
path[i] != path[i - 1]
```

We split the two root-to-node paths at their common prefix (the LCA), count turns on both sides, and separately handle a possible turn at the LCA.

---

## Tags

`Binary Tree` `DFS` `Tree Traversal` `LCA` `Recursion` `Path Finding` `C++` `GeeksForGeeks` `Hard`
