# 📦 Box Stacking – Maximum Height

## 📝 Problem Statement

Given `n` boxes where each box has three dimensions:

* `height[i]`
* `width[i]`
* `length[i]`

You can rotate a box into any orientation.

A box can be placed on top of another box only when **both base dimensions of the upper box are strictly smaller** than the corresponding base dimensions of the lower box.

The goal is to find the **maximum possible height** of the stack.

---

## 💡 Approach

This problem can be solved using **Dynamic Programming (DP)**.

### 1. Generate All Orientations

Every box can be rotated in `6` different orientations.

For a box with dimensions:

```text
a = height[i]
b = width[i]
c = length[i]
```

we generate:

```text
(a, b, c)
(a, c, b)
(b, a, c)
(b, c, a)
(c, a, b)
(c, b, a)
```

Each orientation is stored as:

```text
{base_length, base_width, height}
```

---

### 2. Sort the Boxes

All generated orientations are sorted in **decreasing order of base dimensions**.

Primary sorting is done using the first base dimension, and if they are equal, the second base dimension is used.

This makes it easier to determine whether one box can be placed on another.

---

### 3. Dynamic Programming

Define:

```text
dp[i] = maximum height of a stack when box i is the bottom box
```

Initially:

```cpp
dp[i] = boxes[i][2];
```

For every box `j` after `i`, check whether `j` can be placed on `i`:

```cpp
boxes[i][0] > boxes[j][0] &&
boxes[i][1] > boxes[j][1]
```

If possible:

```cpp
dp[i] = max(dp[i],
            boxes[i][2] + dp[j]);
```

Finally:

```cpp
answer = max(dp[i])
```

---

## 🔄 Example

Suppose a box has dimensions:

```text
Height = 4
Width  = 3
Length = 2
```

Its possible orientations include:

```text
4 × 3 × 2
4 × 2 × 3
3 × 4 × 2
3 × 2 × 4
2 × 4 × 3
2 × 3 × 4
```

The algorithm considers all orientations and finds the combination that produces the greatest total height.

---

## 💻 C++ Solution

```cpp
class Solution {
public:
    int maxHeight(vector<int>& height, vector<int>& width,
                  vector<int>& length) {

        int n = height.size();

        // Store all 6 orientations
        // {length, width, height}
        vector<vector<int>> boxes;
        boxes.reserve(n * 6);

        for (int i = 0; i < n; i++) {

            int a = height[i];
            int b = width[i];
            int c = length[i];

            boxes.push_back({a, b, c});
            boxes.push_back({a, c, b});

            boxes.push_back({b, a, c});
            boxes.push_back({b, c, a});

            boxes.push_back({c, a, b});
            boxes.push_back({c, b, a});
        }

        int m = boxes.size();

        // Sort by base dimensions in decreasing order
        sort(boxes.begin(), boxes.end(),
             [](const vector<int>& a, const vector<int>& b) {

                 if (a[0] == b[0])
                     return a[1] > b[1];

                 return a[0] > b[0];
             });

        // dp[i] = maximum height with box i as bottom
        vector<int> dp(m);

        int ans = 0;

        for (int i = m - 1; i >= 0; i--) {

            dp[i] = boxes[i][2];

            for (int j = i + 1; j < m; j++) {

                // j can be placed on i
                if (boxes[i][0] > boxes[j][0] &&
                    boxes[i][1] > boxes[j][1]) {

                    dp[i] = max(dp[i],
                                boxes[i][2] + dp[j]);
                }
            }

            ans = max(ans, dp[i]);
        }

        return ans;
    }
};
```

---

## ⏱️ Complexity Analysis

Let `n` be the number of boxes.

Each box generates `6` orientations:

```text
m = 6n
```

### Time Complexity

Sorting:

```text
O(6n log(6n)) = O(n log n)
```

DP:

```text
O((6n)²) = O(n²)
```

Therefore:

```text
Overall: O(n²)
```

### Space Complexity

We store `6n` orientations and the DP array:

```text
O(n)
```

---

## 🧠 Key Concepts

* Dynamic Programming
* Box Stacking
* DP on Sorted States
* Permutations / Rotations
* Longest Increasing Subsequence Pattern
* Sorting
* 2D Base-Dimension Comparison

---

## 🔑 Important Condition

For a box `j` to be placed on top of box `i`:

```cpp
boxes[i][0] > boxes[j][0] &&
boxes[i][1] > boxes[j][1]
```

Both dimensions must be **strictly smaller**.

This prevents boxes with equal base dimensions from being stacked.

---

## 📌 Algorithm Summary

```text
Input
  ↓
Generate 6 orientations for every box
  ↓
Store {base_length, base_width, height}
  ↓
Sort orientations by decreasing base dimensions
  ↓
Apply Dynamic Programming
  ↓
Check if smaller box can be placed on larger box
  ↓
Calculate maximum stack height
  ↓
Return maximum height
```

---

## 🚀 Takeaway

The key idea is to convert the **3D box stacking problem** into a **2D DP problem** by generating every possible orientation of each box.

After sorting the orientations, the problem becomes similar to finding a maximum-weight increasing/decreasing sequence, where the **weight is the height of each box**.

---

## 👨‍💻 Author

**Suraj Kumar**

* 🎓 IIT Patna – Computer Science
* 💻 C++ | DSA | MERN Stack
* 🧩 Competitive Programming
