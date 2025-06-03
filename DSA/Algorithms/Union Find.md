---
sources:
  - https://www.geeksforgeeks.org/introduction-to-disjoint-set-data-structure-or-union-find-algorithm/
---
Two sets are called **disjoint sets** if they don't have any element in common. 

The disjoint set data structure is used to store such sets. It supports following operations:
- Merging two disjoint sets to a single set using **Union** operation.
- Finding representative of a disjoint set using **Find** operation.
- Check if two elements belong to same set or not. We mainly find representative of both and check if same.

# Example

Consider a situation with a number of persons and the following tasks to be performed on them:
- Add a new friendship relation, i.e. a person x becomes the friend of another person y i.e adding new element to a set.
- Find whether individual x is a friend of individual y (direct or indirect friend)

```
We are given 10 individuals say, a, b, c, d, e, f, g, h, i, j

Following are relationships to be added:
a <-> b    
b <-> d  
c <-> f  
c <-> i  
j <-> e  
g <-> j

Given queries like whether a is a friend of d or not. We basically need to create following 4 groups and maintain a quickly accessible connection among group items:  
G1 = {a, b, d}  
G2 = {c, f, i}  
G3 = {e, g, j}  
G4 = {h}
```

