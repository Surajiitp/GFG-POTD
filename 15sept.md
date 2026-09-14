# Visit Leaves with Budget

## Problem

Given a binary tree and an integer `k`, you start from the root at level `1`.

The cost of visiting a leaf node is equal to its level in the binary tree. You can visit any number of leaf nodes, but the total cost must not exceed `k`.

Return the maximum number of leaf nodes that can be visited within the given budget.

## Approach

The idea is to use a **Greedy + DFS** approach.

1. Traverse the binary tree using DFS.
2. For every leaf node, store its level as its visiting cost.
3. Sort all leaf costs in ascending order.
4. Visit the cheapest leaves first until the budget is exhausted.
5. Return the number of leaves visited.

Since every leaf contributes exactly `1` to the answer, choosing the cheapest leaves first always gives the maximum count.

## Algorithm

* Start DFS from the root with `level = 1`.
* If the current node is a leaf, add its level to the `costs` vector.
* Recursively visit the left and right subtrees.
* Sort `costs`.
* For every cost:

  * If the cost is greater than the remaining budget, stop.
  * Otherwise, subtract the cost from `k` and increment the answer.
* Return the answer.

## Complexity

Let `N` be the number of nodes and `L` be the number of leaf nodes.

* DFS: `O(N)`
* Sorting leaf costs: `O(L log L)`
* Total: `O(N + L log L)`
* Space: `O(N)`

## C++ Solution

```cpp
class Solution {
public:
    void dfs(Node* root, int level, vector<int>& costs) {
        if (root == nullptr)
            return;

        // Leaf node
        if (root->left == nullptr && root->right == nullptr) {
            costs.push_back(level);
            return;
        }

        dfs(root->left, level + 1, costs);
        dfs(root->right, level + 1, costs);
    }

    int getCount(Node* root, int k) {
        vector<int> costs;

        // Root starts at level 1
        dfs(root, 1, costs);

        // Visit cheapest leaves first
        sort(costs.begin(), costs.end());

        int count = 0;

        for (int cost : costs) {
            if (cost > k)
                break;

            k -= cost;
            count++;
        }

        return count;
    }
};
```

## Key Concept

**Greedy:** Always choose the leaf with the smallest cost first.

**DFS:** Used to find every leaf and calculate its level.

**Sorting:** Ensures that leaves are considered from cheapest to most expensive.

## Example

Consider the leaf costs:

```text
[3, 3, 4, 5]
```

and budget:

```text
k = 10
```

After sorting:

```text
3 + 3 + 4 = 10
```

So, the maximum number of leaves that can be visited is:

```text
3
```

## Topics

* Binary Tree
* DFS
* Tree Traversal
* Greedy
* Sorting
* Recursion
* GFG POTD
