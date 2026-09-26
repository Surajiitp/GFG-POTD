# [Minimum Cost Pizza Selection](https://www.geeksforgeeks.org/problems/pizza-mania0155/1)

**Difficulty:** Medium | **Accuracy:** 64.66% | **Points:** 4

---

## 📋 Problem Statement

Given the area of Small, Medium, and Large pizzas as `s`, `m`, and `l` units, and their respective costs as `cs`, `cm`, and `cl`, find the minimum amount of money required to buy pizzas whose total area is **at least** `x`. You may buy any number of pizzas of each type.

---

## 💡 Examples

**Example 1:**
```text
Input: x = 16, s = 3, m = 6, l = 9, cs = 50, cm = 150, cl = 300
Output: 300
Explanation: We want at least 16 sq. units of Pizza.
- One unit of each s, m and l = 3 + 6 + 9 = 18 sq units, Cost = 500.
- 6 units of s = 18 sq units, Cost = 300.
- 2 units of l = 18 sq units, Cost = 600, etc.
Of all the arrangements, the minimum cost is Rs. 300.
```

**Example 2:**
```text
Input: x = 10, s = 1, m = 3, l = 10, cs = 10, cm = 20, cl = 50
Output: 50
Explanation: Of all the arrangements possible, the minimum cost is Rs. 50.
```

---

## 🔒 Constraints

* `1 ≤ x ≤ 500`
* `1 ≤ s ≤ m ≤ l ≤ 100`
* `1 ≤ cs ≤ cm ≤ cl ≤ 100`

---

## 🧠 Approach (Dynamic Programming)

This problem is a variation of the **Unbounded Knapsack / Coin Change** problem, where we have an unlimited supply of three pizza sizes and want to find the minimum cost to cover an area of **at least** `x`.

### Step-by-Step Logic:
1. **State Definition:**  
   Create a 1D array `dp` of size `x + 1`, where `dp[i]` stores the minimum cost required to get **at least** `i` square units of pizza.
2. **Base Case:**  
   `dp[0] = 0`, because `0` cost is needed to obtain `0` units of pizza.
3. **Transitions:**  
   Iterate `i` from `1` to `x`. For each required area `i`, evaluate all three choices:
   * **Buy Small (`s`):** Cost is `cs + dp[max(0, i - s)]`
   * **Buy Medium (`m`):** Cost is `cm + dp[max(0, i - m)]`
   * **Buy Large (`l`):** Cost is `cl + dp[max(0, i - l)]`
   
   *(Note: We use `max(0, i - size)` because if a single pizza's area exceeds `i`, the remaining area required is `0`, not negative.)*
4. **Optimal Substructure:**  
   `dp[i] = min({buySmall, buyMedium, buyLarge})`
5. **Final Answer:**  
   `dp[x]` holds the minimum cost to achieve at least `x` units of pizza.

---

## 💻 C++ Solution

```cpp
#include <vector>
#include <algorithm>

class Solution {
  public:
    int minimumCost(int x, int s, int m, int l, int cs, int cm, int cl) {
        // dp[i] stores the minimum cost to get at least 'i' units of pizza
        std::vector<int> dp(x + 1, 0);
        
        for (int i = 1; i <= x; i++) {
            int buySmall  = cs + dp[std::max(0, i - s)];
            int buyMedium = cm + dp[std::max(0, i - m)];
            int buyLarge  = cl + dp[std::max(0, i - l)];
            
            dp[i] = std::min({buySmall, buyMedium, buyLarge});
        }
        
        return dp[x];
    }
};
```

---

## ⏱️ Time & Space Complexity

| Metric | Complexity | Explanation |
| :--- | :--- | :--- |
| **Time Complexity** | **$\mathcal{O}(x)$** | The loop runs from `1` to `x`. In each iteration, we perform 3 constant-time $\mathcal{O}(1)$ lookups and comparisons. Given `x ≤ 500`, this executes in well under a millisecond. |
| **Auxiliary Space** | **$\mathcal{O}(x)$** | We allocate a `dp` array of size `x + 1` to store intermediate minimum costs for areas `0` through `x`. |
