# Check If Levels of Two Binary Trees Are Anagrams

## Problem Statement

Given the roots of two binary trees `root1` and `root2`, check whether the nodes at every corresponding level of the two trees are anagrams of each other.

Two levels are considered **anagrams** if they contain the same node values with the same frequencies, regardless of their order.

### Example

```text
Input:
root1 = [1, 3, 2, N, N, 5, 4]
root2 = [1, 2, 3, 4, 5]

Output:
true
```

The values present at each corresponding level have the same frequencies, even if their ordering is different.

---

## Approach

We use **Level Order Traversal (BFS)** for both binary trees simultaneously.

### Steps

1. If either root is `NULL`, check whether both roots are `NULL`.
2. Create two queues:

   * `q1` for the first tree
   * `q2` for the second tree
3. For every level:

   * Get the number of nodes in both queues.
   * If the number of nodes is different, return `false`.
4. Store the frequency of node values from the first tree in a `map`.
5. Decrease the frequency for every corresponding node value from the second tree.
6. If any frequency is not zero, the levels are not anagrams.
7. Continue until both queues become empty.
8. Return `true`.

---

## C++ Solution

```cpp
class Solution {
public:
    bool areAnagrams(Node* root1, Node* root2) {
        if (!root1 || !root2)
            return root1 == root2;

        queue<Node*> q1, q2;

        q1.push(root1);
        q2.push(root2);

        while (!q1.empty() && !q2.empty()) {

            int n1 = q1.size();
            int n2 = q2.size();

            // Number of nodes at current level must be same
            if (n1 != n2)
                return false;

            map<int, int> mp;

            // Process first tree
            for (int i = 0; i < n1; i++) {
                Node* temp = q1.front();
                q1.pop();

                mp[temp->data]++;

                if (temp->left)
                    q1.push(temp->left);

                if (temp->right)
                    q1.push(temp->right);
            }

            // Process second tree
            for (int i = 0; i < n2; i++) {
                Node* temp = q2.front();
                q2.pop();

                mp[temp->data]--;

                if (temp->left)
                    q2.push(temp->left);

                if (temp->right)
                    q2.push(temp->right);
            }

            // Check frequency of every value
            for (auto it : mp) {
                if (it.second != 0)
                    return false;
            }
        }

        return q1.empty() && q2.empty();
    }
};
```

---

## Complexity Analysis

Let `N` be the total number of nodes.

### Time Complexity

```text
O(N log N)
```

Because a `map` is used to store and update frequencies.

### Space Complexity

```text
O(N)
```

The queues and frequency map can contain up to `O(N)` elements.

---

## Key Concept

The important idea is:

```text
Level Order Traversal + Frequency Map
```

For every level:

```text
Tree 1 → increase frequency
Tree 2 → decrease frequency
```

If all frequencies become `0`, that level contains the same values with the same frequencies.

---

## Topics

* Binary Tree
* Breadth First Search (BFS)
* Level Order Traversal
* Queue
* Hashing / Frequency Counting
* Map
* Tree Traversal

---

## Platform

**GeeksforGeeks**

Problem: **Check if all levels of two trees are anagrams or not**

---

## Author

**Suraj Kumar**

IIT Patna | CSE
Competitive Programmer | MERN Stack Developer
