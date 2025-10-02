#DailyCodingProblem #Google #Medium 
# Problem

Given pre-order and in-order traversals of a binary tree, write a function to reconstruct the tree.

For example, given the following preorder traversal:

[a, b, d, e, c, f, g]

And the following inorder traversal:

[d, b, e, a, f, c, g]

You should return the following tree:

```
    a
   / \
  b   c
 / \ / \
d  e f  g
```

# Solution
# Solution

## Recursive Construction - [[O(n)]] time

Reconstruct tree from inorder and preorder (or postorder) traversals.

## From Inorder + Preorder

```cpp
class Solution {
private:
    unordered_map<int, int> inorderMap;
    int preorderIndex = 0;
    
    TreeNode* buildTreeHelper(vector<int>& preorder, int inStart, int inEnd) {
        if (inStart > inEnd) return nullptr;
        
        // Current root is at preorderIndex
        int rootVal = preorder[preorderIndex++];
        TreeNode* root = new TreeNode(rootVal);
        
        // Find root position in inorder
        int inorderRootIndex = inorderMap[rootVal];
        
        // Build left subtree (elements before root in inorder)
        root->left = buildTreeHelper(preorder, inStart, inorderRootIndex - 1);
        
        // Build right subtree (elements after root in inorder)
        root->right = buildTreeHelper(preorder, inorderRootIndex + 1, inEnd);
        
        return root;
    }
    
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        // Build map for O(1) lookup of inorder indices
        for (int i = 0; i < inorder.size(); i++) {
            inorderMap[inorder[i]] = i;
        }
        
        preorderIndex = 0;
        return buildTreeHelper(preorder, 0, inorder.size() - 1);
    }
};
```

# Algorithm

**Key Insights**:
1. **Preorder**: Root is always first element
2. **Inorder**: Elements left of root are in left subtree, right are in right subtree

**Steps**:
1. Take next element from preorder as root
2. Find root in inorder to determine left/right subtree sizes
3. Recursively build left subtree with elements before root in inorder
4. Recursively build right subtree with elements after root in inorder

# Example

```
Preorder: [3, 9, 20, 15, 7]
Inorder:  [9, 3, 15, 20, 7]

Step 1: Root = 3 (first in preorder)
        In inorder: [9] | 3 | [15, 20, 7]
        Left subtree: [9]
        Right subtree: [15, 20, 7]

Step 2: Build left subtree
        Preorder: [9] (next from original)
        Inorder: [9]
        Root = 9 (leaf)

Step 3: Build right subtree
        Preorder: [20, 15, 7]
        Inorder: [15, 20, 7]
        Root = 20
        In inorder: [15] | 20 | [7]
        
Step 4: Build left child of 20
        Root = 15 (leaf)
        
Step 5: Build right child of 20
        Root = 7 (leaf)

Result:
     3
    / \
   9  20
      / \
    15   7
```

## From Inorder + Postorder

```cpp
class Solution {
private:
    unordered_map<int, int> inorderMap;
    int postorderIndex;
    
    TreeNode* buildTreeHelper(vector<int>& postorder, int inStart, int inEnd) {
        if (inStart > inEnd) return nullptr;
        
        // Current root is at end of postorder
        int rootVal = postorder[postorderIndex--];
        TreeNode* root = new TreeNode(rootVal);
        
        int inorderRootIndex = inorderMap[rootVal];
        
        // Build RIGHT subtree first (postorder processes right before left)
        root->right = buildTreeHelper(postorder, inorderRootIndex + 1, inEnd);
        
        // Then build LEFT subtree
        root->left = buildTreeHelper(postorder, inStart, inorderRootIndex - 1);
        
        return root;
    }
    
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        for (int i = 0; i < inorder.size(); i++) {
            inorderMap[inorder[i]] = i;
        }
        
        postorderIndex = postorder.size() - 1;
        return buildTreeHelper(postorder, 0, inorder.size() - 1);
    }
};
```

# Time Complexity

[[O(n)]] - each node processed once, O(1) lookups with hash map

# Space Complexity

[[O(n)]] - hash map and recursion stack

# Important Notes

- Cannot reconstruct from preorder + postorder alone (without inorder)
- Need inorder + (preorder OR postorder) to uniquely determine tree
- Assumes all values are unique

# Related Problems

- LeetCode #105 - Construct Binary Tree from Preorder and Inorder
- LeetCode #106 - Construct Binary Tree from Inorder and Postorder
- [[Tree Traversal]]
- [[Recursion]]
