Marks from Ranks

🟢 Question

Given two arrays "l[]" and "r[]", where each pair "[l[i], r[i]]" represents an interval of consecutive valid marks.

The intervals:

- Are sorted in increasing order.
- Do not overlap.
- Contain all valid marks.

The rank of a mark is its position among all valid marks in increasing order.

Given an array "rank[]", find the corresponding mark for every rank.

Example

Input:
l = [1, 6, 14]
r = [3, 9, 15]
rank = [2, 5, 8]

Valid marks:
1, 2, 3, 6, 7, 8, 9, 14, 15

Output:
[2, 7, 14]

---

💡 Approach

We need to find the actual mark corresponding to each given rank.

Each interval "[l[i], r[i]]" contains:

length = r[i] - l[i] + 1

valid marks.

For the given example:

[1, 3]   → 3 marks
[6, 9]   → 4 marks
[14, 15] → 2 marks

We create a prefix sum array containing the total number of valid marks up to each interval.

prefix = [3, 7, 9]

This means:

- Ranks "1 - 3" belong to "[1, 3]"
- Ranks "4 - 7" belong to "[6, 9]"
- Ranks "8 - 9" belong to "[14, 15]"

For every rank, use binary search to find the first prefix value that is greater than or equal to that rank.

Then calculate the position of the required mark inside that interval.

Formula

If "previous" is the number of valid marks before the current interval:

position = rank - previous

Since the position is 1-indexed:

mark = l[i] + position - 1

---

🔍 Dry Run

For:

l = [1, 6, 14]
r = [3, 9, 15]
rank = [2, 5, 8]

Step 1: Build Prefix Array

[1, 3] → length = 3
[6, 9] → length = 4
[14,15] → length = 2

Therefore:

prefix = [3, 7, 9]

---

Step 2: Rank = 2

First prefix value ">= 2" is "3".

So the rank belongs to:

[1, 3]

previous = 0
position = 2 - 0 = 2

mark = 1 + 2 - 1
     = 2

Answer:

2

---

Step 3: Rank = 5

First prefix value ">= 5" is "7".

So the rank belongs to:

[6, 9]

previous = 3
position = 5 - 3 = 2

mark = 6 + 2 - 1
     = 7

Answer:

7

---

Step 4: Rank = 8

First prefix value ">= 8" is "9".

So the rank belongs to:

[14, 15]

previous = 7
position = 8 - 7 = 1

mark = 14 + 1 - 1
     = 14

Answer:

14

Therefore:

Output = [2, 7, 14]

---

🚀 C++ Solution

class Solution {
public:
    vector<int> getMarks(vector<int>& l, vector<int>& r, vector<int>& rank) {
        int n = l.size();

        // prefix[i] = total number of valid marks
        // from interval 0 to i
        vector<long long> prefix(n);

        for (int i = 0; i < n; i++) {
            long long len = r[i] - l[i] + 1;

            if (i == 0)
                prefix[i] = len;
            else
                prefix[i] = prefix[i - 1] + len;
        }

        vector<int> ans;

        for (long long k : rank) {

            // Find first interval whose cumulative count >= k
            int i = lower_bound(prefix.begin(), prefix.end(), k)
                    - prefix.begin();

            // Number of valid marks before this interval
            long long previous = (i == 0 ? 0 : prefix[i - 1]);

            // Position inside current interval
            long long position = k - previous;

            // Calculate actual mark
            long long mark = l[i] + position - 1;

            ans.push_back((int)mark);
        }

        return ans;
    }
};

---

⏱️ Complexity Analysis

Let:

- "n" = number of intervals
- "m" = number of ranks

Time Complexity

Building the prefix array:

O(n)

For each rank, binary search takes:

O(log n)

For all ranks:

O(m log n)

Therefore, total:

O(n + m log n)

Space Complexity

The prefix array requires:

O(n)

The answer array requires:

O(m)

Total:

O(n + m)

---

⭐ Key Takeaway

The main idea is to convert the intervals into cumulative rank ranges.

Intervals → Prefix Counts → Binary Search → Actual Mark

Using "lower_bound()" allows us to efficiently find which interval contains a given rank.

Important Formula

mark = l[i] + (rank - previous) - 1

This gives the actual mark corresponding to the required rank.
