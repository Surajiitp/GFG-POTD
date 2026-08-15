Here is the updated Markdown text complete with a **C++ Solution** section added at the end. You can replace the text in your editor with this block:

```markdown
# Numbers Without d as Digit

## Problem Statement
Given a number `n`, count the numbers from `1` to `n` that do not contain the digit `d` in their decimal representation.

---

## Examples

### Example 1
- **Input:** `n = 25`, `d = 3`
- **Output:** `22`
- **Explanation:** From `1` to `25`, the numbers containing the digit `3` are `3`, `13`, `23`.  
  Therefore: $25 - 3 = 22$.

### Example 2
- **Input:** `n = 5`, `d = 3`
- **Output:** `4`
- **Explanation:** Only `3` contains the digit `3`, so the valid count is `4`.

---

## Approach

This problem can be solved efficiently using **Digit DP / Combinatorics** instead of checking every number individually from `1` to `n`.

### Step 1: Count numbers with fewer digits than `n`
Suppose `n` has `len` digits. For a number with `i` digits (`i < len`):
* The first digit cannot be `0`.
  * If `d != 0`, there are **8** valid choices for the first digit (1–9, excluding `d`).
  * If `d == 0`, there are **9** valid choices for the first digit (1–9).
* Every remaining digit has **9** valid choices (0–9, excluding `d`).

### Step 2: Count valid numbers with the same length as `n`
Convert `n` to a string and process the digits from left to right:
1. At position `idx`, iterate through all valid digits smaller than the digit at `n[idx]`.
2. For each smaller digit, multiply by $9^{\text{remaining\_positions}}$.
3. If the current digit of `n` itself equals `d`, stop the process because no valid numbers can prefix with this branch.

---

## C++ Implementation

```cpp
#include <iostream>
#include <string>
#include <cmath>

using namespace std;

class Solution {
public:
    int countValidNumbers(int n, int d) {
        string s = to_string(n);
        int len = s.length();
        int total = 0;

        // Step 1: Count valid numbers with fewer digits than n
        for (int i = 1; i < len; ++i) {
            int firstChoices = (d == 0) ? 9 : 8;
            total += firstChoices * pow(9, i - 1);
        }

        // Step 2: Count valid numbers with the same length as n
        for (int i = 0; i < len; ++i) {
            int currentDigit = s[i] - '0';
            int start = (i == 0) ? 1 : 0; // First digit cannot be 0

            for (int digit = start; digit < currentDigit; ++digit) {
                if (digit == d) continue;
                total += pow(9, len - 1 - i);
            }

            // If current digit itself is 'd', stop branching further
            if (currentDigit == d) {
                return total;
            }
        }

        // Include n itself if it doesn't contain d
        total += 1;

        return total;
    }
};

```

---

## Complexity Analysis

| Type | Complexity |
| --- | --- |
| **Time Complexity** | $\mathcal{O}(\log_{10} n)$ — proportional to the number of digits in `n`. |
| **Space Complexity** | $\mathcal{O}(1)$ auxiliary space. |

```

```
