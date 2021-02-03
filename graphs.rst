Graphs
======
A graph is a pair :math:`(V, E)` where :math:`V` is the set of vertices (nodes) and :math:`E \subseteq V \times V` is
the set of edges (i.e. pairs :math:`(u, v)` where :math:`u, v \in V`).

.. image:: _static/graphs1.png
    :width: 250

.. note::
    The image above is a directed graph (digraph), where the direction of the nodes matter. In an undirected graph,
    the direction doesn't matter, so :math:`\forall u,v \in V, (u, v) \in E \iff (v, u) \in E`.

For a vertex :math:`v \in V`, its *outdegree* is the number of edges going out from it, and its *indegree* is
the number of edges going into it.

In an undirected graph, this is simplified to *degree*: the number of edges connected to an edge.

A *path* in a graph :math:`G = (V, E)` is a sequence of vertices :math:`v_1, v_2,... ,v_k \in V` where
for all :math:`i < k, (v_i, v_{i+1}) \in E` (i.e. there is an edge from each vertex in the path to the next).

.. image:: _static/graphs2.png
    :width: 250

A path is *simple* if it contains no repeated vertices.

A *cycle* is a nontrivial path starting and ending at the same vertex. A graph is *cyclic* if
it contains a cycle, or *acyclic* if not.

.. note::
    For a digraph, nontrivial means a path of at least 2 nodes. For an undirected graph, it's at least 3
    (cannot use the same edge to go back and forth)

**Ex.** This digraph is acyclic: :math:`(x, y, z, w, x)` is not a path, since there is no path from *w* to *x*.

.. image:: _static/graphs3.png
    :width: 250

Some more examples:

.. image:: _static/graphs4.png
    :width: 750

A graph is *connected* if :math:`\forall u, v \in V`, there is a path from *u* to *v* or vice versa.

A digraph is *strongly connected* if :math:`\forall u, v \in V`, there is a path from *u* to *v*. (For undirected
graphs, connected = strongly connected.)

.. note::
    For a directed graph mith more than one vertex, being strongly connected implies that the graph is cyclic,
    since a path from *u* to *v* and vice versa exists.

.. image:: _static/graphs5.png
    :width: 500

Algorithms
----------

S-T Connectivity Problem
^^^^^^^^^^^^^^^^^^^^^^^^
Given vertices :math:`s, t \in V`, is there a path from *s* to *t*? (Later: what's the shortest such path?)

.. image:: _static/graphs6.png
    :width: 250

BFS
^^^

.. image:: https://imgs.xkcd.com/comics/depth_and_breadth_2x.png
    :width: 350

Traverse graph starting from a given vertex, processing vertices in the order they can be reached. Only visit each
vertex once, keeping track of the set of already visited vertices. Often implemented using a FIFO queue.

The traversal can be visualized as a tree, where we visit nodes in top-down, left-right order.

.. image:: _static/graphs7.png
    :width: 250

.. code-block:: c

    BFS(v):
        Initialize a FIFO queue with a single item v
        Initialize a set of seen vertices with a single item v
        while the queue is not empty:
            u = pop the next element in the queue
            Visit vertex u
            for each edge (u, w):
                if w is not already seen:
                    add it to the queue
                    add it to set of seen vertices

.. note::
    When we first add a vertex to the queue, we mark it as seen, so it can't be added to the queue again. Since there
    are only finitely-many vertices, BFS will eventually terminate.

    Each edge gets checked at most once (in each direction), so if there are *n* vertices and *m* edges, the runtime
    is :math:`\Theta(n+m)` (assuming you can get the edges out from a vertices in constant time).

DFS
^^^
Traverse like BFS, except recursively visit all descendants of a vertex before moving on to the next one (at the same
depth). This can be implemented without a queue, only using the call stack.

.. image:: _static/graphs8.png
    :width: 150

.. code-block:: c

    DFS(v):
        mark v as visited
        for each edge (v, u):
            if u is not visited, DFS(u)

.. note::
    As with DFS, we visit each vertex at most once, and process each edge at most once in each direction, so runtime
    is :math:`\Theta(n+m)`.

    We call :math:`\Theta(n+m)` linear time for graphs with *n* vertices and *m* edges.

Representing Graphs
-------------------

- Adjacency lists: for each vertex, have a list of outgoing edges
    - Allows for linear iteration over all edges from a vertex
    - Checking if a particular edge exists is expensive
    - Size for *n* vertices, *m* edges = :math:`\Theta(n + m)`
- Adjancency matrix: a matrix with entry :math:`(i, j)` indicating an edge from *i* to *j*
    - Constant time to check if a particular edge exists
    - but the size based on number of vertices is :math:`\Theta(n^2)`

.. image:: _static/graphs9.png
    :width: 350

A graph with *n* vertices has :math:`\leq {n \choose 2}` edges if undirected, or :math:`\leq n^2` if directed and
we allow self-loops (or :math:`\leq n(n-1)` if no self loops).

In all cases, :math:`m=O(n^2)`. This makes the size of an adjacency list :math:`O(n^2)`.

If we really have a ton of edges, the two representations are the same; but if you have a *sparse* graph
(:math:`m << n^2`), then the adjacency list is more memory-efficient.

This means that a :math:`\Theta(n^2)` algorithm can run in linear time (:math:`\Theta(n+m)`) on *dense* graphs
because :math:`m = \Theta(n^2)`.

So :math:`\Theta(n^2)` is optimal for dense graphs, but not for sparse graphs.

More Problems
-------------

Cyclic Test
^^^^^^^^^^^
*testing if a graph is cyclic*

- use DFS to traverse, keep track of vertices visited along the way
- if we find an edge back to any such vertex, we have a cycle

Topological Order
^^^^^^^^^^^^^^^^^
- Finding a topological ordering of a DAG (directed acyclic graph)
    - an ordering :math:`v_1, v_2, v_n` of the vertices such that if :math:`(v_i, v_j) \in E`, then :math:`i < j`
    - we can find topological order using *postorder traversal* in DFS
        - after DFS has visited all descendants of a vertex, prepend the vertex to the topological order
    - make sure to iterate all connected components of the graph, not just one

.. image:: _static/graphs10.png
    :width: 250

.. code-block:: c

    def Topo-Sort(G):
        for v in V:
            if v is not visited:
                Visit(v)

    def Visit(v):
        mark v as visited
        mark v as in progress // temporarily
        for edges (v, u) in E:
            if u is in progress, G is cyclic // there is no topological order
            if u is not visited:
                Visit(u)
        mark v as not in progress
        prepend v to the topological order

.. image:: _static/graphs11.png
    :width: 250

Minimum Spanning Tree
^^^^^^^^^^^^^^^^^^^^^
Suppose you need to wire all houses in a town together (represented as a graph, and the edges represent where power
lines can be built), Build a power line to use the minimal amount of wire.

.. image:: _static/graphs12.png
    :width: 350

Given a weighted graph, (i.e. each edge has a numerical weight), we want to find a minimum spanning tree:
a selection of edges of G that connects every vertex, is a tree (any cycles are unnecessary), and has least total
weight.