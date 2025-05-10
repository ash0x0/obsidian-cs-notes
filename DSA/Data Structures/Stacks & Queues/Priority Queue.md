A queue where ordering is determined not by insertion time but by a priority field.
If we want the highest priority element, the PQ will take care of that at insertion time instead of doing any sorting at extraction time.

This is basically an ADT.

It can be implemented with a [[Balanced Binary Tree]] or [[Heap]]. It's generally implemented by keeping an extra pointer to the highest priority element. On insertion, we update that pointer iff the new element has a higher priority. On deletion, we use find-max/ find-min to reset the pointer to the next highest/ lowest priority element.
# Operations

- Insertion
- Find Min/ Find Max
- Delete Min/ Delete Max

