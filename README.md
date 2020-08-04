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
1. if u is a leaf then
2.      report all points stored at u that are covered by r
3. else
4.      for each child v of u do
5.          if MBR(v) intersects r then
6.              range-query(v,r)
```
### R-Tree Construction: A Common Principle
In general, the construction algorithm of the R-tree aims at minimizing the **perimeter sum** of all the MBRs
### Insertion
Let **p** be the point being inserted
```
Algorithm insert(u, p)
1. if u is a leaf node then
2.     add p to u
3.     if u overflows then
        /* namely, u has B + 1 points */
4.          handle-overflow(u)
5. else
6.     v ← choose-subtree(u, p)
        /* which subtree under u should we insert p into? */
7.     insert(v, p)
insert(root,p)
```
### Choose-subtree
Return the child whose MBR requires the **minimum** increase in perimeter to cover p.
Break ties by favoring the smallest MBR
### Overflow Handling
```
Algorithm handle-overflow(u)
1. split(u) into u and u'
2. if u is the root then
3.      create a new root with u and u' as its child nodes
4. else
5.      w ← the parent of u
6.      update MBR(u) in w
7.      add u' as a child of w
8.      if w overflows then
9.          handle-overflow(w)

```
