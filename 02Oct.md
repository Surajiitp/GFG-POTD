# 🔄 Lexicographically Smallest Rotation

## 📝 Problem

Given a string `s`, find the **lexicographically smallest string** that can be obtained by rotating `s` to the left any number of times, including `0`.

A left rotation moves the first character to the end of the string.

### Example 1

```text
Input:
s = "abcd"

Output:
"abcd"
```

Rotations:

```text
abcd
bcda
cdab
dabc
```

The lexicographically smallest string is:

```text
abcd
```

### Example 2

```text
Input:
s = "baca"

Output:
"abac"
```

Rotations:

```text
baca
acab
caba
abac
```

The lexicographically smallest rotation is:

```text
abac
```

---

## 💡 Approach — Booth's Algorithm

Since:

```text
1 ≤ s.length ≤ 10^6
```

we need an efficient algorithm.

A brute-force approach would generate every rotation and compare them, which can take:

```text
O(n²)
```

This is too slow for `n = 10^6`.

Instead, we use **Booth's Algorithm**, which finds the lexicographically smallest rotation in:

```text
O(n)
```

---

## 🚀 Key Idea

Create:

```cpp
string t = s + s;
```

For example:

```text
s = "baca"

t = "bacabaca"
```

Every rotation of `s` appears as a substring of length `n` inside `t`.

```text
baca → t[0...3]
acab → t[1...4]
caba → t[2...5]
abac → t[3...6]
```

Therefore, the problem becomes:

> Find the starting index of the lexicographically smallest length-`n` substring in `s + s`.

---

## 🔍 Booth's Algorithm

We maintain three pointers:

```text
i → first candidate rotation
j → second candidate rotation
k → number of matched characters
```

Initially:

```cpp
int i = 0;
int j = 1;
int k = 0;
```

We compare:

```cpp
t[i + k]
```

with:

```cpp
t[j + k]
```

### Case 1: Characters are equal

If:

```cpp
t[i + k] == t[j + k]
```

continue comparing:

```cpp
k++;
```

---

### Case 2: Rotation `i` is larger

If:

```cpp
t[i + k] > t[j + k]
```

then rotation starting at `i` cannot be the answer.

We skip the entire matched section:

```cpp
i = i + k + 1;
```

To ensure the two candidates remain different:

```cpp
if (i <= j)
    i = j + 1;
```

Then restart comparison:

```cpp
k = 0;
```

---

### Case 3: Rotation `j` is larger

If:

```cpp
t[i + k] < t[j + k]
```

then rotation starting at `j` cannot be the answer.

So:

```cpp
j = j + k + 1;
```

And:

```cpp
if (j <= i)
    j = i + 1;
```

Then:

```cpp
k = 0;
```

---

## 🧠 Why Does This Work?

Suppose two candidate rotations match for several characters:

```text
Rotation i:
abcdef...

Rotation j:
abcxef...
```

At the first difference:

```text
d < x
```

so the rotation beginning at `j` is definitely larger.

There is no need to compare all the intermediate starting positions again.

Booth's Algorithm uses this observation to **skip impossible candidates**, making the algorithm linear.

---

## 💻 C++ Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    string lexiString(string &s) {

        int n = s.length();

        // Duplicate the string so every rotation
        // becomes a substring of length n.
        string t = s + s;

        int i = 0;
        int j = 1;
        int k = 0;

        while (i < n && j < n && k < n) {

            int t1 = i + k;
            int t2 = j + k;

            // Characters are equal
            if (t[t1] == t[t2]) {
                k++;
            }

            // Rotation starting at i is larger
            else if (t[t1] > t[t2]) {

                i = i + k + 1;

                if (i <= j) {
                    i = j + 1;
                }

                k = 0;
            }

            // Rotation starting at j is larger
            else {

                j = j + k + 1;

                if (j <= i) {
                    j = i + 1;
                }

                k = 0;
            }
        }

        // The smaller candidate is the answer
        int start = min(i, j);

        // Construct the lexicographically smallest rotation
        return s.substr(start, n) + s.substr(0, start);
    }
};
```

---

## 🔎 Dry Run

For:

```text
s = "baca"
```

Create:

```text
t = "bacabaca"
```

Possible rotations:

```text
Start 0 → baca
Start 1 → acab
Start 2 → caba
Start 3 → abac
```

Among these:

```text
abac < acab < baca < caba
```

So the smallest rotation starts at index:

```text
3
```

The answer is:

```text
abac
```

---

## ⏱️ Complexity Analysis

Let `n = s.length()`.

### Time Complexity

Booth's Algorithm runs in:

```text
O(n)
```

Each candidate is eliminated efficiently, so we avoid comparing all rotations independently.

### Space Complexity

We create:

```cpp
string t = s + s;
```

which requires:

```text
O(n)
```

additional space.

Therefore:

```text
Time:  O(n)
Space: O(n)
```

---

## 📌 Important Interview Insight

A brute-force solution would be:

```text
Generate all n rotations
        ↓
Compare each rotation
        ↓
Find minimum
```

This can become:

```text
O(n²)
```

For:

```text
n = 10^6
```

that is too expensive.

Booth's Algorithm reduces it to:

```text
O(n)
```

### Pattern to Remember

```text
Lexicographically smallest rotation
              ↓
       Booth's Algorithm
              ↓
           O(n)
```

---

## 🔑 Key Concepts

* String Rotation
* Lexicographical Comparison
* Booth's Algorithm
* String Matching
* Two Candidates
* Optimization
* Linear Time Algorithm

---

## 📚 Complexity Summary

| Approach          |     Time | Space |
| ----------------- | -------: | ----: |
| Brute Force       |    O(n²) |  O(n) |
| Booth's Algorithm | **O(n)** |  O(n) |

---

## 🔗 Problem

**Problem:** Lexicographically Smallest Rotation

**Difficulty:** Hard

**Language:** C++

**Algorithm:** Booth's Algorithm

**Complexity:** `O(n)` Time, `O(n)` Space
