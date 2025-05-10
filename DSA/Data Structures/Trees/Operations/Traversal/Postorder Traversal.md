1. Traverse the left subtree.
2. Traverse the right subtree.
3. Visit the root.
# When to Use

## Node Deletion

When you delete nodes in a tree, deletion process will be in post-order. You will delete its left child and its right child before you delete the node itself.
## Parsing mathematical expressions

It is easier to write a program to parse a post-order expression. Here is an example:

![](https://leetcode.com/explore/learn/card/data-structure-tree/134/traverse-a-tree/Figures/binary_tree/mathematical_expression.png)

You can figure out the original expression with [[Inorder Traversal]]. But it's not easy for a program to handle the expression since you have to check the priorities of operations.

With postorder, you can easily handle the expression using a [[Stack]]. Each time you meet an operator, you pop 2 elements from the stack for the operands, calculate the result and push the result back into the stack.

# [[145. Binary Tree Postorder Traversal]]