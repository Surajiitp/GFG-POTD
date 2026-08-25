Negative Weight Cycle

Problem

Given a weighted directed graph containing V vertices numbered from 0 to V - 1 and a list of directed edges, determine whether the graph contains a negative weight cycle.

Each edge is represented as:

[u, v, w]

where there is a directed edge from vertex u to vertex v with weight w.

A negative weight cycle is a cycle whose total edge weight is negative.

Approach

We use the Bellman-Ford algorithm.

Key Idea

Bellman-Ford normally finds shortest paths from a source vertex and can also detect negative cycles.

Here, a negative cycle may exist in any connected component, so we initialize all distances to 0:

vector<int> dist(V, 0);

This effectively allows every vertex to act as a starting point.

Relax every edge V - 1 times.

Perform one additional relaxation.

If any distance can still be reduced, a negative weight cycle exists.

Why does the extra iteration work?

For a graph without a negative cycle, the shortest simple path contains at most V - 1 edges.

Therefore, after V - 1 relaxations, no further improvement should be possible.

If an edge can still be relaxed, the improvement must be caused by a negative weight cycle.

C++ Solution

class Solution {
public:
    bool isNegativeWeightCycle(int V, vector<vector<int>>& edges) {

        vector<int> dist(V, 0);

        // Relax all edges V-1 times
        for (int i = 0; i < V - 1; i++) {
            for (auto &edge : edges) {
                int u = edge[0];
                int v = edge[1];
                int w = edge[2];

                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                }
            }
        }

        // One more relaxation to detect negative cycle
        for (auto &edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int w = edge[2];

            if (dist[v] > dist[u] + w) {
                return true;
            }
        }

        return false;
    }
};

Example

V = 4
edges = [[0,3,6], [1,0,4], [1,2,6], [3,1,2]]

Cycle:

0 -> 3 -> 1 -> 0

Total weight:

6 + 2 + 4 = 12

Since the cycle weight is positive, it is not a negative weight cycle.

Output:

false

Complexity

Time Complexity: O(V × E)

Space Complexity: O(V)

Algorithm

Initialize dist[V] = 0

Repeat V-1 times:
    For every edge (u, v, w):
        If dist[v] > dist[u] + w:
            update dist[v]

For every edge (u, v, w):
    If dist[v] > dist[u] + w:
        return true

return false

Important Point

Unlike standard single-source Bellman-Ford, we initialize all distances to 0 because the problem asks whether a negative cycle exists anywhere in the graph, not only from a particular source.
