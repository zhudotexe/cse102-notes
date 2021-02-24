Dynamic Programming
===================
Fundamental idea: write the optimal solution to a problem in terms of optimal solutions to smaller subproblems.

When this is possible, the problem is said to have *optimal substructure*)

When you can do this, you get a recursive algorithm as in divide/conquer. The difference is that usually the
subproblems overlap.

Weighted Interval Solving Problem
---------------------------------
Like ISP from earlier, we have *n* requests for a resource with starting and finishing times, but now each request
has a *value* :math:`v_i`. We want to maximize the total value of granted requests.

.. image:: _static/dynamic1.png
    :width: 150

**Goal**: Find a subset *S* of requests which is compatible and maximixes :math:`\sum_{i \in S} v_i`.

Let's sort the requests by finishing time, so request 1 finishes first, etc.

To use dynamic programming, we need to write an optimal solution in terms of optimal solutions of subproblems. Note
that an optimal solution *O* either includes the last request *n* or doesn't.

If :math:`n \in O`, then *O* doesn't contain any intervals overlapping with *n*; let :math:`p(n)` be the last interval
in the order that *doesn't* overlap with *n*.

Then intervals :math:`p(n)+1, p(n)+2, ..., n-1` are excluded from *O*, but *O* must contain an optimal solution for
intervals :math:`1, 2, ..., p(n)`.

.. image:: _static/dynamic2.png
    :width: 250

.. note::
    If there were a better solution for intervals :math:`1, 2, ..., p(n)`, then we could improve *O* without introducing
    any conflicts with *n* (exchange argument).

If instead :math:`n\notin O`, then *O* must be an optimal solution for intervals :math:`1, 2, ..., n-1`.

.. note::
    If *O* was not an optimal solution for intervals :math:`1, 2, ..., n-1`, we could improve *O* as above.

So, either *O* is an optimal solution for intervals :math:`1, ..., p(n)` plus *n*, or it is an optimal solution for
intervals :math:`1, ..., n-1`.

If we then let :math:`M(i)` be the maximal total value for intervals ``[1, i]``, we have:

.. math::

    M(i) & = \max(M(p(i)) + v_i, M(i-1)) \text{ for } i > 1 \\
    M(1) & = v_1

This recursive equation gives us a recursive algorithm to compute the largest possible total value overall, which is
:math:`M(n)`. We can then read off which intervals to use for an optimal set:

- if :math:`M(n) = M(p(n)) + v_n`, then include interval *n*
- if :math:`M(n) = M(n-1)`, then exclude interval *n*

then continue recursively from :math:`p(n)` or :math:`n-1` respectively.

**Runtime**

Recursion tree has 2 subproblems, but depth :math:`\approx n`. So in total, the runtime is :math:`\approx 2^n`.

But there are only *n* *distinct* subproblems: :math:`M(1), M(2), ..., M(n)`. Our exponentially many calls are just
doing the same work over and over again.

Solution: *memoization* - whenever we solve a subproblem, save its solution in a table; when we need it later, just
look it up instead of doing further recursion.

Here, we use a table :math:`M[1..n]`. Each value is then computed at most once, and work to compute a value is constant,
so total runtime will be linear.