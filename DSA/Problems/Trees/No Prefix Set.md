# Problem
# Solution

## [[Hash Set]] - [[O(n²)]] time - [[O(n)]] space

This uses two [[Hash Set]] structures, one to keep track of the full words and the other to keep track of the prefixes that make up all the words.

1. Iterate on the words
2. If the word exists in the prefixes set, the set is bad. There's a word in the list that the current word is a prefix of.
3. Iterate on the word characters and build a sliding window substring
4. If the substring is in the list of words, the set is bad. There's a word in the list that is a prefix to the current word.

```cpp
void printBadSet(string word) {
	cout << "BAD SET" << endl;
	cout << word << endl;
	return;
}

void noPrefix(vector<string> words) {
	unordered_set<string> hashWords;
	unordered_set<string> prefixes;

	for (string word : words) {
		if (prefixes.count(word) > 0) {
			printBadSet(word);
			return;
		}
		for (int i = 1; i <= word.length(); i++) {
			string sub = word.substr(0, i);
			if (hashWords.count(sub) > 0) {
				printBadSet(word);
				return;
			} else prefixes.insert(sub);
		}
		hashWords.insert(word);
	}
	cout << "GOOD SET" << endl;
}
```

## [[Trie]] - [[O(n*m)]] time - [[O(n*m)]] space


```cpp
void printBadSet(string word) {
	cout << "BAD SET" << endl;
	cout << word << endl;
	return;
}

struct TrieNode {
	TrieNode* children[26] = {};
	bool isEnd = false;
};

bool insertCheck(TrieNode* root, const string& word) {
	TrieNode* node = root;
	
	for (char c : word) {
		int index = c - 'a';
		if (!node->children[index]) {
			node->children[index] = new TrieNode();
		}
		node = node->children[index];
		// Earlier word is prefix of current word
		if (node->isEnd) return true;
	}
	for (int i = 0; i < 26; i++) {
		// Current word is prefix of ealier word
		if (node->children[i]) return true;
	}
	node->isEnd = true;
	return false;
}

void noPrefix(vector<string> words) {
	TrieNode* root = new TrieNode();
	
	for (string word : words) {
		if (insertCheck(root, word)) {	
			printBadSet(word);
			return;
		}
	}
	cout << "GOOD SET" << endl;
}
```