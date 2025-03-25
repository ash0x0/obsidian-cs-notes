A hash structure is a structure that uses a hash index when inserting data to enable access in O(1) time

# How It Works

1. Create a simple data structure
2. Create a hash function that received a key as input and returns an index in the data structure
3. Insert the key and its corresponding value at the index returned
4. When retrieving, get the key index from the hash function then retrieve from the index

# Implementations

1. A fixed-size array
2. A [[Binary Search Tree (BST)]] that has [[O(log n)]] access as we keep the tree balanced

# Challenges

[[Collision Handling]]

# Applications

- [[Cache]]
- Frequency Counting
- Anagrams

# Common Problems

- Check for anagrams
- First unique character in a string