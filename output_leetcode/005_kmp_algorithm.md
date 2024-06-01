# KMP Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Find the Index of the First Occurrence in a String](#find-the-index-of-the-first-occurrence-in-a-string) | 70 | 35 |
| 2 | [KMP Algorithm](#kmp-algorithm) | 65 | 50 |
| 3 | [Repeated DNA Sequences](#repeated-dna-sequences) | 55 | 40 |
| 4 | [Longest Prefix Suffix](#longest-prefix-suffix) | 50 | 45 |
| 5 | [Shortest Palindrome](#shortest-palindrome) | 50 | 50 |
| 6 | [Rotate String](#rotate-string) | 45 | 30 |
---
The Knuth-Morris-Pratt (KMP) algorithm is a string-searching algorithm that searches for occurrences of a "pattern" within a "text" by employing the observation that when a mismatch occurs, the pattern itself embodies sufficient information to determine where the next match could begin, thus bypassing re-examination of previously matched characters.
```python
def kmp_search(pattern, text):
    def compute_lps(pattern):
        lps = [0] * len(pattern)
        length = 0
        i = 1
        while i < len(pattern):
            if pattern[i] == pattern[length]:
                length += 1
                lps[i] = length
                i += 1
            else:
                if length != 0:
                    length = lps[length - 1]
                else:
                    lps[i] = 0
                    i += 1
        return lps

    lps = compute_lps(pattern)
    i = 0
    j = 0
    while i < len(text):
        if pattern[j] == text[i]:
            i += 1
            j += 1
        if j == len(pattern):
            print(f"Found pattern at index {i - j}")
            j = lps[j - 1]
        elif i < len(text) and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1

# Example usage
kmp_search("ABABCABAB", "ABABDABACDABABCABAB")
```
### Insight:
1. The KMP algorithm preprocesses the pattern to create an array (LPS array) that indicates the longest proper prefix which is also a suffix.
2. This preprocessing helps in avoiding redundant comparisons in the text.
3. The main advantage of KMP over naive string matching algorithms is its ability to skip sections of the text, leading to a linear time complexity.
4. The LPS array is crucial for the efficiency of the algorithm, as it allows the search to continue from the longest prefix which is also a suffix, rather than starting over from the beginning.
---
 --- 
# Find the Index of the First Occurrence in a String
>Find the index of the first occurrence of a substring in a string using KMP algorithm.

>Input: haystack = "sadbutsad", needle = "sad"
Output: 0
- https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/
- Difficulty: 35
- Frequent: 70
- Tags: ['String', 'KMP']

### Find the Index of the First Occurrence in a String -- Shortest Solution:
```python
# Function to find the index of the first occurrence of a substring in a string

def first_occurrence(s: str, x: str) -> int:
    if not x:
        return 0
    if not s:
        return -1
    n, m = len(s), len(x)
    if m > n:
        return -1
    for i in range(n - m + 1):
        if s[i:i + m] == x:
            return i
    return -1


# Example
s = "hello world"
x = "lo"
print(first_occurrence(s, x))  # Output: 3
```
#### TC: O(n*m) **SC:** O(1)

The code defines a function that finds the index of the first occurrence of a substring in a string.
It iterates through the string and checks if the substring matches at each position. The time
complexity is O(n*m) where n is the length of the string and m is the length of the substring. The
space complexity is O(1) as no extra space is used.

---
---
---
 --- 
# KMP Algorithm
>Implement the KMP algorithm to find the first occurrence of a pattern in a text.

>Input: text = "abxabcabcaby", pattern = "abcaby"
Output: 6
- https://leetcode.com/problems/kmp-algorithm/
- Difficulty: 50
- Frequent: 65
- Tags: ['String', 'KMP']

### KMP Algorithm -- Shortest Solution:
```python
# KMP Algorithm

def kmp_search(pattern, text):
    if not pattern:
        return 0
    if not text:
        return -1
    lps = [0] * len(pattern)
    j = 0
    i = 1
    while i < len(pattern):
        if pattern[i] == pattern[j]:
            j += 1
            lps[i] = j
            i += 1
        else:
            if j != 0:
                j = lps[j - 1]
            else:
                lps[i] = 0
                i += 1
    i = 0
    j = 0
    while i < len(text):
        if pattern[j] == text[i]:
            i += 1
            j += 1
        if j == len(pattern):
            return i - j
        elif i < len(text) and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    return -1

# Example
pattern = "abc"
text = "abxabcabcaby"
result = kmp_search(pattern, text)
print(result)
```
#### TC: O(N + M) **SC:** O(M)

The code implements the Knuth-Morris-Pratt (KMP) algorithm for pattern searching in a text. It uses
the concept of the Longest Prefix which is also a Suffix (LPS) to efficiently search for the
pattern. The time complexity of the algorithm is O(N + M) where N is the length of the text and M is
the length of the pattern. The space complexity is O(M) where M is the length of the pattern. The
code is structured and easy to understand, making it a clean and efficient solution for pattern
searching.

---
---
---
 --- 
# Repeated DNA Sequences
>Find all the 10-letter-long sequences that occur more than once in a DNA molecule using KMP algorithm.

>Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
Output: ["AAAAACCCCC","CCCCCAAAAA"]
- https://leetcode.com/problems/repeated-dna-sequences/
- Difficulty: 40
- Frequent: 55
- Tags: ['String', 'KMP']

### Repeated DNA Sequences -- Shortest Solution:
```python
from collections import defaultdict
def findRepeatedDnaSequences(s):
    seen = defaultdict(int)
    res = set()
    for i in range(len(s) - 9):
        substring = s[i:i+10]
        if seen[substring] == 1:
            res.add(substring)
        seen[substring] += 1
    return list(res)
```
#### TC: O(N) **SC:** O(N)

The code uses a defaultdict to keep track of the frequency of each 10-character substring in the
input string. It iterates through the input string and checks if the current substring has been seen
before. If it has been seen exactly once, it adds it to the result set. Finally, it returns the list
of repeated DNA sequences. The time complexity is O(N) where N is the length of the input string,
and the space complexity is also O(N) to store the seen substrings.

---
---
---
 --- 
# Longest Prefix Suffix
>Find the length of the longest prefix which is also a suffix using KMP algorithm.

>Input: s = "ababab"
Output: 4
- https://leetcode.com/problems/longest-prefix-suffix/
- Difficulty: 45
- Frequent: 50
- Tags: ['String', 'KMP']

### Longest Prefix Suffix -- Shortest Solution:
```python
# KMP Algorithm Approach

def longest_prefix_suffix(s: str) -> int:
    n = len(s)
    lps = [0] * n
    j = 0
    i = 1
    while i < n:
        if s[i] == s[j]:
            j += 1
            lps[i] = j
            i += 1
        else:
            if j != 0:
                j = lps[j - 1]
            else:
                lps[i] = 0
                i += 1
    return lps[-1]

# Example
s = "abab"
print(longest_prefix_suffix(s))  # Output: 1
```
#### TC: O(n) **SC:** O(n)

The code implements the KMP algorithm to find the length of the longest prefix that is also a suffix
in a given string. It iterates through the string to build the lps array, which stores the length of
the longest proper prefix which is also a suffix for each index. The time complexity of the code is
O(n) as it only iterates through the string once. The space complexity is O(n) due to the lps array
storing values for each index of the input string.

---
---
---
 --- 
# Shortest Palindrome
>Find the shortest palindrome by adding characters in front of it using KMP algorithm.

>Input: s = "aacecaaa"
Output: "aaacecaaa"
- https://leetcode.com/problems/shortest-palindrome/
- Difficulty: 50
- Frequent: 50
- Tags: ['String', 'KMP']

### Shortest Palindrome -- Shortest Solution:
```python
from typing import List

class Solution:
    def shortestPalindrome(self, s: str) -> str:
        rev_s = s[::-1]
        new_s = s + '#' + rev_s
        lps = [0] * len(new_s)
        j = 0
        for i in range(1, len(new_s)):
            while j > 0 and new_s[i] != new_s[j]:
                j = lps[j - 1]
            if new_s[i] == new_s[j]:
                j += 1
            lps[i] = j
        return rev_s[:len(s) - lps[-1]] + s
```
#### TC: O(n) **SC:** O(n)

The code uses the KMP algorithm to find the longest prefix that is also a suffix of the
concatenation of the input string and its reverse. This helps in determining the shortest palindrome
by adding the missing characters from the reverse string to the original string. The time complexity
of the solution is O(n) where n is the length of the input string, and the space complexity is O(n)
to store the LPS array.

---
---
---
 --- 
# Rotate String
>Check if one string is a rotation of another using KMP algorithm.

>Input: s = "abcde", goal = "cdeab"
Output: true
- https://leetcode.com/problems/rotate-string/
- Difficulty: 30
- Frequent: 45
- Tags: ['String', 'KMP']

### Rotate String -- Shortest Solution:
```python
from typing import List

def rotateString(A: str, B: str) -> bool:
    return len(A) == len(B) and B in A + A
```
#### TC: O(N) **SC:** O(N)

The code checks if the lengths of strings A and B are equal. If they are, it concatenates A with
itself and checks if B is a substring of the concatenated string. This approach leverages the
property of rotation where rotating a string is equivalent to checking if one string is a substring
of another string concatenated with itself. The time complexity is O(N) where N is the length of the
input string, and the space complexity is O(N) to store the concatenated string.

---
---
---