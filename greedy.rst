Greedy Algorithms
=================

Idea: build up a solution incrementally, taking the best available option at each step (e.g. Kruskal/Prim)

This kind of "local" reasoning doesn't necessarily yield a globally optimal solution. The trick in designing a
greedy algorithm is to find a greedy strategy that you can prove gives a global optimum.

Interval Scheduling Problem
---------------------------
Have a resource which can only be used by one person at a time, and people want to use it at different time periods.

We have *n* **requests** of the form :math:`(s(i), f(i))` giving the starting and finishing times. Want to find the
largest possible subset (by cardinality) of non-overlapping (compatible) requests.

.. image:: _static/greedy1.png
    :width: 400

Natural greedy approach:

1. Use some heuristic to select a request *i* and add it to our set
2. Delete all requests incompatible with *i*
3. Repeat until there are no requests left

Possible heuristics:

- Earliest request (does not work)
- Shortest request (does not work)
- Fewest conflicts (does not work)
- Request finishing first (works!)
    - Intuition: maximize the time remaining in which to service other requests

.. image:: _static/greedy2.png
    :width: 400

Optimality
----------
How do you prove optimality of a greedy strategy?

Exchange Argument
^^^^^^^^^^^^^^^^^
Take an optimal solution and transform it into the solution that the algorithm outputs without increasing its cost.

e.g. proof of cut property: we took an MST and showed that using the edge chosen by Kruskal/Prim would decrease its cost

Inductive Argument
^^^^^^^^^^^^^^^^^^
Break up an optimal solution into stages, and show inductively that the greedy algorithm does at least as well as the
optimal solution in each successive stage

ISP
"""
We use this argument to prove the optimality of the "smallest finishing time" heuristic for the interval scheduling
problem:

Call the set of intervals chosen by the algorithm *A*.

First, *A* is compatible, since we throw out incompatible requests each time we add a new request to *A*.

Take an optimal set of requests *O*. Need to show that :math:`|A| = |O|`.

Let :math:`i_1, i_2, ..., i_k` be the intervals of *A* in the order they were added.

Let :math:`j_1, j_2, ..., j_m` be the intervals of *O* in left-to-right order.

Then need to show that :math:`k=m`. Intuition for our heuristic was to make the resource available as soon as possible,
so let's prove that (i.e., for all :math:`r \leq k, f(i_r) \leq f(j_r)`). Prove by induction on *r*.

**Base Case**: :math:`r=1` - Algorithm picks :math:`i_1` to be the request with smallest finishing time, so
:math:`f(i_1)\leq f(j_1)`.

**Inductive Case**: :math:`r > 1` - Assume that :math:`f(i_{r-1}) \leq f(j_{r-1})`. Since :math:`j_{r-1}` is 
compatible with :math:`j_r`, :math:`f(j_{r-1}) \leq s(j_r)`.

So by the hypothesis, :math:`f(i_{r-1}) \leq f(j_{r-1}) \leq s(j_r)`, so :math:`i_{r-1}` is compatible with :math:`j_r`,
and therefore :math:`j_r` is one of the options considered when picking :math:`i_r`.

Since the algorithm picks the request with least finishing time amongst those consistent with the previously-chosen
requests, :math:`f(i_r) \leq f(j_r)`.

So by induction, :math:`f(i_r) \leq f(j_r) \forall r \leq k`.

Now suppose that *A* was not optimal, i.e. :math:`k < m`. By above, :math:`f(i_k) \leq f(j_k)`. Since *O* is compatible,
:math:`f(j_k) \leq s(j_{k+1})`, so :math:`j_{k+1}` is compatible with :math:`i_k`, and the algorithm should not have
terminated after :math:`i_k` - contradiction.