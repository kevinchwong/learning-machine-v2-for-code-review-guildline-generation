# Trie
## Frequent score for this algorithmic framework: 40 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Implement Trie (Prefix Tree)](#implement-trie-prefix-tree) | 80 | 50 |
| 2 | [Add and Search Word - Data structure design](#add-and-search-word-data-structure-design) | 70 | 55 |
| 3 | [Replace Words](#replace-words) | 60 | 45 |
| 4 | [Design Search Autocomplete System](#design-search-autocomplete-system) | 55 | 65 |
| 5 | [Map Sum Pairs](#map-sum-pairs) | 50 | 50 |
| 6 | [Stream of Characters](#stream-of-characters) | 45 | 60 |
| 7 | [Concatenated Words](#concatenated-words) | 40 | 65 |
| 8 | [Palindrome Pairs](#palindrome-pairs) | 35 | 70 |
| 9 | [Longest Word in Dictionary](#longest-word-in-dictionary) | 30 | 50 |
| 10 | [Search Suggestions System](#search-suggestions-system) | 25 | 55 |
| 11 | [Maximum XOR of Two Numbers in an Array](#maximum-xor-of-two-numbers-in-an-array) | 20 | 65 |
| 12 | [Implement Magic Dictionary](#implement-magic-dictionary) | 15 | 55 |
| 13 | [Prefix and Suffix Search](#prefix-and-suffix-search) | 10 | 70 |
| 14 | [Longest Word With All Prefixes](#longest-word-with-all-prefixes) | 5 | 50 |
| 15 | [Search in a Sorted Array of Unknown Size](#search-in-a-sorted-array-of-unknown-size) | 5 | 60 |
| 16 | [Word Squares](#word-squares) | 5 | 70 |
| 17 | [Maximum XOR With an Element From Array](#maximum-xor-with-an-element-from-array) | 5 | 70 |
---
A Trie, or prefix tree, is a tree-like data structure that stores a dynamic set of strings, where each node represents a single character of a string. It is used for efficient retrieval of a key in a dataset of strings, typically for tasks like autocomplete and spell checking.
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

    def starts_with(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True

# Example usage:
trie = Trie()
trie.insert("hello")
print(trie.search("hello"))  # True
print(trie.search("hell"))   # False
print(trie.starts_with("hell"))  # True
print(trie.starts_with("helloa"))  # False
```
### Insight:
1. Tries are particularly useful for prefix-based searches.
2. Each node in a Trie represents a single character of a string.
3. The root node is typically an empty node.
4. Tries can use a lot of memory if the dataset is large, but they offer fast retrieval times.
5. The `is_end_of_word` flag helps to distinguish between words and prefixes.
---
 --- 
# Implement Trie (Prefix Tree)
>Implement a trie with insert, search, and startsWith methods.

>Trie trie = new Trie();
trie.insert("apple");
trie.search("apple"); // returns true
trie.search("app"); // returns false
trie.startsWith("app"); // returns true
trie.insert("app");
trie.search("app"); // returns true
- https://leetcode.com/problems/implement-trie-prefix-tree/
- Difficulty: 50
- Frequent: 80
- Tags: ['Trie', 'Design']

### Implement Trie (Prefix Tree) -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```
#### TC: O(n) for insert, search, and startsWith, where n is the length of the word or prefix **SC:** O(n) for insert, where n is the length of the word inserted

The Trie data structure is initialized with a root node. Each node contains a dictionary of its
children and a boolean to mark the end of a word. The insert method adds words character by
character, creating new nodes as needed. The search method checks for the existence of a word by
traversing the nodes. The startsWith method checks if any word starts with the given prefix by
traversing the nodes similarly to search.

---
### Implement Trie (Prefix Tree) -- Best TC Solution:
```python
# Implement Trie (Prefix Tree)

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False


class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
        
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
        
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```
#### TC: 
- Insert: O(m), where m is the length of the word being inserted
- Search: O(m), where m is the length of the word being searched
- StartsWith: O(m), where m is the length of the prefix being searched **SC:** 
- O(N * M), where N is the number of words in the Trie and M is the average length of the words

The code implements a Trie data structure for efficient prefix-based operations like insert, search,
and startsWith. The TrieNode class represents each node in the Trie, with children mapping
characters to TrieNodes. The Trie class uses the root node to perform operations. Insertion, search,
and prefix search have a time complexity of O(m) where m is the length of the word or prefix. The
space complexity is O(N * M) where N is the number of words and M is the average word length. The
code is clean, easy to understand, and provides efficient operations for prefix-based queries.

---
### Implement Trie (Prefix Tree) -- Best SC Solution:
```python
# Implement Trie (Prefix Tree)

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False


class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
        
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
        
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```
#### TC: 
- Insert: O(m), where m is the length of the word being inserted
- Search: O(m), where m is the length of the word being searched
- StartsWith: O(m), where m is the length of the prefix being searched **SC:** 
- O(n*m), where n is the number of words in the Trie and m is the average length of the words

The Trie data structure is used to efficiently store and retrieve strings. In this implementation,
each node represents a character in the word, and the children of a node are the characters that can
follow it in the word. The is_end_of_word flag is used to mark the end of a word.  - Insert:
Traverse the Trie character by character and create nodes as needed. Set the is_end_of_word flag for
the last character.  - Search: Traverse the Trie character by character. Return True if the word
exists in the Trie.  - StartsWith: Similar to search but does not require the is_end_of_word flag to
be True. Returns True if the prefix exists in the Trie.

---
---
---
 --- 
# Add and Search Word - Data structure design
>Design a data structure that supports adding new words and finding if a string matches any previously added string.

>WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // returns false
wordDictionary.search("bad"); // returns true
wordDictionary.search(".ad"); // returns true
wordDictionary.search("b.."); // returns true
- https://leetcode.com/problems/add-and-search-word-data-structure-design/
- Difficulty: 55
- Frequent: 70
- Tags: ['Trie', 'Design', 'Backtracking']

### Add and Search Word - Data structure design -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word):
        def dfs(j, node):
            for i in range(j, len(word)):
                char = word[i]
                if char == '.':
                    return any(dfs(i + 1, child) for child in node.children.values())
                if char not in node.children:
                    return False
                node = node.children[char]
            return node.is_end_of_word
        return dfs(0, self.root)
```
#### TC: Time Complexity: O(N) for addWord, where N is the length of the word. For search, in the worst case, it could be O(M * 26^L), where M is the number of characters in the word and L is the length of the word. **SC:** Space Complexity: O(N * 26) for addWord, where N is the length of the word. For search, the space complexity is O(1) for the DFS stack in the worst case.

1. The `TrieNode` class is a basic structure with a dictionary to hold child nodes and a boolean to
mark the end of a word. 2. The `WordDictionary` class initializes with a root `TrieNode`. 3. The
`addWord` method iterates through each character of the word, creating new `TrieNode` objects as
needed and marking the end of the word. 4. The `search` method uses a depth-first search (DFS) to
handle the '.' wildcard, recursively checking all possible nodes at each level. 5. The DFS function
is called starting from the root node and processes each character, branching out when a '.' is
encountered.

---
### Add and Search Word - Data structure design -- Best TC Solution:
```python
# Trie Node class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False


class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word: str) -> bool:
        def search_in_node(word, node) -> bool:
            for i, char in enumerate(word):
                if char not in node.children:
                    if char == ".":
                        for child in node.children:
                            if search_in_node(word[i + 1:], node.children[child]):
                                return True
                    return False
                else:
                    node = node.children[char]
            return node.is_end_of_word

        return search_in_node(word, self.root)


# Example
word_dict = WordDictionary()
word_dict.addWord("apple")
print(word_dict.search("apple"))  # Output: True
print(word_dict.search("app"))    # Output: False
print(word_dict.search(".pple"))  # Output: True
print(word_dict.search(".ppl."))  # Output: False
```
#### TC: 
AddWord: O(N)
Search: O(M) where M is the length of the word being searched **SC:** 
AddWord: O(N)
Search: O(1)

The code implements a WordDictionary class using a Trie data structure. The addWord function adds a
word to the Trie, and the search function searches for a word in the Trie. The search function
handles '.' as a wildcard character. The Trie structure allows for efficient storage and retrieval
of words, making the search operation faster. The use of recursion in the search function simplifies
the process of searching for words with wildcard characters. Overall, the code is clean, easy to
understand, and provides a smart solution to the problem.

---
### Add and Search Word - Data structure design -- Best SC Solution:
```python
# Trie Node class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False


class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word: str) -> bool:
        def dfs(node, i):
            if i == len(word):
                return node.is_end_of_word
            if word[i] == '.' :
                for child in node.children.values():
                    if dfs(child, i + 1):
                        return True
            if word[i] in node.children:
                return dfs(node.children[word[i]], i + 1)
            return False

        return dfs(self.root, 0)
```
#### TC: 
AddWord: O(N)
Search: O(N)
 **SC:** 
AddWord: O(N)
Search: O(1)


The code implements a Trie data structure to efficiently store and search for words. The TrieNode
class represents each node in the Trie, with children representing the next characters and
is_end_of_word indicating the end of a word.  The WordDictionary class uses the Trie to add words
and search for words. The addWord function adds a word by traversing the Trie and marking the end of
the word. The search function uses a depth-first search (dfs) approach to search for a word,
handling '.' as a wildcard character.  The time complexity for both addWord and search operations is
O(N), where N is the length of the word. The space complexity for addWord is also O(N) as it stores
the word in the Trie. However, the space complexity for search is O(1) as it does not use additional
space proportional to the input word length during the search process.

---
---
---
 --- 
# Replace Words
>Replace words in a sentence with their root forms from a dictionary.

>List<String> dict = Arrays.asList("cat", "bat", "rat");
String sentence = "the cattle was rattled by the battery";
replaceWords(dict, sentence); // returns "the cat was rat by the bat"
- https://leetcode.com/problems/replace-words/
- Difficulty: 45
- Frequent: 60
- Tags: ['Trie', 'Hash Table', 'String']

### Replace Words -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def search_root(self, word):
        node = self.root
        prefix = ''
        for char in word:
            if char not in node.children:
                break
            node = node.children[char]
            prefix += char
            if node.is_end:
                return prefix
        return word

class Solution:
    def replaceWords(self, dictionary, sentence):
        trie = Trie()
        for root in dictionary:
            trie.insert(root)
        return ' '.join(trie.search_root(word) for word in sentence.split())
```
#### TC: O(N + M) **SC:** O(N)

1. **TrieNode Class**: Represents each node in the Trie. Each node has a dictionary of children and
a boolean flag to mark the end of a word. 2. **Trie Class**: Manages the Trie. It has methods to
insert words and search for the shortest root in a given word. 3. **Insert Method**: Adds a word to
the Trie by iterating through its characters and creating nodes as needed. 4. **Search Root
Method**: Finds the shortest root for a given word by traversing the Trie. If a root is found, it
returns the root; otherwise, it returns the original word. 5. **Solution Class**: Uses the Trie to
replace words in the sentence with their corresponding roots from the dictionary. It first inserts
all dictionary roots into the Trie and then processes each word in the sentence to find and replace
with the shortest root.

---
### Replace Words -- Best TC Solution:
```python
from typing import List

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Solution:
    def replaceWords(self, dictionary: List[str], sentence: str) -> str:
        root = TrieNode()
        result = []
        dictionary.sort()
        for word in dictionary:
            node = root
            for char in word:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.is_end = True
        for word in sentence.split():
            node = root
            prefix = []
            for char in word:
                if char not in node.children or node.is_end:
                    break
                prefix.append(char)
                node = node.children[char]
            result.append(''.join(prefix) if node.is_end else word)
        return ' '.join(result)
```
#### TC: O(N*M) where N is the number of words in the sentence and M is the average length of the words **SC:** O(N) where N is the total number of characters in the dictionary

The code uses a Trie data structure to efficiently find the shortest root of each word in the
sentence. It first builds a Trie from the dictionary words and then iterates through each word in
the sentence to find the shortest root. By using the Trie, it avoids unnecessary comparisons and
improves the overall efficiency of the algorithm. The time complexity is O(N*M) where N is the
number of words in the sentence and M is the average length of the words. The space complexity is
O(N) where N is the total number of characters in the dictionary.

---
---
---
 --- 
# Design Search Autocomplete System
>Design a search autocomplete system.

>AutocompleteSystem obj = new AutocompleteSystem(sentences, times);
List<String> param_1 = obj.input(c);
- https://leetcode.com/problems/design-search-autocomplete-system/
- Difficulty: 65
- Frequent: 55
- Tags: ['Trie', 'Design']

### Design Search Autocomplete System -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.sentences = {}

class AutocompleteSystem:
    def __init__(self, sentences, times):
        self.root = TrieNode()
        self.search_term = ''
        for i, sentence in enumerate(sentences):
            self.add_record(sentence, times[i])

    def add_record(self, sentence, time):
        node = self.root
        for char in sentence:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            if sentence in node.sentences:
                node.sentences[sentence] += time
            else:
                node.sentences[sentence] = time
        node.sentences = dict(sorted(node.sentences.items(), key=lambda item: (-item[1], item[0]))[:3])

    def input(self, c):
        results = []
        if c == '#':
            self.add_record(self.search_term, 1)
            self.search_term = ''
            return results
        self.search_term += c
        node = self.root
        for char in self.search_term:
            if char not in node.children:
                return results
            node = node.children[char]
        results = sorted(node.sentences, key=lambda x: (-node.sentences[x], x))[:3]
        return results
```
#### TC: O(n * l + m * l) **SC:** O(T)

The code uses a Trie data structure to store sentences and their corresponding frequencies. The
`add_record` function inserts sentences into the Trie and maintains a sorted list of top 3 sentences
at each node. The `input` function processes each character input, updating the search term and
traversing the Trie to fetch the top 3 matching sentences. The time complexity is O(n * l + m * l),
where n is the number of sentences, l is the average length of the sentences, and m is the length of
the search term. The space complexity is O(T), where T is the total length of all sentences stored
in the Trie.

---
### Design Search Autocomplete System -- Best TC Solution:
```python
# TrieNode class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
        self.hotness = 0


class AutocompleteSystem:
    def __init__(self, sentences, times):
        self.root = TrieNode()
        self.keyword = ""
        self.search_trie = self.root
        for i, sentence in enumerate(sentences):
            self.add_sentence(sentence, times[i])

    def add_sentence(self, sentence, hotness):
        node = self.root
        for char in sentence:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
        node.hotness -= hotness

    def search(self, char):
        if char == '#':
            self.add_sentence(self.keyword, 1)
            self.keyword = ""
            self.search_trie = self.root
            return []
        self.keyword += char
        if char not in self.search_trie.children:
            self.search_trie.children[char] = TrieNode()
        self.search_trie = self.search_trie.children[char]
        return self.get_hot_sentences()

    def get_hot_sentences(self):
        heap = []
        self.dfs(self.search_trie, self.keyword, heap)
        return [sentence for hotness, sentence in heapq.nsmallest(3, heap)]

    def dfs(self, node, path, heap):
        if node.is_end_of_word:
            heapq.heappush(heap, (node.hotness, path))
        for char, child in node.children.items():
            self.dfs(child, path + char, heap)

# Example
sentences = ["i love you", "island", "ironman", "i love leetcode"]
times = [5, 3, 2, 2]
obj = AutocompleteSystem(sentences, times)
print(obj.search('i'))
print(obj.search(' '))
print(obj.search('a'))
print(obj.search('#'))
```
#### TC: 
Time Complexity:
- Initialization: O(n * m), where n is the number of sentences and m is the average length of a sentence
- Search: O(p + q + m * log(k)), where p is the length of the input string, q is the number of nodes in the trie for the input string, m is the average length of a sentence, and k is the number of hot sentences to return

Overall, the time complexity is dominated by the search operation.
 **SC:** 
Space Complexity:
- Trie data structure: O(n * m), where n is the number of sentences and m is the average length of a sentence
- Heap for storing hot sentences: O(k), where k is the number of hot sentences to return

Overall, the space complexity is dominated by the trie data structure and the heap for storing hot sentences.

Insights: - The code uses a Trie data structure to efficiently store and search for sentences. -
Each TrieNode represents a character in a sentence, and the Trie is built based on the input
sentences. - The hotness value is used to determine the popularity of a sentence. - The search
function navigates the Trie based on the input characters and returns the top 3 hot sentences
matching the input. - Adding a new sentence updates the Trie structure and adjusts the hotness
values accordingly. - The code demonstrates an efficient autocomplete system using Trie and heap
data structures.

---
### Design Search Autocomplete System -- Best SC Solution:
```python
# TrieNode class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
        self.hotness = 0


class AutocompleteSystem:
    def __init__(self, sentences, times):
        self.root = TrieNode()
        self.keyword = ""
        self.search_trie = self.root
        for i, sentence in enumerate(sentences):
            self.add_sentence(sentence, times[i])

    def add_sentence(self, sentence, hotness):
        node = self.root
        for char in sentence:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
        node.hotness -= hotness

    def search(self, char):
        if char == '#':
            self.add_sentence(self.keyword, 1)
            self.keyword = ""
            self.search_trie = self.root
            return []
        self.keyword += char
        if char not in self.search_trie.children:
            self.search_trie.children[char] = TrieNode()
        self.search_trie = self.search_trie.children[char]
        return self.get_hot_sentences()

    def get_hot_sentences(self):
        heap = []
        self.dfs(self.search_trie, self.keyword, heap)
        return [sentence for hotness, sentence in heapq.nsmallest(3, heap)]

    def dfs(self, node, path, heap):
        if node.is_end_of_word:
            heapq.heappush(heap, (node.hotness, path))
        for char, child in node.children.items():
            self.dfs(child, path + char, heap)

# Time complexity: O(n) for initialization, O(m*log(m)) for search (m is the number of sentences)
# Space complexity: O(n) for initialization, O(m) for search
```
#### TC: O(n) for initialization, O(m*log(m)) for search (m is the number of sentences) **SC:** O(n) for initialization, O(m) for search

The code implements the AutocompleteSystem class using a Trie data structure. The TrieNode class
represents each node in the Trie. The AutocompleteSystem class initializes with sentences and their
frequencies. It provides methods to add a sentence with its hotness, search for autocomplete
suggestions, and get the hottest sentences. The search method handles input characters and returns
autocomplete suggestions. The code efficiently stores and retrieves sentences based on hotness. The
time complexity for initialization is O(n), and for search, it is O(m*log(m)), where m is the number
of sentences. The space complexity is O(n) for initialization and O(m) for search, making it an
optimal solution for the Design Search Autocomplete System problem.

---
---
---
 --- 
# Map Sum Pairs
>Implement a MapSum class with insert and sum methods.

>MapSum mapSum = new MapSum();
mapSum.insert("apple", 3);
mapSum.sum("ap"); // returns 3
mapSum.insert("app", 2);
mapSum.sum("ap"); // returns 5
- https://leetcode.com/problems/map-sum-pairs/
- Difficulty: 50
- Frequent: 50
- Tags: ['Trie', 'Design']

### Map Sum Pairs -- Shortest Solution:
```python
class MapSum:
    def __init__(self):
        self.map = {}
        self.trie = {}

    def insert(self, key: str, val: int) -> None:
        delta = val - self.map.get(key, 0)
        self.map[key] = val
        cur = self.trie
        for c in key:
            if c not in cur:
                cur[c] = {'#': 0}
            cur = cur[c]
            cur['#'] += delta

    def sum(self, prefix: str) -> int:
        cur = self.trie
        for c in prefix:
            if c not in cur:
                return 0
            cur = cur[c]
        return cur['#']
```
#### TC: O(N) for insert, O(P) for sum **SC:** O(N)

The solution uses a Trie (prefix tree) to store the cumulative sums of all prefixes. The `insert`
method updates the Trie with the new value and adjusts the sums for all relevant prefixes. The `sum`
method traverses the Trie to compute the sum of all keys that start with the given prefix. The `#`
key in the Trie nodes is used to store the cumulative sum for that prefix.

---
### Map Sum Pairs -- Best TC Solution:
```python
# Implementing the MapSum class using Trie

class TrieNode:
    def __init__(self):
        self.children = {}
        self.value = 0


class MapSum:
    def __init__(self):
        self.root = TrieNode()
        self.map = {}

    def insert(self, key: str, val: int) -> None:
        delta = val - self.map.get(key, 0)
        self.map[key] = val
        node = self.root
        node.value += delta
        for char in key:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            node.value += delta

    def sum(self, prefix: str) -> int:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return 0
            node = node.children[char]
        return node.value
```
#### TC: Time Complexity:
- Insert: O(N)
- Sum: O(N)

N is the length of the key or prefix. **SC:** Space Complexity: O(N)

N is the total number of characters in all keys combined.

The code implements the MapSum class using a Trie data structure. The insert function updates the
Trie nodes with the delta value of the new key. The sum function traverses the Trie based on the
prefix to calculate the sum. This approach provides efficient insertion and retrieval of key-value
pairs. The time complexity for both insert and sum operations is O(N), where N is the length of the
key or prefix. The space complexity is O(N) due to the storage of characters in the Trie.

---
### Map Sum Pairs -- Best SC Solution:
```python
# Implementing the MapSum class using Trie

class TrieNode:
    def __init__(self):
        self.children = {}
        self.value = 0


class MapSum:
    def __init__(self):
        self.root = TrieNode()
        self.map = {}

    def insert(self, key: str, val: int) -> None:
        delta = val - self.map.get(key, 0)
        self.map[key] = val
        node = self.root
        node.value += delta
        for char in key:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            node.value += delta

    def sum(self, prefix: str) -> int:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return 0
            node = node.children[char]
        return node.value
```
#### TC: O(N) for both insert and sum operations, where N is the length of the key or prefix **SC:** O(N) for both insert and sum operations, where N is the total number of characters in all keys

The code implements the MapSum class using a Trie data structure. The insert function updates the
Trie nodes with the delta value of the new key. The sum function traverses the Trie based on the
prefix to calculate the sum. The time complexity for both insert and sum operations is O(N), where N
is the length of the key or prefix. The space complexity for both operations is O(N), where N is the
total number of characters in all keys. The Trie approach efficiently handles prefix-based queries
for summing values.

---
---
---
 --- 
# Stream of Characters
>Design a data structure that supports querying a stream of characters for any suffix of a given set of words.

>StreamChecker streamChecker = new StreamChecker(["cd","f","kl"]);
streamChecker.query('a'); // returns false
streamChecker.query('b'); // returns false
streamChecker.query('c'); // returns false
streamChecker.query('d'); // returns true
- https://leetcode.com/problems/stream-of-characters/
- Difficulty: 60
- Frequent: 45
- Tags: ['Trie', 'Design']

### Stream of Characters -- Shortest Solution:
```python
class StreamChecker:
    def __init__(self, words):
        self.trie = {}
        self.stream = []
        for word in words:
            node = self.trie
            for char in word[::-1]:
                if char not in node:
                    node[char] = {}
                node = node[char]
            node['#'] = True
    def query(self, letter):
        self.stream.append(letter)
        node = self.trie
        for char in reversed(self.stream):
            if '#' in node:
                return True
            if char not in node:
                return False
            node = node[char]
        return '#' in node
```
#### TC: O(N) per query, where N is the length of the stream **SC:** O(M), where M is the total number of characters in all words

1. The `StreamChecker` class initializes a Trie data structure to store the reversed words and a
list to keep track of the stream of characters. 2. The `__init__` method constructs the Trie by
inserting each word in reverse order. 3. The `query` method appends the new letter to the stream and
checks if any suffix of the stream forms a word in the Trie. 4. The Trie is traversed in reverse
order of the stream to check for the presence of any word ending at the current character.

---
### Stream of Characters -- Best TC Solution:
```python
# Implementing the StreamChecker class

class StreamChecker:
    def __init__(self, words: List[str]):
        self.trie = {}
        self.stream = deque()
        for word in words:
            node = self.trie
            for char in word[::-1]:
                if char not in node:
                    node[char] = {}
                node = node[char]
            node['#'] = True
        self.max_length = max(len(word) for word in words)

    def query(self, letter: str) -> bool:
        self.stream.appendleft(letter)
        node = self.trie
        for char in self.stream:
            if '#' in node:
                return True
            if char not in node:
                return False
            node = node[char]
        return '#' in node

# Your StreamChecker object will be instantiated and called as such:
# obj = StreamChecker(words)
# param_1 = obj.query(letter)
```
#### TC: O(N) for initialization, O(1) for each query **SC:** O(N) for initialization, O(M) for each query

The code implements the StreamChecker class using a Trie data structure to efficiently check for the
presence of words in a stream of characters. The initialization of the Trie structure takes O(N)
time and space complexity, where N is the total number of characters in the input words. Each query
operation takes O(1) time complexity as it only involves traversing the Trie structure. The space
complexity for each query is O(M), where M is the maximum length of the words in the input. The code
efficiently handles queries by storing the stream of characters in a deque and traversing the Trie
structure accordingly.

---
### Stream of Characters -- Best SC Solution:
```python
# Implementing the StreamChecker class

class StreamChecker:
    def __init__(self, words: List[str]):
        self.trie = {}
        self.stream = deque()
        for word in words:
            node = self.trie
            for char in word[::-1]:
                if char not in node:
                    node[char] = {}
                node = node[char]
            node['#'] = True
        self.max_length = max(len(word) for word in words)

    def query(self, letter: str) -> bool:
        self.stream.appendleft(letter)
        node = self.trie
        for char in self.stream:
            if '#' in node:
                return True
            if char not in node:
                return False
            node = node[char]
        return '#' in node


# Your StreamChecker object will be instantiated and called as such:
# obj = StreamChecker(words)
# param_1 = obj.query(letter)
```
#### TC: 
Time Complexity:
- Initialization: O(N * M) where N is the number of words and M is the average length of a word
- Query: O(M) where M is the maximum length of the words
 **SC:** 
Space Complexity:
- Initialization: O(N * M) where N is the number of words and M is the average length of a word
- Query: O(M) where M is the maximum length of the words


The code implements the StreamChecker class using a Trie data structure to efficiently check for the
presence of words in a stream of characters. The Trie is constructed during initialization by
inserting the reversed words. The query function appends the incoming letter to the stream and
traverses the Trie to check for the presence of any complete words. The use of a deque for the
stream and efficient Trie traversal helps achieve optimal time and space complexities. The code is
clean, easy to understand, and provides a smart solution to the problem.

---
---
---
 --- 
# Concatenated Words
>Find all concatenated words in a given list of words.

>List<String> words = Arrays.asList("cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat");
findAllConcatenatedWordsInADict(words); // returns ["catsdogcats","dogcatsdog","ratcatdogcat"]
- https://leetcode.com/problems/concatenated-words/
- Difficulty: 65
- Frequent: 40
- Tags: ['Trie', 'Dynamic Programming', 'Depth-First Search']

### Concatenated Words -- Shortest Solution:
```python
class Solution:
    def findAllConcatenatedWordsInADict(self, words):
        word_set = set(words)
        memo = {}
        def canForm(word):
            if word in memo:
                return memo[word]
            for i in range(1, len(word)):
                prefix, suffix = word[:i], word[i:]
                if prefix in word_set and (suffix in word_set or canForm(suffix)):
                    memo[word] = True
                    return True
            memo[word] = False
            return False
        return [word for word in words if canForm(word)]
```
#### TC: O(N * L^2) **SC:** O(N * L)

1. **Word Set**: We use a set for quick look-up of words. 2. **Memoization**: A dictionary `memo` is
used to store results of subproblems to avoid redundant calculations. 3. **Recursive Function**: The
`canForm` function checks if a word can be formed by concatenating other words in the set. 4.
**Prefix and Suffix**: For each word, we split it into a prefix and suffix and check if both parts
are in the set or can be formed from other words in the set. 5. **List Comprehension**: Finally, we
use list comprehension to filter and return all concatenated words.

---
### Concatenated Words -- Best TC Solution:
```python
from typing import List
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Solution:
    def findAllConcatenatedWordsInADict(self, words: List[str]) -> List[str]:
        def dfs(word, index, count):
            if index == len(word):
                return count > 1
            if count > 1 and index in memo:
                return memo[index]
            node = root
            for i in range(index, len(word)):
                if word[i] in node.children:
                    node = node.children[word[i]]
                    if node.is_end_of_word and dfs(word, i + 1, count + 1):
                        if count > 1:
                            memo[index] = True
                        return True
                else:
                    break
            return False
        root = TrieNode()
        for w in words:
            node = root
            for char in w:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.is_end_of_word = True
        res = []
        for word in words:
            if dfs(word, 0, 0):
                res.append(word)
        return res
```
#### TC: O(N * M^2) **SC:** O(N * M)

The code defines a Trie data structure to efficiently store and search for words. The main function,
findAllConcatenatedWordsInADict, uses depth-first search (DFS) to check if a word can be formed by
concatenating other words in the given list. The DFS function recursively checks if substrings of
the word match words in the Trie. The memoization technique is used to optimize the search process
by storing intermediate results. The time complexity is O(N * M^2), where N is the number of words
and M is the average length of a word. The space complexity is O(N * M) to store the Trie structure.

---
---
---
 --- 
# Palindrome Pairs
>Given a list of unique words, find all pairs of distinct indices (i, j) such that the concatenation of words[i] and words[j] is a palindrome.

>List<String> words = Arrays.asList("bat","tab","cat");
List<List<Integer>> result = palindromePairs(words); // returns [[0,1],[1,0]]
- https://leetcode.com/problems/palindrome-pairs/
- Difficulty: 70
- Frequent: 35
- Tags: ['Trie', 'Hash Table', 'String']

### Palindrome Pairs -- Shortest Solution:
```python
class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        def is_palindrome(word):
            return word == word[::-1]
        word_dict = {word: i for i, word in enumerate(words)}
        results = []
        for i, word in enumerate(words):
            n = len(word)
            for j in range(n + 1):
                prefix, suffix = word[:j], word[j:]
                if is_palindrome(prefix):
                    back = suffix[::-1]
                    if back != word and back in word_dict:
                        results.append([word_dict[back], i])
                if j != n and is_palindrome(suffix):
                    front = prefix[::-1]
                    if front != word and front in word_dict:
                        results.append([i, word_dict[front]])
        return results
```
#### TC: O(n * k^2) **SC:** O(n * k)

1. **Palindrome Check**: The function `is_palindrome` checks if a word is a palindrome by comparing
it to its reverse. 2. **Dictionary for Indices**: `word_dict` maps each word to its index for quick
look-up. 3. **Iterate Through Words**: For each word, split it into all possible prefixes and
suffixes. 4. **Check Prefix and Suffix**: For each split, check if the prefix or suffix is a
palindrome. 5. **Find Reverse in Dictionary**: If the reverse of the non-palindromic part exists in
the dictionary, add the pair to the results. 6. **Avoid Duplicates**: Ensure the word is not paired
with itself by checking `back != word` and `front != word`. 7. **Return Results**: Finally, return
the list of palindrome pairs.

---
### Palindrome Pairs -- Best TC Solution:
```python
# Trie Node

class TrieNode:
    def __init__(self):
        self.children = {}
        self.word_id = -1
        self.palindrome_suffixes = []


class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]:
        def is_palindrome(word: str) -> bool:
            return word == word[::-1]

        def add_word_to_trie(root, word, index):
            for i in range(len(word)):
                if is_palindrome(word[i:]):
                    root.palindrome_suffixes.append(index)
                if word[i] not in root.children:
                    root.children[word[i]] = TrieNode()\n                root = root.children[word[i]]
            root.word_id = index
            root.palindrome_suffixes.append(index)

        def search_word_in_trie(root, word, index):
            res = []
            while word:
                if root.word_id >= 0 and is_palindrome(word):
                    res.append([index, root.word_id])
                if word[0] not in root.children:
                    return res
                root = root.children[word[0]]
                word = word[1:]
            for palindrome_id in root.palindrome_suffixes:
                if palindrome_id == index:
                    continue
                res.append([index, palindrome_id])
            return res

        trie = TrieNode()
        for i, word in enumerate(words):
            add_word_to_trie(trie, word, i)

        res = []
        for i, word in enumerate(words):
            res.extend(search_word_in_trie(trie, word[::-1], i))

        return res
```
#### TC: O(N * K^2) **SC:** O(N * K)

The code implements the Palindrome Pairs problem using a Trie data structure. It first defines a
TrieNode class to represent the nodes in the Trie. The solution involves adding words to the Trie
while considering palindrome suffixes and searching for palindrome pairs efficiently. The
is_palindrome function checks if a word is a palindrome. The add_word_to_trie function adds words to
the Trie, and the search_word_in_trie function searches for palindrome pairs. The main logic
involves iterating through the words, adding them to the Trie, and searching for palindrome pairs.
The time complexity is O(N * K^2) where N is the number of words and K is the average length of a
word, and the space complexity is O(N * K).

---
---
---
 --- 
# Longest Word in Dictionary
>Find the longest word in a dictionary that can be built one character at a time by other words in the dictionary.

>String[] words = {"w","wo","wor","worl","world"};
longestWord(words); // returns "world"
- https://leetcode.com/problems/longest-word-in-dictionary/
- Difficulty: 50
- Frequent: 30
- Tags: ['Trie', 'Array', 'Hash Table', 'String']

### Longest Word in Dictionary -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.end_of_word = False

class Solution:
    def longestWord(self, words):
        root = TrieNode()
        for word in words:
            node = root
            for char in word:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.end_of_word = True
        longest = ""
        stack = [(root, "")]
        while stack:
            node, path = stack.pop()
            if node.end_of_word or node == root:
                if len(path) > len(longest) or (len(path) == len(longest) and path < longest):
                    longest = path
                for char in sorted(node.children.keys(), reverse=True):
                    stack.append((node.children[char], path + char))
        return longest
```
#### TC: O(N * M) **SC:** O(N * M)

1. **Trie Construction**: The code first constructs a Trie (prefix tree) from the list of words.
Each node in the Trie represents a character in a word. 2. **DFS for Longest Word**: It then uses
Depth-First Search (DFS) to find the longest word that can be built one character at a time by other
words in the list. 3. **Stack for DFS**: A stack is used to perform the DFS. The stack stores tuples
of the current Trie node and the path (word) formed so far. 4. **End of Word Check**: The code
checks if the current node marks the end of a word and updates the longest word accordingly. 5.
**Sorting for Lexicographical Order**: The children of each node are processed in reverse sorted
order to ensure the lexicographically smallest word is found if multiple words have the same length.

---
### Longest Word in Dictionary -- Best TC Solution:
```python
from collections import defaultdict
class Solution:
    def longestWord(self, words: List[str]) -> str:
        words.sort()
        word_set = set(words)
        longest_word = ''
        for word in words:
            if len(word) > len(longest_word) or (len(word) == len(longest_word) and word < longest_word):
                if all(word[:k] in word_set for k in range(1, len(word))):
                    longest_word = word
        return longest_word
```
#### TC: O(N * K) **SC:** O(N)

The code sorts the input list of words and then iterates through each word to find the longest word
that can be built one character at a time. It uses a set to efficiently check if the prefixes of the
word exist in the set of words. The time complexity is O(N * K) where N is the number of words and K
is the average length of a word. The space complexity is O(N) to store the set of words.

---
---
---
 --- 
# Search Suggestions System
>Given an array of product names and a search word, return a list of lists of the suggested products after each character of the search word is typed.

>String[] products = {"mobile","mouse","moneypot","monitor","mousepad"};
String searchWord = "mouse";
suggestedProducts(products, searchWord); // returns [["mobile","moneypot","monitor"],["mouse","mousepad"],["mouse","mousepad"],["mouse","mousepad"],["mouse","mousepad"]]
- https://leetcode.com/problems/search-suggestions-system/
- Difficulty: 55
- Frequent: 25
- Tags: ['Trie', 'Array', 'String']

### Search Suggestions System -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.suggestions = []

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            if len(node.suggestions) < 3:
                node.suggestions.append(word)

    def search(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return []
            node = node.children[char]
        return node.suggestions

class Solution:
    def suggestedProducts(self, products, searchWord):
        trie = Trie()
        for product in sorted(products):
            trie.insert(product)
        result = []
        prefix = ''
        for char in searchWord:
            prefix += char
            result.append(trie.search(prefix))
        return result
```
#### TC: O(N * M + L * M) **SC:** O(N * M)

1. **TrieNode Class**: Represents each node in the Trie. Each node contains a dictionary of children
and a list of suggestions. 2. **Trie Class**: Manages the Trie. It has methods to insert words and
search for suggestions based on a prefix. 3. **Insert Method**: Adds words to the Trie. As it adds
each character, it also updates the suggestions list for each node, ensuring it contains at most 3
words. 4. **Search Method**: Retrieves the list of suggestions for a given prefix by traversing the
Trie. 5. **Solution Class**: Manages the overall process. It first inserts all products into the
Trie (sorted to ensure lexicographical order). Then, for each character in the search word, it
retrieves the suggestions and appends them to the result.

---
### Search Suggestions System -- Best TC Solution:
```python
# Trie Node class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False


class Trie:
    def __init__(self):
        self.root = TrieNode()
        self.suggestions = []

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return []
            node = node.children[char]
        self.suggestions = []
        self._dfs(node, list(prefix))
        return self.suggestions[:3]

    def _dfs(self, node, path):
        if node.is_end_of_word:
            self.suggestions.append(''.join(path))
            if len(self.suggestions) == 3:
                return
        for char, child_node in node.children.items():
            self._dfs(child_node, path + [char])


class Solution:
    def suggestedProducts(self, products: List[str], searchWord: str) -> List[List[str]]:
        trie = Trie()
        for product in products:
            trie.insert(product)
        result = []
        prefix = ''
        for char in searchWord:
            prefix += char
            result.append(trie.search(prefix))
        return result
```
#### TC: O(N*M) where N is the number of products and M is the length of the search word **SC:** O(N*M) where N is the number of products and M is the average length of the products

The code implements a Trie data structure to efficiently store and search for products based on a
search word. The Trie allows for prefix-based search, providing suggestions as the search word is
typed. The insert method inserts products into the Trie, and the search method finds suggestions for
a given prefix. The _dfs method performs a depth-first search to find suggestions. The
suggestedProducts method utilizes the Trie to generate suggestions for each character of the search
word. The time complexity is O(N*M) where N is the number of products and M is the length of the
search word, and the space complexity is O(N*M) where N is the number of products and M is the
average length of the products.

---
### Search Suggestions System -- Best SC Solution:
```python
from typing import List

class TrieNode:
    def __init__(self):
        self.children = {}
        self.words = []


class Solution:
    def suggestedProducts(self, products: List[str], searchWord: str) -> List[List[str]]:
        root = TrieNode()
        products.sort()
        res = []
        for product in products:
            node = root
            for char in product:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
                node.words.append(product)
                if len(node.words) > 3:
                    node.words.pop(0)
        for char in searchWord:
            if root:
                root = root.children.get(char)
            res.append(root.words if root else [])
        return res
```
#### TC: O(NlogN + M) where N is the total number of characters in all products and M is the length of the searchWord **SC:** O(N) where N is the total number of characters in all products

The code implements a Trie data structure to efficiently store and retrieve words. It sorts the
products list and builds a Trie with each product. The Trie structure allows for quick lookup of
words matching the searchWord prefix. The time complexity is O(NlogN + M) due to sorting the
products and iterating through the searchWord. The space complexity is O(N) to store the Trie
structure.

---
---
---
 --- 
# Maximum XOR of Two Numbers in an Array
>Find the maximum XOR of two numbers in an array.

>int[] nums = {3, 10, 5, 25, 2, 8};
findMaximumXOR(nums); // returns 28
- https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/
- Difficulty: 65
- Frequent: 20
- Tags: ['Trie', 'Bit Manipulation']

### Maximum XOR of Two Numbers in an Array -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, num):
        node = self.root
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            if bit not in node.children:
                node.children[bit] = TrieNode()
            node = node.children[bit]

    def find_max_xor(self, num):
        node = self.root
        max_xor = 0
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            toggled_bit = 1 - bit
            if toggled_bit in node.children:
                max_xor = (max_xor << 1) | 1
                node = node.children[toggled_bit]
            else:
                max_xor = max_xor << 1
                node = node.children[bit]
        return max_xor

class Solution:
    def findMaximumXOR(self, nums):
        trie = Trie()
        for num in nums:
            trie.insert(num)
        max_xor = 0
        for num in nums:
            max_xor = max(max_xor, trie.find_max_xor(num))
        return max_xor
```
#### TC: O(n) **SC:** O(n)

1. **TrieNode and Trie Classes**: These classes are used to build a Trie (prefix tree) where each
node represents a bit (0 or 1). 2. **Insert Method**: This method inserts a number into the Trie bit
by bit. 3. **Find Max XOR Method**: This method finds the maximum XOR for a given number by
traversing the Trie and trying to take the opposite bit at each step to maximize the XOR value. 4.
**Solution Class**: This class contains the main method `findMaximumXOR` which first inserts all
numbers into the Trie and then finds the maximum XOR for each number by comparing it with all other
numbers in the Trie.

---
### Maximum XOR of Two Numbers in an Array -- Best TC Solution:
```python
from typing import List

class TrieNode:
    def __init__(self):
        self.children = {}

class Solution:
    def findMaximumXOR(self, nums: List[int]) -> int:
        root = TrieNode()
        MAX_BIT = 30
        ans = 0
        for num in nums:
            node = root
            xorNode = root
            curr_xor = 0
            for i in range(MAX_BIT, -1, -1):
                bit = (num >> i) & 1
                if bit not in node.children:
                    node.children[bit] = TrieNode()
                node = node.children[bit]

                toggled_bit = 1 - bit
                if toggled_bit in xorNode.children:
                    curr_xor = (curr_xor << 1) | 1
                    xorNode = xorNode.children[toggled_bit]
                else:
                    curr_xor = curr_xor << 1
                    xorNode = xorNode.children[bit]

            ans = max(ans, curr_xor)

        return ans
```
#### TC: O(N) **SC:** O(N)

The code uses a Trie data structure to efficiently find the maximum XOR of two numbers in the given
array. It iterates through each number in the array, building the Trie based on the binary
representation of the numbers. By toggling bits and checking for the presence of complementary bits
in the Trie, it calculates the XOR value. The code has a time complexity of O(N) and a space
complexity of O(N), where N is the number of elements in the input array.

---
---
---
 --- 
# Implement Magic Dictionary
>Design a data structure that supports adding new words and finding if a string matches any previously added string with exactly one modified character.

>MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict(new String[]{"hello", "leetcode"});
magicDictionary.search("hello"); // returns false
magicDictionary.search("hhllo"); // returns true
magicDictionary.search("hell"); // returns false
magicDictionary.search("leetcoded"); // returns false
- https://leetcode.com/problems/implement-magic-dictionary/
- Difficulty: 55
- Frequent: 15
- Tags: ['Trie', 'Hash Table', 'String']

### Implement Magic Dictionary -- Shortest Solution:
```python
class MagicDictionary:
    def __init__(self):
        self.words = set()

    def buildDict(self, dictionary):
        self.words = set(dictionary)

    def search(self, searchWord):
        for word in self.words:
            if len(word) == len(searchWord):
                mismatch = 0
                for a, b in zip(word, searchWord):
                    if a != b:
                        mismatch += 1
                    if mismatch > 1:
                        break
                if mismatch == 1:
                    return True
        return False
```
#### TC: O(N * K) **SC:** O(N * K)

The code defines a MagicDictionary class with methods to build a dictionary and search for words
with exactly one character mismatch. The buildDict method stores the dictionary words in a set for
O(1) average-time complexity lookups. The search method iterates through each word in the set,
checking if it has the same length as the searchWord. If so, it counts the number of character
mismatches. If exactly one mismatch is found, it returns True; otherwise, it continues checking. The
time complexity is O(N * K), where N is the number of words and K is the average length of the
words. The space complexity is also O(N * K) due to storing the words in a set.

---
### Implement Magic Dictionary -- Best TC Solution:
```python
# TrieNode class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False


class MagicDictionary:
    def __init__(self):
        self.root = TrieNode()

    def buildDict(self, dictionary: List[str]) -> None:
        for word in dictionary:
            node = self.root
            for char in word:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.is_end_of_word = True

    def search(self, searchWord: str) -> bool:
        def dfs(node, index, mismatch):
            if mismatch > 1:
                return False
            if index == len(searchWord):
                return node.is_end_of_word and mismatch == 1
            char = searchWord[index]
            for key, child in node.children.items():
                if key == char:
                    if dfs(child, index + 1, mismatch):
                        return True
                else:
                    if dfs(child, index + 1, mismatch + 1):
                        return True
            return False

        return dfs(self.root, 0, 0)
```
#### TC: O(N*L) for both buildDict and search functions, where N is the number of words in the dictionary and L is the average length of the words. **SC:** O(N*L) where N is the number of words in the dictionary and L is the average length of the words.

The code implements a Trie data structure to store the dictionary words efficiently. The buildDict
function inserts each word into the Trie. The search function performs a depth-first search on the
Trie to find a word with exactly one mismatch. The time complexity for both functions is O(N*L),
where N is the number of words in the dictionary and L is the average length of the words. The space
complexity is also O(N*L) due to the Trie structure storing the dictionary words.

---
### Implement Magic Dictionary -- Best SC Solution:
```python
# TrieNode class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False


class MagicDictionary:
    def __init__(self):
        self.root = TrieNode()

    def buildDict(self, dictionary: List[str]) -> None:
        for word in dictionary:
            node = self.root
            for char in word:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.is_end_of_word = True

    def search(self, searchWord: str) -> bool:
        def dfs(node, index, mismatch):
            if mismatch > 1:
                return False
            if index == len(searchWord):
                return node.is_end_of_word and mismatch == 1
            char = searchWord[index]
            for key, child in node.children.items():
                if key == char:
                    if dfs(child, index + 1, mismatch):
                        return True
                else:
                    if dfs(child, index + 1, mismatch + 1):
                        return True
            return False

        return dfs(self.root, 0, 0)
```
#### TC: Time Complexity: 
- For buildDict: O(N * M) where N is the number of words in the dictionary and M is the average length of the words
- For search: O(M) where M is the length of the search word **SC:** Space Complexity: O(N * M) where N is the number of words in the dictionary and M is the average length of the words

The code implements a Magic Dictionary using a Trie data structure. The buildDict function
constructs the Trie by inserting each word from the dictionary. The search function performs a
depth-first search on the Trie to find a word with exactly one mismatch. The use of Trie helps in
efficient storage and retrieval of words with minimal space complexity. The time complexity for both
buildDict and search operations is optimized to handle the dictionary and search efficiently.

---
---
---
 --- 
# Prefix and Suffix Search
>Design a special dictionary with some search functions.

>WordFilter wordFilter = new WordFilter(new String[]{"apple"});
wordFilter.f("a", "e"); // returns 0
- https://leetcode.com/problems/prefix-and-suffix-search/
- Difficulty: 70
- Frequent: 10
- Tags: ['Trie', 'Design']

### Prefix and Suffix Search -- Shortest Solution:
```python
class WordFilter:
    def __init__(self, words):
        self.trie = {}
        for weight, word in enumerate(words):
            word_key = word + '#' + word
            for i in range(len(word) + 1):
                current = self.trie
                for j in range(i, 2 * len(word) + 1):
                    if word_key[j] not in current:
                        current[word_key[j]] = {}
                    current = current[word_key[j]]
                    current['#'] = weight
    
    def f(self, prefix, suffix):
        current = self.trie
        for char in suffix + '#' + prefix:
            if char not in current:
                return -1
            current = current[char]
        return current['#']
```
#### TC: O(N * K^2 + Q * K) **SC:** O(N * K^2)

1. The `WordFilter` class uses a Trie data structure to store the words with their respective
weights. 2. In the `__init__` method, each word is processed by creating a combined key of the word
and itself separated by a '#'. This allows us to handle both prefix and suffix searches. 3. The
nested loops in the `__init__` method ensure that all possible prefix and suffix combinations are
stored in the Trie with their corresponding weights. 4. The `f` method searches the Trie using the
combined key of the suffix and prefix to find the maximum weight. 5. The time complexity is O(N *
K^2 + Q * K), where N is the number of words, K is the maximum length of a word, and Q is the number
of queries. 6. The space complexity is O(N * K^2) due to the storage of all possible prefix and
suffix combinations in the Trie.

---
### Prefix and Suffix Search -- Best TC Solution:
```python
# Implementing the Trie data structure

class TrieNode:
    def __init__(self):
        self.children = {}
        self.index = -1


class WordFilter:
    def __init__(self, words: List[str]):
        self.trie = TrieNode()
        for weight, word in enumerate(words):
            for i in range(len(word) + 1):
                cur = self.trie
                cur.index = weight
                for j in range(i, 2 * len(word) + 1):
                    cur = cur.children.setdefault(word[j % len(word)], TrieNode())
                    cur.index = weight

    def f(self, prefix: str, suffix: str) -> int:
        cur = self.trie
        for letter in suffix + '#' + prefix:
            if letter not in cur.children:
                return -1
            cur = cur.children[letter]
        return cur.index


# Your WordFilter object will be instantiated and called as such:
# obj = WordFilter(words)
# param_1 = obj.f(prefix, suffix)
```
#### TC: 
Time Complexity:
- Constructor: O(N * M^2) where N is the number of words and M is the average length of a word
- f function: O(M) where M is the length of the prefix or suffix
 **SC:** 
Space Complexity:
- Constructor: O(N * M^2) where N is the number of words and M is the average length of a word
- f function: O(1)

The code implements a Trie data structure to efficiently store and search for words based on
prefixes and suffixes. The TrieNode class represents a node in the Trie with children nodes and an
index to store the weight of the word. The WordFilter class initializes the Trie with words and
their prefixes and suffixes. The f function searches for a given prefix and suffix combination in
the Trie to return the weight of the word. The time complexity for the constructor and f function is
optimized to handle the search efficiently. The space complexity is also optimized to store the Trie
structure effectively.

---
### Prefix and Suffix Search -- Best SC Solution:
```python
# Trie Node

class TrieNode:
    def __init__(self):
        self.children = {}
        self.index = []


class WordFilter:

    def __init__(self, words: List[str]):
        self.trie = TrieNode()
        for weight, word in enumerate(words):
            for i in range(len(word) + 1):
                cur = self.trie
                cur.index.append(weight)
                for j in range(i, 2 * len(word) + 1):
                    cur = cur.children.setdefault(word[j % len(word)], TrieNode())
                    cur.index.append(weight)

    def f(self, prefix: str, suffix: str) -> int:
        cur = self.trie
        for letter in suffix + '#' + prefix:
            if letter not in cur.children:
                return -1
            cur = cur.children[letter]
        return max(cur.index)
```
#### TC: 
Time Complexity:
- Initialization: O(N * M^2) where N is the number of words and M is the average length of a word
- Search: O(M) where M is the length of the prefix or suffix
 **SC:** 
Space Complexity:
- O(N * M^2) where N is the number of words and M is the average length of a word


The code uses a Trie data structure to store the words and their prefixes and suffixes. During
initialization, the code creates a Trie with all possible prefixes and suffixes of each word. The
`f` function then traverses the Trie based on the given prefix and suffix to find the maximum weight
of a word that matches the criteria. The time complexity for initialization is O(N * M^2) where N is
the number of words and M is the average length of a word, and the time complexity for search is
O(M) where M is the length of the prefix or suffix. The space complexity is O(N * M^2) where N is
the number of words and M is the average length of a word.

---
---
---
 --- 
# Longest Word With All Prefixes
>Find the longest word in a dictionary that can be built one character at a time by other words in the dictionary.

>String[] words = {"k","ki","kir","kira","kiran"};
longestWord(words); // returns "kiran"
- https://leetcode.com/problems/longest-word-with-all-prefixes/
- Difficulty: 50
- Frequent: 5
- Tags: ['Trie', 'Array', 'Hash Table', 'String']

### Longest Word With All Prefixes -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def all_prefixes_exist(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
            if not node.is_end:
                return False
        return True

class Solution:
    def longestWord(self, words):
        trie = Trie()
        for word in words:
            trie.insert(word)
        words.sort(key=lambda x: (-len(x), x))
        for word in words:
            if trie.all_prefixes_exist(word):
                return word
        return ""
```
#### TC: O(N * L + M * log M) **SC:** O(N * L)

1. **TrieNode and Trie Classes**: These classes are used to build a Trie (prefix tree) where each
node represents a character in a word. The `insert` method adds words to the Trie, and the
`all_prefixes_exist` method checks if all prefixes of a word exist in the Trie. 2. **Sorting
Words**: The words are sorted by length in descending order and lexicographically. This ensures that
the longest word with all prefixes is checked first. 3. **Checking Prefixes**: For each word, the
`all_prefixes_exist` method is used to check if all its prefixes are present in the Trie. The first
word that satisfies this condition is returned as the result.

---
### Longest Word With All Prefixes -- Best TC Solution:
```python
from typing import List

class Solution:
    def longestWord(self, words: List[str]) -> str:
        words.sort()
        word_set = set(words)
        longest_word = ''
        for word in words:
            if len(word) > len(longest_word) or (len(word) == len(longest_word) and word < longest_word):
                if all(word[:i] in word_set for i in range(1, len(word))):
                    longest_word = word
        return longest_word
```
#### TC: O(N * L) **SC:** O(N)

The code defines a class Solution with a method longestWord that takes a list of words as input. It
sorts the words and creates a set for quick lookup. It then iterates through each word, checking if
it is a valid prefix of any longer word. The longest valid word is returned at the end. The time
complexity is O(N * L) where N is the number of words and L is the average length of a word. The
space complexity is O(N) to store the set of words.

---
### Longest Word With All Prefixes -- Best SC Solution:
```python
from collections import defaultdict
class TrieNode:
    def __init__(self):
        self.children = defaultdict(TrieNode)
        self.is_end = False
        

class Solution:
    def longestWord(self, words: List[str]) -> str:
        trie = TrieNode()
        for word in words:
            node = trie
            for char in word:
                node = node.children[char]
            node.is_end = True
        
        longest_word = ""
        stack = [(trie, "")]
        while stack:
            node, word = stack.pop()
            if len(word) > len(longest_word) or (len(word) == len(longest_word) and word < longest_word):
                longest_word = word
            for char, child in node.children.items():
                if child.is_end:
                    stack.append((child, word + char))
        
        return longest_word
```
#### TC: O(N * M) **SC:** O(N * M)

The code uses a Trie data structure to store the words efficiently. It iterates through each word to
build the Trie. Then, it uses a stack to traverse the Trie and find the longest word with all
prefixes. The time complexity is O(N * M), where N is the total number of characters in all words
and M is the average length of the words. The space complexity is also O(N * M) to store the Trie
structure.

---
---
---
 --- 
# Search in a Sorted Array of Unknown Size
>Given a sorted array of unknown size, search for a target value.

>ArrayReader reader = ...;
int target = 10;
search(reader, target); // returns 4
- https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/
- Difficulty: 60
- Frequent: 5
- Tags: ['Trie', 'Binary Search']

### Search in a Sorted Array of Unknown Size -- Shortest Solution:
```python
class ArrayReader:
    def get(self, index: int) -> int:
        pass

def search(reader: 'ArrayReader', target: int) -> int:
    left, right = 0, 1
    while reader.get(right) < target:
        left = right
        right <<= 1
    while left <= right:
        mid = left + (right - left) // 2
        num = reader.get(mid)
        if num == target:
            return mid
        elif num < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
#### TC: O(log T) where T is the index of the target **SC:** O(1)

1. **ArrayReader Interface**: The problem assumes an `ArrayReader` interface with a `get` method to
fetch elements at a given index. 2. **Exponential Search**: Start with a range [0, 1] and
exponentially increase the range until the target is within the range. This helps in efficiently
narrowing down the search space. 3. **Binary Search**: Once the range is identified, perform a
binary search within this range to find the target. 4. **Edge Cases**: Handles cases where the
target is smaller than the smallest element or larger than the largest element in the array.

---
### Search in a Sorted Array of Unknown Size -- Best TC Solution:
```python
# Approach: Binary Search to find the bounds of the array

class Solution:
    def search(self, reader, target):
        left, right = 0, 1
        while reader.get(right) < target:
            left = right
            right <<= 1
        while left <= right:
            mid = left + (right - left) // 2
            num = reader.get(mid)
            if num == target:
                return mid
            elif num < target:
                left = mid + 1
            else:
                right = mid - 1
        return -1
```
#### TC: O(log n) **SC:** O(1)

The code implements a binary search algorithm to find the target in a sorted array of unknown size.
It first finds the bounds of the array by doubling the right pointer until the value at that index
is greater than the target. Then, it performs a standard binary search within the identified bounds
to locate the target. The time complexity is O(log n) as it halves the search space in each
iteration, and the space complexity is O(1) as it uses a constant amount of extra space.

---
---
---
 --- 
# Word Squares
>Given a set of words, find all word squares.

>List<String> words = Arrays.asList("area","lead","wall","lady","ball");
wordSquares(words); // returns [["wall","area","lead","lady"],["ball","area","lead","lady"]]
- https://leetcode.com/problems/word-squares/
- Difficulty: 70
- Frequent: 5
- Tags: ['Trie', 'Backtracking']

### Word Squares -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.words = []

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            node.words.append(word)

    def find_words_with_prefix(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return []
            node = node.children[char]
        return node.words

class Solution:
    def wordSquares(self, words):
        def build_trie(words):
            trie = Trie()
            for word in words:
                trie.insert(word)
            return trie

        def backtrack(step):
            if step == n:
                results.append(square[:])
                return
            prefix = ''.join([square[i][step] for i in range(step)])
            for candidate in trie.find_words_with_prefix(prefix):
                square.append(candidate)
                backtrack(step + 1)
                square.pop()

        n = len(words[0])
        trie = build_trie(words)
        results = []
        square = []
        for word in words:
            square.append(word)
            backtrack(1)
            square.pop()
        return results
```
#### TC: O(N * 26^L + N * L^2) **SC:** O(N * L)

1. **Trie Construction**: The Trie is built to store all words and facilitate quick prefix-based
searches. Each node in the Trie keeps track of words that share the same prefix. 2.
**Backtracking**: The backtracking function constructs word squares by adding words that match the
required prefix at each step. It recursively builds the square and backtracks when necessary. 3.
**Prefix Matching**: For each step in the backtracking process, the prefix is formed by taking
characters from the current partially formed square. The Trie is then queried to find all words that
match this prefix. 4. **Efficiency**: The Trie allows for efficient prefix searches, and the
backtracking ensures that all possible word squares are explored. The combination of these
techniques results in an optimal solution.

---
### Word Squares -- Best TC Solution:
```python
from collections import defaultdict
def wordSquares(words):
    n = len(words[0])
    prefix_dict = defaultdict(list)
    for word in words:
        for i in range(len(word)):
            prefix_dict[word[:i]].append(word)
    def backtrack(square):
        if len(square) == n:
            squares.append(square[:])
            return
        prefix = ''
        for i in range(len(square)):
            prefix += square[i][len(square)]
        for word in prefix_dict[prefix]:
            square.append(word)
            backtrack(square)
            square.pop()
    squares = []
    for word in words:
        backtrack([word])
    return squares
```
#### TC: O(N 	imes N! 	imes N) **SC:** O(N^2)

The code uses a backtracking approach to generate word squares. It first creates a prefix dictionary
to store prefixes of words. The backtrack function recursively builds word squares by adding words
that match the prefix. The time complexity is O(N       imes N!         imes N) where N is the
length of words, and the space complexity is O(N^2) to store the prefix dictionary and the word
squares.

---
---
---
 --- 
# Maximum XOR With an Element From Array
>Find the maximum XOR of an element from an array with a given integer.

>int[] nums = {0,1,2,3,4};
int[][] queries = {{3,1},{1,3},{5,6}};
maximizeXor(nums, queries); // returns [3,3,7]
- https://leetcode.com/problems/maximum-xor-with-an-element-from-array/
- Difficulty: 70
- Frequent: 5
- Tags: ['Trie', 'Array', 'Bit Manipulation']

### Maximum XOR With an Element From Array -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.value = -1

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, num):
        node = self.root
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            if bit not in node.children:
                node.children[bit] = TrieNode()
            node = node.children[bit]
        node.value = num

    def getMaxXOR(self, num):
        node = self.root
        for i in range(31, -1, -1):
            bit = (num >> i) & 1
            toggled_bit = 1 - bit
            if toggled_bit in node.children:
                node = node.children[toggled_bit]
            else:
                node = node.children[bit]
        return num ^ node.value

class Solution:
    def maximizeXor(self, nums, queries):
        nums.sort()
        queries = sorted([(x, m, i) for i, (x, m) in enumerate(queries)], key=lambda x: x[1])
        res = [-1] * len(queries)
        trie = Trie()
        j = 0
        for x, m, i in queries:
            while j < len(nums) and nums[j] <= m:
                trie.insert(nums[j])
                j += 1
            if j != 0:
                res[i] = trie.getMaxXOR(x)
        return res
```
#### TC: O(N log N + Q log Q + Q * 32) **SC:** O(N * 32)

1. **TrieNode and Trie Classes**: These classes are used to build a Trie (prefix tree) where each
node represents a bit (0 or 1) of the numbers in the array. The Trie helps in efficiently finding
the maximum XOR for a given number.  2. **Insert Method**: This method inserts a number into the
Trie by breaking it down into its binary representation and storing each bit in the Trie.  3.
**getMaxXOR Method**: This method finds the maximum XOR of a given number with the numbers already
inserted in the Trie. It tries to take the opposite bit (to maximize XOR) at each level of the Trie.
4. **maximizeXor Method**: This method processes the queries. It first sorts the `nums` array and
the `queries` based on the maximum allowed value `m`. It then inserts numbers from `nums` into the
Trie as long as they are less than or equal to `m` and uses the `getMaxXOR` method to find the
maximum XOR for each query.  5. **Sorting**: Sorting `nums` and `queries` ensures that we only
insert relevant numbers into the Trie, optimizing the process.  6. **Result Array**: The results of
the queries are stored in the `res` array, which is returned at the end.

---
### Maximum XOR With an Element From Array -- Best TC Solution:
```python
from typing import List

class TrieNode:
    def __init__(self):
        self.children = {}

class Trie:
    def __init__(self):
        self.root = TrieNode()
        self.max_bits = 32
    
    def insert(self, num):
        node = self.root
        for i in range(self.max_bits-1, -1, -1):
            bit = (num >> i) & 1
            if bit not in node.children:
                node.children[bit] = TrieNode()
            node = node.children[bit]
        
    def find_max_xor(self, num):
        node = self.root
        xor_num = 0
        for i in range(self.max_bits-1, -1, -1):
            bit = (num >> i) & 1
            if 1 - bit in node.children:
                xor_num |= (1 << i)
                node = node.children[1 - bit]
            else:
                node = node.children[bit]
        return xor_num


def maximizeXor(nums: List[int], queries: List[List[int]]) -> List[int]:
    nums.sort()
    queries = sorted(enumerate(queries), key=lambda x: x[1][1])
    trie = Trie()
    res = [-1] * len(queries)
    j = 0
    for i, (x, m) in queries:
        while j < len(nums) and nums[j] <= m:
            trie.insert(nums[j])
            j += 1
        if j > 0:
            res[i] = trie.find_max_xor(x)
    return res
```
#### TC: O(NlogN + QlogQ) **SC:** O(N + Q)

The code defines a Trie data structure to efficiently find the maximum XOR value for each query. It
sorts the input array and queries to optimize the search process. The insert method inserts numbers
into the Trie, and the find_max_xor method calculates the maximum XOR value. The maximizeXor
function processes the queries by iterating through the sorted queries and input array. Overall, the
code achieves a time complexity of O(NlogN + QlogQ) and a space complexity of O(N + Q), where N is
the number of elements in the input array and Q is the number of queries.

---
---
---