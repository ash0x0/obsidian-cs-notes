---
aliases:
  - Breadth-First Traversal
  - Breadth-First Search
  - BFS
---
We scan through the tree level by level, following the order of height, from top to bottom. The nodes on higher levels would be visited before the ones with lower levels.

The algorithm starts with a root node and visit the node itself first. Then traverse its neighbors, traverse its second level neighbors, traverse its third level neighbors, so on and so forth.

When we do breadth-first search in a tree, the order of the nodes we visited is in level order.

Typically, we use a [[Queue]] to help us to do BFS.

# [[102. Binary Tree Level Order Traversal]]