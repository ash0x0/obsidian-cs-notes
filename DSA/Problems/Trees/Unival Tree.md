#DailyCodingProblem #Google #Easy 
# Problem

A unival tree (which stands for "universal value") is a tree where all nodes under it have the same value.

Given the root to a binary tree, count the number of unival subtrees.

For example, the following tree has 5 unival subtrees:

```
   0
  / \
 1   0
    / \
   1   0
  / \
 1   1
```
# Solution

## Recursive DFS - [[O(n)]] time - [[O(h)]] space

Count unival subtrees by checking if all nodes in subtree have same value.

```cpp
class Solution {
private:
    int count = 0;
    
    // Returns true if subtree is unival, false otherwise
    bool isUnival(TreeNode* node) {
        if (!node) return true;
        
        bool leftUnival = isUnival(node->left);
        bool rightUnival = isUnival(node->right);
        
        // Check if current node's children (if exist) have same value
        if (node->left && node->left->val != node->val) {
            leftUnival = false;
        }
        if (node->right && node->right->val != node->val) {
            rightUnival = false;
        }
        
        // If both subtrees are unival and match current node
        if (leftUnival && rightUnival) {
            count++;
            return true;
        }
        
        return false;
    }
    
public:
    int countUnivalSubtrees(TreeNode* root) {
        count = 0;
        isUnival(root);
        return count;
    }
};
```

# Algorithm

For each node:
1. Recursively check if left subtree is unival
2. Recursively check if right subtree is unival
3. Check if children (if exist) have same value as current node
4. If both subtrees are unival AND children match current node, increment count

# Example Walkthrough

```
Tree:
     5
    / \
   1   5
  / \   \
 5   5   5

Analysis:
- Node(5) [bottom-left]: unival ✓ (leaf) → count = 1
- Node(5) [middle-left]: unival ✓ (leaf) → count = 2
- Node(1): children don't match (5 != 1) → not unival
- Node(5) [bottom-right]: unival ✓ (leaf) → count = 3
- Node(5) [middle-right]: children match ✓ → count = 4
- Node(5) [root]: left child is 1 ≠ 5 → not unival

Total: 4 unival subtrees
```

# Given Example

```
     0
    / \
   1   0
      / \
     1   0
    / \
   1   1

Unival subtrees:
1. Node(1) bottom-left-left: leaf
2. Node(1) bottom-left-right: leaf
3. Node(1) middle-left: both children are 1
4. Node(0) bottom-right: leaf
5. Node(0) middle-right: left child is 1 ≠ 0, not unival

Total: 5 unival subtrees
```

# Time Complexity

[[O(n)]] - visit each node once

# Space Complexity

[[O(h)]] - recursion stack depth = tree height

# Alternative: Single Pass with Return Value

```cpp
class Solution {
private:
    int count = 0;
    
    // Returns {isUnival, value}
    pair<bool, int> helper(TreeNode* node) {
        if (!node) return {true, 0};
        
        if (!node->left && !node->right) {
            count++;
            return {true, node->val};
        }
        
        auto [leftUnival, leftVal] = helper(node->left);
        auto [rightUnival, rightVal] = helper(node->right);
        
        bool isUnival = leftUnival && rightUnival;
        
        if (node->left && leftVal != node->val) isUnival = false;
        if (node->right && rightVal != node->val) isUnival = false;
        
        if (isUnival) count++;
        
        return {isUnival, node->val};
    }
    
public:
    int countUnivalSubtrees(TreeNode* root) {
        count = 0;
        helper(root);
        return count;
    }
};
```

# Related Problems

- LeetCode #250 - Count Univalue Subtrees
- [[Tree Traversal]]
- [[Recursion]]
- Same tree problem
