#Google  #DailyCodingProblem #Medium 
# Problem

Implement locking in a binary tree. A binary tree node can be locked or unlocked only if all of its descendants or ancestors are not locked.

Design a binary tree node class with the following methods:

- `is_locked`, which returns whether the node is locked
- `lock`, which attempts to lock the node. If it cannot be locked, then it should return false. Otherwise, it should lock it and return true.
- `unlock`, which unlocks the node. If it cannot be unlocked, then it should return false. Otherwise, it should unlock it and return true.

You may augment the node to add parent pointers or any other property you would like. You may assume the class is used in a single-threaded program, so there is no need for actual locks or mutexes. Each method should run in O(h), where h is the height of the tree.
# Solution

## Track Locked Descendants - [[O(h)]] time per operation

Use parent pointers and track locked descendant count at each node.

```cpp
class LockingTreeNode {
public:
    int val;
    LockingTreeNode* left;
    LockingTreeNode* right;
    LockingTreeNode* parent;
    
    bool locked;
    int lockedDescendants;  // Count of locked nodes in subtree
    
    LockingTreeNode(int v, LockingTreeNode* p = nullptr)
        : val(v), left(nullptr), right(nullptr), parent(p),
          locked(false), lockedDescendants(0) {}
    
    bool is_locked() {
        return locked;
    }
    
    bool lock() {
        // Can't lock if any ancestor is locked
        if (!canLockOrUnlock()) {
            return false;
        }
        
        locked = true;
        
        // Update all ancestors' locked descendant counts
        LockingTreeNode* curr = parent;
        while (curr) {
            curr->lockedDescendants++;
            curr = curr->parent;
        }
        
        return true;
    }
    
    bool unlock() {
        if (!locked) {
            return false;
        }
        
        // Can't unlock if any ancestor is locked
        if (!canLockOrUnlock()) {
            return false;
        }
        
        locked = false;
        
        // Update all ancestors' locked descendant counts
        LockingTreeNode* curr = parent;
        while (curr) {
            curr->lockedDescendants--;
            curr = curr->parent;
        }
        
        return true;
    }
    
private:
    bool canLockOrUnlock() {
        // Check if this node has any locked descendants
        if (lockedDescendants > 0) {
            return false;
        }
        
        // Check if any ancestor is locked
        LockingTreeNode* curr = parent;
        while (curr) {
            if (curr->locked) {
                return false;
            }
            curr = curr->parent;
        }
        
        return true;
    }
};
```

# Algorithm

**Key Insights**:
1. Track locked descendant count at each node
2. Check ancestors for locks (traverse up)
3. Check descendants for locks (use count, [[O(1)]])

**Lock Operation**:
1. Check no ancestors are locked ([[O(h)]])
2. Check no descendants are locked ([[O(1)]] via count)
3. Set locked = true
4. Increment lockedDescendants for all ancestors

**Unlock Operation**:
1. Similar checks
2. Set locked = false
3. Decrement lockedDescendants for all ancestors

# Example

```
Tree:
        1
       / \
      2   3
     / \
    4   5

Operations:
1. lock(4):
   - Check ancestors (2, 1): none locked ✓
   - Check descendants: 0 ✓
   - Lock 4
   - Update: 2.lockedDescendants++, 1.lockedDescendants++

2. lock(2):
   - Check ancestors (1): not locked ✓
   - Check descendants: 1 (node 4) ✗
   - Cannot lock → return false

3. unlock(4):
   - Check ancestors: none locked ✓
   - Unlock 4
   - Update: 2.lockedDescendants--, 1.lockedDescendants--

4. lock(2):
   - Check ancestors (1): not locked ✓
   - Check descendants: 0 ✓
   - Lock 2 ✓
```

# Why O(h)?

- **is_locked**: [[O(1)]] - just check boolean
- **lock/unlock**: 
  - Check ancestors: [[O(h)]]
  - Check descendants: [[O(1)]] via counter
  - Update ancestors: [[O(h)]]
  - Total: [[O(h)]]

# Without Locked Descendant Count

If we didn't track `lockedDescendants`, checking descendants would require DFS:

```cpp
bool hasLockedDescendants(LockingTreeNode* node) {
    if (!node) return false;
    if (node->locked) return true;
    return hasLockedDescendants(node->left) || 
           hasLockedDescendants(node->right);
}
```

This would be [[O(n)]], not [[O(h)]]!

# Alternative: Bit Vector for Ancestors

```cpp
class LockingTreeNode {
private:
    set<LockingTreeNode*> lockedAncestors;
    int lockedDescendants;
    
public:
    bool canLock() {
        return lockedAncestors.empty() && lockedDescendants == 0;
    }
    
    void lock() {
        // Update locked ancestors set for all descendants
        updateDescendants(this, true);
        
        // Increment count for all ancestors
        LockingTreeNode* curr = parent;
        while (curr) {
            curr->lockedDescendants++;
            curr = curr->parent;
        }
    }
    
    void updateDescendants(LockingTreeNode* lockedNode, bool adding) {
        if (!this) return;
        
        if (adding) {
            lockedAncestors.insert(lockedNode);
        } else {
            lockedAncestors.erase(lockedNode);
        }
        
        if (left) left->updateDescendants(lockedNode, adding);
        if (right) right->updateDescendants(lockedNode, adding);
    }
};
```

# Time Complexity Per Operation

- **is_locked**: [[O(1)]]
- **lock/unlock**: [[O(h)]]

# Space Complexity

[[O(n)]] - parent pointers and locked descendant counts for all nodes

# Related Problems

- [[Binary Tree]]
- Concurrent data structures
- Tree node state management
- Reader-writer locks
