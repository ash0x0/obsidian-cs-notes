#DailyCodingProblem #Microsoft #Easy 
# Problem

Suppose an arithmetic expression is given as a binary tree. Each leaf is an integer and each internal node is one of '+', '−', '∗', or '/'.

Given the root to such a tree, write a function to evaluate it.

For example, given the following tree:

```
    *
   / \
  +    +
 / \  / \
3  2  4  5
```

You should return 45, as it is (3 + 2) * (4 + 5).

# Solution
## Recursive Evaluation - [[O(n)]] time - [[O(h)]] space

Recursively evaluate left and right subtrees, then apply operator.

```cpp
// Assuming TreeNode can hold int for operands or char for operators
struct TreeNode {
    string val;  // either number or operator
    TreeNode* left;
    TreeNode* right;
    
    TreeNode(string v) : val(v), left(nullptr), right(nullptr) {}
};

double evaluate(TreeNode* root) {
    if (!root) return 0;
    
    // Leaf node: return the number
    if (!root->left && !root->right) {
        return stod(root->val);
    }
    
    // Recursively evaluate left and right subtrees
    double leftVal = evaluate(root->left);
    double rightVal = evaluate(root->right);
    
    // Apply operator
    char op = root->val[0];
    switch (op) {
        case '+': return leftVal + rightVal;
        case '-': return leftVal - rightVal;
        case '*': return leftVal * rightVal;
        case '/': return leftVal / rightVal;
        default: return 0;
    }
}
```

# Algorithm

1. **Base case**: If leaf node, return its value
2. **Recursive case**: 
   - Evaluate left subtree
   - Evaluate right subtree
   - Apply operator to results

# Example

```
Tree:
        *
       / \
      +   +
     / \ / \
    3  2 4  5

Evaluation:
1. evaluate(left subtree of *):
   - evaluate(3) = 3
   - evaluate(2) = 2
   - apply +: 3 + 2 = 5

2. evaluate(right subtree of *):
   - evaluate(4) = 4
   - evaluate(5) = 5
   - apply +: 4 + 5 = 9

3. evaluate(root):
   - left = 5
   - right = 9
   - apply *: 5 * 9 = 45

Result: 45
```

# With Integer Node Values

If using separate types for operators and operands:

```cpp
struct TreeNode {
    bool isOperator;
    union {
        char op;
        int num;
    };
    TreeNode* left;
    TreeNode* right;
};

int evaluate(TreeNode* root) {
    if (!root->isOperator) {
        return root->num;
    }
    
    int leftVal = evaluate(root->left);
    int rightVal = evaluate(root->right);
    
    switch (root->op) {
        case '+': return leftVal + rightVal;
        case '-': return leftVal - rightVal;
        case '*': return leftVal * rightVal;
        case '/': return leftVal / rightVal;
    }
    
    return 0;
}
```

# Postorder Traversal View

This is essentially postorder traversal:
- Process left child
- Process right child
- Process root (apply operation)

# With Error Handling

```cpp
double evaluate(TreeNode* root) {
    if (!root) {
        throw runtime_error("Null node");
    }
    
    // Leaf node: parse and return number
    if (!root->left && !root->right) {
        try {
            return stod(root->val);
        } catch (...) {
            throw runtime_error("Invalid number: " + root->val);
        }
    }
    
    if (!root->left || !root->right) {
        throw runtime_error("Operator node must have two children");
    }
    
    double leftVal = evaluate(root->left);
    double rightVal = evaluate(root->right);
    
    char op = root->val[0];
    switch (op) {
        case '+': return leftVal + rightVal;
        case '-': return leftVal - rightVal;
        case '*': return leftVal * rightVal;
        case '/': 
            if (rightVal == 0) {
                throw runtime_error("Division by zero");
            }
            return leftVal / rightVal;
        default:
            throw runtime_error("Unknown operator: " + root->val);
    }
}
```

# Iterative Approach (Postorder)

```cpp
double evaluateIterative(TreeNode* root) {
    if (!root) return 0;
    
    stack<TreeNode*> nodes;
    stack<double> values;
    TreeNode* curr = root;
    TreeNode* lastVisited = nullptr;
    
    while (!nodes.empty() || curr) {
        if (curr) {
            nodes.push(curr);
            curr = curr->left;
        } else {
            TreeNode* peek = nodes.top();
            
            if (peek->right && lastVisited != peek->right) {
                curr = peek->right;
            } else {
                nodes.pop();
                
                if (!peek->left && !peek->right) {
                    // Leaf: push value
                    values.push(stod(peek->val));
                } else {
                    // Operator: pop two values and apply
                    double right = values.top(); values.pop();
                    double left = values.top(); values.pop();
                    
                    char op = peek->val[0];
                    double result;
                    switch (op) {
                        case '+': result = left + right; break;
                        case '-': result = left - right; break;
                        case '*': result = left * right; break;
                        case '/': result = left / right; break;
                    }
                    values.push(result);
                }
                
                lastVisited = peek;
            }
        }
    }
    
    return values.top();
}
```

# Time Complexity

[[O(n)]] - visit each node once

# Space Complexity

- Recursive: [[O(h)]] - call stack depth
- Iterative: [[O(n)]] - stack storage

# Related Problems

- [[Binary Tree]]
- [[Postorder Traversal]]
- Expression tree evaluation
- Calculator problems (LeetCode #224, #227)
