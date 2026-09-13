Problem Statement

Geek Town has n houses numbered from 1 to n. We need to choose one
house to host a party such that its distance from the farthest house
is as small as possible.

The houses are connected by exactly n - 1 bidirectional roads, so the
connections form a tree.

The connections are given as an adjacency list adj, where adj[i]
contains all houses directly connected to house i + 1.

Goal

Return the minimum possible value of the maximum distance from the
selected party house to any other house.

Example

Input

adj = [[2], [1, 4, 3], [2], [2]]

The tree is:

      1
      |
      2
     / \
    3   4

Output

1

Explanation

If the party is hosted at house 2:

Distance to house 1 = 1

Distance to house 3 = 1

Distance to house 4 = 1

Therefore, the maximum distance is 1.

No other house can achieve a smaller maximum distance.

Key Observation

The problem asks us to find a vertex whose maximum distance to every
other vertex is minimum.

For a tree, this value is called the radius of the tree.

The important relationship is:

Radius = ceil(Diameter / 2)

So instead of checking every house separately, we can:

Find the diameter of the tree.

Calculate half of the diameter, rounded up.

What is Tree Diameter?

The diameter of a tree is the maximum distance between any two
vertices.

For example:

1 -- 2 -- 3 -- 4 -- 5

The diameter is:

1 → 2 → 3 → 4 → 5

So:

Diameter = 4

The best party house is the middle of the diameter:

1 -- 2 -- [3] -- 4 -- 5

Its maximum distance from any house is:

2

Hence:

Radius = ceil(4 / 2) = 2

How to Find the Diameter Efficiently?

We can find the diameter of a tree using two BFS traversals.

Step 1: Start BFS from any node

Start from house 1.

The farthest node obtained from this BFS is guaranteed to be one
endpoint of the tree's diameter.

Let this node be A.

Step 2: Start another BFS from A

Now run BFS again, starting from A.

The farthest node from A will be the other endpoint of the diameter.

The distance obtained is the diameter.

Step 3: Calculate the Radius

Once the diameter is known:

answer = ceil(diameter / 2)

For integer arithmetic:

(diameter + 1) / 2

Why Does Two BFS Work?

Consider any tree.

If we start BFS from an arbitrary node, BFS finds the farthest node from
that starting point.

In a tree, one of the farthest nodes from an arbitrary starting node is
an endpoint of a diameter.

Therefore:

Any node
   ↓
BFS
   ↓
Diameter endpoint A
   ↓
BFS
   ↓
Other endpoint B

The distance from A to B is the diameter.

Because a tree has a unique path between any two nodes, BFS correctly
calculates shortest distances.

Approach

We first convert the given adjacency list from 1-based house numbers
to 0-based indexing.

Then:

1. BFS from house 1

Find the farthest house.

auto p1 = bfs(0, graph);

The first value returned is the farthest node.

2. BFS from the farthest house

Run BFS again from that node.

auto p2 = bfs(p1.first, graph);

The second value returned is the diameter.

3. Find the minimum possible maximum distance

return (diameter + 1) / 2;

C++ Solution

class Solution {
public:

    pair<int, int> bfs(int start, vector<vector<int>>& adj) {

        int n = adj.size();

        vector<int> dist(n, -1);
        queue<int> q;

        q.push(start);
        dist[start] = 0;

        int farthest = start;

        while (!q.empty()) {

            int u = q.front();
            q.pop();

            for (int v : adj[u]) {

                if (dist[v] == -1) {

                    dist[v] = dist[u] + 1;
                    q.push(v);

                    if (dist[v] > dist[farthest]) {
                        farthest = v;
                    }
                }
            }
        }

        return {farthest, dist[farthest]};
    }

    int partyHouse(vector<vector<int>>& adj) {

        int n = adj.size();

        // Convert 1-based house numbers to 0-based indexing
        vector<vector<int>> graph(n);

        for (int i = 0; i < n; i++) {

            for (int v : adj[i]) {

                graph[i].push_back(v - 1);
            }
        }

        // First BFS: find one endpoint of the diameter
        auto p1 = bfs(0, graph);

        // Second BFS: find the diameter
        auto p2 = bfs(p1.first, graph);

        int diameter = p2.second;

        // Radius = ceil(diameter / 2)
        return (diameter + 1) / 2;
    }
};

Dry Run

For:

adj = [[2], [1, 4, 3], [2], [2]]

The tree is:

      1
      |
      2
     / \
    3   4

Start BFS from house 1.

Distances:

1 → 1 = 0
1 → 2 = 1
1 → 3 = 2
1 → 4 = 2

A farthest node is 3.

Now start BFS from house 3.

Distances:

3 → 3 = 0
3 → 2 = 1
3 → 1 = 2
3 → 4 = 2

Therefore:

Diameter = 2

Now calculate:

Radius = ceil(2 / 2)
       = 1

So the answer is:

1

Complexity Analysis

Let n be the number of houses.

A tree contains exactly:

n - 1 edges

Each BFS visits every vertex and edge at most once.

We perform BFS twice.

Time Complexity

O(n)

More precisely:

O(V + E)

Since for a tree:

E = V - 1

we get:

O(n)

Space Complexity

The adjacency list, distance array, and BFS queue require:

O(n)

Therefore:

Time:  O(n)
Space: O(n)

Alternative Way to Understand the Formula

The diameter represents the longest path:

A ---------------- B

If we want a house that minimizes its maximum distance from all houses,
we should choose the middle of this longest path.

For an even diameter:

Diameter = 4

A -- 1 -- 2 -- 3 -- B
          ↑
        center

Answer:

2

For an odd diameter:

Diameter = 5

A -- 1 -- 2 -- 3 -- 4 -- B
             ↑
        two middle nodes

The minimum maximum distance is:

3

Therefore:

answer = ceil(diameter / 2)

or:

(diameter + 1) / 2

Important Concepts Used

Tree

Graph Traversal

BFS

Shortest Path in an Unweighted Graph

Tree Diameter

Tree Radius

Center of a Tree

1-Based to 0-Based Index Conversion

Common Mistakes

1. Running BFS from every house

A simple approach would calculate the maximum distance for every house.

That can take:

O(n²)

The two-BFS diameter approach reduces it to:

O(n)

2. Forgetting the 1-based indexing

The adjacency list uses house numbers:

1, 2, 3, ..., n

while C++ vectors use:

0, 1, 2, ..., n-1

So we use:

v - 1

when converting the input.

3. Using integer division incorrectly

The required value is:

ceil(diameter / 2)

For integers, use:

(diameter + 1) / 2

not simply:

diameter / 2

because an odd diameter must be rounded upward.

Final Takeaway

The central idea is:

Minimum possible maximum distance
              ↓
        Tree Radius
              ↓
      ceil(Diameter / 2)
              ↓
       Find diameter
              ↓
         Two BFS

This converts what looks like an expensive all-houses distance
problem into a simple linear-time tree traversal.

Complexity

Time  : O(n)
Space : O(n)
