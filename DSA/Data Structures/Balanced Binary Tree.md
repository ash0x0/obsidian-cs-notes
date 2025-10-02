A Balanced Binary Tree is a [[Binary Tree]] where the height difference between the left and right subtrees of any node is at most 1. This ensures [[O(log n)]] time complexity for most operations.

# Definition

A binary tree is balanced if:
- The height of left and right subtrees of every node differ by at most 1
- Both left and right subtrees are also balanced

# Why Balance Matters

Unbalanced trees can degrade to linked lists with [[O(n)]] operations. Balanced trees maintain [[O(log n)]] performance.

```
Unbalanced:          Balanced:
    1                   4
     \                 / \
      2               2   6
       \             / \
        3           1   3
         \
          4
```

# Checking if Tree is Balanced

```cpp
int height(Node* node) {
    if (node == nullptr) return 0;
    
    int leftHeight = height(node->left);
    if (leftHeight == -1) return -1;
    
    int rightHeight = height(node->right);
    if (rightHeight == -1) return -1;
    
    if (abs(leftHeight - rightHeight) > 1) {
        return -1;  // Not balanced
    }
    
    return 1 + max(leftHeight, rightHeight);
}

bool isBalanced(Node* root) {
    return height(root) != -1;
}
```

# Time Complexity

- Check if balanced: [[O(n)]]
- Height calculation: [[O(n)]]

# Self-Balancing Trees

Trees that automatically maintain balance during insertions and deletions:

## AVL Tree
- **Balance Factor**: |height(left) - height(right)| ≤ 1
- **Rotations**: Single and double rotations to maintain balance
- **Strictly balanced**: Fastest searches
- **Time**: Insert [[O(log n)]], Delete [[O(log n)]], Search [[O(log n)]]

## Red-Black Tree
- **Colors**: Each node is red or black
- **Rules**: Root is black, red nodes have black children, all paths have same number of black nodes
- **Less strict**: Faster insertions/deletions
- **Used in**: std::map, std::set in C++
- **Time**: Insert [[O(log n)]], Delete [[O(log n)]], Search [[O(log n)]]

# Rotation Operations

Used to rebalance trees:

## Left Rotation
```
    y                x
   / \    ←         / \
  x   C             A   y
 / \                   / \
A   B                 B   C
```

## Right Rotation
```
  x                  y
 / \      →        / \
y   C             A   x
   / \               / \
  A   B             B   C
```

# Applications

- Database indexing (B-trees, B+ trees)
- Memory management
- Filesystem organization
- Compiler symbol tables
- Network routing tables

# Comparison

| Tree Type | Balance | Search | Insert | Delete |
|-----------|---------|--------|--------|--------|
| Unbalanced BST | No | [[O(n)]] | [[O(n)]] | [[O(n)]] |
| AVL Tree | Strict | [[O(log n)]] | [[O(log n)]] | [[O(log n)]] |
| Red-Black Tree | Relaxed | [[O(log n)]] | [[O(log n)]] | [[O(log n)]] |

# Related Topics

- [[Binary Search Tree (BST)]]
- [[AVL Tree]]
- [[Red-Black Tree]]
- [[Tree Rotations]]
- [[Height of Binary Tree]]
