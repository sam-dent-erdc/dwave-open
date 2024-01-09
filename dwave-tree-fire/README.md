# Tree Clumps Fire

This notebook simulates the process of determining which tree clumps to target to prevent fire spread in a forest. The trees are represented by a graph where nodes are the tree clumps and the edges are connections though which fire can spread. The separation is achieved though the use of the D-Wave hybrid solver performing Quadratic Unconstrained Binary Optimization (QUBO), based on the Immunization Strategy example available through D-Wave (https://github.com/dwave-examples/immunization-strategy). The goal is to assign each node into two main groups and a separator group in such a way that no edges exist between the two main groups, a situation which we will call *complete separation*. The results for four variations are compared:
1. Setting $k$, the number of separator nodes, and starting with $k=1$, assign nodes to groups in an attempt to achieve complete separation. Increase the number of separators by one each iteration until complete separation is achieved. Say complete separation is achieved at $k=n$. 
    * Note that each itertion does not depend on the previous one, i.e. the separator nodes for $k$ do not depend on those for $(k-1)$
2. Calculate *hubness* for each node (discussed in the Optimization section). Repeat the optimization for $k=1,\ldots,n$ but set the separator nodes based on descending order of hubness, i.e. use the $k$ nodes with the highest hubness as the separators. 
    * Note that 1. did not set the separator nodes, just the number of separator nodes, whereas here we are assigning the separator nodes before the optimization.
3. Repeat the optimization for $k=1,\ldots,n$ but set the separator nodes by randomly selecting from the final optimal set of $n$ nodes.
4. Repeat the optimization for $k=1,\ldots,n$ but set the separator nodes from the final optimal set of $n$ nodes based on descending order of hubness. 
    * Note that 2. did not require the nodes to be from the final optimal set

Specifically, we will compare the number of *illegal edges*, those connecting the two main groups.

To run this notebook you will need:
* A D-Wave API key from the D-Wave Leap IDE (https://cloud.dwavesys.com/leap/login/?next=/leap/)
* File for the tree clumps graph (here called `fire_graph.pkl`)
* If you wish to visualize the graph on a map,
    * File for the coordinates of the graph nodes (here called `fire_centroids.pkl`)
    * Coordinates for the bounding box of the graph on a map, of the form `active_zone_bounds =  ((long1, long2),(lat1, lat2))`