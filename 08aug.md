# Min Edge Movements to Connect a Graph

## Problem Statement

Given a graph with `n` vertices numbered from `0` to `n-1` and `m` edges.

In one operation, we can remove an existing edge and add that edge between any two vertices.

Find the minimum number of operations required to make the graph connected.

If it is not possible to connect the graph, return `-1`.

---

## Approach

We use **Disjoint Set Union (DSU / Union-Find)**.

### Key Observation

For a graph with `n` vertices:

- At least `n - 1` edges are required to connect all vertices.
- If `m < n - 1`, it is impossible to make the graph connected.
- Otherwise, we count the number of connected components using DSU.
- If there are `components` connected components, we need exactly:

```text
components - 1



class Solution {
public:
    vector<int> parent, sz;

    // Find parent with Path Compression
    int find(int x) {
        while (x != parent[x]) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }

        return x;
    }

    int minEdgesReq(int n, vector<vector<int>>& edges) {

        // At least n-1 edges are required
        if (edges.size() < n - 1)
            return -1;

        // Initialize DSU
        parent.resize(n);
        sz.assign(n, 1);

        for (int i = 0; i < n; i++)
            parent[i] = i;

        int components = n;

        // Process every edge
        for (auto &e : edges) {

            int u = find(e[0]);
            int v = find(e[1]);

            // Different components
            if (u != v) {

                // Union by Size
                if (sz[u] < sz[v])
                    swap(u, v);

                parent[v] = u;
                sz[u] += sz[v];

                components--;
            }
        }

        // Need components - 1 operations
        return components - 1;
    }
};
