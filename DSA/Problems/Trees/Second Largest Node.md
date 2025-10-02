#Dropbox #DailyCodingProblem #Medium 
# Problem

Given the root to a binary search tree, find the second largest node in the tree.

# Solution

## Reverse Inorder Traversal - [[O(h)]] time - [[O(h)]] space

In BST, inorder traversal gives sorted order. Reverse inorder gives descending order.
Second largest is the second node in reverse inorder traversal.

```cpp
class Solution {
private:
    int count = 0;
    int result = -1;
    
    void reverseInorder(TreeNode* node) {
        if (!node || count >= 2) return;
        
        // Right -> Root -> Left (descending order)
        reverseInorder(node->right);
        
        count++;
        if (count == 2) {
            result = node->val;
            return;
        }
        
        reverseInorder(node->left);
    }
    
public:
    int secondLargest(TreeNode* root) {
        reverseInorder(root);
        return result;
    }
};
```

# Algorithm

1. Traverse in reverse inorder (right-root-left)
2. First node visited is the largest
3. Second node visited is the second largest

# Example

```
BST:
      5
     / \
    3   7
   / \   \
  1   4   9

Reverse inorder: 9, 7, 5, 4, 3, 1
Second largest: 7
```

# Iterative Approach - [[O(h)]] time - [[O(1)]] space

```cpp
int secondLargest(TreeNode* root) {
    if (!root) return -1;
    
    TreeNode* curr = root;
    TreeNode* prev = nullptr;
    
    // Find largest (rightmost)
    while (curr->right) {
        prev = curr;
        curr = curr->right;
    }
    
    // Case 1: Largest has left subtree
    // Second largest is max in left subtree
    if (curr->left) {
        curr = curr->left;
        while (curr->right) {
            curr = curr->right;
        }
        return curr->val;
    }
    
    // Case 2: Largest is leaf
    // Second largest is parent
    return prev ? prev->val : -1;
}
```

# Special Cases

```
Case 1: Largest has left child
      5
     / \
    3   7
       /
      6

Largest: 7
Second: 6 (max in left subtree of 7)

Case 2: Largest is rightmost leaf
      5
     / \
    3   7
       
Largest: 7
Second: 5 (parent)

Case 3: Only one node
    5

No second largest
```

# Using BST Property

```cpp
int secondLargest(TreeNode* root) {
    if (!root || (!root->left && !root->right)) {
        return -1;  // Need at least 2 nodes
    }
    
    // If root has no right child, second largest is in left subtree
    if (!root->right) {
        return findMax(root->left);
    }
    
    // Find rightmost node
    TreeNode* parent = root;
    TreeNode* curr = root->right;
    
    while (curr->right) {
        parent = curr;
        curr = curr->right;
    }
    
    // If largest has left child, second largest is max in that subtree
    if (curr->left) {
        return findMax(curr->left);
    }
    
    // Otherwise, second largest is parent
    return parent->val;
}

int findMax(TreeNode* node) {
    while (node->right) {
        node = node->right;
    }
    return node->val;
}
```

# Time Complexity

[[O(h)]] where h is height of tree
- Best case (balanced): [[O(log n)]]
- Worst case (skewed): [[O(n)]]

# Space Complexity

- Recursive: [[O(h)]] for call stack
- Iterative: [[O(1)]]

# Related Problems

- [[Binary Search Tree]]
- Kth largest element in BST
- LeetCode #230 - Kth Smallest Element in BST
- Inorder traversal
