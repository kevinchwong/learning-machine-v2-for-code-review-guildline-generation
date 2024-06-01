# Suffix Tree
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Longest Substring with At Most Three Distinct Characters](#longest-substring-with-at-most-three-distinct-characters) | 65 | 70 |
| 2 | [Longest Substring with At Most Four Distinct Characters](#longest-substring-with-at-most-four-distinct-characters) | 65 | 70 |
| 3 | [Longest Substring with At Most Five Distinct Characters](#longest-substring-with-at-most-five-distinct-characters) | 65 | 70 |
| 4 | [Longest Substring with At Most Six Distinct Characters](#longest-substring-with-at-most-six-distinct-characters) | 65 | 70 |
| 5 | [Longest Repeated Substring](#longest-repeated-substring) | 60 | 80 |
| 6 | [Find the Longest Substring Containing Vowels in Even Counts](#find-the-longest-substring-containing-vowels-in-even-counts) | 55 | 75 |
| 7 | [Distinct Substrings](#distinct-substrings) | 50 | 70 |
| 8 | [Count of Distinct Substrings](#count-of-distinct-substrings) | 50 | 70 |
| 9 | [Number of Distinct Substrings in a String](#number-of-distinct-substrings-in-a-string) | 50 | 70 |
| 10 | [Lexicographical Order](#lexicographical-order) | 45 | 65 |
---
A Suffix Tree is a compressed trie of all suffixes of a given text. It allows for fast substring queries, like searching, locating, and finding the longest repeated substring. Each path from the root to a leaf represents a suffix of the text.
```python
class SuffixTreeNode:
    def __init__(self):
        self.children = {}
        self.indexes = []

class SuffixTree:
    def __init__(self, text):
        self.root = SuffixTreeNode()
        self.text = text
        self._build_suffix_tree()

    def _build_suffix_tree(self):
        for i in range(len(self.text)):
            current = self.root
            for char in self.text[i:]:
                if char not in current.children:
                    current.children[char] = SuffixTreeNode()
                current = current.children[char]
                current.indexes.append(i)

    def search(self, pattern):
        current = self.root
        for char in pattern:
            if char not in current.children:
                return []
            current = current.children[char]
        return current.indexes

# Example usage
text = "banana"
st = SuffixTree(text)
print(st.search("ana"))  # Output: [1, 3]
```
### Insight:
1. Suffix Trees are particularly useful for fast pattern matching and substring operations.
2. The construction of the Suffix Tree ensures that all suffixes of the text are represented in a compressed form.
3. The search operation traverses the tree based on the input pattern, making it efficient.
4. Understanding the tree structure and traversal is key to leveraging its full potential.
---
 --- 
# Longest Substring with At Most Three Distinct Characters
>Find the longest substring with at most three distinct characters using Suffix Tree.

>Input: s = "eceba"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-most-three-distinct-characters/
- Difficulty: 70
- Frequent: 65
- Tags: ['String', 'Hash Table', 'Sliding Window', 'Suffix Array']

### Longest Substring with At Most Three Distinct Characters -- Shortest Solution:
```python
class Solution:
    def lengthOfLongestSubstringTwoDistinct(self, s: str) -> int:
        if len(s) < 3:
            return len(s)
        left, right = 0, 0
        hashmap = {}
        max_length = 2
        while right < len(s):
            if len(hashmap) <= 2:
                hashmap[s[right]] = right
                right += 1
            if len(hashmap) == 3:
                del_idx = min(hashmap.values())
                del hashmap[s[del_idx]]
                left = del_idx + 1
            max_length = max(max_length, right - left)
        return max_length
```
#### TC: O(n) **SC:** O(1)

The code defines a class Solution with a method lengthOfLongestSubstringTwoDistinct that takes a
string s as input. It uses a sliding window approach to find the longest substring with at most two
distinct characters. The hashmap is used to keep track of the characters and their indices in the
current window. The left and right pointers are used to expand and contract the window. The code
efficiently handles the condition of at most two distinct characters by updating the hashmap and
adjusting the window boundaries. The time complexity of the code is O(n) as it iterates through the
string once, and the space complexity is O(1) as the hashmap stores at most 3 characters.

---
---
---
 --- 
# Longest Substring with At Most Four Distinct Characters
>Find the longest substring with at most four distinct characters using Suffix Tree.

>Input: s = "eceba"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-most-four-distinct-characters/
- Difficulty: 70
- Frequent: 65
- Tags: ['String', 'Hash Table', 'Sliding Window', 'Suffix Array']

### Longest Substring with At Most Four Distinct Characters -- Shortest Solution:
```python
from collections import defaultdict

def lengthOfLongestSubstringTwoDistinct(s: str) -> int:
    if len(s) < 3:
        return len(s)
    left, right = 0, 0
    char_map = defaultdict(int)
    max_length = 2
    while right < len(s):
        char_map[s[right]] += 1
        while len(char_map) > 2:
            char_map[s[left]] -= 1
            if char_map[s[left]] == 0:
                del char_map[s[left]]
            left += 1
        max_length = max(max_length, right - left + 1)
        right += 1
    return max_length
```
#### TC: O(n) **SC:** O(1)

The code uses a sliding window approach to find the longest substring with at most two distinct
characters. It maintains a window with at most two distinct characters and updates the maximum
length of the substring as it moves the window. The defaultdict is used to keep track of the count
of characters in the window. The code efficiently handles the sliding window and updates the maximum
length in O(n) time complexity and O(1) space complexity.

---
### Longest Substring with At Most Four Distinct Characters -- Best TC Solution:
```python
from collections import defaultdict
def lengthOfLongestSubstringFourDistinct(s: str) -> int:
    if not s:
        return 0
    left, right = 0, 0
    char_count = defaultdict(int)
    max_length = 0
    while right < len(s):
        char_count[s[right]] += 1
        while len(char_count) > 4:
            char_count[s[left]] -= 1
            if char_count[s[left]] == 0:
                del char_count[s[left]]
            left += 1
        max_length = max(max_length, right - left + 1)
        right += 1
    return max_length
```
#### TC: O(N) **SC:** O(1)

The code uses a sliding window approach to find the longest substring with at most four distinct
characters. It maintains a window defined by the left and right pointers and a dictionary to store
the count of characters within the window. The code iterates through the string, updating the
character count and adjusting the window size to ensure there are at most four distinct characters.
The maximum length of the valid substring is tracked and returned at the end. The time complexity is
O(N) where N is the length of the input string, and the space complexity is O(1) as the dictionary
size is limited to at most 4 characters.

---
---
---
 --- 
# Longest Substring with At Most Five Distinct Characters
>Find the longest substring with at most five distinct characters using Suffix Tree.

>Input: s = "eceba"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-most-five-distinct-characters/
- Difficulty: 70
- Frequent: 65
- Tags: ['String', 'Hash Table', 'Sliding Window', 'Suffix Array']

### Longest Substring with At Most Five Distinct Characters -- Shortest Solution:
```python
from collections import defaultdict
def lengthOfLongestSubstringKDistinct(s: str, k: int) -> int:
    if k == 0:
        return 0
    left, right = 0, 0
    char_count = defaultdict(int)
    max_length = 0
    while right < len(s):
        char_count[s[right]] += 1
        while len(char_count) > k:
            char_count[s[left]] -= 1
            if char_count[s[left]] == 0:
                del char_count[s[left]]
            left += 1
        max_length = max(max_length, right - left + 1)
        right += 1
    return max_length
```
#### TC: O(n) **SC:** O(k)

The code uses a sliding window approach to find the longest substring with at most k distinct
characters. It maintains a window with at most k distinct characters and updates the maximum length
of the substring as it moves the window. The defaultdict is used to keep track of the count of
characters in the window. The time complexity is O(n) where n is the length of the input string, and
the space complexity is O(k) where k is the number of distinct characters allowed in the substring.

---
### Longest Substring with At Most Five Distinct Characters -- Best TC Solution:
```python
# Function to find the longest substring with at most five distinct characters

def lengthOfLongestSubstringFiveDistinct(s: str) -> int:
    if not s:
        return 0
    left = 0
    right = 0
    char_count = {}
    max_length = 0
    while right < len(s):
        char_count[s[right]] = char_count.get(s[right], 0) + 1
        while len(char_count) > 5:
            char_count[s[left]] -= 1
            if char_count[s[left]] == 0:
                del char_count[s[left]]
            left += 1
        max_length = max(max_length, right - left + 1)
        right += 1
    return max_length

# Example
s = "eceba"
print(lengthOfLongestSubstringFiveDistinct(s))  # Output: 3
```
#### TC: O(N) **SC:** O(1)

The code uses a sliding window approach to find the longest substring with at most five distinct
characters. It maintains a window defined by the left and right pointers and a dictionary to store
the count of characters within the window. The code iterates through the string, updating the
character count and adjusting the window size to ensure there are at most five distinct characters.
The maximum length of the valid substring is tracked and returned at the end. This approach has a
time complexity of O(N) and a space complexity of O(1), making it efficient and easy to understand.

---
---
---
 --- 
# Longest Substring with At Most Six Distinct Characters
>Find the longest substring with at most six distinct characters using Suffix Tree.

>Input: s = "eceba"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-most-six-distinct-characters/
- Difficulty: 70
- Frequent: 65
- Tags: ['String', 'Hash Table', 'Sliding Window', 'Suffix Array']

### Longest Substring with At Most Six Distinct Characters -- Shortest Solution:
```python
from collections import defaultdict

def length_of_longest_substring(s: str) -> int:
    if not s:
        return 0
    left, right = 0, 0
    char_count = defaultdict(int)
    max_length = 0
    while right < len(s):
        char_count[s[right]] += 1
        while len(char_count) > 6:
            char_count[s[left]] -= 1
            if char_count[s[left]] == 0:
                del char_count[s[left]]
            left += 1
        max_length = max(max_length, right - left + 1)
        right += 1
    return max_length
```
#### TC: O(N) **SC:** O(1)

The code uses a sliding window approach to find the longest substring with at most six distinct
characters. It maintains a window defined by the left and right pointers and a dictionary to store
the count of characters within the window. The algorithm iterates through the string, updating the
window and the character count accordingly. The maximum length of the valid substring is tracked and
returned at the end. The time complexity is O(N) where N is the length of the input string, and the
space complexity is O(1) since the dictionary will have at most 6 entries.

---
---
---
 --- 
# Longest Repeated Substring
>Find the longest repeated substring in a given string using Suffix Tree.

>Input: s = "banana"
Output: "ana"
- https://leetcode.com/problems/longest-repeated-substring/
- Difficulty: 80
- Frequent: 60
- Tags: ['String', 'Binary Search', 'Suffix Array', 'Hash Function', 'Rolling Hash']

### Longest Repeated Substring -- Shortest Solution:
```python
from collections import defaultdict
def longest_repeated_substring(s: str) -> str:
    def build_suffix_tree(s):
        tree = defaultdict(dict)
        for i in range(len(s)):
            curr = tree
            for char in s[i:]:
                curr = curr.setdefault(char, {})
        return tree
    
    def dfs(node, depth):
        nonlocal max_depth, max_node
        if depth > max_depth:
            max_depth, max_node = depth, node
        for child in node:
            dfs(node[child], depth + 1)
    
    suffix_tree = build_suffix_tree(s)
    max_depth, max_node = 0, None
    dfs(suffix_tree, 0)
    
    result = []
    while max_node:
        result.append(list(max_node.keys())[0])
        max_node = max_node[list(max_node.keys())[0]]
    
    return ''.join(result)

# Example
s = "banana"
print(longest_repeated_substring(s))  # Output: "ana"
```
#### TC: O(N^2) **SC:** O(N^2)

The code defines a function to build a suffix tree from the input string and then performs a depth-
first search (DFS) on the suffix tree to find the longest repeated substring. The build_suffix_tree
function constructs the suffix tree using a defaultdict of dictionaries. The DFS function
recursively traverses the suffix tree to find the deepest node, which corresponds to the longest
repeated substring. The time complexity is O(N^2) due to the nested loops in building the suffix
tree, and the space complexity is also O(N^2) due to the storage of the suffix tree.

---
### Longest Repeated Substring -- Best TC Solution:
```python
# Function to find the longest repeated substring

def longest_repeated_substring(s: str) -> str:
    def search(sub_len: int) -> str:
        seen = set()
        for i in range(len(s) - sub_len + 1):
            sub = s[i:i + sub_len]
            if sub in seen:
                return sub
            seen.add(sub)
        return ""

    left, right = 1, len(s)
    result = ""
    while left <= right:
        mid = left + (right - left) // 2
        found = search(mid)
        if found:
            result = found
            left = mid + 1
        else:
            right = mid - 1
    return result

# Example usage
s = "banana"
print(longest_repeated_substring(s))
```
#### TC: O(n^2 * log n) **SC:** O(n)

The code uses binary search to find the longest repeated substring. It iterates through different
substring lengths and uses a set to check for duplicates. The search function checks if a substring
of a certain length is repeated within the string. The binary search approach helps in optimizing
the search process. The time complexity is O(n^2 * log n) due to the binary search, and the space
complexity is O(n) to store the substrings in the set.

---
### Longest Repeated Substring -- Best SC Solution:
```python
# Function to find the longest repeated substring

def longest_repeated_substring(s: str) -> str:
    def search(sub_len: int) -> str:
        seen = set()
        for i in range(len(s) - sub_len + 1):
            sub = s[i:i + sub_len]
            if sub in seen:
                return sub
            seen.add(sub)
        return ""

    left, right = 1, len(s)
    result = ""
    while left <= right:
        mid = left + (right - left) // 2
        found = search(mid)
        if found:
            result = found
            left = mid + 1
        else:
            right = mid - 1
    return result

# Example usage
s = "banana"
print(longest_repeated_substring(s))
```
#### TC: O(N^2 * log N) **SC:** O(N)

The code uses a binary search approach to find the longest repeated substring. It iterates through
different substring lengths and uses a set to keep track of seen substrings. The search function
checks if a substring has been seen before and returns it if found. The binary search is used to
optimize the process of finding the longest repeated substring. The time complexity is O(N^2 * log
N) due to the binary search, and the space complexity is O(N) to store the substrings in the set.

---
---
---
 --- 
# Find the Longest Substring Containing Vowels in Even Counts
>Find the longest substring containing vowels in even counts using Suffix Tree.

>Input: s = "eleetminicoworoep"
Output: 13
- https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/
- Difficulty: 75
- Frequent: 55
- Tags: ['String', 'Bit Manipulation', 'Suffix Array']

### Find the Longest Substring Containing Vowels in Even Counts -- Shortest Solution:
```python
from collections import defaultdict
def findTheLongestSubstring(s: str) -> int:
    vowels = {'a': 1, 'e': 2, 'i': 4, 'o': 8, 'u': 16}
    seen = {0: -1}
    state = 0
    res = 0
    for i, c in enumerate(s):
        if c in vowels:
            state ^= vowels[c]
        if state not in seen:
            seen[state] = i
        res = max(res, i - seen[state])
    return res
```
#### TC: O(N) **SC:** O(N)

The code uses a bitmask approach to keep track of the count of vowels encountered so far. It
utilizes a dictionary to store the state of the bitmask at each index of the string. By XORing the
current state with the bitmask of vowels, it checks if the current state has been seen before. If
not, it updates the index in the dictionary. The code efficiently finds the longest substring with
even counts of vowels by calculating the maximum length between the current index and the last
occurrence of the same state.

---
### Find the Longest Substring Containing Vowels in Even Counts -- Best TC Solution:
```python
# Function to find the longest substring containing vowels in even counts

def findTheLongestSubstring(s: str) -> int:
    vowels = {'a': 1, 'e': 2, 'i': 4, 'o': 8, 'u': 16}
    seen = {0: -1}
    state = 0
    max_len = 0
    for i, c in enumerate(s):
        if c in vowels:
            state ^= vowels[c]
        if state not in seen:
            seen[state] = i
        max_len = max(max_len, i - seen[state])
    return max_len

# Example usage
s = "eleetminicoworoep"
result = findTheLongestSubstring(s)
print(result)
```
#### TC: O(n) **SC:** O(1)

The code uses a bitmask approach to keep track of the count of vowels encountered so far. It
iterates through the input string, updating the state based on the presence of vowels. The seen
dictionary stores the state and its corresponding index. By checking if the current state has been
seen before, the code efficiently finds the longest substring with vowels in even counts. The time
complexity is O(n) as it iterates through the input string once, and the space complexity is O(1) as
the seen dictionary has a fixed size.

---
---
---
 --- 
# Distinct Substrings
>Count the number of distinct substrings in a given string using Suffix Tree.

>Input: s = "abc"
Output: 6
- https://leetcode.com/problems/distinct-substrings/
- Difficulty: 70
- Frequent: 50
- Tags: ['String', 'Suffix Array']

### Distinct Substrings -- Shortest Solution:
```python
from collections import defaultdict
class SuffixTree:
    def __init__(self, s):
        self.s = s
        self.nodes = [{}, -1]
        self.build()

    def build(self):
        s = self.s
        nodes = self.nodes
        for i in range(len(s)):
            cur = 0
            for j in range(i, len(s)):
                if s[j] not in nodes[cur][0]:
                    nodes.append({})
                    nodes[cur][s[j]] = len(nodes) - 1
                cur = nodes[cur][s[j]]
        return nodes

    def count_distinct_substrings(self):
        return len(self.build()) - 1


def count_distinct_substrings(s: str) -> int:
    st = SuffixTree(s)
    return st.count_distinct_substrings()

# Example
s = "ababa"
print(count_distinct_substrings(s))  # Output: 10
```
#### TC: O(N^2) **SC:** O(N^2)

The code defines a SuffixTree class to build a suffix tree from the input string. The
count_distinct_substrings method calculates the number of distinct substrings using the built suffix
tree. The time complexity is O(N^2) due to the nested loops in building the suffix tree, and the
space complexity is also O(N^2) to store the nodes of the suffix tree.

---
---
---
 --- 
# Count of Distinct Substrings
>Count the number of distinct substrings in a given string using Suffix Tree.

>Input: s = "ababa"
Output: 10
- https://leetcode.com/problems/count-of-distinct-substrings/
- Difficulty: 70
- Frequent: 50
- Tags: ['String', 'Suffix Array']

### Count of Distinct Substrings -- Shortest Solution:
```python
from collections import defaultdict
class SuffixTree:
    def __init__(self, s):
        self.s = s
        self.root = {}
        self.build_suffix_tree()
    
    def build_suffix_tree(self):
        for i in range(len(self.s)):
            cur = self.root
            for c in self.s[i:]:
                if c not in cur:
                    cur[c] = {}
                cur = cur[c]
    
    def count_distinct_substrings(self):
        count = 0
        stack = [self.root]
        while stack:
            node = stack.pop()
            count += 1
            for child in node.values():
                stack.append(child)
        return count - 1


def count_distinct_substrings(s: str) -> int:
    suffix_tree = SuffixTree(s)
    return suffix_tree.count_distinct_substrings()
```
#### TC: O(n^2) **SC:** O(n^2)

The code defines a SuffixTree class to build a suffix tree from the input string. The
count_distinct_substrings method calculates the count of distinct substrings using the suffix tree.
The time complexity is O(n^2) due to the nested loops in building the suffix tree, and the space
complexity is also O(n^2) due to storing the suffix tree structure.

---
---
---
 --- 
# Number of Distinct Substrings in a String
>Count the number of distinct substrings in a given string using Suffix Tree.

>Input: s = "abc"
Output: 6
- https://leetcode.com/problems/number-of-distinct-substrings-in-a-string/
- Difficulty: 70
- Frequent: 50
- Tags: ['String', 'Suffix Array']

### Number of Distinct Substrings in a String -- Shortest Solution:
```python
from collections import defaultdict
def count_distinct_substrings(s: str) -> int:
    substrings = set()
    for i in range(len(s)):
        for j in range(i + 1, len(s) + 1):
            substrings.add(s[i:j])
    return len(substrings)

# Example
s = "abcabc"
print(count_distinct_substrings(s))  # Output: 15
```
#### TC: O(n^3) **SC:** O(n^2)

The code uses a simple approach to count the number of distinct substrings in a given string. It
iterates through all possible substrings of the input string and adds them to a set to ensure
uniqueness. The time complexity is O(n^3) due to the nested loops, where n is the length of the
input string. The space complexity is O(n^2) because of the set storing all the substrings. This
solution is straightforward and easy to understand, but it may not be the most efficient for large
inputs.

---
---
---
 --- 
# Lexicographical Order
>Return the lexicographical order of numbers from 1 to n using Suffix Tree.

>Input: n = 13
Output: [1,10,11,12,13,2,3,4,5,6,7,8,9]
- https://leetcode.com/problems/lexicographical-order/
- Difficulty: 65
- Frequent: 45
- Tags: ['String', 'Trie', 'Suffix Array']

### Lexicographical Order -- Shortest Solution:
```python
from typing import List

class Solution:
    def lexicalOrder(self, n: int) -> List[int]:
        res = []
        curr = 1
        for _ in range(n):
            res.append(curr)
            if curr * 10 <= n:
                curr *= 10
            elif curr % 10 != 9 and curr + 1 <= n:
                curr += 1
            else:
                while (curr // 10) % 10 == 9:
                    curr //= 10
                curr = curr // 10 + 1
        return res
```
#### TC: O(n) **SC:** O(n)

The code implements a lexicographical order of integers from 1 to n. It iterates through the numbers
in lexicographical order by appending them to the result list. The algorithm efficiently determines
the next number to consider based on the current number. It avoids unnecessary calculations and
ensures the numbers are in lexicographical order. The time complexity is O(n) as it iterates through
n numbers, and the space complexity is O(n) to store the result list.

---
---
---