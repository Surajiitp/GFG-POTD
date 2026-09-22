# Longest Word in Dictionary — GFG

## 📌 Problem

Given a lowercase string `s` and a dictionary `d[]` containing lowercase words, find the **longest word** in the dictionary that can be obtained by deleting some characters from `s` without changing the order of the remaining characters.

If multiple words have the same maximum length, return the **lexicographically smallest** word.

If no valid word exists, return an empty string.

---

## 🧩 Example

### Example 1

```text
Input:
d = ["ale", "apple", "monkey", "plea"]
s = "abpcplea"

Output:
"apple"
```

### Explanation

`"apple"` can be obtained from `"abpcplea"` by deleting some characters while maintaining the order:

```text
abpcplea
↓
apple
```

---

### Example 2

```text
Input:
d = ["a", "b", "c"]
s = "abpcplea"

Output:
"a"
```

All three words can be subsequences, but `"a"` is the lexicographically smallest among the longest valid words.

---

## 💡 Approach

The main task is to check whether every dictionary word is a **subsequence** of `s`.

Since `s` can have up to `5 × 10⁵` characters, repeatedly scanning the complete string for every dictionary word can be inefficient.

To optimize this, we build a **Next Occurrence Table**.

### Next Occurrence Table

```text
next[i][c]
```

stores the first position at or after index `i` where character `c` occurs.

For example:

```text
s = "abpcplea"
```

For every character, we can quickly find its next occurrence.

This allows us to check a dictionary word efficiently.

---

## 🔍 Subsequence Checking

Suppose:

```text
word = "apple"
```

We search for:

```text
a → p → p → l → e
```

using the next occurrence table.

If every character is found in the correct order, the word is a valid subsequence.

---

## 🏆 Choosing the Answer

For every valid word:

```cpp
if (word.size() > ans.size() ||
    (word.size() == ans.size() && word < ans))
```

We update the answer when:

* The current word is longer, or
* Both words have the same length and the current word is lexicographically smaller.

---

## ⏱️ Complexity

Let:

* `N = |s|`
* `D = number of dictionary words`
* `M = maximum length of a dictionary word`

### Time Complexity

Building the next occurrence table:

```text
O(26 × N)
```

Checking all dictionary words:

```text
O(D × M)
```

Overall:

```text
O(26N + DM)
```

Since `26` is constant:

```text
O(N + DM)
```

### Space Complexity

```text
O(26 × N)
```

Effectively:

```text
O(N)
```

---

## 💻 C++ Solution

```cpp
class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        int n = s.size();

        // next[i][c] = first position >= i
        // where character c occurs
        vector<array<int, 26>> next(n + 1);

        for (int c = 0; c < 26; c++)
            next[n][c] = -1;

        for (int i = n - 1; i >= 0; i--) {
            next[i] = next[i + 1];
            next[i][s[i] - 'a'] = i;
        }

        string ans = "";

        for (string &word : d) {
            int pos = 0;
            bool possible = true;

            for (char ch : word) {
                if (pos > n || next[pos][ch - 'a'] == -1) {
                    possible = false;
                    break;
                }

                pos = next[pos][ch - 'a'] + 1;
            }

            if (possible) {
                if (word.size() > ans.size() ||
                    (word.size() == ans.size() && word < ans)) {
                    ans = word;
                }
            }
        }

        return ans;
    }
};
```

---

## 🧠 Key Concept

```text
Dictionary Words
       ↓
Check Subsequence
       ↓
Next Occurrence Table
       ↓
Find Valid Words
       ↓
Longest Word
       ↓
Lexicographically Smallest
```

---

## 📚 Concepts Used

* Strings
* Subsequence
* Greedy Technique
* Next Occurrence Array
* Lexicographical Comparison
* Preprocessing
* Array / Hashing Concepts

---

## 📌 Constraints

```text
1 ≤ |s| ≤ 5 × 10⁵
1 ≤ n ≤ 10⁴
1 ≤ |word| ≤ 100
```

`s` and all dictionary words contain only lowercase English letters.

---

## 🏷️ Tags

`#GFG` `#C++` `#Strings` `#Subsequence` `#Greedy` `#CompetitiveProgramming` `#DSA`

---

## ⭐ Key Takeaway

Instead of scanning the entire string for every character, preprocess the string using a **next occurrence table**.

This makes subsequence checking fast and allows the solution to handle the large value of `|s|` efficiently.
