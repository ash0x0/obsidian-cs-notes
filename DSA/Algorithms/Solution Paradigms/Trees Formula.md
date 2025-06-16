# Examples

## Total Nodes

> Given the root to a binary tree, count the total number of nodes there are.

Solving any binary tree question involves just two steps.

## Solving the base case

This usually means solving the leaf node case (a leaf node has no left or right children) or the null case. 
For the above problem, we can see that a null should represent 0 nodes while a leaf node should represent 1 node.

## Recursive step

Assuming you knew the solution to the left subtree and the right subtree, how could you combine the two results to give you the final solution? It’s important to not get caught up on how this works and just have faith that it works. If you start tracing the recursion, you’re going to needlessly use up time and energy during the interview. 

Intuitively though, it works for similar reasons as why regular induction works.  P(0) or the base case works which causes P(1) or the leaf node to work which causes P(2) to work and so on. 

For this problem, it’s easy to combine the results of the left and right subtrees. Just add the two numbers and then another 1 for the root. Here’s the code:

```python
def count(node):
  return count(node.left) + count(node.right) + 1 if node else 0
```

## Deepest Node

> Given the root to a binary tree, return the deepest node.

Base case for this question actually can’t be null, because it’s not a real result that can be combined (null is not a node). Here we should use the leaf node as the base case and return itself.

The recursive step for this problem is a little bit different because we can’t actually use the results of the left and right subtrees directly. So we need to ask, what other information do we need to solve this question? It turns out if we tagged with each sub-result node their depths, we could get the final solution by picking the higher depth leaf and then incrementing it:

```python
def deepest(node):
    if node and not node.left and not node.right:
        return (node, 1) # Leaf and its depth

    if not node.left: # Then the deepest node is on the right subtree
        return increment_depth(deepest(node.right))
    elif not node.right: # Then the deepest node is on the left subtree
        return increment_depth(deepest(node.left))

    return increment_depth(
            max(deepest(node.left), deepest(node.right),
                    key=lambda x: x[1])) # Pick higher depth tuple and then increment its depth

def increment_depth(node_depth_tuple):
    node, depth = node_depth_tuple
    return (node, depth + 1)
```