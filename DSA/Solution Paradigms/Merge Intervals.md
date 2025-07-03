Efficient technique to deal with overlapping intervals.
## Mechanism

1. Given two intervals (`a` and `b`), there will be six different ways the two intervals can relate to each other
	1. `a` and `b` don't overlap, `b` ends after `a`
	2. `a` and `b` overlap, `b` ends after `a`
	3. `a` completely overlaps `b`
	4. `a` and `b` overlap, `a` ends after `b`
	5. `b` completely overlaps `a`
	6. `a` and `b` don't overlap, `a` ends after `b`

![[Pasted image 20250509230738.png]]
## When to use

1. If you hear the term “overlapping intervals”.
2. Merge intervals if they overlap
3. If you’re asked to produce a list with only mutually exclusive intervals