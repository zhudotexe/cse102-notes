Recurrence
==========
When analyzing the runtime of a recursive algorithm, you can write the *total* runtime in terms of the runtimes
of the recursive calls, creating a *recurrence relation*.

E.g. the divide-and-conquer convex hull algorithm, with 3 main steps:

.. code-block:: text

    Split points into left and right halves                 # O(n)
    Recursively find the convex hulls of each half          # 2T(n/2)
    Merge the 2 hulls into a single one by adding tangents  # O(n)

So in total:

.. math::
    T(n) & =O(n)+2T(\frac{n}{2})+O(n) \\
         & =2T(\frac{n}{2})+O(n)

Note: A base case is required for recurrences; problem-specific (for convex-hull :math:`n \leq 3` is the base case)

Solving
-------

Recursion Tree
^^^^^^^^^^^^^^
e.g. :math:`T(n)=2T(\frac{n}{2})+O(n)`

Solving for the max depth of the base case:
:math:`\frac{n}{2^d}\leq 3 \to 2^d \geq \frac{n}{3} \to d \geq \log_2(\frac{n}{3}) = \log_2(n) - \log_2(3) = \Theta(\log n)`

.. image:: _static/recurrence1.png
    :width: 750

In conclusion, the work at each level is :math:`\Theta(n)`, with :math:`\Theta(\log n)` levels, so the total runtime
is :math:`\Theta(n \log n)`.