---
layout: post
title: "Matching Nuts and Bolts Problem"
#img: react.png # Add image post (optional)
date: 2019-02-17
description: Algorithm # Add post description (optional)
tag: [Algorithm]
---

**Question:**

You are given a collection of nuts of different size and corresponding bolts. You can choose any nut & any bolt together, from which you can determine whether the nut is larger than bolt, smaller than bolt or matches the bolt exactly. However there is no way to compare two nuts together or two bolts together. Suggest an algorithm to match each bolt to its matching nut.

Compute time complexity of your algorithm.

**Answer**:

Simple algorithm:

- compare every nut with every undetected bolt.

- In worst case, the number of comparisons required is:

  - $$
    \begin{align*}
    \sum_{i=1}^{n}(i-1)=n^2-n/2=\theta(n^2)
    \end{align*}
    $$


Using the idea of divide-and-conquer, which is very similar to randomized quicksort:

Keep in mind that if a Nut[N] is smaller than a Bolt(B) then N is fit only for bolts smaller than B. Similarly, if a Bolt(B') is smaller than a Nut(N') then N is fit only for bolts  smaller than B.

The problem statement does not describe exactly how the nuts and bolts are specified, so we first need come up with a reasonable representation for the input. ***Let’s assume the nuts and bolts are provided to us in two arrays N[1 . . . n] and B[1 . . . n],*** where we are allowed to compare elements across, but not within, these two arrays.

The algorithm first performs a partition operation as follows:

- pick a random nut N[i] as pivot.
- Using this nut, rearrange the array of bolts into three groups of elements:
  - first the bolts smaller than N[i]
  - the bolt that matches N[i]
  - the bolts larger than N[i]
- using the bolt that matches N[i] we perform a similar partition of the array of nuts.

The pair of partitioning operations can implemented in 
$$
\begin{align*}
2n-1=\theta(n)
\end{align*}
$$
where,

- n: pivot nut vs. every bolt.
- n-1: meathicng bolt vs every nut except the pivot nut.

After partition, smaller nuts and bolts precede the pivots, and larger nuts and bolts follow the pivots.Our algorithm then finishes by recursively applying itself to the subarrays to the left and right of the pivot position to match these remaining nuts and bolts. We can assume by induction on n that these recursive calls will properly match the remaining bolts.



Worst case for this algorithm:

In the worst case, each pivot selected is the smallest or the largest bolt, so the algorithm match every nut-bolt pair, same as quicksort.
$$
T(n) = T(n-1) + 2n-1
$$

$$
T(n) = \sum_{i=1}^{n} {2i-1}=n^2
$$

Best case:
$$
T(n) = 2T(n/2) + \theta(n) = \theta(nlgn)
$$
Each pivot evently divides the set into two subsets.

Use a randomized version of this algorithm to analyze the average case:

- Always choose the pivot bolt uniformly at random.
- let $$B_i$$ denote the 𝑖  th smallest bolt, and $$N_j$$denote the $$j$$ th smallest nut.
-  For each i and j, define an indicator random variable $$x_{ij}$$that equals 1 if our algorithm compares $$B_i$$with $$N_j$$ and 0 otherwise.
- Then the total number of nut/bolt comparisons is exactly

$$
T(n)=\sum_{i=1}^n\sum_{j=1}^nX_{ij}
$$

- Then, we want to calculate the expectation of T(n)

  ​
  $$
  E(T(n)) = E[\sum_{i=1}^n \sum_{j=1}^nX_{ij}]=\sum_{i=1}^n\sum_{j=1}^nE[X_{ij}]=\sum_{i=1}^n\sum_{j=1}^nPr[X_{ij}=1]
  $$


We first assume that i<j:

- If the pivot is $$N_i$$ or $$N_j$$, $$N_i$$ and $$B_j$$ will be compared.
- if the pivot is one of {$$N_{i+1}$$,….,$$N_{j-1}$$}, $$N_i$$ and $$B_j$$ will never be compared
- Therefore, the alogirthm compares $$N_i$$ and $$B_j$$ if and only if the first pivot nut randomly chosen from the set {$$N_i$$, $$N_{i+1}$$,….,$$N_j$$} is either $$N_i$$ or $$N_j$$
- since the set {$$N_i$$, $$N_{i+1}$$,….,$$N_j$$} contains j-i+1 nuts, each of which is equally likely to be chosen first, we have:

$$
E[X_{ij}]=2/j-i+1 \ 
(for\ all \ i<j)
$$

and Every nut will always compared with its partner, so $$X_{ii}$$=1 for all i.

Thus,
$$
E[T(n)]=O(nlgn)
$$
