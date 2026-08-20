# Transform String

## Problem

Given two strings `s1` and `s2`, find the minimum number of steps required to transform `s1` into `s2`.

The only allowed operation is:

* Select any character from `s1`.
* Remove it from its current position.
* Insert it at the **beginning** of `s1`.

If the transformation is not possible, return `-1`.

---

## Examples

### Example 1

```text
Input:
s1 = "abd"
s2 = "bad"

Output:
1
```

**Explanation:**

Move `'b'` to the front:

```text
abd → bad
```

So the minimum number of operations is `1`.

---

### Example 2

```text
Input:
s1 = "GeeksForGeeks"
s2 = "ForGeeksGeeks"

Output:
3
```

**Explanation:**

Move the required characters to the front:

```text
GeeksForGeeks
rGeeksFoGeeks
orGeeksFGeeks
ForGeeksGeeks
```

Therefore, the answer is `3`.

---

## Approach

The key observation is that characters which are **not moved** keep their relative order.

Since every operation moves one character to the front, we try to keep the **longest suffix of `s2`** that can already appear as a subsequence of `s1`.

We compare both strings from **right to left**:

1. First, check whether `s1` and `s2` contain exactly the same characters.
2. Start from the last character of both strings.
3. If `s1[i] == s2[j]`, keep that character and move both pointers.
4. Otherwise, move only the pointer of `s1`.
5. After matching the maximum possible suffix, the remaining prefix of `s2` contains the characters that need to be moved to the front.
6. The number of required operations is `j + 1`.

If the character frequencies are different, transformation is impossible, so return `-1`.

---

## Algorithm

```text
1. Let n = length of s1.
2. If lengths are different, return -1.
3. Count the frequency of every character in s1.
4. Subtract the frequency of every character in s2.
5. If any frequency is non-zero, return -1.
6. Set i = n - 1 and j = n - 1.
7. While i >= 0 and j >= 0:
      If s1[i] == s2[j]:
          Decrease j
      Decrease i
8. Return j + 1.
```

---

## C++ Solution

```cpp
class Solution {
public:
    int transform(string &s1, string &s2) {
        int n = s1.size();

        if (n != s2.size())
            return -1;

        // Check both strings have same characters
        int freq[256] = {0};

        for (char c : s1)
            freq[c]++;

        for (char c : s2)
            freq[c]--;

        for (int i = 0; i < 256; i++) {
            if (freq[i] != 0)
                return -1;
        }

        // Find longest suffix of s2
        // which is a subsequence of s1
        int i = n - 1;
        int j = n - 1;

        while (i >= 0 && j >= 0) {
            if (s1[i] == s2[j]) {
                j--;
            }

            i--;
        }

        // s2[0...j] characters need to be moved
        return j + 1;
    }
};
```

---

## Dry Run

For:

```text
s1 = "bcad"
s2 = "abcd"
```

Compare from the end:

```text
s1: b c a d
         ↑
s2: a b c d
         ↑
```

`d == d` → match

Then:

```text
s1: b c a d
       ↑
s2: a b c d
       ↑
```

`a != c` → skip `a` in `s1`

Then:

```text
s1: b c a d
     ↑
s2: a b c d
     ↑
```

`c == c` → match

Then:

```text
s1: b c a d
   ↑
s2: a b c d
 ↑
```

`b == b` → match

Only `'a'` needs to be moved to the front:

```text
bcad → abcd
```

Answer:

```text
1
```

---

## Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)` because the frequency array has a fixed size of `256`.

---

## Key Concept

> Keep the longest possible suffix of `s2` that can be obtained from `s1` without moving characters. Every remaining character must be moved to the front.

---

## Related Concepts

* Greedy
* Two Pointers
* String Manipulation
* Frequency Counting
* Subsequence
* Character Frequency
