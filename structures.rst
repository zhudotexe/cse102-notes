Data Structures
===============

Priority Queues
---------------
A set of items with a numerical "key" giving its priority in the queue (smaller key = higher priority).

Useful when we want to track several items to process, like FIFO queue, but we want to be able to insert high-priority
items that get handled before elements already in the queue.

Operations
^^^^^^^^^^

- Insert: add a new item *x* with a key *k*
- Find-Min: find the item with the least key value
- Delete-Min: remove the item with the least key value
- Delete: remove a particular item *x* from the queue given a pointer to it
- Decrease-Key: change the key of item *x* to a smaller *k*
- Merge/Meld/Join (rarely): combine 2 disjoint priority queues

.. note::
    Certain textbooks will use higher key as higher priority, rather than lower. Formally, this is "max-priority" vs
    "min-priority" queues.

Implementations
^^^^^^^^^^^^^^^

.. note::
    In the table below, :math:`O(n)` is assumed as a tight upper bound, i.e. :math:`\Theta(n)` so I don't have to type
    ``\Theta`` each time.

+----------------------+----------+----------+----------+------------+--------------+
| Implementation       | Insert   | Delete   | Find-Min | Delete-Min | Decrease-Key |
+======================+==========+==========+==========+============+==============+
| Unsorted Linked List | O(1)     | O(1)     | O(n)     | O(n)       | O(1)         |
+----------------------+----------+----------+----------+------------+--------------+
| Sorted Linked List   | O(n)     | O(1)     | O(1)     | O(1)       | O(n)         |
+----------------------+----------+----------+----------+------------+--------------+
| Heap (min-heap*)     | O(log n) | O(log n) | O(1)     | O(log n)   | O(log n)     |
+----------------------+----------+----------+----------+------------+--------------+
| Fibonacci Heap       | O(1)     | O(log n) | O(1)     | O(log n)** | O(1)**       |
+----------------------+----------+----------+----------+------------+--------------+

*: A tree where each node's parent has a smaller value than it.

**: Time averaged over a larger sequence of operations (amortized).

The best implementation to use depends on how the priority queue is being used in your algorithm. We only discuss
the top 3 implementations in this class.

Amortized Runtimes
------------------
Instead of finding worst-case bounds for individual operations, find the worst case over *any sequence* of *m*
operations and average over them. This can yield better bounds if the worst case for an individual operation can't
happen every time.

.. note::
    Ex: automatically-expanding arrays (C++ vector, Python list)

    Allocate a fixed-size array to store a list; when it fills up, allocate an array of twice the size and copy
    the old array into it.



