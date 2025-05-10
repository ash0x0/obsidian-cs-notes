We scan through the tree level by level, following the order of height, from top to bottom. The nodes on higher levels would be visited before the ones with lower levels.

The algorithm starts with a root node and visit the node itself first. Then traverse its neighbors, traverse its second level neighbors, traverse its third level neighbors, so on and so forth.

When we do breadth-first search in a tree, the order of the nodes we visit is in level order.

Typically, we use a [[Queue]] to keep track of all the nodes of a level before jumping onto the next level.
# [[102. Binary Tree Level Order Traversal]]