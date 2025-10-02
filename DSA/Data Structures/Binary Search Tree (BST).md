A Binary Search Tree is a [[Binary Tree]] where for each node:
- All values in the **left subtree** are **less than** the node's value
- All values in the **right subtree** are **greater than** the node's value

This property makes BSTs efficient for searching, insertion, and deletion operations.

# Properties

- Each node has at most two children
- Left subtree contains only nodes with keys less than the node's key
- Right subtree contains only nodes with keys greater than the node's key
- Both left and right subtrees must also be BSTs
- No duplicate values (in standard BST)

# Operations & Time Complexity

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Search | [[O(log n)]] | [[O(n)]] |
| Insert | [[O(log n)]] | [[O(n)]] |
| Delete | [[O(log n)]] | [[O(n)]] |
| Min/Max | [[O(log n)]] | [[O(n)]] |

Worst case occurs when tree becomes skewed (unbalanced).

# Implementation

```cpp
struct Node {
    int data;
    Node* left;
    Node* right;
    
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

class BST {
private:
    Node* root;
    
    Node* insertHelper(Node* node, int value) {
        if (node == nullptr) {
            return new Node(value);
        }
        
        if (value < node->data) {
            node->left = insertHelper(node->left, value);
        } else if (value > node->data) {
            node->right = insertHelper(node->right, value);
        }
        
        return node;
    }
    
    bool searchHelper(Node* node, int value) {
        if (node == nullptr) return false;
        if (node->data == value) return true;
        
        if (value < node->data) {
            return searchHelper(node->left, value);
        }
        return searchHelper(node->right, value);
    }
    
    Node* findMin(Node* node) {
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }
    
    Node* deleteHelper(Node* node, int value) {
        if (node == nullptr) return nullptr;
        
        if (value < node->data) {
            node->left = deleteHelper(node->left, value);
        } else if (value > node->data) {
            node->right = deleteHelper(node->right, value);
        } else {
            // Node to be deleted found
            
            // Case 1: No children (leaf node)
            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }
            
            // Case 2: One child
            if (node->left == nullptr) {
                Node* temp = node->right;
                delete node;
                return temp;
            }
            if (node->right == nullptr) {
                Node* temp = node->left;
                delete node;
                return temp;
            }
            
            // Case 3: Two children
            Node* successor = findMin(node->right);
            node->data = successor->data;
            node->right = deleteHelper(node->right, successor->data);
        }
        
        return node;
    }
    
public:
    BST() : root(nullptr) {}
    
    void insert(int value) {
        root = insertHelper(root, value);
    }
    
    bool search(int value) {
        return searchHelper(root, value);
    }
    
    void remove(int value) {
        root = deleteHelper(root, value);
    }
};
```

# Traversals

- **Inorder** (Left-Root-Right): Gives sorted order
- **Preorder** (Root-Left-Right): Used for tree copy
- **Postorder** (Left-Right-Root): Used for tree deletion
- **Level Order**: BFS traversal

# Advantages

- Efficient searching [[O(log n)]] on average
- Maintains sorted order (inorder traversal)
- Dynamic size
- Can efficiently find min, max, successor, predecessor

# Disadvantages

- Can become unbalanced ([[O(n)]] worst case)
- More complex than simple array/list
- Requires extra memory for pointers

# Balanced BST Variants

- [[AVL Tree]]: Strictly balanced, height difference ≤ 1
- [[Red-Black Tree]]: Less strictly balanced, faster insertions
- [[B-Tree]]: Generalized BST for disk-based storage

# Applications

- Database indexing
- File system organization
- Expression parsing
- Autocomplete features
- Priority queues

# Related Topics

- [[Binary Tree]]
- [[Balanced Binary Tree]]
- [[Tree Traversal]]
- [[AVL Tree]]
