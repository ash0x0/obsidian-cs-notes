Insertion is the operation of adding a new node to a tree data structure. The specific process depends on the type of tree.

# Binary Tree Insertion

In a general binary tree, insert at the first available position (level order).

```cpp
Node* insert(Node* root, int value) {
    if (root == nullptr) {
        return new Node(value);
    }
    
    queue<Node*> q;
    q.push(root);
    
    while (!q.empty()) {
        Node* temp = q.front();
        q.pop();
        
        if (temp->left == nullptr) {
            temp->left = new Node(value);
            return root;
        } else {
            q.push(temp->left);
        }
        
        if (temp->right == nullptr) {
            temp->right = new Node(value);
            return root;
        } else {
            q.push(temp->right);
        }
    }
    
    return root;
}
```

**Time Complexity**: [[O(n)]]
**Space Complexity**: [[O(n)]] for queue

# Binary Search Tree Insertion

Insert maintaining BST property (left < root < right).

```cpp
Node* insert(Node* root, int value) {
    if (root == nullptr) {
        return new Node(value);
    }
    
    if (value < root->data) {
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        root->right = insert(root->right, value);
    }
    
    return root;
}
```

**Time Complexity**: [[O(log n)]] average, [[O(n)]] worst (unbalanced)
**Space Complexity**: [[O(log n)]] for recursion stack

# AVL Tree Insertion

Insert and maintain balance using rotations.

```cpp
int height(Node* node) {
    return node ? node->height : 0;
}

int getBalance(Node* node) {
    return node ? height(node->left) - height(node->right) : 0;
}

Node* rightRotate(Node* y) {
    Node* x = y->left;
    Node* T2 = x->right;
    
    x->right = y;
    y->left = T2;
    
    y->height = 1 + max(height(y->left), height(y->right));
    x->height = 1 + max(height(x->left), height(x->right));
    
    return x;
}

Node* leftRotate(Node* x) {
    Node* y = x->right;
    Node* T2 = y->left;
    
    y->left = x;
    x->right = T2;
    
    x->height = 1 + max(height(x->left), height(x->right));
    y->height = 1 + max(height(y->left), height(y->right));
    
    return y;
}

Node* insert(Node* node, int value) {
    if (node == nullptr) {
        return new Node(value);
    }
    
    if (value < node->data) {
        node->left = insert(node->left, value);
    } else if (value > node->data) {
        node->right = insert(node->right, value);
    } else {
        return node;  // Duplicates not allowed
    }
    
    node->height = 1 + max(height(node->left), height(node->right));
    
    int balance = getBalance(node);
    
    // Left Left Case
    if (balance > 1 && value < node->left->data) {
        return rightRotate(node);
    }
    
    // Right Right Case
    if (balance < -1 && value > node->right->data) {
        return leftRotate(node);
    }
    
    // Left Right Case
    if (balance > 1 && value > node->left->data) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }
    
    // Right Left Case
    if (balance < -1 && value < node->right->data) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }
    
    return node;
}
```

**Time Complexity**: [[O(log n)]]
**Space Complexity**: [[O(log n)]]

# Related Topics

- [[Binary Search Tree (BST)]]
- [[AVL Tree]]
- [[Deletion]]
- [[Search]]
