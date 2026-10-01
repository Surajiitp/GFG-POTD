Minimum Time to Finish Project

## Problem Statement

An IT company is working on a large project consisting of `n` modules.

- `duration[i]` represents the time required (in months) to complete module `i`.
- `dependencies[i] = [u, v]` means module `v` can be started only after module `u` is completed.
- Multiple modules can be worked on simultaneously if all their dependencies are completed.

The task is to find the **minimum time required to complete the entire project**.

If the project cannot be completed because of a **cyclic dependency**, return `-1`.

---

## Example

### Input

```text
duration = [10, 20, 30, 10, 30, 20]

dependencies = [
    [5, 2],
    [5, 0],
    [4, 0],
    [4, 1],
    [2, 3],
    [3, 1]
]
Output
80
Approach

This problem can be solved using Topological Sort (Kahn's Algorithm) along with Dynamic Programming.

Key Idea

For every module, we maintain:

startTime[i]

which represents the earliest time at which module i can start.

If module u is completed at:

startTime[u] + duration[u]

then its dependent module v can start only after that time.

Therefore:

startTime[v] = max(startTime[v],
                   startTime[u] + duration[u])

We process modules in topological order.

Steps
Build a directed graph using the dependencies.
Calculate the indegree of every module.
Add all modules having indegree = 0 to a queue.
Their startTime is initially 0.
Process modules using BFS (Kahn's Algorithm).
For every edge u -> v:
Update the earliest start time of v.
Decrease the indegree of v.
If its indegree becomes 0, push it into the queue.
For every processed module, calculate its finish time:
finishTime = startTime[u] + duration[u]
The project completion time is the maximum finish time of all modules.
If all modules are not processed, a cycle exists, so return -1.
Why max() Is Used?

Suppose module v has two dependencies:

u1 -> v
u2 -> v

If:

u1 finishes at 30
u2 finishes at 50

then v cannot start at 30.

It must wait until both dependencies are completed.

Therefore:

startTime[v] = max(30, 50)
             = 50

This is the key dynamic programming idea in the problem.

C++ Solution
class Solution {
public:
    long long minTime(vector<int>& duration,
                      vector<vector<int>>& dependencies) {

        int n = duration.size();

        // Build graph
        vector<vector<int>> adj(n);
        vector<int> indegree(n, 0);

        for (auto &edge : dependencies) {
            int u = edge[0];
            int v = edge[1];

            adj[u].push_back(v);
            indegree[v]++;
        }

        // Earliest time at which each module can start
        vector<long long> startTime(n, 0);

        queue<int> q;

        // Modules without dependencies can start immediately
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) {
                q.push(i);
                startTime[i] = 0;
            }
        }

        int processedCount = 0;
        long long totalMinTime = 0;

        // Kahn's Algorithm
        while (!q.empty()) {

            int u = q.front();
            q.pop();

            processedCount++;

            // Time when module u finishes
            long long finishTime =
                startTime[u] + duration[u];

            // Project completion time
            totalMinTime =
                max(totalMinTime, finishTime);

            // Process dependent modules
            for (int v : adj[u]) {

                // v can start only after u finishes
                startTime[v] =
                    max(startTime[v], finishTime);

                indegree[v]--;

                // All dependencies of v are completed
                if (indegree[v] == 0) {
                    q.push(v);
                }
            }
        }

        // If not all modules were processed,
        // there is a cycle.
        if (processedCount != n) {
            return -1;
        }

        return totalMinTime;
    }
};
Dry Run

For:

duration = [10, 20, 30, 10, 30, 20]

and dependencies:

5 -> 2
5 -> 0
4 -> 0
4 -> 1
2 -> 3
3 -> 1

Initially, modules 4 and 5 have no dependencies.

So:

4 starts at 0
5 starts at 0
Module 4
duration[4] = 30

finish time = 0 + 30 = 30

Therefore:

startTime[0] = 30
startTime[1] = 30
Module 5
duration[5] = 20

finish time = 0 + 20 = 20

Therefore:

startTime[2] = 20

and module 0 must wait for both 4 and 5:

startTime[0] = max(30, 20)
             = 30
Module 2
startTime[2] = 20
duration[2] = 30

finish = 50

So:

startTime[3] = 50
Module 3
startTime[3] = 50
duration[3] = 10

finish = 60

Module 1 depends on both 4 and 3:

startTime[1] = max(30, 60)
             = 60

Finally:

module 1 finishes at 60 + 20 = 80

Hence:

Answer = 80
Cycle Detection

Kahn's Algorithm also helps detect cycles.

If there is a cycle, some modules will always have:

indegree > 0

and therefore they will never enter the queue.

If:

processedCount != n

then the graph contains a cycle.

So we return:

-1
Complexity Analysis

Let:

N = number of modules
E = number of dependencies
Time Complexity
O(N + E)

Every module and every dependency is processed once.

Space Complexity
O(N + E)

For the adjacency list, indegree array, start-time array, and queue.

Concepts Used
Directed Graph
Topological Sort
Kahn's Algorithm
BFS
Dynamic Programming
Cycle Detection
Dependency Scheduling
Earliest Start Time
Pattern to Remember

This problem follows a very useful Topological Sort + DP pattern.

For every dependency:

u -> v

update:

dp[v] = max(dp[v], dp[u] + duration[u]);

Then the answer is:

max(dp[i] + duration[i])

This pattern is useful in problems involving:

Project scheduling
Course prerequisites
Task dependencies
Build systems
Parallel jobs
Minimum completion time
Critical path in a DAG
