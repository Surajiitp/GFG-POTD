# Secret Cipher

## Problem

Given an original string `s`, Geek wants to encrypt it by inserting `*`.

The decoding rules are:

* A normal character is appended to the original string.
* When `*` is encountered, it is replaced by **all characters before it** in the encoded string.
* The goal is to find the **lexicographically smallest encrypted string** that decodes to `s`.

### Example 1

```text
Input:
s = "ababcababcd"

Output:
ab*c*d
```

### Example 2

```text
Input:
s = "zzzzzzz"

Output:
z*z*z
```

---

## Approach

We use:

* **Z-Algorithm** to efficiently check whether two parts of the string are equal.
* **Greedy approach from right to left** to obtain the smallest valid encrypted string.

### Key Observation

Suppose the current prefix has an even length:

```text
2 * k
```

If:

```text
s[0 ... k-1] == s[k ... 2k-1]
```

then the second half is an exact copy of the first half.

Therefore, instead of keeping both halves, we can represent them using:

```text
first_half + "*"
```

For example:

```text
zzzz
```

can be represented as:

```text
z*
```

because `*` duplicates everything before it.

---

## Why Z-Algorithm?

For every possible split, we need to check:

```text
s[0 ... k-1] == s[k ... 2k-1]
```

Doing this character by character could take `O(n²)`.

The Z-array gives us this information efficiently.

### Z-array

`Z[i]` stores the length of the longest substring starting at index `i` that matches the prefix of `s`.

For example:

```text
s = "aaaaaa"

Z[1] = 5
Z[2] = 4
Z[3] = 3
...
```

Therefore, for a prefix of length `2 * k`, we check:

```cpp
Z[k] >= k
```

If true, the two halves are identical.

---

## Greedy Strategy

We process the string **from right to left**.

For the current index `i`:

```cpp
int mid = i / 2;
```

If `i + 1` is even, we check whether:

```cpp
Z[mid + 1] >= mid + 1
```

If true:

```cpp
ans += '*';
i = mid;
```

Otherwise, we simply keep the current character:

```cpp
ans += s[i];
i--;
```

Finally, since we constructed the answer backwards, we reverse it.

---

## C++ Solution

```cpp
class Solution {
public:
    string compress(string& s) {
        int n = s.size();

        if (n == 0)
            return "";

        // Step 1: Build the Z-array
        // Z[i] = length of the longest substring
        // starting at i which matches the prefix of s.

        vector<int> Z(n, 0);

        int l = 0, r = 0;

        for (int i = 1; i < n; i++) {

            if (i <= r) {
                Z[i] = min(r - i + 1, Z[i - l]);
            }

            while (i + Z[i] < n &&
                   s[Z[i]] == s[i + Z[i]]) {
                Z[i]++;
            }

            if (i + Z[i] - 1 > r) {
                l = i;
                r = i + Z[i] - 1;
            }
        }

        // Step 2: Process from right to left
        string ans = "";

        int i = n - 1;

        while (i >= 0) {

            // Current prefix length is i + 1.
            // It can be compressed only if its length is even.

            if (i % 2 == 1) {

                int mid = i / 2;

                // Check:
                // s[0 ... mid] == s[mid + 1 ... i]

                if (Z[mid + 1] >= mid + 1) {

                    ans += '*';

                    // The first half is already represented.
                    i = mid;

                    continue;
                }
            }

            // Cannot compress
            ans += s[i];
            i--;
        }

        // We processed from right to left
        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```

---

## Dry Run

Consider:

```text
s = "zzzzzzz"
```

We process from the end.

### Step 1

Current prefix:

```text
zzzz
```

It consists of two identical halves:

```text
zz | zz
```

So:

```text
zzzz → zz*
```

### Step 2

The remaining prefix:

```text
zz
```

Again:

```text
z | z
```

So:

```text
zz → z*
```

### Step 3

The remaining character:

```text
z
```

cannot be compressed.

Final answer:

```text
z*z*z
```

---

## Example

### Input

```text
ababcababcd
```

The optimal encryption is:

```text
ab*c*d
```

The `*` operations duplicate the characters before them and reconstruct the original string during decoding.

---

## Complexity

### Time Complexity

```text
O(n)
```

The Z-array is constructed in linear time, and the final traversal is also `O(n)`.

### Space Complexity

```text
O(n)
```

for the Z-array and answer string.

---

## Important Code Logic

The most important condition is:

```cpp
if (i % 2 == 1) {
    int mid = i / 2;

    if (Z[mid + 1] >= mid + 1) {
        ans += '*';
        i = mid;
        continue;
    }
}
```

### Meaning

* `i % 2 == 1` → current prefix length is even.
* `mid + 1` → length of each half.
* `Z[mid + 1] >= mid + 1` → both halves are identical.
* `ans += '*'` → replace the second half using `*`.
* `i = mid` → continue processing the first half.

---

## Key Takeaways

* Use **Z-Algorithm** for fast prefix matching.
* Process the string **right to left**.
* Only an **even-length prefix** can be divided into two equal halves.
* `Z[mid + 1] >= mid + 1` verifies that both halves are equal.
* If equal, replace the second half with `*`.
* Reverse the constructed answer at the end.
* Overall complexity is **O(n)**.

### Tags

`Greedy` `Strings` `Z-Algorithm` `Pattern Matching` `String Compression` `GeeksForGeeks`
