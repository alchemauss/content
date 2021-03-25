---
title: Numerical Analysis - Complete Cumulative Notes
date:updated: 2021-03-24
---

## Introduction

### Floating Point Arithmetics

$\text{\large{Backward and Forward Error}}$

$\text{Forward Error}$ is a measure of the difference between the approximation of $\hat{y}$ and the true value of $y$, that is

$$
\begin{aligned}
  \text{absolute} &: \quad|\hat{y}-y|
  \\[1em]
  \text{relative} &: \quad\frac{ |\hat{y}-y| }{ |y| }
\end{aligned}
$$

Its a natural quantity to measure, but usually (since we don't know the true $y$) we can only get an upper bound on this error and very difficult to get tight upper bounds on the error.

***

$\text{Backward Error}$ would be asked as a question: *For what set of data have we actually solved the problem?* Specifically, we would like to find the smallest $\Delta{x}$ such that

$$\hat{y} = f(x+\Delta{x})$$

***

$\bold{Example.}\text{ Suppose we want to compute }y=\sqrt{2},\text{ and we obtain the approximation }\hat{y}=1.4.$

- $\text{FE:}\ |\Delta{y}| = |\hat{y}-y| = |1.4-1.41421...| \approx 0.0142$
- $\text{BE: Note that}\ \sqrt{1.96} = 1.4,\ \text{so}\ |\Delta{x}| = |2-1.96| = 0.04$

***

![!YouTube#d](RuKkePyo9zk "IEEE-754 Standard | Floating Point Binary Arithmetic")

![reserved exponent values table#d](assets/uploads/curated/notes/numerical-analysis.reserved-exponent-values.png "Reserved Exponent Values Table")

![decimal to binary#d](assets/uploads/curated/notes/numerical-analysis.decimal2binary.png "Step-by-step Decimal to Binary Conversion")

![binary to denary#d](assets/uploads/curated/notes/numerical-analysis.binary2denary.png "Step-by-step Binary to Denary Conversion")

### Error Propagation and Loss of Significance

Loss of significance typically happens when one gets too few significant digits in subtraction of two numbers very close to each other

## General Linear System

### LU Decomposition & Factorization

$\bold{Example.}$ Given $A$ below, use Gaussian elimination with partial pivoting to find the LU decomposition $PA=LU$ where $P$ is the associated permutation matrix.

**Note**: $L$ is Lower, $U$ is Upper, and $P$ is Permutation.

$$
A =
\begin{bmatrix}
  1 & 2 & 3 \\
  4 & 5 & 6 \\
  7 & 8 & 0
\end{bmatrix}
$$

1. Find the entry in the left column with the largest value, this is called the pivot.
2. Perform row interchange / swap (if necessary) so the pivot is in the first row.

    $$
    \begin{aligned}
      P_1A &=
      \begin{bmatrix}
        0 & 0 & 1 \\
        0 & 1 & 0 \\
        1 & 0 & 0
      \end{bmatrix}
      \sdot
      \begin{bmatrix}
        1 & 2 & 3 \\
        7 & 8 & 0 \\
        4 & 5 & 6
      \end{bmatrix}
      \\ P_1A &=
      \begin{bmatrix}
        7 & 8 & 0 \\
        4 & 5 & 6 \\
        1 & 2 & 3
      \end{bmatrix}
    \end{aligned}
    $$

3. Perform first column elimination (directly to the whole column)

    - Eliminate from top row to bottom row
    - Only use rows above its own as sources
    - e.g. (1) when working on the 2nd row, it can only be manipulated with the first row
    - e.g. (2) when working on the 3rd row, the first and second row can be used as source

4. Find the new pivot and switch rows (if necessary, *L rows are switched accordingly)

    - Only look for the constant when searching for new pivot for row switching, the $\plusmn$ sign in front should be neglected
    - e.g. If $-1$ is in the 3rd row and $\frac{1}{2}$ in the 2nd, switch them because $1 > \frac{1}{2}$

5. Apply second column elimination
6. Therefore, $PA=LU$ () where
7. Lower triangular matrix in $L$ is the minus result of each elimination permutation $(-P)$ from each $P$, resulting in $(l_{2,1}, l_{3,1}, l_{3,2})$ which is

    $$
    L =
    \begin{bmatrix}
      1 & 0 & 0 \\
      \frac{1}{7} & 1 & 0 \\
      \frac{4}{7} & \frac{1}{2} & 1
    \end{bmatrix}
    $$

## Special Linear System

## Least Square Problems

## Non-Linear Equation

## Non-Linear System

<!-- Midway -->

## Optimization

### Unconstrained - Steepest Descent

### Unconstrained - Gradient Descent

### Unconstrained - Newton Method

### Constrained - Lagrange Multiplier's Technique

## Polynomial Interpolation

## Extras - Online Tools & References

- [Octave Online REPL](https://octave-online.net/)
- [Decimal Binary Converter](https://www.rapidtables.com/convert/number/decimal-to-binary.html)
- [IEEE-754 Floating Point Converter](https://www.h-schmidt.net/FloatConverter/IEEE754.html)
- [Matrix Calculator](https://matrixcalc.org/en/)
