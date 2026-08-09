# High Effort vs Low Effort

## Problem

Given two integer arrays `h[]` and `l[]`:

- `h[i]` = number of tasks that can be completed by performing a high-effort task on day `i`.
- `l[i]` = number of tasks that can be completed by performing a low-effort task on day `i`.

For every day, we can choose exactly one:

1. Do no task.
2. Perform a low-effort task.
3. Perform a high-effort task.

A high-effort task can only be performed on the first day or if no task was performed on the previous day.

Return the maximum total number of tasks that can be completed.

---

## Example

### Input

```text
h = [2, 8, 1]
l = [1, 2, 1]


class Solution {
public:
    int maxTask(vector<int>& h, vector<int>& l) {
        int best = 0;
        int noTask = 0;

        for (int i = 0; i < h.size(); i++) {

            int high = noTask + h[i];
            int low = best + l[i];

            int newBest = max({best, high, low});

            // If today we do no task,
            // today's maximum is yesterday's best
            int newNoTask = best;

            best = newBest;
            noTask = newNoTask;
        }

        return best;
    }
};
