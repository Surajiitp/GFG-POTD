Subset Sum on Generated Sequence

Problem

There are n children standing in a queue. Each child is assigned a number arr[i].

The teacher initially writes s on a paper and gives it to the first child.

Each child writes:

sum of all numbers already written on the paper + arr[i]

The newly written number is then added to the paper and passed to the next child.

We need to return true if x can be formed by adding some of the numbers written on the paper. Otherwise, return false.

Example 1

Input

arr = [1, 2, 2]
s = 1
x = 7

Generated Sequence

Start with:

1

First child:

1 + 1 = 2

Second child:

1 + 2 + 2 = 5

Third child:

1 + 2 + 5 + 2 = 10

So the sequence is:

[1, 2, 5, 10]

We can form 7 using:

2 + 5 = 7

Therefore:

Output: true

Example 2

Input

arr = [51, 88]
s = 100
x = 500

Generated sequence:

100, 151, 339

Using these numbers, we cannot form 500.

Therefore:

Output: false

Approach

The problem has two main parts:

Generate the sequence.

Check whether x can be formed from the generated numbers.

Step 1: Generate the Sequence

Initially:

current_sum = s

For every arr[i]:

next_value = current_sum + arr[i]

Then add this value to the sequence and update:

current_sum += next_value

We only need values that are <= x.

If:

next_value > x

we can stop because the sequence keeps increasing and such a value cannot be part of a subset that forms x.

Step 2: Find the Subset

After generating the useful sequence, traverse it from the largest value to the smallest.

For every value:

if value <= x:
    x -= value

At the end:

x == 0

means we successfully formed the target.

Otherwise:

x != 0

means it is impossible.

Why Greedy Works

The generated sequence grows very quickly.

For example:

1 → 2 → 5 → 12 → ...

Each new generated value is larger than the sum of all previous generated values when arr[i] is non-negative.

Therefore, when processing from the largest value, if the value is not greater than the remaining target, taking it is safe.

This allows us to avoid a normal O(n*x) subset-sum DP.

Dry Run

Consider:

arr = [1, 2, 2]
s = 1
x = 7

Generate the sequence

Initially:

A = [1]
current_sum = 1

For arr[0] = 1:

next = 1 + 1
     = 2

Now:

A = [1, 2]
current_sum = 3

For arr[1] = 2:

next = 3 + 2
     = 5

Now:

A = [1, 2, 5]
current_sum = 8

For arr[2] = 2:

next = 8 + 2
     = 10

Since:

10 > 7

we stop generating.

Now:

A = [1, 2, 5]
x = 7

Greedy Selection

Start from the largest value:

x = 7

Take 5:

7 - 5 = 2

Take 2:

2 - 2 = 0

Therefore:

return true

C++ Solution

class Solution {
public:
    bool isPossible(vector<int>& arr, int s, int x) {

        vector<long long> A;
        A.push_back(s);

        long long current_sum = s;

        // Step 1: Generate the sequence
        for (int i = 0; i < arr.size(); i++) {

            long long next_val = current_sum + arr[i];

            // Future values will also be greater than x
            if (next_val > x)
                break;

            A.push_back(next_val);
            current_sum += next_val;
        }

        // Step 2: Greedy subset selection
        for (int i = A.size() - 1; i >= 0; i--) {

            if (x >= A[i]) {
                x -= A[i];
            }
        }

        return x == 0;
    }
};

Complexity

Let k be the number of generated values.

Time Complexity

O(n)

We generate the sequence once and then traverse it once.

Space Complexity

O(n)

We store the generated sequence.

Important Point

Use long long because the generated sequence can grow very quickly.

vector<long long> A;
long long current_sum;
long long next_val;

Using int can cause integer overflow for large values.

Simulation Array C++
