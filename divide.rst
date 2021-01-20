Divide-and-Conquer Algorithms
=============================
*general paradigm used by many algorithms*

1. *Divide* the problem into subproblems
2. *Conquer* (solve) each subproblem with a recursive call
3. *Combine* the subproblem solution into an overall solution

This approach is helpful when it isn't clear how to solve a large problem directly, but you can rewrite it in terms
of smaller problems. Coming up with a way to divide/conquer may be easier than solving the whole problem.

Mergesort
---------
e.g. Mergesort: sort a list by recursively sorting two halves - the hard part is merging two sorted lists.

.. code-block:: text

    define merge(B, C):

    B = [3, 5, 5, 7]  # example data
    C = [1, 4, 6]
    D = []

    look at the first element of each list
    pop the smaller and append it to D (or use a pointer to track where in each list you are, etc)

    B = [3, 5, 5, 7]
    C = [4, 6]
    D = [1]

    repeat until at least one list is empty

    B = [5, 5, 7]
    C = [4, 6]
    D = [1, 3]

    B = [5, 5, 7]
    C = [6]
    D = [1, 3, 4]

    B = [5, 7]
    C = [6]
    D = [1, 3, 4, 5]

    B = [7]
    C = [6]
    D = [1, 3, 4, 5, 5]

    B = [7]
    C = []
    D = [1, 3, 4, 5, 5, 6]

    append the rest of the non-empty list to D

The runtime of the merge operation is linear, i.e. :math:`\Theta(|B|+|C|)`

Now we can define mergesort:

.. code-block:: py

    def mergesort(A[0..n-1]):
        if n <= 1:
            return A
        mid = floor(n/2)
        left = mergesort(A[0..mid-1])
        right = mergesort(A[mid..n-1])
        return merge(left, right)

The runtime is covered in section (a/n: I didn't attend, but it's :math:`\Theta(n \log n)`).

Proving Mergesort
^^^^^^^^^^^^^^^^^
How do we prove its correctness? For D/C, induction - assume the recursive calls are correct, and prove correctness
given that.

Need to prove that for any list ``A[0..n-1]``, ``mergesort(A)`` is a sorted permutation of A. Prove by (strong)
induction on n.

- Base Case: :math:`n \leq 1`
    - Then ``mergesort(A) = A``
    - but A is already sorted.
- Inductive Case: Take any :math:`n > 1`, and assume the inductive hypothesis holds for all :math:`k < n`.
    - Then :math:`m = \lfloor n/2 \rfloor < n`
    - the length of the left half is :math:`m`
    - the length of the right half is :math:`(n-1)-m+1=n-m=n-\lfloor n/2 \rfloor=\lceil n/2 \rceil < n`
    - so ``B = mergesort(A[0..m-1])`` is a sorted permutation of ``A`` (by IH)
    - similarly, ``C = mergesort(A[m..n-1])`` is a sorted permutation of ``A``
    - since B and C are sorted, by correctness of ``merge()``, D is a sorted permutation of B concatenated with C
    - since B is a permutation of the ``A[0..m-1]`` and C is a permutation of ``A[m..n-1]``, D is a permutation of ``A[0..n-1]``

Designing
---------
Two questions:

- How do you divide the problem into subproblems?
- How do you combine the subproblem solutions to get an overall solution?

Integer Multiplication
----------------------
Given integers x, y, compute ``x * y``.

.. note::
    On a RAM machine, you can only multiply numbers in constant time if they fit in registers; otherwise you
    need an algorithm.

Elementary Multiplication
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: text

        37
    x  114
    ------
       148
       370
    + 3700
    ------
      4218

If *x* and *y* have *n* digits each, how long does this algorithm take? Each iteration takes linear time, and there are
linearly-many iterations, so :math:`\Theta(n^2)` in total.

Divide-and-Conquer
^^^^^^^^^^^^^^^^^^
Simple idea: split the digits in half, e.g. :math:`4216 = 42 * 10^2 + 16`

In general, given an :math:`\leq n`-digit integer *x*, :math:`x = x_1*10^{n/2}+x_0` (assuming *n* is a power of 2).

Likewise, we can write :math:`y = y_1*10^{n/2}+y_0`.

Then:

.. math::

    xy & = (x_1*10^{n/2}+x_0) * (y_1*10^{n/2}+y_0) \\
    & = x_1y_1*10^n + (x_1y_0+x_0y_1)*10^{n/2} + x_0y_0

This results in 4 multiplications of :math:`n/2`-digit numbers: do these recursively. The multiplications by
:math:`10^n` are just adding zeros, and the additions can be done in linear time.

So, as there are 4 recursive calls, we have:

.. math::

    T(n) = 4T(n/2) + \Theta(n)

By the master theorem, this results in :math:`T(n) = \Theta(n^2)` - which is not faster than the naive algorithm.

Gotta Go Fast
^^^^^^^^^^^^^
Let's look at

.. math::

    z & = (x_1+x_0)(y_1+y_0) \\
    & = x_1y_1 + (x_1y_0 + x_0y_1) + x_0y_0

Then :math:`x_1y_0 + x_0y_1 = z - x_1y_1 - x_0y_0`.

So with only 3 multiplications, we can get all 3 terms we need for the algorithm:

- :math:`x_1y_1`
- :math:`x_0y_0`
- :math:`(x_1+x_0)(y_1+y_0) = z`

and you can substitute into the earlier equation :math:`xy = x_1y_1*10^n + (z - x_1y_1 - x_0y_0)*10^{n/2} + x_0y_0`
to obtain the product.

.. math::

    & \text{KoratsubaMult}(x, y): \\
    & \text{If x and y fit in registers (e.g. } x, y < 2^{32} \text{), return } x*y. \\
    & \text{Split x into } x_1, x_0 \text{ s.t. } x = x_1*10^{n/2}+x_0 \text{ and } y = y = y_1*10^{n/2}+y_0 \\
    & z \leftarrow \text{KoratsubaMult}(x_1+x_0, y_1+y_0) \\
    & x_1y_1 \leftarrow \text{KoratsubaMult}(x_1, y_1) \\
    & x_0y_0 \leftarrow \text{KoratsubaMult}(x_0, y_0) \\
    & \text{return } x_1y_1*10^n + (z - x_1y_1 - x_0y_0)*10^{n/2} + x_0y_0

This results in a speedup at every recursive call, which adds up.

Now our recurrence is :math:`T(n) = 3T(n/2) + \Theta(n)`, which by the master theorem results in
:math:`T(n) = \Theta(n^{\log_2 3}) \approx \Theta(n^{1.58})`

This is faster than the naive algorithm!

Gotta Go Faster
^^^^^^^^^^^^^^^
There is, in fact, a faster algorithm that runs in :math:`O(n \log n \log \log n)` based on the Fast Fourier Transform -
but it only really starts being faster when you reach *n* in the thousands.

This algorithm is not covered in this class - see AD 5.6 or CLRS 30.

.. image:: https://what-if.xkcd.com/imgs/a/13/laser_pointer_more_power.png

Well.

In 2019, a :math:`O(n \log n)` algorithm was found, but the point at which it becomes faster is only for astronomically
large *n*.


