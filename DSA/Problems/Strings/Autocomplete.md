#Twitter #DailyCodingProblem #Medium 
# Problem

Implement an autocomplete system. That is, given a query string `s` and a set of all possible query strings, return all strings in the set that have s as a prefix.

For example, given the query string `de` and the set of strings [`dog`, `deer`, `deal`], return [`deer`, `deal`].

Hint: Try preprocessing the dictionary into a more efficient data structure to speed up queries.
# Solution

## Trie-Based Approach - [[O(m)]] query time

Preprocess dictionary into a [[Trie]], then search for all words with given prefix.

```cpp
struct TrieNode {
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord;
    string word; // Store complete word at leaf
    
    TrieNode() : isEndOfWord(false) {}
};

class Autocomplete {
private:
    TrieNode* root;
    
    void insertWord(const string& word) {
        TrieNode* node = root;
        for (char c : word) {
            if (node->children.find(c) == node->children.end()) {
                node->children[c] = new TrieNode();
            }
            node = node->children[c];
        }
        node->isEndOfWord = true;
        node->word = word;
    }
    
    void collectAllWords(TrieNode* node, vector<string>& results) {
        if (node == nullptr) return;
        
        if (node->isEndOfWord) {
            results.push_back(node->word);
        }
        
        for (auto& [ch, child] : node->children) {
            collectAllWords(child, results);
        }
    }
    
public:
    Autocomplete(const vector<string>& dictionary) {
        root = new TrieNode();
        for (const string& word : dictionary) {
            insertWord(word);
        }
    }
    
    vector<string> search(const string& prefix) {
        vector<string> results;
        TrieNode* node = root;
        
        // Navigate to prefix node
        for (char c : prefix) {
            if (node->children.find(c) == node->children.end()) {
                return results; // Prefix not found
            }
            node = node->children[c];
        }
        
        // Collect all words from this node
        collectAllWords(node, results);
        
        return results;
    }
};

// Usage
int main() {
    vector<string> dictionary = {"dog", "deer", "deal", "dear"};
    Autocomplete ac(dictionary);
    
    vector<string> results = ac.search("de");
    // Returns: ["deer", "deal", "dear"]
    
    return 0;
}
```

# Time Complexity

- **Preprocessing**: [[O(n*m)]] where n = number of words, m = average length
- **Query**: [[O(m + k)]] where m = prefix length, k = number of results
- **Space**: [[O(n*m)]] for storing the Trie

# Alternative: Sorted Array with Binary Search

```cpp
class Autocomplete {
private:
    vector<string> sortedWords;
    
public:
    Autocomplete(vector<string> dictionary) {
        sortedWords = dictionary;
        sort(sortedWords.begin(), sortedWords.end());
    }
    
    vector<string> search(const string& prefix) {
        vector<string> results;
        
        // Find first word with prefix using binary search
        auto it = lower_bound(sortedWords.begin(), sortedWords.end(), prefix);
        
        // Collect all words with this prefix
        while (it != sortedWords.end() && 
               it->substr(0, prefix.length()) == prefix) {
            results.push_back(*it);
            ++it;
        }
        
        return results;
    }
};
```

**Query Time**: [[O(log n + k + m)]] where k = number of results, m = string comparison

# Optimizations

1. **Limit results**: Return top k results only
2. **Ranking**: Sort by frequency or relevance
3. **Caching**: Cache popular prefixes
4. **Compressed Trie**: Use PATRICIA trie for space efficiency

# Related Problems

- [[Trie]]
- LeetCode #208 - Implement Trie
- LeetCode #677 - Map Sum Pairs
- Search suggestions system
