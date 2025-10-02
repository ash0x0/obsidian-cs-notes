#DailyCodingProblem #Google #Medium 
# Problem

Given the root to a binary tree, implement `serialize(root)`, which serializes the tree into a string, and `deserialize(s)`, which deserializes the string back into the tree.

For example, given the following `Node` class

```python
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

The following test should pass:

```python
node = Node('root', Node('left', Node('left.left')), Node('right'))
assert deserialize(serialize(node)).left.left.val == 'left.left'
```
# Solution

## Preorder Traversal with Markers - [[O(n)]] time - [[O(n)]] space

Use preorder traversal and mark null nodes explicitly.

```cpp
class Codec {
public:
    // Encodes a tree to a single string
    string serialize(TreeNode* root) {
        if (!root) return "#";
        
        return to_string(root->val) + "," + 
               serialize(root->left) + "," + 
               serialize(root->right);
    }
    
    // Decodes your encoded data to tree
    TreeNode* deserialize(string data) {
        queue<string> nodes;
        stringstream ss(data);
        string token;
        
        while (getline(ss, token, ',')) {
            nodes.push(token);
        }
        
        return deserializeHelper(nodes);
    }
    
private:
    TreeNode* deserializeHelper(queue<string>& nodes) {
        string val = nodes.front();
        nodes.pop();
        
        if (val == "#") {
            return nullptr;
        }
        
        TreeNode* node = new TreeNode(stoi(val));
        node->left = deserializeHelper(nodes);
        node->right = deserializeHelper(nodes);
        
        return node;
    }
};

// Usage
int main() {
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->right->left = new TreeNode(4);
    root->right->right = new TreeNode(5);
    
    Codec codec;
    string serialized = codec.serialize(root);
    // Output: "1,2,#,#,3,4,#,#,5,#,#"
    
    TreeNode* deserialized = codec.deserialize(serialized);
    // Reconstructs the original tree
    
    return 0;
}
```

# Algorithm

**Serialize**:
1. Use preorder traversal (root, left, right)
2. Represent null nodes as "#"
3. Separate values with commas

**Deserialize**:
1. Parse string into tokens
2. Recursively reconstruct tree using preorder
3. When encountering "#", return nullptr
4. Otherwise, create node and recursively build left and right subtrees

# Example

```
Tree:
     1
    / \
   2   3
      / \
     4   5

Serialized: "1,2,#,#,3,4,#,#,5,#,#"

Parsing:
1 → create node(1)
  2 → create node(2)
    # → null (left of 2)
    # → null (right of 2)
  3 → create node(3)
    4 → create node(4)
      # → null (left of 4)
      # → null (right of 4)
    5 → create node(5)
      # → null (left of 5)
      # → null (right of 5)
```

# Alternative: Level Order (BFS)

```cpp
class Codec {
public:
    string serialize(TreeNode* root) {
        if (!root) return "";
        
        string result = "";
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            
            if (node) {
                result += to_string(node->val) + ",";
                q.push(node->left);
                q.push(node->right);
            } else {
                result += "#,";
            }
        }
        
        return result;
    }
    
    TreeNode* deserialize(string data) {
        if (data.empty()) return nullptr;
        
        stringstream ss(data);
        string token;
        getline(ss, token, ',');
        
        TreeNode* root = new TreeNode(stoi(token));
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            
            // Left child
            if (getline(ss, token, ',')) {
                if (token != "#") {
                    node->left = new TreeNode(stoi(token));
                    q.push(node->left);
                }
            }
            
            // Right child
            if (getline(ss, token, ',')) {
                if (token != "#") {
                    node->right = new TreeNode(stoi(token));
                    q.push(node->right);
                }
            }
        }
        
        return root;
    }
};
```

# Time Complexity

- **Serialize**: [[O(n)]] - visit each node once
- **Deserialize**: [[O(n)]] - process each token once

# Space Complexity

- [[O(n)]] for the serialized string
- [[O(h)]] for recursion stack (preorder) or [[O(w)]] for queue (level order)

# Related Problems

- LeetCode #297 - Serialize and Deserialize Binary Tree
- [[Tree Traversal]]
- [[BFS]]
- [[DFS]]
