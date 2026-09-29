🐴 Min Steps by Knight

📝 Problem Statement

Given a square chessboard of size N x N, the initial position of a Knight KnightPos and the target position TargetPos are given.

Find the minimum number of moves required for the Knight to reach the target position.

A Knight moves in an L-shape:

(x + 2, y + 1)

(x + 2, y - 1)

(x - 2, y + 1)

(x - 2, y - 1)

(x + 1, y + 2)

(x + 1, y - 2)

(x - 1, y + 2)

(x - 1, y - 2)

Positions are given using 1-based indexing.

💡 Approach

We use Breadth First Search (BFS) to find the minimum number of moves.

The chessboard can be considered as an unweighted graph:

Each chessboard cell = Node

Each valid Knight move = Edge

Every move has cost = 1

Since every move has the same cost, BFS gives the shortest path.

Why BFS?

BFS explores positions level by level:

Level 0 → Starting position
Level 1 → Positions reachable in 1 move
Level 2 → Positions reachable in 2 moves
Level 3 → Positions reachable in 3 moves
...

Therefore, the first time we reach the target, we have found the minimum number of moves.

🔑 Algorithm

Create a visited matrix of size (N + 1) x (N + 1) because the board uses 1-based indexing.

Store all 8 possible Knight moves.

Insert the starting position into a queue with 0 steps.

Mark the starting position as visited.

Run BFS until the queue becomes empty.

For every current position:

Try all 8 Knight moves.

Check whether the new position is inside the board.

Check whether the position is already visited.

If the target position is reached, return the current number of steps.

Otherwise, push the new position into the queue with steps + 1.

If the target cannot be reached, return -1.

💻 C++ Solution

class Solution {
public:
    int minStepToReachTarget(vector<int>& KnightPos,
                             vector<int>& TargetPos,
                             int N) {

        // 8 possible Knight moves
        int dx[] = {2, 2, -2, -2, 1, 1, -1, -1};
        int dy[] = {1, -1, 1, -1, 2, -2, 2, -2};

        // Visited matrix for 1-based indexing
        vector<vector<bool>> visited(
            N + 1,
            vector<bool>(N + 1, false)
        );

        // Queue stores {x, y, steps}
        queue<vector<int>> q;

        int startX = KnightPos[0];
        int startY = KnightPos[1];

        int targetX = TargetPos[0];
        int targetY = TargetPos[1];

        // Starting position
        q.push({startX, startY, 0});
        visited[startX][startY] = true;

        while (!q.empty()) {

            vector<int> curr = q.front();
            q.pop();

            int x = curr[0];
            int y = curr[1];
            int steps = curr[2];

            // Target reached
            if (x == targetX && y == targetY) {
                return steps;
            }

            // Try all 8 possible Knight moves
            for (int i = 0; i < 8; i++) {

                int nx = x + dx[i];
                int ny = y + dy[i];

                // Check boundaries and visited status
                if (nx >= 1 && nx <= N &&
                    ny >= 1 && ny <= N &&
                    !visited[nx][ny]) {

                    visited[nx][ny] = true;

                    q.push({nx, ny, steps + 1});
                }
            }
        }

        // Target cannot be reached
        return -1;
    }
};

📌 Example

Input

N = 6
KnightPos = [4, 3]
TargetPos = [1, 2]

Output

2

The Knight reaches the target in a minimum of 2 moves.

🧠 Dry Run Idea

Suppose the Knight starts at:

(4, 3)

BFS first checks all positions that can be reached in 1 move.

Then it checks all unvisited positions that can be reached in 2 moves.

It continues level by level until:

(1, 2)

is reached.

Because BFS processes smaller distances first, the answer is guaranteed to be minimum.

⏱️ Complexity Analysis

Time Complexity

O(N²)

Each cell is visited at most once, and each cell has at most 8 possible moves.

Space Complexity

O(N²)

For the visited matrix and BFS queue.

🔥 Key Concept

Minimum Moves
      ↓
Shortest Path
      ↓
Unweighted Graph
      ↓
BFS

Whenever you need the minimum number of moves and every move has the same cost, think about BFS.

🏷️ Tags

BFS Graph Shortest Path Matrix Knight Chessboard GeeksForGeeks C++
