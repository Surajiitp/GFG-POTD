# Triplets with Sum in Range

## Problem

Given an array `arr[]` and a range `[l, r]`, count the number of triplets whose sum lies within the range:

```text
l <= arr[i] + arr[j] + arr[k] <= r
```

where `i < j < k`.

---

## Approach

We can convert the range condition into two simpler conditions:

```text
sum <= r
```

and

```text
sum <= l - 1
```

Therefore:

```text
Answer = count(sum <= r) - count(sum <= l - 1)
```

To efficiently count triplets with sum `<= x`, we use **Sorting + Two Pointers**.

### Steps

1. Sort the array.
2. Fix the first element using index `i`.
3. Use two pointers:

   * `left = i + 1`
   * `right = n - 1`
4. Calculate:

```text
arr[i] + arr[left] + arr[right]
```

5. If the sum is `<= x`:

   * Because the array is sorted, every element between `left` and `right` will also form a valid triplet.
   * So add:

```text
right - left
```

* Then increment `left`.

6. Otherwise, decrease `right`.

Finally:

```text
countTriplets = countLessEqual(r) - countLessEqual(l - 1)
```

---

## Why `right - left`?

Suppose:

```text
arr[i] + arr[left] + arr[right] <= x
```

Since the array is sorted:

```text
arr[left] <= arr[left+1] <= ... <= arr[right]
```

So all these triplets are valid:

```text
(i, left, left+1)
(i, left, left+2)
...
(i, left, right)
```

Number of such triplets:

```text
right - left
```

Hence:

```cpp
count += right - left;
```

---

## C++ Solution

```cpp
class Solution {
public:

    long long countLessEqual(vector<int>& arr, int x) {
        int n = arr.size();
        long long count = 0;

        for(int i = 0; i < n - 2; i++) {

            int left = i + 1;
            int right = n - 1;

            while(left < right) {

                long long sum = 1LL * arr[i]
                              + arr[left]
                              + arr[right];

                if(sum <= x) {
                    count += (right - left);
                    left++;
                }
                else {
                    right--;
                }
            }
        }

        return count;
    }

    int countTriplets(vector<int> &arr, int l, int r) {

        sort(arr.begin(), arr.end());

        return countLessEqual(arr, r)
             - countLessEqual(arr, l - 1);
    }
};
```

---

## Example

### Input

```text
arr = [8, 3, 5, 2]
l = 7
r = 11
```

After sorting:

```text
[2, 3, 5, 8]
```

Valid triplet:

```text
2 + 3 + 5 = 10
```

Since:

```text
7 <= 10 <= 11
```

Answer:

```text
1
```

---

## Second Example

```text
arr = [5, 1, 4, 3, 2]
l = 2
r = 7
```

Sorted array:

```text
[1, 2, 3, 4, 5]
```

Valid triplets:

```text
1 + 4 + 2 = 7
1 + 3 + 2 = 6
```

Therefore:

```text
Answer = 2
```

---

## Complexity

### Time Complexity

```text
Sorting:       O(n log n)
Two Pointers:  O(n²)

Overall:       O(n²)
```

### Space Complexity

```text
O(1) extra space
```

Apart from the sorting implementation's internal space.

---

## Key Concept

The main trick is:

```text
Count(sum in [l, r])
=
Count(sum <= r) - Count(sum <= l - 1)
```

Then use **Sorting + Two Pointers** to calculate each `Count(sum <= x)` in `O(n²)`.

### Tags

`Array` `Sorting` `Two Pointers` `Triplets` `Counting`
