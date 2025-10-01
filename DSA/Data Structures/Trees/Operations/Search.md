Search is the operation of finding a specific node or value in a tree data structure.

# Binary Tree Search

For general binary trees, search all nodes (no ordering property).

```cpp
bool search(Node* root, int value) {
    if (root == nullptr) {
        return false;
    }
    
    if (root->data == value) {
        return true;
    }
    
    return search(root->left, value) || search(root->right, value);
}
```

**Time Complexity**: [[O(n)]] - must check all nodes
**Space Complexity**: [[O(h)]] where h is height (recursion stack)

# Binary Search Tree Search

Leverage BST property for efficient search.

```cpp
bool search(Node* root, int value) {
    if (root == nullptr) {
        return false;
    }
    
    if (root->data == value) {
        return true;
    }
    
    if (value < root->data) {
        return search(root->left, value);
    }
    
    return search(root->right, value);
}
```

## Iterative Version

```cpp
bool searchIterative(Node* root, int value) {
    Node* current = root;
    
    while (current != nullptr) {
        if (current->data == value) {
            return true;
        }
        
        if (value < current->data) {
            current = current->left;
        } else {
            current = current->right;
        }
    }
    
    return false;
}
```

**Time Complexity**: [[O(log n)]] average, [[O(n)]] worst (unbalanced)
**Space Complexity**: [[O(1)]] iterative, [[O(log n)]] recursive

# Breadth-First Search (BFS)

Level-order search using queue.

```cpp
bool bfs(Node* root, int value) {
    if (root == nullptr) return false;
    
    queue<Node*> q;
    q.push(root);
    
    while (!q.empty()) {
        Node* current = q.front();
        q.pop();
        
        if (current->data == value) {
            return true;
        }
        
        if (current->left) q.push(current->left);
        if (current->right) q.push(current->right);
    }
    
    return false;
}
```

**Time Complexity**: [[O(n)]]
**Space Complexity**: [[O(w)]] where w is max width

# Depth-First Search (DFS)

Three variants based on traversal order.

## Preorder (Root-Left-Right)

```cpp
bool dfsPreorder(Node* root, int value) {
    if (root == nullptr) return false;
    if (root->data == value) return true;
    return dfsPreorder(root->left, value) || dfsPreorder(root->right, value);
}
```

## Inorder (Left-Root-Right)

```cpp
bool dfsInorder(Node* root, int value) {
    if (root == nullptr) return false;
    return dfsInorder(root->left, value) || 
           root->data == value || 
           dfsInorder(root->right, value);
}
```

## Postorder (Left-Right-Root)

```cpp
bool dfsPostorder(Node* root, int value) {
    if (root == nullptr) return false;
    return dfsPostorder(root->left, value) || 
           dfsPostorder(root->right, value) || 
           root->data == value;
}
```

# Finding Min/Max in BST

```cpp
Node* findMin(Node* root) {
    if (root == nullptr) return nullptr;
    while (root->left != nullptr) {
        root = root->left;
    }
    return root;
}

Node* findMax(Node* root) {
    if (root == nullptr) return nullptr;
    while (root->right != nullptr) {
        root = root->right;
    }
    return root;
}
```

**Time Complexity**: [[O(log n)]] average, [[O(n)]] worst

# Finding Successor/Predecessor

```cpp
Node* inorderSuccessor(Node* root, Node* target) {
    if (target->right != nullptr) {
        return findMin(target->right);
    }
    
    Node* successor = nullptr;
    Node* current = root;
    
    while (current != nullptr) {
        if (target->data < current->data) {
            successor = current;
            current = current->left;
        } else if (target->data > current->data) {
            current = current->right;
        } else {
            break;
        }
    }
    
    return successor;
}
```

# Comparison

| Tree Type | Search Time | Notes |
|-----------|-------------|-------|
| Binary Tree | [[O(n)]] | No ordering |
| BST (balanced) | [[O(log n)]] | Maintains ordering |
| BST (skewed) | [[O(n)]] | Degrades to linked list |
| AVL Tree | [[O(log n)]] | Always balanced |

# Related Topics

- [[Binary Search Tree (BST)]]
- [[Traversal]]
- [[BFS]]
- [[DFS]]
- [[Insertion]]
- [[Deletion]]
