# 🐍 Snake and Ladder

## Problem Statement

Given an `n × n` Snakes and Ladders board with cells numbered from `1` to `n²`, find the **minimum number of dice throws** required to reach cell `n²` starting from cell `1`.

You have complete control over the dice outcome. In one throw, you can move forward by any number of cells from **1 to 6**.

If you land on the starting cell of a:

* **Ladder** → You must immediately move to its destination.
* **Snake** → You must immediately move to its destination.

If it is impossible to reach cell `n²`, return `-1`.

---

## Examples

### Example 1

```text
Input:
n = 6
lad = [3, 22, 5, 8, 11, 35, 20, 32]
sn  = [17, 4, 19, 7, 34, 1, 21, 9]

Output:
3
```

### Explanation

One optimal path is:

```text
1 --(4)--> 5 --ladder--> 8
8 --(3)--> 11 --ladder--> 35
35 --(1)--> 36
```

Therefore, the minimum number of dice throws is:

```text
3
```

---

### Example 2

```text
Input:
n = 3
lad = [2, 8]
sn  = [7, 3]

Output:
2
```

### Explanation

```text
1 --(1)--> 2 --ladder--> 8
8 --(1)--> 9
```

Therefore:

```text
Answer = 2
```

---

# Approach

The key observation is that this is a **Shortest Path Problem**.

From every cell, we can move to at most 6 different cells because a dice throw can produce:

```text
1, 2, 3, 4, 5, 6
```

Each dice throw has the same cost of `1`.

Therefore, we can model the board as an **unweighted graph**:

* Each cell is a node.
* An edge exists between two cells if we can reach the second cell using one dice throw.
* If the destination cell contains a snake or ladder, we immediately move to its endpoint.

Since all edges have equal weight, **BFS (Breadth First Search)** gives the minimum number of dice throws.

---

# Algorithm

### Step 1: Create the Board Mapping

Create an array:

```cpp
board[i] = i;
```

Initially, every cell points to itself.

For every ladder:

```text
start → end
```

store:

```cpp
board[start] = end;
```

Similarly, for every snake:

```text
start → end
```

store:

```cpp
board[start] = end;
```

---

### Step 2: Apply BFS

Start BFS from cell `1`.

The queue stores:

```text
{current_cell, number_of_throws}
```

Initially:

```text
{1, 0}
```

---

### Step 3: Try Every Possible Dice Throw

For every current cell, try:

```text
1, 2, 3, 4, 5, 6
```

So:

```cpp
next_cell = current + dice;
```

If `next_cell <= n²`, check whether it has a snake or ladder.

The actual destination becomes:

```cpp
dest = board[next_cell];
```

If `dest` has not been visited, add it to the queue with:

```cpp
throws + 1
```

---

### Step 4: Stop at the Destination

As soon as BFS reaches:

```cpp
n * n
```

return the number of throws.

Because BFS explores states level by level, the first time we reach the destination is guaranteed to be the minimum number of dice throws.

If BFS finishes without reaching the destination, return:

```cpp
-1
```

---

# C++ Solution

```cpp
class Solution {
public:
    int minThrows(int n, vector<int>& lad, vector<int>& sn) {
        int target = n * n;

        // board[i] represents the final destination
        // after landing on cell i.
        vector<int> board(target + 1);

        // Initially every cell points to itself.
        for (int i = 1; i <= target; i++) {
            board[i] = i;
        }

        // Store ladders
        for (int i = 0; i < lad.size(); i += 2) {
            board[lad[i]] = lad[i + 1];
        }

        // Store snakes
        for (int i = 0; i < sn.size(); i += 2) {
            board[sn[i]] = sn[i + 1];
        }

        // BFS queue:
        // {current cell, number of throws}
        queue<pair<int, int>> q;

        vector<bool> visited(target + 1, false);

        // Start from cell 1
        q.push({1, 0});
        visited[1] = true;

        while (!q.empty()) {

            auto [curr, throws] = q.front();
            q.pop();

            // Destination reached
            if (curr == target) {
                return throws;
            }

            // Try all possible dice values
            for (int dice = 1; dice <= 6; dice++) {

                int next_cell = curr + dice;

                // Check board boundary
                if (next_cell <= target) {

                    // Apply snake or ladder
                    int dest = board[next_cell];

                    // Visit only unvisited cells
                    if (!visited[dest]) {
                        visited[dest] = true;

                        q.push({
                            dest,
                            throws + 1
                        });
                    }
                }
            }
        }

        // Destination cannot be reached
        return -1;
    }
};
```

---

# Why BFS?

Suppose we are at cell `1`.

In one dice throw, we can reach:

```text
2, 3, 4, 5, 6, 7
```

These are all at distance `1`.

From those cells, we explore all possible cells reachable in `2` throws, then `3` throws, and so on.

This is exactly how BFS works:

```text
Level 0 → Cell 1

Level 1 → Cells reachable in 1 throw

Level 2 → Cells reachable in 2 throws

Level 3 → Cells reachable in 3 throws

...
```

Therefore, when we first reach `n²`, we have found the minimum number of throws.

---

# Complexity Analysis

Let:

```text
N = n²
```

There are `N` cells.

For every cell, we try at most `6` dice outcomes.

### Time Complexity

```text
O(n² × 6)
```

Since `6` is constant:

```text
O(n²)
```

### Space Complexity

The `board`, `visited`, and BFS queue can contain up to `n²` cells.

```text
O(n²)
```

---

# Key Concepts Used

* Breadth First Search (BFS)
* Shortest Path in an Unweighted Graph
* Queue
* Visited Array
* Graph Traversal
* Array Mapping
* Snakes and Ladders Simulation

---

# Important Insight

The most important idea is:

> **Snakes and Ladders can be treated as an unweighted shortest-path problem.**

Every dice throw has the same cost (`1`), so **BFS is the optimal choice**.

The snake/ladder movement is handled immediately after landing:

```cpp
int dest = board[next_cell];
```

This makes the implementation simple and efficient.

---

## ⏱️ Complexity

```text
Time  : O(n²)
Space : O(n²)
```

## 🚀 Technique

```text
BFS + Shortest Path
```
