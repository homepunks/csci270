# Lecture 7: Sorting Algorithms - Lower Bounds & Quick Sort

## Sorting Algorithms Review

In previous lectures, we discussed several sorting algorithms:
- **Selection Sort**: Simple but always Θ(n²)
- **Insertion Sort**: Efficient for small/nearly-sorted data, worst-case Θ(n²)
- **Merge Sort**: Always Θ(n log n) but requires additional memory

All these algorithms are **comparison-based**: they only use comparisons between elements to determine their order.

## Lower Bounds for Comparison-Based Sorting

### Comparison (Decision Tree) Model

The **decision tree model** represents comparison-based sorting algorithms as binary trees:
- Each **internal node** represents a comparison `A[i] ≤ A[j]`
- Each **leaf** represents a possible sorted permutation
- The **execution path** from root to leaf shows the sequence of comparisons made

**Example**: Decision tree for sorting 3 elements A[0], A[1], A[2] has leaves for all 3! = 6 possible permutations.

### Theorem: Any decision tree sorting n elements has height Ω(n log n)

**Proof**:
1. There are n! possible permutations of n elements (each requires a unique leaf)
2. A binary tree of height h has at most 2ʰ leaves
3. Therefore: 2ʰ ≥ n!
4. Taking logs: h ≥ log(n!)
5. Using Stirling's approximation: n! > (n/e)ⁿ
6. Therefore: h ≥ log((n/e)ⁿ) = n log n - n log e
7. Thus: h = Ω(n log n)

**Implications**:
- Merge Sort is **optimal** for comparison-based sorting with Θ(n log n) worst-case
- To beat Ω(n log n), we must use more than just comparisons (e.g., exploit integer properties)
- Algorithms like Radix Sort and Counting Sort can achieve Θ(n) for integers with bounded range

## Quick Sort

Quick Sort is a **divide-and-conquer**, **in-place** sorting algorithm that is often faster than Merge Sort in practice.

### Divide-and-Conquer Strategy

1. **Divide**: Partition the array around a pivot element x:
   - Left subarray: elements ≤ x
   - Right subarray: elements ≥ x
   
2. **Conquer**: Recursively sort the two subarrays

3. **Combine**: Trivial (subarrays are already in place)

### Partitioning Subroutine

The key to Quick Sort is the linear-time partitioning algorithm:

```
PARTITION(A, p, q)  // A[p...q]
    x = A[p]        // pivot element
    i = p
    for j = p+1 to q
        if A[j] ≤ x
            i = i + 1
            exchange A[i] with A[j]
    exchange A[p] with A[i]
    return i
```

**Invariant maintained during partitioning**:
- Elements A[p...i] are ≤ x
- Elements A[i+1...j-1] are > x
- Elements A[j...q] are unprocessed

### Example of Partitioning

Array: `6 10 13 5 8 3 2 11` (pivot = 6)

Step-by-step:
1. `6 10 13 5 8 3 2 11`  (j=1, 10>6, no swap)
2. `6 10 13 5 8 3 2 11`  (j=2, 13>6, no swap)
3. `6 5 13 10 8 3 2 11`  (j=3, 5≤6, swap with 10)
4. `6 5 3 10 8 13 2 11`  (j=4, 3≤6, swap with 13)
5. `6 5 3 2 8 13 10 11`  (j=5, 2≤6, swap with 8)
6. Final: `2 5 3 6 8 13 10 11` (swap pivot with last ≤ element)

### Quick Sort Pseudocode

```
QUICKSORT(A, p, r)
    if p < r
        q = PARTITION(A, p, r)
        QUICKSORT(A, p, q-1)
        QUICKSORT(A, q+1, r)
```

Initial call: `QUICKSORT(A, 1, n)`

## Running Time Analysis

### Worst-Case: Θ(n²)
- Occurs when partition is maximally unbalanced
- Example: Input already sorted or reverse sorted
- Recurrence: T(n) = T(n-1) + Θ(n) = Θ(n²)

### Best-Case: Θ(n log n)
- Occurs when partition splits array exactly in half
- Recurrence: T(n) = 2T(n/2) + Θ(n) = Θ(n log n)

### Average-Case: Θ(n log n)
- Expected performance with random input
- Even with 1:9 splits: T(n) = T(n/10) + T(9n/10) + Θ(n) = Θ(n log n)
- Intuition: Random pivots usually yield reasonably balanced partitions

## Comparison: Quick Sort vs Merge Sort

| Aspect | Quick Sort | Merge Sort |
|--------|------------|------------|
| Worst-case | Θ(n²) | Θ(n log n) |
| Average-case | Θ(n log n) | Θ(n log n) |
| In-place | Yes | No (requires O(n) extra) |
| Stable | Typically no | Yes |
| Practical speed | 2-3x faster (cache efficient) | Slower (memory movement) |
| Optimal | No (worst-case Θ(n²)) | Yes (worst-case optimal) |

## Practical Considerations

1. **Pivot selection** matters:
   - First/last element: vulnerable to sorted inputs
   - Random element: good expected performance
   - Median-of-three: better protection against bad splits

2. **Small subarrays**: For small n (e.g., n ≤ 10), Insertion Sort is often faster
3. **Stack depth**: Naive recursion can cause O(n) stack depth in worst-case
4. **Equal elements**: Standard implementation is not stable

---

## Practice Tasks

1. **Decision Tree Analysis**
   - Draw the decision tree for sorting 3 elements (A, B, C)
   - What is the minimum height required? Verify it satisfies h ≥ log(3!)
   - How many leaves does a decision tree for sorting 4 elements need?

2. **Partitioning Execution**
   - Trace the PARTITION algorithm on array `[9, 7, 5, 11, 12, 2, 14, 3, 10, 6]` with pivot = 9
   - Show the array after each iteration of the for loop
   - Identify the final position of the pivot

3. **Quick Sort Analysis**
   - What input produces the worst-case running time for Quick Sort?
   - Prove that T(n) = T(n-1) + Θ(n) yields Θ(n²)
   - Explain why average-case Quick Sort is Θ(n log n) even with uneven splits

4. **Comparative Analysis**
   - When would you choose Quick Sort over Merge Sort?
   - When would you choose Merge Sort over Quick Sort?
   - How does Quick Sort's in-place property affect its cache performance?

5. **Algorithm Design**
   - Design a modified PARTITION that handles duplicate elements efficiently
   - Propose a pivot selection strategy to avoid worst-case behavior
   - How could you make Quick Sort stable? What would be the trade-offs?

6. **Complexity Proof**
   - Using the decision tree model, prove that any comparison-based sorting algorithm requires at least Ω(n log n) comparisons in the worst case
   - Why doesn't this lower bound apply to Radix Sort or Counting Sort?
