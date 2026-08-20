# Node and Ancestor Max Diff

## Problem Statement

Given the root of a binary tree, find the maximum difference between an ancestor node `A` and its descendant node `B`.

We need to maximize:

```text
A - B
```

where `A` is an ancestor of `B`.

---

## Examples

### Example 1

```text
Input:
root[] = [5, 2, 1]

Output:
4
```

**Explanation:**

The maximum difference is:

```text
5 - 1 = 4
```

Here, `5` is an ancestor of `1`.

---

### Example 2

```text
Input:
root[] = [1, 2, 3, N, N, N, 7]

Output:
-1
```

**Explanation:**

Possible ancestor-descendant differences include:

```text
1 - 2 = -1
1 - 3 = -2
3 - 7 = -4
```

The maximum value is:

```text
-1
```

So the answer is `-1`.

---

## Approach

We use **postorder traversal** to find the minimum value present in every subtree.

For each node:

1. Recursively find the minimum value in the left subtree.
2. Recursively find the minimum value in the right subtree.
3. Take the minimum of both subtrees.
4. If a descendant exists, calculate:

```text
root->data - minimum descendant
```

5. Update the global maximum answer.
6. Return the minimum value of the current subtree to its parent.

### Why Minimum?

For a fixed ancestor `A`, we want to maximize:

```text
A - B
```

So `B` should be as small as possible.

Therefore, for every node, we only need to know the **minimum value in its descendant subtree**.

---

## Algorithm

```text
helper(root):

    if root is NULL:
        return INT_MAX

    leftMin = helper(root->left)
    rightMin = helper(root->right)

    minDescendant = min(leftMin, rightMin)

    if minDescendant exists:
        ans = max(ans, root->data - minDescendant)

    return min(root->data, minDescendant)
```

The main function initializes `ans` and calls `helper(root, ans)`.

---

## C++ Solution

```cpp
class Solution {
public:
    int helper(Node* root, int& ans) {
        if (root == nullptr)
            return INT_MAX;

        int leftMin = helper(root->left, ans);
        int rightMin = helper(root->right, ans);

        int minDescendant = min(leftMin, rightMin);

        // If there is at least one descendant
        if (minDescendant != INT_MAX) {
            ans = max(ans, root->data - minDescendant);
        }

        // Minimum value in the current subtree
        return min(root->data, minDescendant);
    }

    int maxDiff(Node* root) {
        int ans = INT_MIN;

        helper(root, ans);

        return ans;
    }
};
```

---

## Dry Run

For:

```text
        5
       / \
      2   1
```

### Node `2`

It has no descendants.

Returns:

```text
2
```

### Node `1`

It has no descendants.

Returns:

```text
1
```

### Node `5`

Minimum descendant:

```text
min(2, 1) = 1
```

Calculate:

```text
5 - 1 = 4
```

Update:

```text
ans = 4
```

Return minimum of the subtree:

```text
min(5, 1) = 1
```

Final answer:

```text
4
```

---

## Complexity Analysis

### Time Complexity

```text
O(N)
```

Every node is visited exactly once.

### Space Complexity

```text
O(H)
```

where `H` is the height of the binary tree due to recursion.

For a skewed tree:

```text
O(N)
```

For a balanced tree:

```text
O(log N)
```

---

## Key Idea

> **For every node, find the minimum value among its descendants and maximize `node->data - minimumDescendant`.**

This can be solved efficiently using a **postorder DFS**.

---

## Tags

`Binary Tree` `DFS` `Postorder Traversal` `Recursion` `Tree` `GFG` `Medium`
