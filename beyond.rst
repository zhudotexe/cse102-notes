Beyond 102
==========

Interactable Problems and Optimal Runtimes
------------------------------------------
How to tell whether an algorithm is as fast as possible?

We need a *lower bound* on how many steps *any* algorithm needs to solve the problem - the subject of
*complexity theory*. There are problems for which you can *prove* that no poly-time algorithm exists; however, there
are many open problems in complexity theory, e.g. P v. NP

**Ex**: Boolean Satisfiability Problem (SAT): given a Boolean formula built up from Boolean variables with AND, OR, and
NOT, is it possible for the formula to evaluate to true?

- ``(x OR y) AND (NOT x)`` is satisfiable (x=false, y=true)
- ``x AND (NOT x)`` is not

This problem has lots of practical applicattions, e.g. software verification and other formal methods problems. But
there's no known poly-time algorithm! In fact, :math:`SAT \in NP` since checking if a given assignment satisfies the
formula is easy. Can prove that if :math:`SAT \in P`, then :math:`P=NP`.

.. note::
    In 103, we only examine P/NP; a current area of research is *fine-grained complexity*, which looks at differences
    between :math:`n^2` and :math:`n^3` time. For example, there is some evidence that edit distance can't be
    computed in less than :math:`n^{2-\epsilon}` time.