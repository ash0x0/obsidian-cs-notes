Deletion is the operation of removing a node from a tree data structure. The process varies based on the tree type and node position.

# Binary Search Tree Deletion

Three cases to handle:

## Case 1: Node is a Leaf (No Children)

Simply remove the node.

## Case 2: Node has One Child

Replace node with its child.

## Case 3: Node has Two Children

Find inorder successor (smallest node in right subtree) or inorder predecessor (largest in left subtree), copy its value to the node, and delete the successor/predecessor.

```cpp
Node* findMin(Node* node) {
    while (node->left != nullptr) {
        node = node->left;
    }
    return node;
}

Node* deleteNode(Node* root, int value) {
    if (root == nullptr) return nullptr;
    
    // Find the node to delete
    if (value < root->data) {
        root->left = deleteNode(root->left, value);
    } else if (value > root->data) {
        root->right = deleteNode(root->right, value);
    } else {
        // Node found
        
        // Case 1: No children (leaf)
        if (root->left == nullptr && root->right == nullptr) {
            delete root;
            return nullptr;
        }
        
        // Case 2: One child
        if (root->left == nullptr) {
            Node* temp = root->right;
            delete root;
            return temp;
        }
        if (root->right == nullptr) {
            Node* temp = root->left;
            delete root;
            return temp;
        }
        
        // Case 3: Two children
        Node* successor = findMin(root->right);
        root->data = successor->data;
        root->right = deleteNode(root->right, successor->data);
    }
    
    return root;
}
```

**Time Complexity**: [[O(log n)]] average, [[O(n)]] worst
**Space Complexity**: [[O(log n)]] for recursion

# Binary Tree Deletion

Delete the deepest rightmost node and replace target with it.

```cpp
void deleteDeepest(Node* root, Node* delNode) {
    queue<Node*> q;
    q.push(root);
    
    while (!q.empty()) {
        Node* temp = q.front();
        q.pop();
        
        if (temp == delNode) {
            temp = nullptr;
            delete delNode;
            return;
        }
        
        if (temp->right) {
            if (temp->right == delNode) {
                temp->right = nullptr;
                delete delNode;
                return;
            } else {
                q.push(temp->right);
            }
        }
        
        if (temp->left) {
            if (temp->left == delNode) {
                temp->left = nullptr;
                delete delNode;
                return;
            } else {
                q.push(temp->left);
            }
        }
    }
}

Node* deletion(Node* root, int value) {
    if (root == nullptr) return nullptr;
    
    if (root->left == nullptr && root->right == nullptr) {
        if (root->data == value) {
            delete root;
            return nullptr;
        }
        return root;
    }
    
    queue<Node*> q;
    q.push(root);
    Node* target = nullptr;
    Node* temp = nullptr;
    
    // Find target and last node
    while (!q.empty()) {
        temp = q.front();
        q.pop();
        
        if (temp->data == value) {
            target = temp;
        }
        if (temp->left) q.push(temp->left);
        if (temp->right) q.push(temp->right);
    }
    
    if (target) {
        int x = temp->data;
        deleteDeepest(root, temp);
        target->data = x;
    }
    
    return root;
}
```

**Time Complexity**: [[O(n)]]
**Space Complexity**: [[O(n)]]

# AVL Tree Deletion

Delete like BST then rebalance with rotations.

```cpp
Node* deleteNode(Node* root, int value) {
    if (root == nullptr) return nullptr;
    
    if (value < root->data) {
        root->left = deleteNode(root->left, value);
    } else if (value > root->data) {
        root->right = deleteNode(root->right, value);
    } else {
        // Node found - same as BST deletion
        if (root->left == nullptr || root->right == nullptr) {
            Node* temp = root->left ? root->left : root->right;
            if (temp == nullptr) {
                temp = root;
                root = nullptr;
            } else {
                *root = *temp;
            }
            delete temp;
        } else {
            Node* temp = findMin(root->right);
            root->data = temp->data;
            root->right = deleteNode(root->right, temp->data);
        }
    }
    
    if (root == nullptr) return root;
    
    // Update height
    root->height = 1 + max(height(root->left), height(root->right));
    
    // Check balance and rotate if needed
    int balance = getBalance(root);
    
    // Left Left
    if (balance > 1 && getBalance(root->left) >= 0) {
        return rightRotate(root);
    }
    
    // Left Right
    if (balance > 1 && getBalance(root->left) < 0) {
        root->left = leftRotate(root->left);
        return rightRotate(root);
    }
    
    // Right Right
    if (balance < -1 && getBalance(root->right) <= 0) {
        return leftRotate(root);
    }
    
    // Right Left
    if (balance < -1 && getBalance(root->right) > 0) {
        root->right = rightRotate(root->right);
        return leftRotate(root);
    }
    
    return root;
}
```

**Time Complexity**: [[O(log n)]]
**Space Complexity**: [[O(log n)]]

# Related Topics

- [[Binary Search Tree (BST)]]
- [[Insertion]]
- [[Search]]
- [[AVL Tree]]
