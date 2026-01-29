# Lecture 3: Asymptotic Notation, Divide & Conquer, Recurrence Relations

## Asymptotic Notation Review

Asymptotic notation provides a way to compare the **growth rates** of functions, which is essential for analyzing algorithm running times. Just as we compare numbers using relational operators (≤, ≥, =, <, >), we compare function growth rates using asymptotic notations.

### Comparing Functions vs. Numbers

**For numbers:**
- `x ≤ 10`: x is at most 10
- `x ≥ 10`: x is at least 10
- `x ≤ 6` and `x ≥ 6` implies `x = 6`
- `x < 5`: x is strictly less than 5

**For functions (growth rates):**
- `f(n) = O(g(n))`: f grows at most as fast as g (similar to ≤)
- `f(n) = Ω(g(n))`: f grows at least as fast as g (similar to ≥)
- `f(n) = Θ(g(n))`: f grows at the same rate as g (similar to =)
- `f(n) = o(g(n))`: f grows strictly slower than g (similar to <)
- `f(n) = ω(g(n))`: f grows strictly faster than g (similar to >)

### Formal Relationships

If `f(n) = O(g(n))` and `f(n) = Ω(g(n))`, then `f(n) = Θ(g(n))`.  
This is analogous to: if `x ≤ 6` and `x ≥ 6`, then `x = 6`.

### Strict Comparisons

- `2n² + 1 = o(n³)` because `2n² + 1` grows strictly slower than `n³`
- `½n⁵ + n - 1000 = ω(n²)` because it grows strictly faster than `n²`

**Check these:**
- Is `100n³ + 4n² + 2 = o(n⁴)`? Yes, because the growth rate is strictly less.
- Is `100n³ + 4n² + 2 = ω(n³)`? No, because they have the same growth rate (it's Θ(n³), not ω(n³)).

## Comparing Growth Rates Using Limits

We can compare function growth rates using limits:

- If `lim[n→∞] f(n)/g(n) ≤ c₁` where `c₁ > 0`, then `f(n) = O(g(n))`
- If `lim[n→∞] f(n)/g(n) ≥ c₂` where `c₂ > 0`, then `f(n) = Ω(g(n))`
- If `c₂ ≤ lim[n→∞] f(n)/g(n) ≤ c₁` where `c₁, c₂ > 0`, then `f(n) = Θ(g(n))`

**Note**: This is analogous to comparing numbers:
- If `x/y ≤ 1`, then `x ≤ y`
- If `x/y ≥ 1`, then `x ≥ y`
- If `1 ≤ x/y ≤ 1` (which means `x/y = 1`), then `x = y`

## Divide and Conquer Paradigm

**Divide and Conquer** is a fundamental algorithm design paradigm that involves:

1. **Divide**: Break the problem into smaller subproblems
2. **Conquer**: Solve the subproblems recursively
3. **Combine**: Merge the solutions to solve the original problem

### Example: Merge Sort

1. **Divide**: Split the n-element array into two subarrays of roughly n/2 elements each
2. **Conquer**: Recursively sort the two subarrays using merge sort
3. **Combine**: Merge the two sorted subarrays to produce the final sorted array

## Recurrence Relations

**Recurrence relations** are equations that define functions in terms of their values on smaller inputs. They're used to characterize the running times of recursive algorithms.

### Examples of Recurrences

- `T(n) = T(n/2) + c`
- `T(n) = 3T(n/2) + cn²`

### Recurrence for Merge Sort

For Merge Sort, the recurrence is:
```
T(n) = { c               if n = 1
         2T(n/2) + cn    if n > 1 }
```

## Solving Recurrences: Iteration Method

The **iteration method** involves repeatedly substituting the recurrence relation until a pattern emerges.

### Example 1: `T(n) = T(n/2) + c`

Assume `n = 2^k` for simplicity:

```
T(n) = T(n/2) + c
     = T(n/4) + c + c
     = T(n/8) + c + c + c
     ...
     = T(1) + c + c + ... + c  [k times]
     = c' + c·k
     = c' + c·log₂n
     = O(log n)
```

**Explanation**: We can divide n by 2 exactly k = log₂n times before reaching 1.

### Example 2: `T(n) = 2T(n/2) + cn`

```
T(n) = cn + 2T(n/2)
     = cn + 2(c(n/2) + 2T(n/4))
     = cn + cn + 4T(n/4)
     = cn + cn + 4(c(n/4) + 2T(n/8))
     = 3cn + 8T(n/8)
     ...
     = k·cn + 2^k·T(n/2^k)
```

When `n = 2^k`, we have:
```
T(n) = k·cn + n·T(1)
     = cn·log₂n + n·c'
     = Θ(n log n)
```

## Solving Recurrences: Substitution Method

The **substitution method** involves:
1. Guess the form of the solution
2. Use mathematical induction to prove it

### Example: `T(n) = 4T(n/2) + 50n`

**Guess**: `T(n) = O(n³)`

**Inductive hypothesis**: Assume `T(k) ≤ c·k³` for all `k < n`

**Proof**:
```
T(n) = 4T(n/2) + 50n
     ≤ 4c(n/2)³ + 50n
     = (c/2)n³ + 50n
     = cn³ - ((c/2)n³ - 50n)
```

We need `(c/2)n³ - 50n ≥ 0` for `T(n) ≤ cn³`.  
This holds when `c ≥ 100` and `n ≥ 1`.

### Tighter Upper Bound

We can actually prove `T(n) = O(n²)` with a stronger inductive hypothesis.

**Inductive hypothesis**: `T(k) ≤ c₁k² - c₂k` for all `k < n`

**Proof**:
```
T(n) = 4T(n/2) + 50n
     ≤ 4(c₁(n/2)² - c₂(n/2)) + 50n
     = c₁n² - 2c₂n + 50n
     = c₁n² - c₂n - (c₂n - 50n)
```

We need `c₂n - 50n ≥ 0`, which holds when `c₂ ≥ 50`.  
Thus `T(n) ≤ c₁n² - c₂n ≤ c₁n²`, so `T(n) = O(n²)`.

---

## Practice Tasks

1. **Asymptotic Notation Comparisons**
   - Prove that `5n³ + 2n² + 100 = Θ(n³)`
   - Prove that `n² log n = ω(n²)` but `n² log n = o(n³)`
   - Determine if `√n = Ω(log n)` and prove your answer

2. **Divide and Conquer Application**
   Describe how the Divide and Conquer paradigm would apply to:
   - Finding the maximum element in an array
   - Computing the factorial of a number
   - Searching for an element in a sorted array (binary search)

3. **Iteration Method Practice**
   Use the iteration method to solve:
   - `T(n) = T(n/3) + 1` with `T(1) = 1`
   - `T(n) = 3T(n/4) + n`

4. **Substitution Method Practice**
   Use the substitution method to prove:
   - `T(n) = 2T(n/2) + n = O(n log n)`
   - `T(n) = T(n-1) + n = O(n²)`

5. **Recurrence Analysis**
   For Merge Sort's recurrence `T(n) = 2T(n/2) + cn`:
   - Derive the exact solution when `n` is a power of 2
   - Explain why the solution is `Θ(n log n)` even when `n` is not a power of 2
   - Compare this with Insertion Sort's `Θ(n²)` complexity for large `n`
