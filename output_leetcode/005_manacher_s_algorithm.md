# Manacher's Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Palindrome Pairs](#palindrome-pairs) | 70 | 60 |
---
Manacher's algorithm finds the longest palindromic substring in linear time. It transforms the input string to avoid even/odd length complications, then uses a center-expansion technique to find palindromes efficiently. The algorithm maintains an array to store the radius of the palindrome centered at each position, allowing it to skip unnecessary comparisons.
```python
def manacher(s):
    s = '#' + '#'.join(s) + '#'
    n = len(s)
    p = [0] * n
    c = r = 0
    for i in range(n):
        mirr = 2 * c - i
        if i < r:
            p[i] = min(r - i, p[mirr])
        while i + p[i] + 1 < n and i - p[i] - 1 >= 0 and s[i + p[i] + 1] == s[i - p[i] - 1]:
            p[i] += 1
        if i + p[i] > r:
            c, r = i, i + p[i]
    max_len, center_index = max((n, i) for i, n in enumerate(p))
    return s[center_index - max_len:center_index + max_len].replace('#', '')

# Example usage
print(manacher("babad"))  # Output: "bab" or "aba"
print(manacher("cbbd"))   # Output: "bb"
```
### Insight:
1. The transformation of the string with '#' helps in handling even-length palindromes uniformly.
2. The array `p` stores the radius of the palindrome centered at each position, allowing efficient expansion checks.
3. The variables `c` and `r` keep track of the center and right edge of the current rightmost palindrome, optimizing the process by skipping unnecessary comparisons.
4. The final palindrome is extracted by finding the maximum value in the `p` array and mapping it back to the original string.
---
 --- 
# Palindrome Pairs
>Given a list of unique words, find all pairs of distinct indices (i, j) in the given list, so that the concatenation of the two words, i.e., words[i] + words[j] is a palindrome.

>Input: words = ["abcd","dcba","lls","s","sssll"]
Output: [[0,1],[1,0],[3,2],[2,4]]
- https://leetcode.com/problems/palindrome-pairs/
- Difficulty: 60
- Frequent: 70
- Tags: ['String', 'Trie', 'Hash Table']

### Palindrome Pairs -- Shortest Solution:
```python
# Function to check if a word is a palindrome
    def is_palindrome(word):
        return word == word[::-1]

    def palindrome_pairs(words):
        word_dict = {word: i for i, word in enumerate(words)}
        result = []
        for i, word in enumerate(words):
            for j in range(len(word) + 1):
                prefix = word[:j]
                suffix = word[j:]
                if is_palindrome(prefix) and suffix[::-1] != word and suffix[::-1] in word_dict:
                    result.append([word_dict[suffix[::-1]], word_dict[word]])
                if j != len(word) and is_palindrome(suffix) and prefix[::-1] != word and prefix[::-1] in word_dict:
                    result.append([word_dict[word], word_dict[prefix[::-1]]])
        return result
```
#### TC: O(n*k^2) **SC:** O(n)

The code defines a function to check if a word is a palindrome and then iterates through the list of
words to find palindrome pairs. It uses a dictionary to store the index of each word for quick
lookup. The code checks all possible combinations of prefixes and suffixes for each word to
determine if they form a palindrome pair. The time complexity is O(n*k^2) where n is the number of
words and k is the average length of a word. The space complexity is O(n) due to the dictionary
storing the words and their indices.

---
---
---