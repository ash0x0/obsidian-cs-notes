---
Leetcode: https://leetcode.com/problems/binary-tree-preorder-traversal/editorial/
sources:
  - https://www.sciencedirect.com/science/article/pii/0020019079900681
---
Optimize the space complexity for [[Traversal]]. It doesn't not use additional space, and the memory is only used to keep the output. If one prints the output directly along the computation, the space complexity would be O(1).
# Concept

The idea is to go down from the node to its predecessor, and each predecessor will be visited twice. Go one step left if possible and then always right till the end. 
When we visit a leaf (node's predecessor) first time, it has a zero right child, so we update output and establish the pseudo link `predecessor.right = root` to mark the fact the predecessor is visited. 
When we visit the same predecessor the second time, it already points to the current node, thus we remove the pseudo link and move right to the next node.

If the first step left is impossible, update the output and move right to the next node.

![[Pasted image 20250324163450.png]]
![[Pasted image 20250324163536.png]]
![[Pasted image 20250324163553.png]]
![[Pasted image 20250324163607.png]]
