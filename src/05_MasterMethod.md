# Lecture 5: Master Method & Strassen's Algorithm

## Master Method (Review)

The **Master Method** provides a cookbook solution for solving recurrence relations of the form:

\[
T(n) = aT\left(\frac{n}{b}\right) + f(n)
\]

where:
- \(a \geq 1\) (number of subproblems)
- \(b > 1\) (factor by which input size is reduced)
- \(f(n)\) is asymptotically positive

Let:
\[
g(n) = n^{\log_b a}
\]

### Three Cases:

**Case 1**: If \(f(n) = O(g(n)/n^\epsilon)\) for some \(\epsilon > 0\)
\[
\text{Then } T(n) = \Theta(g(n))
\]

**Case 2**: If \(f(n) = \Theta(g(n) \log^k n)\) for some \(k \geq 0\)
\[
\text{Then } T(n) = \Theta(g(n) \log^{k+1} n)
\]

**Case 3**: If \(f(n) = \Omega(g(n) \cdot n^\epsilon)\) for some \(\epsilon > 0\), **and** the regularity condition holds:
\[
af\left(\frac{n}{b}\right) \leq cf(n) \text{ for some } c < 1 \text{ and all sufficiently large } n
\]
\[
\text{Then } T(n) = \Theta(f(n))
\]

## Master Method Exercises

### Exercise 1
\[
T(n) = 3T\left(\frac{n}{2}\right) + n
\]

**Solution**:
- \(a = 3\), \(b = 2\)
- \(g(n) = n^{\log_2 3} \approx n^{1.585}\)
- \(f(n) = n\)

Since \(f(n) = n = O(n^{\log_2 3 - \epsilon})\) for \(\epsilon = \log_2 3 - 1 > 0\), this is **Case 1**.
\[
T(n) = \Theta(n^{\log_2 3})
\]

### Exercise 2
\[
T(n) = 4T\left(\frac{n}{3}\right) + n^{3/2}
\]

**Solution**:
- \(a = 4\), \(b = 3\)
- \(g(n) = n^{\log_3 4} \approx n^{1.262}\)
- \(f(n) = n^{3/2}\)

Since \(f(n) = n^{3/2} = \Omega(n^{\log_3 4 + \epsilon})\) for some \(\epsilon > 0\) (e.g., \(\epsilon = 0.238\)), this is **Case 3**.

Check regularity condition:
\[
af\left(\frac{n}{b}\right) = 4\left(\frac{n}{3}\right)^{3/2} = \frac{4}{3\sqrt{3}} n^{3/2} \leq c n^{3/2}
\]
For \(c = \frac{4}{3\sqrt{3}} \approx 0.77 < 1\), condition holds.
\[
T(n) = \Theta(n^{3/2})
\]

### Exercise 3
\[
T(n) = 4T\left(n^{2/3}\right) + \lg^2 n
\]

**Solution**:
Use substitution to transform the recurrence:
1. Let \(m = \lg n \Rightarrow n = 2^m\)
2. \(T(2^m) = 4T(2^{2m/3}) + m^2\)
3. Let \(S(m) = T(2^m)\)
4. \(S(m) = 4S\left(\frac{m}{3/2}\right) + m^2 = 4S\left(\frac{2m}{3}\right) + m^2\)

Now \(a = 4\), \(b = 3/2\)
- \(g(m) = m^{\log_{3/2} 4} \approx m^{3.419}\)
- \(f(m) = m^2\)

Since \(f(m) = m^2 = O(m^{\log_{3/2} 4 - \epsilon})\) for some \(\epsilon > 0\), this is **Case 1**.
\[
S(m) = \Theta(m^{\log_{3/2} 4})
\]
\[
T(n) = \Theta((\log n)^{\log_{3/2} 4})
\]

## Matrix Multiplication

### Standard Algorithm
Given two \(n \times n\) matrices \(A\) and \(B\), their product \(C = A \times B\) is computed as:

\[
c_{ij} = \sum_{k=1}^{n} a_{ik} \cdot b_{kj}
\]

**Standard Algorithm Complexity**: \(\Theta(n^3)\)

**Pseudocode**:
```
SQUARE-MATRIX-MULTIPLY(A, B):
    n = A.rows
    let C be new n × n matrix
    for i = 1 to n
        for j = 1 to n
            c[i][j] = 0
            for k = 1 to n
                c[i][j] = c[i][j] + a[i][k] * b[k][j]
    return C
```

## Divide-and-Conquer Approach

Assume \(n = 2^k\). Partition matrices into 4 submatrices of size \(n/2 \times n/2\):

\[
A = \begin{pmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{pmatrix},
\quad B = \begin{pmatrix} B_{11} & B_{12} \\ B_{21} & B_{22} \end{pmatrix},
\quad C = \begin{pmatrix} C_{11} & C_{12} \\ C_{21} & C_{22} \end{pmatrix}
\]

Then:
\[
\begin{aligned}
C_{11} &= A_{11}B_{11} + A_{12}B_{21} \\
C_{12} &= A_{11}B_{12} + A_{12}B_{22} \\
C_{21} &= A_{21}B_{11} + A_{22}B_{21} \\
C_{22} &= A_{21}B_{12} + A_{22}B_{22}
\end{aligned}
\]

**Recurrence**: \(T(n) = 8T(n/2) + \Theta(n^2)\)

By Master Method: \(a = 8\), \(b = 2\), \(g(n) = n^{\log_2 8} = n^3\)
Since \(f(n) = \Theta(n^2) = O(n^{3-\epsilon})\) for \(\epsilon = 1\), this is **Case 1**:
\[
T(n) = \Theta(n^3)
\]
No improvement over standard algorithm.

## Strassen's Algorithm

Strassen discovered a way to multiply matrices using only 7 multiplications instead of 8.

### Steps:
1. Compute 10 sums:
\[
\begin{aligned}
S_1 &= B_{12} - B_{22} \\
S_2 &= A_{11} + A_{12} \\
S_3 &= A_{21} + A_{22} \\
S_4 &= B_{21} - B_{11} \\
S_5 &= A_{11} + A_{22} \\
S_6 &= B_{11} + B_{22} \\
S_7 &= A_{12} - A_{22} \\
S_8 &= B_{21} + B_{22} \\
S_9 &= A_{11} - A_{21} \\
S_{10} &= B_{11} + B_{12}
\end{aligned}
\]

2. Compute 7 products:
\[
\begin{aligned}
P_1 &= A_{11} \cdot S_1 \\
P_2 &= S_2 \cdot B_{22} \\
P_3 &= S_3 \cdot B_{11} \\
P_4 &= A_{22} \cdot S_4 \\
P_5 &= S_5 \cdot S_6 \\
P_6 &= S_7 \cdot S_8 \\
P_7 &= S_9 \cdot S_{10}
\end{aligned}
\]

3. Compute result submatrices:
\[
\begin{aligned}
C_{11} &= P_5 + P_4 - P_2 + P_6 \\
C_{12} &= P_1 + P_2 \\
C_{21} &= P_3 + P_4 \\
C_{22} &= P_5 + P_1 - P_3 - P_7
\end{aligned}
\]

### Complexity Analysis
**Recurrence**: \(T(n) = 7T(n/2) + \Theta(n^2)\)

By Master Method: \(a = 7\), \(b = 2\), \(g(n) = n^{\log_2 7} \approx n^{2.807}\)
Since \(f(n) = \Theta(n^2) = O(n^{\log_2 7 - \epsilon})\) for \(\epsilon = \log_2 7 - 2 > 0\), this is **Case 1**:
\[
T(n) = \Theta(n^{\log_2 7}) \approx \Theta(n^{2.807})
\]

### Improved Algorithms
- Coppersmith-Winograd: \(O(n^{2.376})\)
- Virginia Williams: \(O(n^{2.3728642})\)
- François Le Gall: \(O(n^{2.3728639})\)

---

## Practice Tasks

1. **Master Method Application**
   Solve using the Master Method:
   a) \(T(n) = 2T(n/4) + \sqrt{n}\)
   b) \(T(n) = 4T(n/2) + n^2 \log n\)
   c) \(T(n) = 8T(n/2) + n^3\)

2. **Matrix Multiplication Analysis**
   For the standard divide-and-conquer matrix multiplication:
   a) Write the complete recurrence relation including all terms
   b) Explain why it doesn't improve over the naive \(\Theta(n^3)\) algorithm
   c) What would be the complexity if we could reduce the multiplications to 6 instead of 8?

3. **Strassen's Algorithm Verification**
   Given:
   \[
   A = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix},
   \quad B = \begin{pmatrix} 5 & 6 \\ 7 & 8 \end{pmatrix}
   \]
   a) Compute the product using Strassen's algorithm
   b) Verify the result matches standard matrix multiplication
   c) Count the exact number of scalar multiplications and additions in both methods

4. **Recurrence Transformation**
   Transform and solve: \(T(n) = T(\sqrt{n}) + \log n\)
   Hint: Use substitution \(m = \log n\)

5. **Algorithm Design**
   Design a divide-and-conquer algorithm for multiplying two \(n \times n\) matrices where \(n\) is not a power of 2. What modifications are needed? How does this affect the complexity?

6. **Comparative Analysis**
   For \(n = 1024\), compare the theoretical number of operations for:
   a) Standard matrix multiplication (\(\Theta(n^3)\))
   b) Strassen's algorithm (\(\Theta(n^{\log_2 7})\))
   c) At what size \(n\) would Strassen's algorithm theoretically become faster, assuming constant factors are equal?
