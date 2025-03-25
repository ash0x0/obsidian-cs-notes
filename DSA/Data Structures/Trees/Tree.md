# Trees

A tree is a data structure to simulate a hierarchical tree structure.

Each node of the tree will have a root value and a list of references to other nodes which are called child nodes.
From graph view, a tree can also be defined as a directed acyclic graph which has `N nodes` and `N-1 edges`.
## Types

- [[Binary Tree]]
- [[Binary Search Tree (BST)]]
- [[Trie]]
- [[AVL Trees]]
- [[Red Black Tree]]

# [[Recursion]]

Recursion is one of the most powerful and frequently used techniques for solving tree problems.

A tree can be defined recursively as a node(the root node) that includes a value and a list of references to children nodes. Recursion is one of the natural features of a tree.

For each recursive function call, we only focus on the problem for the current node and call the function recursively to solve its children.

When you meet a tree problem, ask yourself two questions: 

1. Can you determine some parameters to help the node know its answer? 
	1. Can you use these parameters and the value of the node itself to determine what should be the parameters passed to its children? 
	2. If the answers are both yes, try to solve this problem using a "`top-down`" recursive solution.
2. For a node in a tree, if you know the answer of its children, can you calculate the answer of that node? 
	1. If the answer is yes, solving the problem recursively using a `bottom-up` approach might be a good idea.

For instance, consider this problem: 

> Given a binary tree, find its maximum depth.

## Top-Down Approach

"Top-down" means that in each recursive call, we will visit the node first to come up with some values, and pass these values to its children when calling the function recursively. 

This can be considered as a kind of [[Preorder Traversal]]. To be specific, the recursive function `top_down(root, params)` works like this:

1. return specific value for null node
2. update the answer if needed                                 // answer <-- params
3. left_ans = top_down(root.left, left_params)          // left_params <-- root.val, params
4. right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params
5. return the answer if needed                                  // answer <-- left_ans, right_ans

### Algorithm

1. Define the depth of the root node as 1 (although often, the depth of the root node is defined as 0). 
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
This can be regarded as a kind of [[Postorder Traversal]]. Typically, a "bottom-up" recursive function `bottom_up(root)` will be something like this:

1. return specific value for null node
2. left_ans = bottom_up(root.left)         // call function recursively for left child
3. right_ans = bottom_up(root.right)     // call function recursively for right child
4. return answers                                   // answer <-- left_ans, right_ans, root.val

For a single node of the tree, what will be the maximum depth `x` of the subtree rooted at itself?

If we know the maximum depth `l` of the subtree rooted at its **left** child and the maximum depth `r` of the subtree rooted at its **right** child, can we answer the previous question? 

We can choose the maximum between them and add 1 to get the maximum depth of the subtree rooted at the current node. That is `x = max(l, r) + 1`.

It means that for each node, we can get the answer after solving the problem for its children. Therefore, we can solve this problem using a "bottom-up" solution.

```cpp
int maximum_depth(TreeNode* root) {
    if (!root) return 0;
    int left_depth = maximum_depth(root->left);
    int right_depth = maximum_depth(root->right);
    return max(left_depth, right_depth) + 1;
}
```