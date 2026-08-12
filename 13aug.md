# Longest Path in a Directed Acyclic Graph (DAG)

A C++ implementation to find the longest path from a given source vertex to all other reachable vertices in a weighted **Directed Acyclic Graph (DAG)**.

## 📌 Overview

Finding the longest path in a general graph is an **NP-Hard** problem. However, for a **Directed Acyclic Graph (DAG)**, it can be solved in linear time $\mathcal{O}(V + E)$ using **Topological Sorting**.

This algorithm finds the maximum distance from a specified source vertex `src` to every other vertex in the graph. If a vertex is unreachable from the source, its distance remains `INT_MIN` (represented as `INF`).

---

## 🚀 How It Works

1. **Topological Sort (Kahn's Algorithm):** 
   - Computes the in-degrees of all vertices.
   - Uses a queue to process vertices with an in-degree of 0, generating a valid topological order.
2. **Distance Initialization:**
   - Sets `dist[src] = 0` and `dist[v] = INT_MIN` for all other vertices.
3. **Edge Relaxation:**
   - Iterates through the vertices in topological order.
   - For each reachable vertex `u`, relaxes its outgoing edges `(u, v, weight)`:
     $$\text{dist}[v] = \max(\text{dist}[v], \text{dist}[u] + \text{weight})$$

---

## 💻 Code Structure

```cpp
class Solution {
public:
    vector<int> maxDistance(int V, int src, vector<vector<int>>& edges) {
        // 1. Build adjacency list & calculate in-degrees
        vector<vector<pair<int, int>>> adj(V);
        vector<int> indegree(V, 0);
        for (const auto& edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            adj[u].push_back({v, w});
            indegree[v]++;
        }

        // 2. Kahn's Algorithm for Topological Sort
        queue<int> q;
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) q.push(i);
        }

        vector<int> topoOrder;
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            topoOrder.push_back(node);

            for (const auto& neighbor : adj[node]) {
                int v = neighbor.first;
                if (--indegree[v] == 0) q.push(v);
            }
        }

        // 3. Relax edges in topological order
        vector<int> dist(V, INT_MIN);
        dist[src] = 0;

        for (int u : topoOrder) {
            if (dist[u] != INT_MIN) {
                for (const auto& neighbor : adj[u]) {
                    int v = neighbor.first;
                    int weight = neighbor.second;
                    if (dist[u] + weight > dist[v]) {
                        dist[v] = dist[u] + weight;
                    }
                }
            }
        }

        return dist;
    }
};
