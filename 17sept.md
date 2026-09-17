# Minimum Edge Reversal

## Problem

Given a directed graph with `n` vertices numbered from `1` to `n`, represented by a list of directed edges `edges[i] = [u, v]`.

You are also given:

* `src` — source vertex
* `dst` — destination vertex

Find the **minimum number of edges that need to be reversed** so that there exists a path from `src` to `dst`.

If it is impossible, return `-1`.

---

## Approach

We can solve this problem using **0-1 BFS**.

For every directed edge:

```text
u → v
```

we create two possibilities:

1. **Original direction:** `u → v` with cost `0`

   * No reversal is required.

2. **Reverse direction:** `v → u` with cost `1`

   * One edge reversal is required.

So the graph becomes a weighted graph where every edge has a weight of either `0` or `1`.

Then we use **0-1 BFS** to find the minimum cost path from `src` to `dst`.

---

## Why 0-1 BFS?

All edge weights are either:

```text
0 or 1
```

0-1 BFS is specifically designed for this type of graph.

It uses a `deque`:

* If edge cost is `0`, add the vertex to the **front**.
* If edge cost is `1`, add the vertex to the **back**.

This ensures that lower-cost paths are processed first.

---

## Algorithm

1. Create an adjacency list.
2. For every edge `[u, v]`:

   * Add `{v, 0}` to `adj[u]`.
   * Add `{u, 1}` to `adj[v]`.
3. Initialize all distances to `INF`.
4. Set:

   ```cpp
   dist[src] = 0;
   ```
5. Start 0-1 BFS using a deque.
6. For every adjacent vertex:

   * If a shorter path is found:

     ```cpp
     dist[v] = dist[u] + cost;
     ```
   * If `cost == 0`, push to the front.
   * Otherwise, push to the back.
7. Return `dist[dst]`.
8. If `dst` remains unreachable, return `-1`.

---

## C++ Solution

```cpp
class Solution {
public:
    int minimumEdgeReversal(vector<vector<int>>& edges, int n, int src, int dst) {

        vector<vector<pair<int, int>>> adj(n + 1);

        for (auto &e : edges) {
            int u = e[0];
            int v = e[1];

            // Original edge: cost 0
            adj[u].push_back({v, 0});

            // Reverse edge: cost 1
            adj[v].push_back({u, 1});
        }

        const int INF = 1e9;
        vector<int> dist(n + 1, INF);

        deque<int> dq;

        dist[src] = 0;
        dq.push_front(src);

        while (!dq.empty()) {
            int u = dq.front();
            dq.pop_front();

            for (auto &[v, cost] : adj[u]) {

                if (dist[u] + cost < dist[v]) {
                    dist[v] = dist[u] + cost;

                    if (cost == 0)
                        dq.push_front(v);
                    else
                        dq.push_back(v);
                }
            }
        }

        return dist[dst] == INF ? -1 : dist[dst];
    }
};
```

---

## Example

Suppose the graph contains:

```text
1 → 2
3 → 2
3 → 4
```

and we need to go from:

```text
src = 1
dst = 4
```

The path can be:

```text
1 → 2 ← 3 → 4
```

The edge:

```text
3 → 2
```

needs to be reversed to:

```text
2 → 3
```

Therefore, the minimum number of reversals is:

```text
1
```

---

## Complexity

Let:

* `n` = number of vertices
* `m` = number of edges

Each original edge creates two adjacency-list entries.

### Time Complexity

```text
O(n + m)
```

### Space Complexity

```text
O(n + m)
```

---

## Key Concept

The main trick is to convert the problem into a **shortest path problem**:

```text
Original edge  → cost 0
Reversed edge  → cost 1
```

Then apply **0-1 BFS**.

### Pattern to Remember

Whenever a graph problem asks for the minimum number of:

* edge reversals
* changes
* modifications
* special operations

and each transition has a cost of **0 or 1**, consider using:

> **0-1 BFS**

---

## Topics

* Graph
* Directed Graph
* Shortest Path
* 0-1 BFS
* Deque
* Adjacency List
* Graph Traversal
* Greedy Shortest Path

---

## Author

**Suraj Kumar**

CSE, IIT Patna

**Language:** C++

**Difficulty:** Medium
