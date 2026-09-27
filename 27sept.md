# Longest Colored Path

[![Difficulty: Hard](https://img.shields.io/badge/Difficulty-Hard-red.svg)](https://www.geeksforgeeks.org/problems/longest-colored-path--151454/1)
[![Platform: GeeksforGeeks](https://img.shields.io/badge/Platform-GeeksforGeeks-2F8D46.svg)](https://www.geeksforgeeks.org/problems/longest-colored-path--151454/1)
[![Language: C++17](https://img.shields.io/badge/Language-C%2B%2B17-00599C.svg)]()

Solution and explanation for the GeeksforGeeks problem **[Longest Colored Path](https://www.geeksforgeeks.org/problems/longest-colored-path--151454/1)**.

---

## 📋 Problem Statement

Given an undirected acyclic graph (tree) with `n` nodes numbered from `1` to `n`, each node is colored either **Red (`R`)** or **Blue (`B`)**.

The colors of the nodes are given by a string `s` of length `n`, where:
- `s[i] = 'R'` means node `i + 1` is Red.
- `s[i] = 'B'` means node `i + 1` is Blue.

You are also given a list of `n - 1` edges `edges[][]`, where each `edges[i] = [u, v]` represents an undirected edge between nodes `u` and `v`.

You can start from any node and traverse along the edges to form a simple path. A path is called **valid** if, once you visit a Blue node, you cannot visit any Red node after it on the same path.

In other words, a valid path must have one of the following forms:
- Only **Red** nodes (`R -> R -> ... -> R`)
- Only **Blue** nodes (`B -> B -> ... -> B`)
- Some **Red** nodes followed by some **Blue** nodes (`R -> ... -> R -> B -> ... -> B`)
- A path containing a pattern like `Blue -> Red` is invalid.

Find the **maximum number of nodes** in a valid path.

---

## 🔍 Examples

**Example 1:**
```text
Input:  s = "RBB", edges = [[1, 2], [1, 3]]
Output: 2
Explanation: The longest path is either 1 -> 2 or 1 -> 3. In both cases, the length of the path is 2.
```

**Example 2:**
```text
Input:  s = "BB", edges = [[1, 2]]
Output: 2
Explanation: The longest path is 1 -> 2. The length of the path is 2.
```

---

## 🔒 Constraints

- `1 <= s.size() <= 10^5`
- `1 <= edges[i][j] <= s.size()`
- `s` consists only of the characters `'R'` and `'B'`
- `edges.size() = s.size() - 1`

---

## 💡 Approach (Monochromatic Forest + Re-rooting Tree DP)

### Intuition
A valid path can transition from Red to Blue **at most once**. Because the graph is an undirected tree:
1. Any single-color path (`R...R` or `B...B`) stays entirely within one connected component of same-colored nodes.
2. Any valid two-color path (`R...R -> B...B`) consists of a simple Red path ending at node `u`, a single cross-color edge `(u, v)`, and a simple Blue path starting at node `v`.

Thus, if we know `L[u]` — the maximum number of nodes in a monochromatic path starting at node `u` — for every node `u`, the answer is simply the maximum of:
- `L[u]` across all nodes `u` (purely Red or purely Blue paths).
- `L[u] + L[v]` across all edges `(u, v)` where `u` and `v` have different colors.

### Step-by-Step Algorithm
1. **Decompose into Monochromatic Trees:**
   - Add edges where `s[u - 1] == s[v - 1]` to the adjacency list `adj` (forming a forest of same-color trees).
   - Store edges where `s[u - 1] != s[v - 1]` in a separate list `diff_edges`.
2. **BFS Ordering:**
   - Run BFS on each monochromatic component to root the tree and record a topological `order` and `parent` array (avoiding deep recursion limits).
3. **Bottom-Up DP (`max1`, `max2`):**
   - Process nodes in reverse BFS order.
   - For each node `u`, compute the lengths (in edges) of the longest (`max1[u]`) and second-longest (`max2[u]`) downward monochromatic branches in its subtree.
4. **Top-Down / Re-rooting DP (`up`):**
   - Process nodes in forward BFS order to compute `up[v]`: the longest monochromatic branch starting at `v` and going upward through `parent[v]`.
   - If `v` is on the longest downward branch of `u` (`max1[v] + 1 == max1[u]`), use `max2[u]`; otherwise, use `max1[u]`.
5. **Compute Best Path per Node & Combine:**
   - For each node `i`, the longest monochromatic path starting at `i` has `L[i] = 1 + max(max1[i], up[i])` nodes.
   - Initialize `ans = max(L[i])` over all `i`.
   - For every cross-color edge `(u, v)` in `diff_edges`, update `ans = max(ans, L[u] + L[v])`.

---

## ⏱️ Complexity

- **Time Complexity:** `O(N)` — Building the graph, BFS traversal, bottom-up pass, top-down pass, and evaluating cross-color edges each run in linear time.
- **Space Complexity:** `O(N)` — Adjacency list and DP state vectors (`parent`, `order`, `max1`, `max2`, `up`, `L`) use `O(N)` auxiliary memory.

---

## 💻 C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int longestColoredPath(string s, vector<vector<int>>& edges) {
        int n = s.size();
        vector<vector<int>> adj(n + 1);
        vector<pair<int, int>> diff_edges;

        // Separate same-color edges and different-color edges
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            if (s[u - 1] == s[v - 1]) {
                adj[u].push_back(v);
                adj[v].push_back(u);
            } else {
                diff_edges.push_back({u, v});
            }
        }

        vector<int> parent(n + 1, 0), order;
        order.reserve(n);
        vector<bool> vis(n + 1, false);

        // BFS to establish tree hierarchy per monochromatic component
        for (int i = 1; i <= n; ++i) {
            if (!vis[i]) {
                vis[i] = true;
                queue<int> q;
                q.push(i);
                while (!q.empty()) {
                    int u = q.front();
                    q.pop();
                    order.push_back(u);
                    for (int v : adj[u]) {
                        if (!vis[v]) {
                            vis[v] = true;
                            parent[v] = u;
                            q.push(v);
                        }
                    }
                }
            }
        }

        // Bottom-up DP: two longest downward monochromatic branches
        vector<int> max1(n + 1, 0), max2(n + 1, 0);
        for (int i = n - 1; i >= 0; --i) {
            int u = order[i];
            int p = parent[u];
            if (p != 0) {
                int len = max1[u] + 1;
                if (len > max1[p]) {
                    max2[p] = max1[p];
                    max1[p] = len;
                } else if (len > max2[p]) {
                    max2[p] = len;
                }
            }
        }

        // Top-down DP: longest monochromatic branch going up through parent[u]
        vector<int> up(n + 1, 0);
        for (int i = 0; i < n; ++i) {
            int u = order[i];
            for (int v : adj[u]) {
                if (v != parent[u]) {
                    int best_from_u = (max1[v] + 1 == max1[u]) ? max2[u] : max1[u];
                    up[v] = 1 + max(up[u], best_from_u);
                }
            }
        }

        // L[u] = max nodes in a monochromatic path starting at u
        vector<int> L(n + 1, 1);
        int ans = 1;
        for (int i = 1; i <= n; ++i) {
            L[i] = 1 + max(max1[i], up[i]);
            ans = max(ans, L[i]);
        }

        // Combine longest Red path and longest Blue path across every R-B edge
        for (auto& e : diff_edges) {
            ans = max(ans, L[e.first] + L[e.second]);
        }

        return ans;
    }
};
```
