# Lecture 1: Introduction & Analysis of Algorithms

## What is an Algorithm?

An **algorithm** is a set of precise, step-by-step instructions designed to solve a specific problem or perform a defined task. It must be unambiguous and detailed enough to be executed by a computer.

### Characteristics of an Algorithm
1. **Input**: Must take zero or more inputs.
2. **Output**: Must produce at least one output (e.g., a value, a decision).
3. **Definiteness**: Each instruction must be clear and unambiguous.
4. **Finiteness**: The algorithm must terminate after a finite number of steps.
5. **Effectiveness**: Every operation must be basic enough to be performed exactly and in a finite amount of time.

## Analysis of Algorithms

**Analysis of algorithms** is the theoretical study of the performance and resource usage (time, memory) of computer programs. While performance is important, other factors are equally or more critical in software engineering:

- **Correctness**
- **Modularity**
- **Maintainability**
- **Functionality**
- **Robustness**
- **User-friendliness**
- **Programmer time**
- **Simplicity**
- **Extensibility**
- **Reliability**

### Why Study Algorithms and Performance?
- Algorithms help us understand **scalability**.
- Performance often determines the **feasibility** of solving a problem.
- Algorithmic mathematics provides a language for describing **program behavior**.
- Performance is the **currency of computing**—lessons about performance generalize to other computing resources.

## The Sorting Problem

**Input**: A sequence of numbers:  
`<a₁, a₂, ..., aₙ>`

**Output**: A permutation (reordering) of the input sequence:  
`<a′₁, a′₂, ..., a′ₙ>`  
such that:  
`a′₁ ≤ a′₂ ≤ ... ≤ a′ₙ`

**Example**:  
Input: `8 2 4 9 3 6`  
Output: `2 3 4 6 8 9`

## Insertion Sort

Insertion Sort is a simple, iterative sorting algorithm that builds the final sorted array one element at a time.

### Pseudocode
```
INSERTION-SORT(A, n)  // A[1...n]
for j = 2 to n
    key = A[j]
    i = j - 1
    while i > 0 and A[i] > key
        A[i+1] = A[i]
        i = i - 1
    A[i+1] = key
```

### How It Works
1. Start with the second element (`j = 2`).
2. Compare it with the elements before it (left side), which are already sorted.
3. Shift all larger elements one position to the right.
4. Insert the current element into its correct position.

### Example
Input: `8 2 4 9 3 6`  
Step-by-step:
1. `2 8 4 9 3 6`
2. `2 4 8 9 3 6`
3. `2 4 8 9 3 6`
4. `2 3 4 8 9 6`
5. `2 3 4 6 8 9`

## Running Time Analysis

The **running time** of an algorithm depends on:
1. **Input size**: Longer sequences take more time to sort.
2. **Input order**: Already sorted sequences are easier to sort.

We typically analyze the **worst-case** running time to guarantee performance.

### Types of Analysis
- **Worst-case**: `T(n) = maximum time on any input of size n`
- **Average-case**: `T(n) = expected time over all inputs of size n` (requires a statistical distribution assumption)
- **Best-case**: Often misleading—a slow algorithm may perform well on specific inputs.

### Asymptotic Analysis
We focus on **growth rates** as `n → ∞`, ignoring machine-dependent constants. This is called **asymptotic analysis**.

## Asymptotic Notation: Θ (Theta)

The **Theta notation** provides a tight bound on a function’s growth rate.

**Mathematical Definition**:  
`Θ(g(n)) = { f(n): there exist positive constants c₁, c₂, and n₀ such that 0 ≤ c₁·g(n) ≤ f(n) ≤ c₂·g(n) for all n ≥ n₀ }`

**Engineering Interpretation**:  
Drop lower-order terms and leading constants.

**Example**:  
`3n³ + 90n² − 5n + 6046 = Θ(n³)`

## Asymptotic Performance

- When `n` is large, a `Θ(n²)` algorithm outperforms a `Θ(n³)` algorithm.
- However, asymptotically slower algorithms may still be useful in practice where constants or lower-order terms matter.
- Asymptotic analysis helps structure our thinking about algorithm efficiency.

## Insertion Sort Analysis

**Worst-case**: Input is reverse sorted.  
The running time is:  
`T(n) = Σ(from j=2 to n) Θ(j) = Θ(n²)` (arithmetic series)

### Is Insertion Sort Fast?
- **Moderately fast** for small `n`.
- **Inefficient** for large `n`.

---

## Practice Tasks

1. **Algorithm Characteristics**  
   Explain why each characteristic of an algorithm (input, output, definiteness, finiteness, effectiveness) is necessary.

2. **Insertion Sort Execution**  
   Trace Insertion Sort step-by-step on the input sequence: `5 1 4 2 8`. Show the array after each iteration of the outer loop.

3. **Running Time Scenarios**  
   For Insertion Sort:
   - What is the best-case input? What is its running time?
   - What is the worst-case input? What is its running time?

4. **Asymptotic Notation**  
   Prove or disprove:  
   a) `7n³ + 2n² + 100 = Θ(n³)`  
   b) `n² + 50n log n = Θ(n²)`

5. **Comparative Analysis**  
   Suppose Algorithm A runs in `Θ(n log n)` and Algorithm B runs in `Θ(n²)`. For what values of `n` would you prefer Algorithm B if it has much smaller constant factors? Explain.
