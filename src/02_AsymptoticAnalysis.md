# Lecture 2: Asymptotic Notation & Algorithm Analysis

## Recap: Analysis of Algorithms

**Analysis of algorithms** is the theoretical study of computer program performance and resource usage. While performance is important, other software engineering qualities are equally or more critical:

- **Correctness**: Does the algorithm produce the right output?
- **Modularity**: Is the design composed of independent, interchangeable components?
- **Maintainability**: How easy is it to modify and update the algorithm?
- **Functionality**: Does it meet the specified requirements?
- **Robustness**: How well does it handle invalid inputs or unexpected conditions?
- **User-friendliness**: Is the interface intuitive?
- **Programmer time**: How long does it take to implement?
- **Simplicity**: Is the design straightforward?
- **Extensibility**: Can it be easily extended with new features?
- **Reliability**: Does it perform consistently under various conditions?

## Recap: Insertion Sort

Insertion Sort is a simple comparison-based sorting algorithm that builds the final sorted array one element at a time.

### Pseudocode
```
INSERTION-SORT(A, n)  // A[1...n]
for j ← 2 to n
    key ← A[j]
    i ← j - 1
    while i > 0 and A[i] > key
        A[i+1] ← A[i]
        i ← i - 1
    A[i+1] ← key
```

### How It Works
1. Start from the second element (index 2).
2. Compare it with elements to its left.
3. Shift larger elements right to make space.
4. Insert the current element into its correct position.

## Recap: Types of Analysis

### Worst-case Analysis
`T(n) = maximum time of algorithm on any input of size n`
- Most common analysis method
- Provides a performance guarantee

### Average-case Analysis
`T(n) = expected time over all inputs of size n`
- Requires assumption about statistical distribution of inputs
- More realistic for many applications

### Best-case Analysis
- Often misleading
- Can "cheat" by designing algorithms that work well only on specific inputs

## Asymptotic Notations

Asymptotic notations describe the growth rate of functions, particularly useful for comparing algorithm running times.

### O-notation (Big-O) - Upper Bound
`O(g(n))` represents the set of functions that grow **no faster than** `g(n)`.

**Formal Definition**:  
`O(g(n)) = { f(n): ∃ constants c > 0, n₀ > 0 such that 0 ≤ f(n) ≤ c·g(n) ∀ n ≥ n₀ }`

**Example**: `2n² ∈ O(n³)`  
We can choose `c = 1` and `n₀ = 2`:
- `0 ≤ 2n² ≤ n³` for all `n ≥ 2`

### Ω-notation (Omega) - Lower Bound
`Ω(g(n))` represents the set of functions that grow **at least as fast as** `g(n)`.

**Formal Definition**:  
`Ω(g(n)) = { f(n): ∃ constants c > 0, n₀ > 0 such that 0 ≤ c·g(n) ≤ f(n) ∀ n ≥ n₀ }`

**Example**: `√n = Ω(log n)`  
We can choose `c = 1` and `n₀ = 16`:
- `0 ≤ 1·log n ≤ √n` for all `n ≥ 16`

### Θ-notation (Theta) - Tight Bound
`Θ(g(n))` represents the set of functions that grow **at the same rate as** `g(n)`.

**Formal Definition**:  
`Θ(g(n)) = { f(n): ∃ positive constants c₁, c₂, n₀ such that 0 ≤ c₁·g(n) ≤ f(n) ≤ c₂·g(n) ∀ n ≥ n₀ }`

**Equivalence**: `Θ(g(n)) = O(g(n)) ∩ Ω(g(n))`

**Example**: `½n² - 2n = Θ(n²)`

## Examples of Asymptotic Relationships

### Proving Bounds
1. **n³ + 20n + 1 is O(n³)**  
   For large n, n³ dominates.

2. **n³ + 20n + 1 is not O(n²)**  
   The cubic term grows faster than any constant multiple of n².

3. **n³ + 20n + 1 is O(n⁴)**  
   True, but not a tight bound.

4. **2n³ - 7n + 1 = Ω(n³)**  
   The leading term ensures growth at least as fast as n³.

5. **n³ + 20n = Ω(n²)**  
   Cubic growth exceeds quadratic.

### Important Notes
- The choice of `n₀` and `c` is **not unique**
- Multiple valid combinations can prove the same relationship
- The proof depends on the inequalities used for bounding

## Common Function Classes in Asymptotic Notation

### Functions in O(n²)
```
n²
n² + n
n² + 1000n
1000n² + 1000n
n
n/1000
n¹·⁹⁹⁹⁹⁹
n²/(log log log n)
```

### Functions in Ω(n²)
```
n²
n² + n
n² - n
1000n² + 1000n
1000n² - 1000n
n³
n²·⁰⁰⁰⁰¹
n² log log log n
2^(2^n)
```

## Analyzing Algorithm Running Time

### Example: Nested Loops
```python
def foo(lst):
    result1 = 1
    result2 = 6
    for i in range(len(lst)):
        for j in range(len(lst)):
            result1 += i * j
            result2 = lst[j] + i
    return
```

**Analysis**:
- Let `n = len(lst)`
- Outer loop: `n` iterations
- Inner loop: `n` iterations per outer iteration
- Each inner iteration: constant time (2 operations)
- Lines 1-2: constant time (2 operations)
- Total: `2 + n × n × 2 = 2n² + 2 = Θ(n²)`

## Growth Rate Ranking of Functions

From fastest to slowest growth:
```
nⁿ           (fastest)
2ⁿ
n³
n²
n log n
n
√n
log n
1           (slowest)
```

**Key Insight**:  
`n log n` grows faster than `n` but slower than `n²`. This explains why `Θ(n log n)` sorting algorithms (like Merge Sort) are more efficient than `Θ(n²)` algorithms (like Insertion Sort) for large inputs.

## Summary

1. **Function Description**: We use functions to describe algorithm runtime as a function of input size `n`.

2. **Asymptotic Notations**: 
   - `O` for upper bounds (≤)
   - `Ω` for lower bounds (≥)
   - `Θ` for tight bounds (=)

3. **Proof Technique**: Proving asymptotic relationships involves finding constants `n₀` and `c` that satisfy the inequality chain.

4. **Practical Application**: Asymptotic analysis helps compare algorithms independent of hardware and implementation details.

---

## Practice Tasks

### Task 1: Formal Proofs
Prove the following using formal definitions:
1. `3n³ + 2n² + 5 = O(n³)`
2. `n² + 100n = Ω(n²)`
3. `2n² - n = Θ(n²)`

### Task 2: True or False
Determine if each statement is true or false. Justify your answer.
1. `n³ = O(n²)`
2. `2ⁿ = Ω(n¹⁰⁰)`
3. `log n = O(√n)`
4. `n! = O(nⁿ)`
5. `n² log n = Ω(n²)`

### Task 3: Algorithm Analysis
Analyze the time complexity of the following pseudocode:
```
function mystery(A, n):
    sum = 0
    for i = 1 to n:
        for j = 1 to i:
            sum = sum + A[i] * A[j]
    return sum
```

### Task 4: Function Classification
Classify each function into the most specific asymptotic class:
1. `f(n) = 5n³ + 2n log n + 7`
2. `f(n) = 100n + √n + log n`
3. `f(n) = n² log n + n(log n)²`
4. `f(n) = 2ⁿ + n³⁰`
5. `f(n) = n! + 2ⁿ`

### Task 5: Practical Implications
Suppose you have two algorithms:
- Algorithm A: `T(n) = 100n²`
- Algorithm B: `T(n) = 2n³`

1. For what values of `n` is Algorithm A faster than Algorithm B?
2. If you expect `n ≤ 50`, which algorithm would you choose?
3. How would your choice change if `n` could be up to 500?
