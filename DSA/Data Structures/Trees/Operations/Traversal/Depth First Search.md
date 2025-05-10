Traverse a tree going all the way to a leaf then back to the root to get to another branch. Usually using a stack to keep track of the parent nodes.

> **Note**: It is easy to do traversal with [[Recursion]] but when the depth of the tree is too large, we might suffer from `stack overflow`. That's one of the main reasons why we want to traverse iteratively with [[Stack]].
# Mechanism

1. Start at the root of the tree
2. If the node is not a leaf you need to do one of three things
	1. Process the current node now ([[Preorder Traversal]])
	2. Between processing two children ([[Inorder Traversal]])
	3. After processing both children ([[Postorder Traversal]])
3. Make two recursive calls for both the children of the current node to process them.
# When to use

1. If the problem requires searching for something where the node is closer to a leaf