A data structure that simulate a hierarchical tree structure.

- Each node in the tree has a value and a list of references to other nodes, called child nodes.
- At most 1 reference can point to any node in the tree.
- No node in the tree points toward the root.
- A node's depth in a tree is the number of edges from the tree's root to the node. A root node has a depth of `0.
- A node's height in a tree is the number of edges on the longest path from the node to a leaf. A leaf node has a height of `0`
- From graph view, a tree can also be defined as a special case of a [[Directed Acyclic Graph]] which has `n` nodes and `n-1` edges.
- Full binary tree is a tree in which every non-leaf node has 2 children
- Complete binary tree is a tree in which every level is completely filled (except last level) and all nodes are as far left as possible, i.e. the tree is as compact as possible
- Full and Complete binary tree is a tree in which all leaves are at the bottom and each non-leaf node has exactly 2 children
- Perfect tree is a tree which has `h = O(log n)` because h = log<sub>2</sub>(n+1) - 1

# [[Recursion]]

Recursion is one of the natural features of a tree.
A tree can be defined recursively as a node (root node) that includes a value and a list of references to children nodes. 

For each recursive call, we focus on the problem for the current node and call the function recursively to solve its children.

When you meet a tree problem, ask yourself  

1. Can you determine some parameters to help the node know its answer? 
	1. Can you use these parameters and the value of the node itself to determine what should be the parameters passed to its children? 
	2. If the answers are both yes, try to solve this problem using a "`top-down`" recursive solution.
2. For a node in a tree, if you know the answer of its children, can you calculate the answer of that node? 
	1. If the answer is yes, solving the problem recursively using a `bottom-up` approach might be a good idea.

## Example Problem

> Given a binary tree, find its maximum depth.

## Top-Down Approach

"Top-down" means that in each recursive call, we will visit the node first to come up with some values, and pass these values to its children when calling the function recursively. 

This can be considered [[Preorder Traversal]].

1. return specific value for null node
2. update the answer if needed                                 // answer <-- params
3. left_ans = top_down(root.left, left_params)          // left_params <-- root.val, params
4. right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params
5. return the answer if needed                                  // answer <-- left_ans, right_ans

### Algorithm

1. Define the depth of the root node as 1 (the depth of the root node is usually 0). 
2. For each node, if we know its depth, we will know the depth of its children. 
3. If we pass the depth of the node as a parameter when calling the function recursively, all the nodes will know their depth. 
4. For leaf nodes, we can use the depth to update the final answer. 

```cpp
int answer; // don't forget to initialize the answer before calling maximum_depth
void maximum_depth(TreeNode* root, int depth) {
    if (!root) return;
    if (!root->left && !root->right) {
        answer = max(answer, depth);
    }
    maximum_depth(root->left, depth + 1);
    maximum_depth(root->right, depth + 1);
}
```
## Bottom-Up Approach

In each recursive call, we will first call the function recursively for all the children nodes and then come up with the answer according to the returned values and the value of the current node itself. 

This can be considered [[Postorder Traversal]]. 

1. return specific value for null node
2. left_ans = bottom_up(root.left)         // call function recursively for left child
3. right_ans = bottom_up(root.right)     // call function recursively for right child
4. return answers                                   // answer <-- left_ans, right_ans, root.val

For a single node of the tree, what will be the maximum depth `x` of the subtree rooted at itself?

If we know the maximum depth `l` of the subtree rooted at its **left** child and the maximum depth `r` of the subtree rooted at its **right** child, can we answer the question? 

We can choose the maximum between them and add 1 to get the maximum depth of the subtree rooted at the current node. That is `x = max(l, r) + 1`.

It means that for each node, we can get the answer after solving the problem for its children. Therefore, this is a "bottom-up" solution.

```cpp
int maximum_depth(TreeNode* root) {
    if (!root) return 0;
    int left_depth = maximum_depth(root->left);
    int right_depth = maximum_depth(root->right);
    return max(left_depth, right_depth) + 1;
}
```

# In-order Successor

This refers to the next node in an [[Inorder Traversal]]. The node with the smallest key that's still greater than the current node's key.