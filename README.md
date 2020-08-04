# DataStructures
## R- tree
R-tree can be consider as a multi-dimensional extension of the B-tree

The R-tree supports efficiently a variety of queires, and is implemented in numerous database systems.

### 2D Orthogonal Range Reporting

Let S be a set of points in 2D space. Given an axis-parallel rectangle q, a range quey returns all the points of S that are covered by q

- Each leaf node has between 0.4B and B data points, where B >= 3 is a parameter. The only exception applies when the leaf is the root, in which case it is allowed to have between 1 and B points. All the leaf nodes are at the same level.
- Each internal node has between 0.4B and B child nodes, except when the node is the root, in which casr it needs to have at least 2 child nodes.

For any node **u**, denote by **S_u** the set of points in the subtree of **u**.
Consider now **u** to be an internal node with child nodes **v_1, ..., v_f** ***(f ≤ B)***.
For each **v_i (i ≤ f )**, **u** stores the ***minimum bounding rectangle(MBR)*** of **S_vi**, denoted as **MBR(v_i)**

### Answering a Range Query
**Algorithm** range-query(u,r)
```
1. **if** u is a leaf **then**
2.      report all points stored at u that are covered by r
3. **else**
4.      **for** each child v of u **do**
5.          **if** MBR(v) intersects r **then**
6.              range-query(v,r)
```

