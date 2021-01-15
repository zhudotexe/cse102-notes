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


