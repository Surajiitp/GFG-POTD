# Max Path Sum Between Two Leaves

## Problem

Given the root of a binary tree, where each node contains an integer value, find the **maximum possible path sum between any two leaf nodes**.

If the tree contains fewer than two leaf nodes, return `-1`.

A valid path must start and end at **leaf nodes**.

---

## Examples

### Example 1

**Input:**

```text
root = [3, 4, 5, -10, 4, N, N]
```

**Output:**

```text
16
```

**Explanation:**

The maximum path is:

```text
4 -> 4 -> 3 -> 5
```

Sum:

```text
4 + 4 + 3 + 5 = 16
```

---

### Example 2

**Input:**

```text
root = [3, 4, 1, -10, 4, N, N]
```

**Output:**

```text
12
```

The maximum path is:

```text
4 -> 4 -> 3 -> 1
```

Sum:

```text
4 + 4 + 3 + 1 = 12
```

---

## Approach

We use **postorder traversal (DFS)**.

For every node, calculate the maximum sum of a path starting from that node and ending at a leaf in its subtree.

### Cases

#### 1. Null node

Return `0`.

#### 2. Leaf node

If the node has no children, return its own value.

```cpp
if (!root->left && !root->right)
    return root->data;
```

#### 3. Node with both children

If both left and right children exist, a valid leaf-to-leaf path can pass through the current node.

The path sum is:

```text
leftSum + root->data + rightSum
```

Update the global answer:

```cpp
res = max(res, leftSum + rightSum + root->data);
```

For returning to the parent, we can only continue through **one** child:

```cpp
return max(leftSum, rightSum) + root->data;
```

#### 4. Node with only one child

A leaf-to-leaf path cannot be completed at this node because one side does not contain a leaf path through the other child.

Therefore, return the path sum through the existing child.

---

## C++ Solution

```cpp
class Solution {
private:
    int maxPathSumUtil(Node* root, int& res) {
        // Base case
        if (!root)
            return 0;

        // Leaf node
        if (!root->left && !root->right)
            return root->data;

        // Recursively calculate left and right path sums
        int leftSum = maxPathSumUtil(root->left, res);
        int rightSum = maxPathSumUtil(root->right, res);

        // Both children exist
        if (root->left && root->right) {
            // Path from a leaf in left subtree
            // to a leaf in right subtree
            res = max(res, leftSum + rightSum + root->data);

            // Return the best path that can be extended upward
            return max(leftSum, rightSum) + root->data;
        }

        // Only one child exists
        if (!root->left)
            return rightSum + root->data;

        return leftSum + root->data;
    }

public:
    int maxPathSum(Node* root) {
        if (!root)
            return -1;

        int res = INT_MIN;

        maxPathSumUtil(root, res);

        // No valid leaf-to-leaf path
        if (res == INT_MIN)
            return -1;

        return res;
    }
};
```

---

## Important Observation

We maintain two different values:

### `res`

Stores the **maximum path sum between two leaves** found anywhere in the tree.

### Return value of `maxPathSumUtil()`

Stores the maximum sum of a path from the current node to **one leaf**, which can be extended to the parent.

This distinction is the key to solving the problem.

---

## Complexity

Let `N` be the number of nodes.

### Time Complexity

```text
O(N)
```

Every node is visited exactly once.

### Space Complexity

```text
O(H)
```

where `H` is the height of the tree, due to recursion.

For a skewed tree:

```text
O(N)
```

For a balanced tree:

```text
O(log N)
```

---

## Key Takeaway

For maximum path sum **between two leaves**:

```text
left path + current node + right path
```

is considered only when the current node has **both children**.

For passing the result upward:

```text
max(left path, right path) + current node
```

is returned.

**Pattern:** Postorder DFS + Global Maximum
