# Lecture 4: Recurrence Relations (Continued)

## Recap

This lecture continues the study of **recurrence relations**, which are used to characterize the running times of recursive algorithms. Previously, we covered:

- Asymptotic Notation (Big-O, Omega, Theta)
- The Divide and Conquer paradigm
- Basic recurrence relations

## Divide and Conquer Strategy

Many fundamental algorithms use a **divide and conquer** strategy. These algorithms are recursive—they call themselves one or more times to solve subproblems.

### The Three Steps:
1. **Divide**: Break the problem into smaller subproblems.
2. **Conquer**: Solve the subproblems recursively.
3. **Combine**: Merge the solutions of the subproblems to form the solution for the original problem.

## Recurrence Relations

A **recurrence relation** is an equation or inequality that defines a function in terms of its value on smaller inputs. For recursive algorithms, `T(n)` represents the running time for an input of size `n`.

**Example**:  
`T(n) = T(n/2) + c`  
This recurrence describes an algorithm that solves a problem of size `n` by solving one subproblem of half the size and doing a constant amount of additional work.

## Solving Recurrences: Iteration Method

The **iteration method** involves repeatedly substituting the recurrence into itself until a pattern emerges, then summing the terms.

### Example:
`T(n) = T(n/2) + c`

Assume `n = 2^k`:

1. `T(n) = T(n/2) + c`
2. `= [T(n/4) + c] + c = T(n/4) + 2c`
3. `= [T(n/8) + c] + 2c = T(n/8) + 3c`
4. Continue `k` times until we reach `T(1)`.
5. `T(n) = T(1) + k*c`

Since `k = log₂n`,  
`T(n) = T(1) + c*log₂n = O(log n)`.

**Key Insight**: The number of times we can divide `n` by 2 until we reach 1 is `log₂n`.

## Solving Recurrences: Substitution Method

The **substitution method** involves:
1. Guessing the form of the solution.
2. Using mathematical induction to verify the guess and find constants.

### Example:
`T(n) = 2T(n/2) + 1`

**Guess**: `T(n) = O(n)`  
Assume `T(k) ≤ c*k` for all `k < n`.  
Then:  
`T(n) = 2T(n/2) + 1 ≤ 2*(c*(n/2)) + 1 = c*n + 1`  
This is not ≤ `c*n` for any constant `c` because of the `+1`.

**Strengthen the guess**: Assume `T(k) ≤ c₁*k - c₂` for all `k < n`.  
Then:  
`T(n) = 2T(n/2) + 1 ≤ 2*(c₁*(n/2) - c₂) + 1 = c₁*n - 2c₂ + 1`  
We want this ≤ `c₁*n - c₂`.  
So we need: `-2c₂ + 1 ≤ -c₂` ⇒ `1 ≤ c₂` ⇒ choose `c₂ ≥ 1` and `c₁ > 0`.  
Thus, `T(n) = O(n)`.

## Solving Recurrences: Recursion-Tree Method

The **recursion-tree method** visualizes the recurrence as a tree where each node represents the cost of a subproblem. We sum the costs at each level and across all levels.

### Example 1:
`T(n) = T(n/4) + T(n/2) + n²`

**Tree Structure**:
- Root cost: `n²`
- Two children: costs `(n/4)²` and `(n/2)²`
- Their children: costs `(n/16)²`, `(n/8)²`, `(n/8)²`, `(n/4)²`, etc.

The total cost is a geometric series:  
`T(n) = n² * [1 + 5/16 + (5/16)² + (5/16)³ + ...]`

Since `5/16 < 1`, the series converges to a constant factor.  
Thus, `T(n) = O(n²)`.

### Geometric Series Formula:
For `x ≠ 1`:
`∑_{k=0}^{n} x^k = (x^{n+1} - 1)/(x - 1)`

If `0 ≤ x < 1`, then:
`∑_{k=0}^{∞} x^k = 1/(1 - x)`

### Example 2:
`T(n) = 3T(n/4) + cn²`

The tree has:
- Root cost: `cn²`
- Three children, each with cost `c*(n/4)² = c*n²/16`
- Total cost per level decreases geometrically.
- The total cost is dominated by the root: `T(n) = O(n²)`.

## Useful Trick: Changing Variables

Some recurrences can be simplified by a change of variables.

### Example:
`T(n) = 2T(√n) + log n`

Let `m = log n` ⇒ `n = 2^m`.  
Then: `T(2^m) = 2T(2^{m/2}) + m`

Let `S(m) = T(2^m)`.  
Then: `S(m) = 2S(m/2) + m`  
This is a familiar recurrence with solution `S(m) = O(m log m)`.

Substitute back:  
`T(n) = S(m) = O(m log m) = O(log n * log log n)`.

## Master Method

The **Master Method** provides a cookbook solution for recurrences of the form:

`T(n) = a * T(n/b) + f(n)`

where:
- `a ≥ 1`
- `b > 1`
- `f(n)` is asymptotically positive

Let `g(n) = n^{log_b a}`.

### Three Cases:

**Case 1**: If `f(n) = O(n^{log_b a - ε})` for some `ε > 0`, then:  
`T(n) = Θ(n^{log_b a})`

**Case 2**: If `f(n) = Θ(n^{log_b a} * log^k n)` for some `k ≥ 0`, then:  
`T(n) = Θ(n^{log_b a} * log^{k+1} n)`

**Case 3**: If `f(n) = Ω(n^{log_b a + ε})` for some `ε > 0`, **and** if  
`a * f(n/b) ≤ c * f(n)` for some `c < 1` and all large `n` (regularity condition), then:  
`T(n) = Θ(f(n))`

### Intuitive Summary:
1. `f(n)` grows polynomially slower than `g(n)` → Case 1.
2. `f(n)` grows at the same rate as `g(n)`, up to log factors → Case 2.
3. `f(n)` grows polynomially faster than `g(n)` and satisfies regularity → Case 3.

### Examples:

**Example 1**: `T(n) = 2T(n/2) + n`  
- `a=2, b=2, log_b a = 1, g(n)=n`
- `f(n)=n = Θ(n) = Θ(g(n) * log^0 n)` → Case 2 with `k=0`
- Solution: `T(n) = Θ(n log n)`

**Example 2**: `T(n) = 2T(n/2) + n²`  
- `a=2, b=2, log_b a = 1, g(n)=n`
- `f(n)=n² = Ω(n^{1+ε})` for `ε=1` → Case 3
- Check regularity: `2*(n/2)² = n²/2 ≤ c*n²` for `c=1/2 < 1` ✓
- Solution: `T(n) = Θ(n²)`

**Example 3**: `T(n) = 2T(n/2) + √n`  
- `a=2, b=2, log_b a = 1, g(n)=n`
- `f(n)=√n = n^{0.5} = O(n^{1-ε})` for `ε=0.5` → Case 1
- Solution: `T(n) = Θ(n)`

**Example 4**: `T(n) = 4T(n^{2/3}) + log² n`  
This requires a change of variables before applying the Master Method (similar to the earlier trick).

---

## Practice Tasks

1. **Iteration Method**  
   Use the iteration method to solve:  
   `T(n) = T(n-1) + n` with `T(1)=1`.  
   Show your steps and give the final asymptotic bound.

2. **Substitution Method**  
   Use the substitution method to prove that:  
   `T(n) = T(⌊n/2⌋) + T(⌈n/2⌉) + n` is `O(n log n)`.

3. **Recursion Tree**  
   Draw the recursion tree for:  
   `T(n) = 2T(n/3) + n`.  
   Determine the cost at each level and sum them to find the asymptotic bound.

4. **Change of Variables**  
   Solve:  
   `T(n) = T(√n) + 1`  
   Hint: Let `m = log log n`.

5. **Master Method**  
   Use the Master Method to solve the following recurrences. State which case applies and the solution.
   a) `T(n) = 4T(n/2) + n`  
   b) `T(n) = 4T(n/2) + n²`  
   c) `T(n) = 4T(n/2) + n³`

6. **Regularity Condition**  
   For the recurrence `T(n) = 3T(n/2) + n²`, verify whether the regularity condition holds for Case 3 of the Master Method.

7. **Mixed Method**  
   Solve using any method:  
   `T(n) = 2T(n/4) + √n`  
   Provide your reasoning and solution.
