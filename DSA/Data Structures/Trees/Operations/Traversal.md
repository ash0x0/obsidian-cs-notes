  
# Types

For the tree [1, 2, 3, 4, 5]

![[Pasted image 20250324162551.png]]
## Depth-First Search (DFS)

Start from a root and reach all the way down to a certain leaf, and then back to the root to reach another branch.

- [[Preorder Traversal]]
- [[Inorder Traversal]]
- [[Postorder Traversal]]

> **Note**: It is easy to do traversal with [[Recursion]] but when the depth of the tree is too large, we might suffer from `stack overflow`. That's one of the main reasons why we want to traverse iteratively with [[Stack]].

## Breadth-First Search (BFS)

Scan through the tree level by level, following the order of height, from top to bottom. The nodes on higher levels would be visited before the ones with lower levels.

- [[Level Order Traversal]]
- [[[Zig-Zag Traversal]]

[^1]: 
