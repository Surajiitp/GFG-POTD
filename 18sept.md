# Minimum Absolute Difference in BST

## Problem

Given the root of a **Binary Search Tree (BST)** containing `n > 1` nodes, find the minimum absolute difference between the values of any two different nodes.

Return the minimum absolute difference.

### Example

```text
Input:
        4
       / \
      2   6
     / \
    1   3

Output:
1
```

### Explanation

The inorder traversal of the BST gives the values in sorted order:

```text
1 2 3 4 6
```

The differences between adjacent values are:

```text
2 - 1 = 1
3 - 2 = 1
4 - 3 = 1
6 - 4 = 2
```

Therefore, the minimum absolute difference is:

```text
1
```

---

## Approach

Since this is a **BST**, its inorder traversal visits nodes in **sorted order**.

So, the minimum difference between any two nodes must occur between two **adjacent values in the inorder traversal**.

### Steps

1. Perform an inorder traversal of the BST.
2. Maintain the value of the previously visited node using `prev`.
3. For every current node:

   * Calculate `root->data - prev`.
   * Update `ans` with the minimum difference.
4. Update `prev` to the current node's value.
5. Return `ans`.

---

## C++ Solution

```cpp
class Solution {
public:
    int ans;
    int prev;

    void inorder(Node* root) {
        if (root == NULL)
            return;

        inorder(root->left);

        if (prev != -1) {
            ans = min(ans, root->data - prev);
        }

        prev = root->data;

        inorder(root->right);
    }

    int absDiff(Node* root) {
        // Reset for every test case
        ans = INT_MAX;
        prev = -1;

        inorder(root);

        return ans;
    }
};
```

---

## Why Inorder Traversal?

For a BST:

```text
Left Subtree < Root < Right Subtree
```

Therefore, inorder traversal produces the nodes in sorted order.

For example:

```text
       5
      / \
     3   8
    / \
   2   4
```

Inorder:

```text
2 → 3 → 4 → 5 → 8
```

Checking adjacent differences is enough because the smallest difference in a sorted array always occurs between adjacent elements.

---

## Complexity

### Time Complexity

```text
O(n)
```

Every node is visited exactly once.

### Space Complexity

```text
O(h)
```

where `h` is the height of the BST, due to the recursion stack.

* Balanced BST: `O(log n)`
* Skewed BST: `O(n)`

---

## Key Concept

> **BST + Minimum Difference → Inorder Traversal**

Remember:

```text
BST Inorder = Sorted Order
```

So instead of storing all node values in an array, we can compare each node directly with the previous inorder node.

---

## Tags

* Binary Search Tree
* BST
* Binary Tree
* Inorder Traversal
* Tree
* Recursion
* Minimum Difference
* C++
* DSA

---

## LeetCode / GeeksforGeeks

**Problem:** Minimum Absolute Difference in BST

**Language:** C++

**Topic:** Binary Search Tree
