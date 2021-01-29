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