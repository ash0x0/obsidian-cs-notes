# Problem

Given the root of a binary tree, invert/reverse the tree by swapping the left and right children of every node.

Example:
```
Input:           Output:
     4              4
   /   \          /   \
  2     7        7     2
 / \   / \      / \   / \
1   3 6   9    9   6 3   1
```

This is a famous problem that became popular after a software engineer was rejected from an interview for not being able to solve it.

# Solution

## Recursive Approach

The simplest solution is to recursively swap left and right children.

```cpp
TreeNode* invertTree(TreeNode* root) {
    if (root == nullptr) {
        return nullptr;
    }
    
    // Swap left and right children
    TreeNode* temp = root->left;
    root->left = root->right;
    root->right = temp;
    
    // Recursively invert subtrees
    invertTree(root->left);
    invertTree(root->right);
    
    return root;
}
```

**Time Complexity**: [[O(n)]] - visit each node once
**Space Complexity**: [[O(h)]] - recursion stack, where h is height

## Iterative Approach (BFS)

Use a queue to process nodes level by level.

```cpp
TreeNode* invertTree(TreeNode* root) {
    if (root == nullptr) {
        return nullptr;
    }
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        TreeNode* current = q.front();
        q.pop();
        
        // Swap children
        TreeNode* temp = current->left;
        current->left = current->right;
        current->right = temp;
        
        // Add children to queue
        if (current->left) q.push(current->left);
        if (current->right) q.push(current->right);
    }
    
    return root;
}
```

**Time Complexity**: [[O(n)]]
**Space Complexity**: [[O(w)]] - where w is max width of tree

## Iterative Approach (DFS)

Use a stack for depth-first traversal.

```cpp
TreeNode* invertTree(TreeNode* root) {
    if (root == nullptr) {
        return nullptr;
    }
    
    stack<TreeNode*> s;
    s.push(root);
    
    while (!s.empty()) {
        TreeNode* current = s.top();
        s.pop();
        
        // Swap children
        swap(current->left, current->right);
        
        // Add children to stack
        if (current->left) s.push(current->left);
        if (current->right) s.push(current->right);
    }
    
    return root;
}
```

**Time Complexity**: [[O(n)]]
**Space Complexity**: [[O(h)]]

# Why This Problem is Famous

This problem gained notoriety in 2015 when Max Howell, the creator of Homebrew (a popular package manager), tweeted about being rejected by Google for not solving this problem on a whiteboard, despite having created widely-used open-source software. The incident sparked debate about interview practices in tech companies.

# Related Problems

- [[Symmetric Tree]] - Check if tree is mirror of itself
- [[Tree Traversal]]
- [[Binary Tree]] operations

# Tags

#Easy #BinaryTree #DFS #BFS #Recursion #Google
