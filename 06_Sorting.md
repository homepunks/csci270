# Lecture 6: Sorting Algorithms Part I

## The Problem of Sorting

**Sorting** is one of the most fundamental problems in computer science. Given a sequence of elements, the goal is to rearrange them in a specific order (typically non-decreasing or non-increasing).

### Formal Definition
**Input**: A sequence of numbers:  
`⟨a₁, a₂, ..., aₙ⟩`

**Output**: A permutation (reordering) of the input sequence:  
`⟨a′₁, a′₂, ..., a′ₙ⟩`  
such that:  
`a′₁ ≤ a′₂ ≤ ... ≤ a′ₙ`

**Example**:  
Input: `8 2 4 9 3 6`  
Output: `2 3 4 6 8 9`

## Key Properties of Sorting Algorithms

### Stability
A **stable** sorting algorithm maintains the relative order of records with equal keys (values). If record R appears before record S in the original list and both have the same key, then R will always appear before S in the sorted list.

**Why stability matters**: Useful when sorting by multiple criteria (e.g., sort by last name, then by first name).

### In-Place Sorting
An **in-place** sorting algorithm does not require significant additional memory beyond the input array. It transforms the input within the same memory space, possibly using only a small constant amount of extra space for variables.

**Trade-off**: In-place algorithms are memory-efficient but may be slower than algorithms that use extra space.

## Sorting Algorithms Overview
We will study six major sorting algorithms:
1. **Selection Sort** - Simple but inefficient
2. **Insertion Sort** - Efficient for small or nearly sorted arrays
3. **Merge Sort** - Efficient divide-and-conquer algorithm
4. **Quick Sort** - Efficient in practice, uses partitioning
5. **Linear-time Sorting Algorithms** (Radix Sort and Count Sort) - Specialized algorithms
6. **Heap Sort** - Based on heap data structure

## Selection Sort

### Basic Idea
Selection Sort repeatedly finds the minimum element from the unsorted portion and places it at the beginning.

### Algorithm Steps
1. Find the minimum value in the list
2. Swap it with the value in the first position
3. Repeat for the remainder of the list (starting at the second position each time)

### Iterative Pseudocode
```
for i = 1 to n do:
    minIndex = i
    for j = i + 1 to n do:
        if A[j] < A[minIndex] then:
            minIndex = j
    Swap A[i] with A[minIndex]
```

### Recursive Pseudocode
```
Sort(A, left, right):
    if left ≥ right return
    minIndex = left
    for j = left + 1 to right do:
        if A[j] < A[minIndex] then:
            minIndex = j
    Swap A[left] with A[minIndex]
    Sort(A, left + 1, right)
```

### Time Complexity Analysis
- Finding the minimum in an array of size `n` takes `Θ(n)` time
- Recurrence relation: `T(n) = n + T(n-1)` with `T(1) = 1`
- Solution: `T(n) = Θ(n²)`

**Important**: Selection Sort always takes `Θ(n²)` time, regardless of input order (even if already sorted).

## Insertion Sort

### Basic Idea
Insertion Sort builds the final sorted array one element at a time, similar to how people sort playing cards in their hands.

### Algorithm Steps
1. Start with the second element
2. Compare it with elements before it (in the sorted portion)
3. Shift larger elements to the right
4. Insert the current element in its correct position

### Pseudocode
```
INSERTION-SORT(A, n):  // A[1...n]
    for j = 2 to n do:
        key = A[j]
        i = j - 1
        while i > 0 and A[i] > key do:
            A[i+1] = A[i]
            i = i - 1
        A[i+1] = key
```

### Example
Input: `8 2 4 9 3 6`  
Step-by-step execution:
1. `2 8 4 9 3 6`  (insert 2 before 8)
2. `2 4 8 9 3 6`  (insert 4 between 2 and 8)
3. `2 4 8 9 3 6`  (9 is already in correct position)
4. `2 3 4 8 9 6`  (insert 3 between 2 and 4)
5. `2 3 4 6 8 9`  (insert 6 between 4 and 8)

### Correctness Proof (by Induction)
**Base case**: For i=1, A[1] is trivially sorted  
**Inductive hypothesis**: Assume A[1...i-1] is sorted at the beginning of i-th iteration  
**Inductive step**: The inner while loop finds the correct position for A[i] and inserts it there, so A[1...i] becomes sorted

### Time Complexity Analysis
**Worst-case**: Input is reverse sorted  
`T(n) = Σ(from j=2 to n) Θ(j) = Θ(n²)`

**Best-case**: Input is already sorted  
`T(n) = Θ(n)` (each element is already in its correct position)

**Average-case**: `O(n²)`

**Note**: Insertion Sort runs in time `O(n + I)`, where I is the number of inversions in the array. This makes it efficient for nearly sorted arrays.

## Divide and Conquer Approach

The quadratic time complexity (`n²`) of simple sorting algorithms becomes unacceptable for large inputs:
- Sorting 10⁶ numbers would take hours on even the fastest CPU

**Better approach**: Divide and conquer
- Sorting 10⁶ numbers takes <1 millisecond on an average laptop using divide-and-conquer algorithms

### Basic Idea
1. **Divide**: Split the problem into smaller subproblems
2. **Conquer**: Solve subproblems recursively
3. **Combine**: Merge solutions to solve the original problem

## Merge Sort

Merge Sort is a classic divide-and-conquer sorting algorithm.

### Algorithm Steps
1. **Divide**: Split the array into two halves
2. **Conquer**: Recursively sort each half
3. **Combine**: Merge the two sorted halves

### Pseudocode
```
MERGE-SORT(A, left, right):
    if left < right then:
        mid = ⌊(left + right)/2⌋
        MERGE-SORT(A, left, mid)
        MERGE-SORT(A, mid+1, right)
        MERGE(A, left, mid, right)
```

### Merge Subroutine
The key to Merge Sort is the `MERGE` operation, which combines two sorted arrays into one sorted array in linear time.

**Time complexity of MERGE**: `Θ(n)` to merge a total of n elements

### Example
Input: `20 12 13 11 7 9 2 1`
1. Divide: `[20 12 13 11]` and `[7 9 2 1]`
2. Recursively sort each half
3. Merge sorted halves

### Time Complexity Analysis
Recurrence relation:  
`T(n) = 2T(n/2) + Θ(n)` with `T(1) = Θ(1)`

Solution (using Master Theorem or other methods):  
`T(n) = Θ(n log n)`

**Properties**:
- Always runs in `Θ(n log n)` time (worst, average, and best cases)
- Requires O(n) extra space for merging
- Stable sorting algorithm

## Sorting Algorithms Comparison

### Selection Sort
- **Advantages**: Simple to implement, in-place (no extra memory)
- **Disadvantages**: Always `Θ(n²)`, even for sorted input
- **Use case**: Only for very small arrays or educational purposes

### Insertion Sort
- **Advantages**: Simple, in-place, efficient for small or nearly sorted arrays (`O(n + I)` where I is inversions)
- **Disadvantages**: `O(n²)` in worst case
- **Use case**: Small arrays (n ≤ 10-20), nearly sorted data, or as part of more complex algorithms

### Merge Sort
- **Advantages**: Guaranteed `Θ(n log n)` performance, stable
- **Disadvantages**: Requires O(n) extra space, recursive overhead
- **Use case**: General-purpose sorting, external sorting (when data doesn't fit in memory)

## Performance Comparison
- **n² algorithms** (Selection, Insertion): Sorting 10⁶ numbers takes hours
- **n log n algorithms** (Merge Sort): Sorting 10⁶ numbers takes milliseconds

---

## Practice Tasks

### Task 1: Algorithm Properties
Explain the difference between stable and unstable sorting algorithms. Give an example scenario where stability is crucial.

### Task 2: Selection Sort Trace
Trace Selection Sort step-by-step on the input: `64 25 12 22 11`. Show the array after each swap operation.

### Task 3: Insertion Sort Analysis
Given the array `[5, 2, 4, 6, 1, 3]`:
1. Trace Insertion Sort step-by-step
2. Count the number of inversions in the original array
3. Verify that the running time is `O(n + I)` where I is the number of inversions

### Task 4: Merge Sort Understanding
1. Explain why Merge Sort requires O(n) extra space
2. Write the recurrence relation for Merge Sort's time complexity and solve it
3. Why is Merge Sort considered a "divide and conquer" algorithm?

### Task 5: Algorithm Selection
For each scenario below, recommend the most appropriate sorting algorithm (Selection, Insertion, or Merge Sort) and justify your choice:
1. Sorting 20 exam scores
2. Sorting a list of 1 million customer records by last name
3. Maintaining a sorted list where new elements are added frequently
4. Sorting data that is already 95% sorted

### Task 6: Implementation Challenge
Write pseudocode for the MERGE subroutine that combines two sorted subarrays A[left..mid] and A[mid+1..right] into a single sorted array.

### Task 7: Time Complexity Proof
Prove that Selection Sort has time complexity Θ(n²) by:
1. Writing the recurrence relation
2. Solving it mathematically
3. Explaining why it's the same for best, worst, and average cases

### Task 8: Stability Analysis
Determine whether each algorithm is stable or unstable, and explain why:
1. Selection Sort
2. Insertion Sort
3. Merge Sort

### Task 9: Real-world Application
A mobile app needs to sort contacts by last name, then by first name. Which sorting property (stability) is important here, and why? Which of the three algorithms studied would be appropriate?

### Task 10: Performance Calculation
Estimate how long it would take to sort 1 million integers using:
1. Selection Sort (assume 10⁸ operations per second)
2. Merge Sort (assume 10⁸ operations per second)
Compare the results and explain the practical implications.
