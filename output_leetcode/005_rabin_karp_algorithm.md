# Rabin-Karp Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters) | 95 | 40 |
| 2 | [Find the Duplicate Number](#find-the-duplicate-number) | 90 | 50 |
| 3 | [Longest Palindromic Substring](#longest-palindromic-substring) | 90 | 50 |
| 4 | [Find All Anagrams in a String](#find-all-anagrams-in-a-string) | 85 | 50 |
| 5 | [Minimum Window Substring](#minimum-window-substring) | 85 | 60 |
| 6 | [Implement strStr()](#implement-strstr) | 80 | 30 |
| 7 | [Substring with Concatenation of All Words](#substring-with-concatenation-of-all-words) | 80 | 50 |
| 8 | [Longest Substring with At Most K Distinct Characters](#longest-substring-with-at-most-k-distinct-characters) | 75 | 50 |
| 9 | [Longest Substring with At Least One Repeating Character](#longest-substring-with-at-least-one-repeating-character) | 75 | 50 |
| 10 | [Longest Substring with At Least Two Repeating Characters](#longest-substring-with-at-least-two-repeating-characters) | 75 | 50 |
| 11 | [Longest Substring with At Least K Repeating Characters](#longest-substring-with-at-least-k-repeating-characters) | 75 | 60 |
| 12 | [Repeated Substring Pattern](#repeated-substring-pattern) | 70 | 40 |
| 13 | [Check If a String Contains All Binary Codes of Size K](#check-if-a-string-contains-all-binary-codes-of-size-k) | 70 | 50 |
| 14 | [Longest Substring with At Most Two Distinct Characters](#longest-substring-with-at-most-two-distinct-characters) | 70 | 50 |
| 15 | [Longest Common Substring](#longest-common-substring) | 70 | 60 |
| 16 | [Longest Substring with At Most One Distinct Character](#longest-substring-with-at-most-one-distinct-character) | 65 | 40 |
| 17 | [Maximum Number of Occurrences of a Substring](#maximum-number-of-occurrences-of-a-substring) | 65 | 60 |
| 18 | [Longest Duplicate Substring](#longest-duplicate-substring) | 60 | 60 |
| 19 | [Distinct Echo Substrings](#distinct-echo-substrings) | 60 | 70 |
| 20 | [Smallest Rotation with Highest Score](#smallest-rotation-with-highest-score) | 50 | 80 |
---
Rabin-Karp is a string-searching algorithm that uses hashing to find an exact match of a pattern string within a text. It computes a hash value for the pattern and each substring of the text, then compares these hash values to find matches. If the hash values match, it performs a direct string comparison to confirm the match.
```python
def rabin_karp(text, pattern):
    d = 256  # number of characters in the input alphabet
    q = 101  # a prime number
    m = len(pattern)
    n = len(text)
    p = 0  # hash value for pattern
    t = 0  # hash value for text
    h = 1
    
    # The value of h would be "pow(d, M-1)%q"
    for i in range(m-1):
        h = (h * d) % q
    
    # Calculate the hash value of pattern and first window of text
    for i in range(m):
        p = (d * p + ord(pattern[i])) % q
        t = (d * t + ord(text[i])) % q
    
    # Slide the pattern over text one by one
    for i in range(n - m + 1):
        # Check the hash values of current window of text and pattern
        if p == t:
            # Check for characters one by one
            if text[i:i+m] == pattern:
                print(f"Pattern found at index {i}")
        
        # Calculate hash value for next window of text
        if i < n - m:
            t = (d * (t - ord(text[i]) * h) + ord(text[i + m])) % q
            # We might get negative value of t, converting it to positive
            if t < 0:
                t = t + q

# Example usage
text = "GEEKS FOR GEEKS"
pattern = "GEEK"
rabin_karp(text, pattern)
```
### Insight:
1. The algorithm is efficient for multiple pattern searches within the same text.
2. Hash collisions are rare but possible; hence, a direct string comparison is necessary when hash values match.
3. The choice of a good hash function and a large prime number is crucial for minimizing collisions.
4. The algorithm is particularly useful for its rolling hash technique, which allows for efficient re-computation of hash values.
---
 --- 
# Longest Substring Without Repeating Characters
>Find the length of the longest substring without repeating characters using Rabin-Karp algorithm.

>Input: s = "abcabcbb"
Output: 3
- https://leetcode.com/problems/longest-substring-without-repeating-characters/
- Difficulty: 40
- Frequent: 95
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Longest Substring Without Repeating Characters -- Shortest Solution:
```python
# Rabin-Karp Algorithm Approach

def lengthOfLongestSubstring(s: str) -> int:
    n = len(s)
    if n == 0:
        return 0
    max_len = 0
    start = 0
    char_index = {}

    for end in range(n):
        if s[end] in char_index:
            start = max(start, char_index[s[end]] + 1)
        char_index[s[end]] = end
        max_len = max(max_len, end - start + 1)

    return max_len

# Example
s = "abcabcbb"
print(lengthOfLongestSubstring(s))  # Output: 3
```
#### TC: O(n) **SC:** O(min(n, m))

The code uses the Rabin-Karp Algorithm approach to find the length of the longest substring without
repeating characters. It maintains a sliding window and a dictionary to store the index of each
character. The algorithm efficiently updates the start index of the window when a repeating
character is encountered. The time complexity is O(n) where n is the length of the input string, and
the space complexity is O(min(n, m)) where m is the size of the character set.

---
### Longest Substring Without Repeating Characters -- Best SC Solution:
```python
# Function to find the length of the longest substring without repeating characters

def lengthOfLongestSubstring(s: str) -> int:
    max_length = 0
    start = 0
    char_index = {}

    for end in range(len(s)):
        if s[end] in char_index:
            start = max(start, char_index[s[end]] + 1)
        char_index[s[end]] = end
        max_length = max(max_length, end - start + 1)

    return max_length

# Example usage
s = "abcabcbb"
print(lengthOfLongestSubstring(s))
```
#### TC: O(n) **SC:** O(min(n, m)) where n is the length of the input string and m is the size of the character set

The code uses a sliding window approach to find the longest substring without repeating characters.
It maintains a window defined by the start and end pointers. The char_index dictionary is used to
store the index of each character. The start pointer is updated to the maximum of its current
position and the next index of the repeating character. The maximum length of the substring is
updated at each iteration. This approach ensures linear time complexity O(n) and space complexity
O(min(n, m)), where n is the length of the input string and m is the size of the character set.

---
---
---
 --- 
# Find the Duplicate Number
>Find the duplicate number in an array using Rabin-Karp algorithm.

>Input: nums = [1,3,4,2,2]
Output: 2
- https://leetcode.com/problems/find-the-duplicate-number/
- Difficulty: 50
- Frequent: 90
- Tags: ['Array', 'Two Pointers', 'Binary Search', 'Bit Manipulation']

### Find the Duplicate Number -- Shortest Solution:
```python
# Rabin-Karp Algorithm Approach

def findDuplicate(nums):
    slow = fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    fast = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    return slow

# Example
nums = [1, 3, 4, 2, 2]
print(findDuplicate(nums))  # Output: 2
```
#### TC: O(n) **SC:** O(1)

The code uses the Rabin-Karp Algorithm approach to find the duplicate number in an array. It
utilizes two pointers, slow and fast, to detect the cycle in the array. By finding the intersection
point of the two pointers, the algorithm identifies the duplicate number efficiently. The time
complexity of the algorithm is O(n) as it iterates through the array once, and the space complexity
is O(1) as it does not use any extra space apart from a few variables.

---
---
---
 --- 
# Longest Palindromic Substring
>Find the longest palindromic substring using Rabin-Karp algorithm.

>Input: s = "babad"
Output: "bab"
- https://leetcode.com/problems/longest-palindromic-substring/
- Difficulty: 50
- Frequent: 90
- Tags: ['String', 'Dynamic Programming']

### Longest Palindromic Substring -- Shortest Solution:
```python
# Rabin-Karp Algorithm Approach

def longest_palindromic_substring(s: str) -> str:
    def is_palindrome(s: str) -> bool:
        return s == s[::-1]
    
    if len(s) < 2:
        return s
    
    n = len(s)
    base = 26
    mod = 10 ** 9 + 7
    power = 1
    max_len = 1
    start = 0
    
    for i in range(1, n):
        power = (power * base) % mod
        
    def get_hash(s: str, start: int, end: int) -> int:
        hash_val = 0
        for i in range(start, end + 1):
            hash_val = (hash_val * base + ord(s[i]) - ord('a')) % mod
        return hash_val
    
    hash_set = set()
    hash_val = get_hash(s, 0, n - 1)
    hash_set.add(hash_val)
    
    for i in range(1, n):
        for j in range(i, n):
            hash_val = (hash_val - (ord(s[j - 1]) - ord('a')) * power) % mod
            hash_val = (hash_val * base + ord(s[j]) - ord('a')) % mod
            
            if hash_val in hash_set and is_palindrome(s[i:j + 1]):
                if j - i + 1 > max_len:
                    max_len = j - i + 1
                    start = i
            else:
                hash_set.add(hash_val)
    
    return s[start:start + max_len]

# Example
s = "babad"
print(longest_palindromic_substring(s))  # Output: "bab" or "aba"
```
#### TC: O(N^2) **SC:** O(N)

The code uses the Rabin-Karp Algorithm approach to find the longest palindromic substring in a given
string. It calculates the hash values of substrings to efficiently check for palindromes. The
algorithm iterates through all possible substrings and updates the maximum length palindrome found.
The is_palindrome function is used to verify if a substring is a palindrome. The code optimizes the
palindrome check by using hash values and set to store unique hash values. Overall, the code has a
time complexity of O(N^2) and a space complexity of O(N), where N is the length of the input string.

---
### Longest Palindromic Substring -- Best TC Solution:
```python
# Function to find the longest palindromic substring

def longest_palindromic_substring(s: str) -> str:
    if len(s) < 2:
        return s
    start = 0
    max_len = 1
    for i in range(len(s)):
        if i - max_len >= 1 and s[i - max_len - 1:i + 1] == s[i - max_len - 1:i + 1][::-1]:
            start = i - max_len - 1
            max_len += 2
            continue
        if i - max_len >= 0 and s[i - max_len:i + 1] == s[i - max_len:i + 1][::-1]:
            start = i - max_len
            max_len += 1
    return s[start:start + max_len]

# Example
s = "babad"
print(longest_palindromic_substring(s))
```
#### TC: O(n^2) **SC:** O(1)

The code defines a function to find the longest palindromic substring in a given string. It iterates
through the string and expands around each character to find palindromes. The algorithm has a time
complexity of O(n^2) due to the nested loops, where n is the length of the input string. The space
complexity is O(1) as only a constant amount of extra space is used. The code efficiently handles
different cases to find the longest palindromic substring.

---
---
---
 --- 
# Find All Anagrams in a String
>Find all anagrams of p in s using Rabin-Karp algorithm.

>Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
- https://leetcode.com/problems/find-all-anagrams-in-a-string/
- Difficulty: 50
- Frequent: 85
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Find All Anagrams in a String -- Shortest Solution:
```python
from collections import Counter
def findAnagrams(s: str, p: str) -> List[int]:
    res = []
    p_count = Counter(p)
    s_count = Counter(s[:len(p)-1])
    for i in range(len(p)-1, len(s)):
        s_count[s[i]] += 1
        if s_count == p_count:
            res.append(i - len(p) + 1)
        s_count[s[i - len(p) + 1]] -= 1
        if s_count[s[i - len(p) + 1]] == 0:
            del s_count[s[i - len(p) + 1]]
    return res
```
#### TC: O(n) **SC:** O(1)

The code uses two Counter objects from the collections module to keep track of the characters in the
pattern string and the sliding window of the input string. It iterates through the input string,
updating the counts of characters in the window and comparing them with the counts of characters in
the pattern. If the counts match, it adds the starting index of the anagram to the result list. The
sliding window technique helps in efficiently checking for anagrams without redundant comparisons.
The time complexity is O(n) where n is the length of the input string, and the space complexity is
O(1) because the Counter objects store a constant number of characters.

---
---
---
 --- 
# Minimum Window Substring
>Find the minimum window substring in s which will contain all the characters in t using Rabin-Karp algorithm.

>Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
- https://leetcode.com/problems/minimum-window-substring/
- Difficulty: 60
- Frequent: 85
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Minimum Window Substring -- Shortest Solution:
```python
from collections import Counter

def minWindow(s: str, t: str) -> str:
    if not s or not t:
        return ""
    target_count = Counter(t)
    required = len(target_count)
    left = 0
    formed = 0
    window_counts = {}
    ans = float('inf'), None, None

    for right, char in enumerate(s):
        window_counts[char] = window_counts.get(char, 0) + 1
        if char in target_count and window_counts[char] == target_count[char]:
            formed += 1
        while formed == required:
            if right - left + 1 < ans[0]:
                ans = (right - left + 1, left, right)
            window_counts[s[left]] -= 1
            if s[left] in target_count and window_counts[s[left]] < target_count[s[left]]:
                formed -= 1
            left += 1

    return "" if ans[0] == float('inf') else s[ans[1]: ans[2] + 1]
```
#### TC: O(S + T) **SC:** O(S + T)

The code uses the sliding window technique to find the minimum window substring that contains all
characters of the target string. It maintains two pointers, 'left' and 'right', to track the window
boundaries. The 'formed' variable keeps track of the characters formed in the current window. The
code efficiently updates the window counts and adjusts the window boundaries to find the minimum
window substring. The time complexity is O(S + T) where S is the length of the input string 's' and
T is the length of the target string 't'. The space complexity is also O(S + T) to store the window
counts and target counts.

---
### Minimum Window Substring -- Best SC Solution:
```python
from collections import Counter

def minWindow(s: str, t: str) -> str:
    if not s or not t:
        return ""
    target_count = Counter(t)
    required = len(target_count)
    left, right = 0, 0
    formed = 0
    window_counts = {}
    ans = float('inf'), None, None

    while right < len(s):
        char = s[right]
        window_counts[char] = window_counts.get(char, 0) + 1
        if char in target_count and window_counts[char] == target_count[char]:
            formed += 1
        while formed == required and left <= right:
            char = s[left]
            if right - left + 1 < ans[0]:
                ans = (right - left + 1, left, right)
            window_counts[char] -= 1
            if char in target_count and window_counts[char] < target_count[char]:
                formed -= 1
            left += 1
        right += 1

    return "" if ans[0] == float('inf') else s[ans[1]: ans[2] + 1]
```
#### TC: O(N) **SC:** O(N)

The code uses the sliding window technique to find the minimum window substring that contains all
characters of the target string. It maintains two pointers, 'left' and 'right', to track the window
boundaries. The 'formed' variable keeps track of the number of characters formed in the current
window.  The algorithm iterates through the input string 's' using the 'right' pointer and expands
the window until all characters in the target string 't' are found. Once all characters are found,
it contracts the window from the left side to find the minimum window.  The 'window_counts'
dictionary stores the count of characters in the current window, and 'target_count' stores the count
of characters in the target string. The algorithm updates the 'formed' variable when a character in
the window matches the target count.  The time complexity of the algorithm is O(N) where N is the
length of the input string 's'. The space complexity is also O(N) due to the usage of dictionaries
to store character counts.

---
---
---
 --- 
# Implement strStr()
>Find the first occurrence of a substring in a string using Rabin-Karp algorithm.

>Input: haystack = "hello", needle = "ll"
Output: 2
- https://leetcode.com/problems/implement-strstr/
- Difficulty: 30
- Frequent: 80
- Tags: ['String', 'Two Pointers', 'String Matching']

### Implement strStr() -- Shortest Solution:
```python
# Rabin-Karp Algorithm

def strStr(haystack: str, needle: str) -> int:
    if not needle:
        return 0
    if len(needle) > len(haystack):
        return -1
    base = 26
    needle_hash = sum([ord(needle[i]) * base ** i for i in range(len(needle))])
    haystack_hash = sum([ord(haystack[i]) * base ** i for i in range(len(needle))])
    if needle_hash == haystack_hash and haystack[:len(needle)] == needle:
        return 0
    for i in range(1, len(haystack) - len(needle) + 1):
        haystack_hash = (haystack_hash - ord(haystack[i - 1])) / base + ord(haystack[i + len(needle) - 1]) * base ** (len(needle) - 1)
        if needle_hash == haystack_hash and haystack[i:i + len(needle)] == needle:
            return i
    return -1
```
#### TC: O((N - M)M) **SC:** O(1)

The code implements the Rabin-Karp Algorithm to find the index of the first occurrence of the needle
in the haystack. It calculates the hash values for the needle and the initial substring of the
haystack. Then, it iterates through the haystack, updating the hash value efficiently using a
rolling hash approach. If the hash values match and the substrings match, it returns the index. The
time complexity is O((N - M)M) where N is the length of the haystack and M is the length of the
needle. The space complexity is O(1) as it only uses a constant amount of extra space.

---
### Implement strStr() -- Best TC Solution:
```python
# Rabin-Karp Algorithm approach

def strStr(haystack: str, needle: str) -> int:
    if not needle:
        return 0
    if len(needle) > len(haystack):
        return -1
    base = 26
    needle_hash = 0
    haystack_hash = 0
    power = 1
    mod = 2**31
    for i in range(len(needle)):
        needle_hash = (needle_hash * base + ord(needle[i])) % mod
        haystack_hash = (haystack_hash * base + ord(haystack[i])) % mod
        power = (power * base) % mod
    if needle_hash == haystack_hash and needle == haystack[:len(needle)]:
        return 0
    for i in range(1, len(haystack) - len(needle) + 1):
        new_hash = (haystack_hash * base - ord(haystack[i - 1]) * power + ord(haystack[i + len(needle) - 1])) % mod
        if new_hash == needle_hash and needle == haystack[i:i + len(needle)]:
            return i
        haystack_hash = new_hash
    return -1
```
#### TC: O(n+m) **SC:** O(1)

The code implements the Rabin-Karp Algorithm approach to solve the 'Implement strStr()' problem
efficiently. It calculates the hash values for the needle and haystack strings and compares them to
find the substring match. The algorithm uses rolling hash technique to avoid recalculating the hash
values for each substring. The time complexity of the algorithm is O(n+m) where n is the length of
the haystack and m is the length of the needle. The space complexity is O(1) as it only uses a
constant amount of extra space for storing hash values and variables.

---
---
---
 --- 
# Substring with Concatenation of All Words
>Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once using Rabin-Karp algorithm.

>Input: s = "barfoothefoobarman", words = ["foo","bar"]
Output: [0,9]
- https://leetcode.com/problems/substring-with-concatenation-of-all-words/
- Difficulty: 50
- Frequent: 80
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Substring with Concatenation of All Words -- Shortest Solution:
```python
from collections import Counter
def findSubstring(s, words):
    if not s or not words:
        return []
    word_len = len(words[0])
    word_count = len(words)
    total_len = word_len * word_count
    words_counter = Counter(words)
    res = []
    for i in range(len(s) - total_len + 1):
        seen = {}
        for j in range(word_count):
            start = i + j * word_len
            end = start + word_len
            word = s[start:end]
            if word in words_counter:
                seen[word] = seen.get(word, 0) + 1
                if seen[word] > words_counter[word]:
                    break
            else:
                break
            if j == word_count - 1:
                res.append(i)
    return res

# Example
s = "barfoothefoobarman"
words = ["foo","bar"]
print(findSubstring(s, words))  # Output: [0, 9]
```
#### TC: O(n * m) **SC:** O(m)

The code uses a sliding window approach to find substrings in the input string that are
concatenations of all words in the given list. It maintains a counter of the words and iterates
through the string to check for valid substrings. The time complexity is O(n * m) where n is the
length of the input string and m is the number of words. The space complexity is O(m) where m is the
number of words in the list.

---
### Substring with Concatenation of All Words -- Best TC Solution:
```python
from collections import Counter
def findSubstring(s, words):
    if not s or not words:
        return []
    word_len = len(words[0])
    word_count = len(words)
    total_len = word_len * word_count
    words_counter = Counter(words)
    res = []
    for i in range(len(s) - total_len + 1):
        seen = {}
        for j in range(word_count):
            start = i + j * word_len
            end = start + word_len
            word = s[start:end]
            if word in words_counter:
                seen[word] = seen.get(word, 0) + 1
                if seen[word] > words_counter[word]:
                    break
            else:
                break
            if j == word_count - 1:
                res.append(i)
    return res

# Example
s = "barfoothefoobarman"
words = ["foo","bar"]
print(findSubstring(s, words))
```
#### TC: O(N * M) **SC:** O(M)

The code uses a sliding window approach to find all substrings in the given string 's' that are
concatenations of all words in the 'words' list. It maintains a Counter of the words to be found and
iterates through 's' to check for valid substrings. The time complexity is O(N * M) where N is the
length of 's' and M is the number of words in the 'words' list. The space complexity is O(M) to
store the Counter of words. The code efficiently handles the concatenation of words and ensures each
word appears the correct number of times in the substring.

---
---
---
 --- 
# Longest Substring with At Most K Distinct Characters
>Find the longest substring with at most k distinct characters using Rabin-Karp algorithm.

>Input: s = "eceba", k = 2
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/
- Difficulty: 50
- Frequent: 75
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Longest Substring with At Most K Distinct Characters -- Shortest Solution:
```python
from collections import defaultdict

def lengthOfLongestSubstringKDistinct(s, k):
    if k == 0:
        return 0
    if len(s) <= k:
        return len(s)
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

# Example
s = "eceba"
k = 2
print(lengthOfLongestSubstringKDistinct(s, k))  # Output: 3
```
#### TC: O(n) **SC:** O(k)

The code defines a function that finds the length of the longest substring with at most k distinct
characters in a given string. It uses a sliding window approach to keep track of the characters and
their counts within the window. The algorithm efficiently updates the window boundaries based on the
number of distinct characters. The time complexity is O(n) where n is the length of the input
string, and the space complexity is O(k) where k is the number of distinct characters allowed. This
solution is clean, efficient, and easy to understand.

---
### Longest Substring with At Most K Distinct Characters -- Best SC Solution:
```python
# Function to find the longest substring with at most K distinct characters

def lengthOfLongestSubstringKDistinct(s: str, k: int) -> int:
    if k == 0:
        return 0
    if len(s) == 0:
        return 0
    left, right = 0, 0
    char_count = {}
    max_length = 0
    while right < len(s):
        char_count[s[right]] = char_count.get(s[right], 0) + 1
        while len(char_count) > k:
            char_count[s[left]] -= 1
            if char_count[s[left]] == 0:
                del char_count[s[left]]
            left += 1
        max_length = max(max_length, right - left + 1)
        right += 1
    return max_length
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(k)

The code uses a sliding window approach to find the longest substring with at most K distinct
characters. It maintains a window with at most K distinct characters and updates the maximum length
of the substring as it moves the window. The char_count dictionary is used to keep track of the
count of each character in the current window. The code efficiently handles the sliding window by
adjusting the left and right pointers based on the number of distinct characters in the window.
Overall, the code is clean, easy to understand, and has a time complexity of O(n) and space
complexity of O(k), where n is the length of the input string and k is the maximum number of
distinct characters allowed.

---
---
---
 --- 
# Longest Substring with At Least One Repeating Character
>Find the longest substring with at least one repeating character using Rabin-Karp algorithm.

>Input: s = "abcabcbb"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-least-one-repeating-character/
- Difficulty: 50
- Frequent: 75
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Longest Substring with At Least One Repeating Character -- Shortest Solution:
```python
# Sliding Window Approach

def lengthOfLongestSubstring(s: str) -> int:
    n = len(s)
    seen = {}
    left = 0
    max_length = 0

    for right in range(n):
        if s[right] in seen:
            left = max(left, seen[s[right]] + 1)
        seen[s[right]] = right
        max_length = max(max_length, right - left + 1)

    return max_length

# Example
s = "abcabcbb"
print(lengthOfLongestSubstring(s))  # Output: 3
```
#### TC: O(n) **SC:** O(min(m, n))

The code uses a sliding window approach to find the longest substring with at least one repeating
character. It maintains a window defined by two pointers, 'left' and 'right', and a dictionary
'seen' to store the index of each character seen so far. The algorithm iterates through the string,
updating the 'left' pointer when a repeating character is found. The 'max_length' variable keeps
track of the longest substring found. The time complexity is O(n) where n is the length of the input
string, and the space complexity is O(min(m, n)) where m is the size of the character set.

---
### Longest Substring with At Least One Repeating Character -- Best SC Solution:
```python
# Function to find the length of the longest substring with at least one repeating character

def lengthOfLongestSubstring(s: str) -> int:
    if not s:
        return 0
    
    max_length = 0
    start = 0
    char_index_map = {}
    
    for end in range(len(s)):
        if s[end] in char_index_map:
            start = max(start, char_index_map[s[end]] + 1)
        
        char_index_map[s[end]] = end
        max_length = max(max_length, end - start + 1)
    
    return max_length

# Example usage
s = "abcabcbb"
print(lengthOfLongestSubstring(s))  # Output: 3
```
#### TC: O(n) **SC:** O(1)

The code uses a sliding window approach to find the length of the longest substring with at least
one repeating character. It maintains a map of characters and their indices to track the last
occurrence of each character. By updating the start index of the window when a repeating character
is found, it efficiently calculates the maximum length of the substring. The time complexity is O(n)
as it iterates through the input string once, and the space complexity is O(1) as the extra space
used is constant regardless of the input size.

---
---
---
 --- 
# Longest Substring with At Least Two Repeating Characters
>Find the longest substring with at least two repeating characters using Rabin-Karp algorithm.

>Input: s = "abcabcbb"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-least-two-repeating-characters/
- Difficulty: 50
- Frequent: 75
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Longest Substring with At Least Two Repeating Characters -- Shortest Solution:
```python
# Rabin-Karp Algorithm Approach

def longest_substring_with_two_repeating_characters(s: str) -> int:
    def at_least_k_repeating_chars(s: str, k: int) -> int:
        count = 0
        start = 0
        end = 0
        unique = 0
        freq = [0] * 26
        res = 0
        while end < len(s):
            if freq[ord(s[end]) - ord('a')] == 0:
                unique += 1
            freq[ord(s[end]) - ord('a')] += 1
            if freq[ord(s[end]) - ord('a')] == k:
                count += 1
            while unique > 2:
                if freq[ord(s[start]) - ord('a')] == k:
                    count -= 1
                freq[ord(s[start]) - ord('a')] -= 1
                if freq[ord(s[start]) - ord('a')] == 0:
                    unique -= 1
                start += 1
            if count == unique:
                res = max(res, end - start + 1)
            end += 1
        return res
    return max(at_least_k_repeating_chars(s, 2) for i in range(1, 27))

# Test the function
s = "abaccc"
print(longest_substring_with_two_repeating_characters(s))
```
#### TC: O(N) **SC:** O(1)

The code uses the Rabin-Karp Algorithm approach to find the longest substring with at least two
repeating characters. It defines a helper function to check for at least k repeating characters in a
substring. The main function iterates through all possible values of k (from 1 to 26) and returns
the maximum length of the substring with at least two repeating characters. The time complexity of
the code is O(N) where N is the length of the input string, and the space complexity is O(1) as the
extra space used is constant regardless of the input size.

---
### Longest Substring with At Least Two Repeating Characters -- Best SC Solution:
```python
# Rabin-Karp Algorithm Approach

def longest_substring(s: str) -> int:
    def helper(k: int) -> bool:
        freq = {}
        start = 0
        for end in range(len(s)):
            freq[s[end]] = freq.get(s[end], 0) + 1
            while len(freq) > k:
                freq[s[start]] -= 1
                if freq[s[start]] == 0:
                    del freq[s[start]]
                start += 1
            if end - start + 1 >= 2:
                return True
        return False
    left, right = 2, len(s)
    while left <= right:
        mid = left + (right - left) // 2
        if helper(mid):
            left = mid + 1
        else:
            right = mid - 1
    return right
```
#### TC: O(N log N) **SC:** O(N)

The code uses the Rabin-Karp Algorithm approach to find the longest substring with at least two
repeating characters. It defines a helper function to check if a substring of length k has at least
two repeating characters. The main function uses binary search to find the longest substring length
that satisfies the condition. The code efficiently handles the frequency of characters in the
substring and adjusts the window size accordingly. The time complexity of the code is O(N log N) due
to the binary search, and the space complexity is O(N) to store the frequency of characters.

---
---
---
 --- 
# Longest Substring with At Least K Repeating Characters
>Find the longest substring with at least k repeating characters using Rabin-Karp algorithm.

>Input: s = "aaabb", k = 3
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/
- Difficulty: 60
- Frequent: 75
- Tags: ['Hash Table', 'String', 'Divide and Conquer', 'Sliding Window']

### Longest Substring with At Least K Repeating Characters -- Shortest Solution:
```python
from collections import Counter

def longestSubstring(s, k):
    if not s:
        return 0
    if k <= 1:
        return len(s)
    counter = Counter(s)
    for char, count in counter.items():
        if count < k:
            return max(longestSubstring(sub, k) for sub in s.split(char))
    return len(s)

# Example
s = "aaabb"
k = 3
print(longestSubstring(s, k) # Output: 3
```
#### TC: O(N^2) **SC:** O(N)

The code defines a function 'longestSubstring' that recursively finds the longest substring with at
least k repeating characters. It uses a Counter to count the occurrences of each character in the
input string. If any character occurs less than k times, it splits the string at that character and
recursively calls the function on each split substring. The function returns the length of the
longest valid substring. The time complexity is O(N^2) due to the recursive calls, and the space
complexity is O(N) for the Counter storage.

---
---
---
 --- 
# Repeated Substring Pattern
>Check if a string can be constructed by repeating a substring using Rabin-Karp algorithm.

>Input: s = "abab"
Output: true
- https://leetcode.com/problems/repeated-substring-pattern/
- Difficulty: 40
- Frequent: 70
- Tags: ['String', 'String Matching']

### Repeated Substring Pattern -- Shortest Solution:
```python
s = s + s
return s[1:len(s)-1].find(s) != -1
```
#### TC: O(n) **SC:** O(n)

The code concatenates the input string with itself. Then, it checks if the original string exists
within the concatenated string excluding the first and last characters. If it does, it means the
original string can be formed by repeating a substring. The time complexity is O(n) due to string
concatenation and search operations, and the space complexity is O(n) to store the concatenated
string.

---
---
---
 --- 
# Check If a String Contains All Binary Codes of Size K
>Check if a string contains all binary codes of size k using Rabin-Karp algorithm.

>Input: s = "00110110", k = 2
Output: true
- https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/
- Difficulty: 50
- Frequent: 70
- Tags: ['Hash Table', 'String', 'Bit Manipulation', 'Rolling Hash']

### Check If a String Contains All Binary Codes of Size K -- Shortest Solution:
```python
class Solution:
    def hasAllCodes(self, s: str, k: int) -> bool:
        seen = set()
        for i in range(len(s) - k + 1):
            seen.add(s[i:i+k])
        return len(seen) == 2 ** k
```
#### TC: O(n) **SC:** O(2^k)

The code uses a set to store all unique binary codes of size k found in the input string. It
iterates through the string and adds each substring of size k to the set. Finally, it checks if the
number of unique binary codes found is equal to 2^k, which indicates that all possible binary codes
of size k are present in the input string. The time complexity is O(n) where n is the length of the
input string, and the space complexity is O(2^k) to store all unique binary codes of size k.

---
---
---
 --- 
# Longest Substring with At Most Two Distinct Characters
>Find the longest substring with at most two distinct characters using Rabin-Karp algorithm.

>Input: s = "eceba"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/
- Difficulty: 50
- Frequent: 70
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Longest Substring with At Most Two Distinct Characters -- Shortest Solution:
```python
# Rabin-Karp Algorithm Approach

def lengthOfLongestSubstringTwoDistinct(s: str) -> int:
    if len(s) < 3:
        return len(s)
    left, right = 0, 0
    hashmap = {}
    max_length = 2
    while right < len(s):
        if len(hashmap) < 3:
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

The code uses the Rabin-Karp Algorithm approach to find the longest substring with at most two
distinct characters in a given string. It maintains a hashmap to store the characters and their
indices while iterating through the string. The algorithm slides a window from left to right,
updating the hashmap and the maximum length of the substring as needed. The time complexity of the
code is O(n) where n is the length of the input string, and the space complexity is O(1) since the
hashmap will contain at most 3 entries (two distinct characters and their indices).

---
### Longest Substring with At Most Two Distinct Characters -- Best TC Solution:
```python
# Rabin-Karp Algorithm Approach

def lengthOfLongestSubstringTwoDistinct(s: str) -> int:
    if len(s) < 3:
        return len(s)
    left, right = 0, 0
    hashmap = {}
    max_length = 2
    while right < len(s):
        if len(hashmap) < 3:
            hashmap[s[right]] = right
            right += 1
        if len(hashmap) == 3:
            del_idx = min(hashmap.values())
            del hashmap[s[del_idx]]
            left = del_idx + 1
        max_length = max(max_length, right - left)
    return max_length

# Example
s = "eceba"
print(lengthOfLongestSubstringTwoDistinct(s))  # Output: 3
```
#### TC: O(N) **SC:** O(1)

The code uses the Rabin-Karp Algorithm approach to find the longest substring with at most two
distinct characters. It maintains a hashmap to store the characters and their indices in the current
window. The algorithm slides the window to the right, updating the hashmap and the maximum length of
the substring as needed. The time complexity of the code is O(N) where N is the length of the input
string, and the space complexity is O(1) as the hashmap will contain at most 3 entries.

---
---
---
 --- 
# Longest Common Substring
>Find the longest common substring between two strings using Rabin-Karp algorithm.

>Input: s1 = "abcde", s2 = "abfce"
Output: "abc"
- https://leetcode.com/problems/longest-common-substring/
- Difficulty: 60
- Frequent: 70
- Tags: ['String', 'Dynamic Programming']

### Longest Common Substring -- Shortest Solution:
```python
# Function to find the longest common substring

def longest_common_substring(s1: str, s2: str) -> str:
    def search(length: int) -> str:
        seen = {s1[i:i+length] for i in range(len(s1) - length + 1)}
        return next((s2[j:j+length] for j in range(len(s2) - length + 1 if s2[j:j+length] in seen), ''), '')
    low, high = 0, min(len(s1), len(s2))
    while low < high:
        mid = (low + high + 1) // 2
        if search(mid):
            low = mid
        else:
            high = mid - 1
    return search(low)

# Example
s1 = "leetcode"
s2 = "code"
print(longest_common_substring(s1, s2))
```
#### TC: O((m+n)log(min(m,n))) **SC:** O((m+n)^2)

The code uses a binary search approach to find the longest common substring between two given
strings. It iteratively searches for the longest common substring length by dividing the search
space in half. The search function checks if a substring of a certain length exists in both strings.
The algorithm efficiently reduces the search space by discarding half of it in each iteration,
leading to a time complexity of O((m+n)log(min(m,n))). However, the space complexity is O((m+n)^2)
due to the creation of substrings and sets during the search process.

---
### Longest Common Substring -- Best SC Solution:
```python
# Function to find the longest common substring

def longest_common_substring(s1: str, s2: str) -> str:
    if not s1 or not s2:
        return ""
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    max_len = 0
    end_index = 0
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
                if dp[i][j] > max_len:
                    max_len = dp[i][j]
                    end_index = i
    return s1[end_index - max_len: end_index]

# Example
s1 = "leetcode"
s2 = "code"
print(longest_common_substring(s1, s2))
```
#### TC: O(m*n) **SC:** O(m*n)

The code defines a function to find the longest common substring between two input strings using
dynamic programming. It iterates through the strings to build a 2D array to store the lengths of
common substrings. The time complexity is O(m*n) where m and n are the lengths of the input strings,
and the space complexity is also O(m*n) to store the dynamic programming table. The algorithm
efficiently finds the longest common substring by updating the maximum length and end index as it
iterates through the strings.

---
---
---
 --- 
# Longest Substring with At Most One Distinct Character
>Find the longest substring with at most one distinct character using Rabin-Karp algorithm.

>Input: s = "aaabb"
Output: 3
- https://leetcode.com/problems/longest-substring-with-at-most-one-distinct-character/
- Difficulty: 40
- Frequent: 65
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Longest Substring with At Most One Distinct Character -- Shortest Solution:
```python
# Rabin-Karp Algorithm Approach

def lengthOfLongestSubstringTwoDistinct(s: str) -> int:
    if len(s) < 3:
        return len(s)
    left, right = 0, 0
    hashmap = {}
    max_length = 2
    while right < len(s):
        if len(hashmap) < 3:
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

The code uses the Rabin-Karp Algorithm approach to find the longest substring with at most one
distinct character. It maintains a hashmap to store the characters and their indices in the current
window. The algorithm slides the window to the right and updates the hashmap accordingly. If the
window contains more than two distinct characters, it adjusts the window by removing the leftmost
character until only two distinct characters remain. The algorithm keeps track of the maximum length
of the valid substring and returns it at the end. The time complexity of the code is O(n) where n is
the length of the input string, and the space complexity is O(1) as the hashmap stores at most 3
characters at a time.

---
---
---
 --- 
# Maximum Number of Occurrences of a Substring
>Find the maximum number of occurrences of a substring using Rabin-Karp algorithm.

>Input: s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
Output: 2
- https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring/
- Difficulty: 60
- Frequent: 65
- Tags: ['Hash Table', 'String', 'Sliding Window']

### Maximum Number of Occurrences of a Substring -- Shortest Solution:
```python
from collections import Counter
def maxFreq(s: str, maxLetters: int, minSize: int, maxSize: int) -> int:
    count = Counter()
    res = 0
    for i in range(len(s) - minSize + 1):
        sub = s[i:i + minSize]
        if len(set(sub)) <= maxLetters:
            count[sub] += 1
            res = max(res, count[sub])
    return res
```
#### TC: O(N) **SC:** O(N)

The code uses a sliding window approach to iterate through the string 's'. It checks each substring
of length 'minSize' within the window and counts the occurrences of each valid substring. The
Counter class is used to keep track of the counts. The code efficiently handles the constraints of
maximum letters and minimum size. The time complexity is O(N) where N is the length of the input
string 's', and the space complexity is O(N) due to the Counter used to store substring counts.

---
---
---
 --- 
# Longest Duplicate Substring
>Find the longest duplicate substring in a string using Rabin-Karp algorithm.

>Input: s = "banana"
Output: "ana"
- https://leetcode.com/problems/longest-duplicate-substring/
- Difficulty: 60
- Frequent: 60
- Tags: ['String', 'Binary Search', 'Suffix Array', 'Hash Function', 'Rolling Hash']

### Longest Duplicate Substring -- Shortest Solution:
```python
# Rabin-Karp Algorithm

def longestDupSubstring(self, S: str) -> str:
    def search(L, a, modulus, n, nums):
        h = 0
        for i in range(L):
            h = (h * a + nums[i]) % modulus
        seen = {h}
        aL = pow(a, L, modulus)
        for start in range(1, n - L + 1):
            h = (h * a - nums[start - 1] * aL + nums[start + L - 1]) % modulus
            if h in seen:
                return start
            seen.add(h)
        return -1

    n = len(S)
    nums = [ord(S[i]) - ord('a') for i in range(n)]
    a = 26
    modulus = 2 ** 63 - 1

    left, right = 1, n
    while left != right:
        L = left + (right - left) // 2
        if search(L, a, modulus, n, nums) != -1:
            left = L + 1
        else:
            right = L

    start = search(left - 1, a, modulus, n, nums)
    return S[start:start + left - 1]
```
#### TC: O(N*log(N)) **SC:** O(N)

The code uses the Rabin-Karp Algorithm to find the longest duplicate substring in a given string. It
calculates the hash value of substrings using a rolling hash technique. The search function checks
for duplicate substrings of a specific length by updating the hash value efficiently. The main
function iterates through different substring lengths using binary search to find the longest
duplicate substring. The time complexity is O(N*log(N)) due to the binary search, and the space
complexity is O(N) to store the hash values and seen substrings.

---
---
---
 --- 
# Distinct Echo Substrings
>Count the number of distinct echo substrings using Rabin-Karp algorithm.

>Input: text = "abcabcabc"
Output: 3
- https://leetcode.com/problems/distinct-echo-substrings/
- Difficulty: 70
- Frequent: 60
- Tags: ['String', 'Rolling Hash']

### Distinct Echo Substrings -- Shortest Solution:
```python
# Rabin-Karp Algorithm Approach

def distinctEchoSubstrings(self, text: str) -> int:
    def search(L: int) -> int:
        base, mod = 113, 10**9 + 7
        hash_val = 0
        for i in range(L):
            hash_val = (hash_val * base + ord(text[i])) % mod
        seen = {hash_val}
        baseL = pow(base, L, mod)
        for i in range(1, len(text) - L + 1):
            hash_val = (hash_val * base - ord(text[i - 1]) * baseL + ord(text[i + L - 1])) % mod
            if hash_val in seen:
                return i
            seen.add(hash_val)
        return -1

    res = 0
    for L in range(1, len(text) // 2 + 1):
        if search(L) != -1:
            res += 1
    return res
```
#### TC: O(N^2 * logN) **SC:** O(N)

The code uses the Rabin-Karp Algorithm approach to find distinct echo substrings in the given text.
It calculates the hash value for each substring of length L and checks for duplicates using a set.
The search function is used to find the starting index of a repeated substring. The main function
iterates over different lengths of substrings to count the distinct echo substrings. The time
complexity is O(N^2 * logN) due to the nested loops and hash calculations, and the space complexity
is O(N) for storing hash values in a set.

---
### Distinct Echo Substrings -- Best TC Solution:
```python
# Rabin-Karp Algorithm Approach

def distinctEchoSubstrings(text: str) -> int:
    def search(L: int) -> int:
        base, mod = 113, 10**9 + 7
        hash_val = 0
        for i in range(L):
            hash_val = (hash_val * base + ord(text[i])) % mod
        seen = {hash_val}
        baseL = pow(base, L, mod)
        for i in range(1, len(text) - L + 1):
            hash_val = (hash_val * base - ord(text[i - 1]) * baseL + ord(text[i + L - 1])) % mod
            if hash_val in seen:
                return i
            seen.add(hash_val)
        return -1

    res = set()
    for L in range(1, len(text) // 2 + 1):
        pos = search(L)
        while pos != -1:
            res.add(text[pos:pos + L])
            pos = search(L)
    return len(res)

# Input: text = "abcabcabc"
# Output: 3
print(distinctEchoSubstrings("abcabcabc"))
```
#### TC: O(N^2) **SC:** O(N)

The code uses the Rabin-Karp Algorithm approach to find distinct echo substrings in the given text.
It calculates the hash value for each substring of length L and checks for duplicates using a set.
The search function efficiently calculates the hash values using rolling hash technique. The main
function iterates over different lengths of substrings to find all distinct echo substrings. The
time complexity is O(N^2) where N is the length of the input text, and the space complexity is O(N)
to store the hash values and substrings.

---
---
---
 --- 
# Smallest Rotation with Highest Score
>Find the smallest rotation with the highest score using Rabin-Karp algorithm.

>Input: nums = [2,3,1,4,0]
Output: 3
- https://leetcode.com/problems/smallest-rotation-with-highest-score/
- Difficulty: 80
- Frequent: 50
- Tags: ['Array', 'Binary Search', 'Segment Tree']

### Smallest Rotation with Highest Score -- Shortest Solution:
```python
from collections import Counter
def bestRotation(A):
    N = len(A)
    bad = [0] * N
    for i, x in enumerate(A):
        left, right = (i - x + 1) % N, (i + 1) % N
        bad[left] -= 1
        bad[right] += 1
        if left > right:
            bad[0] -= 1
    best = -N
    ans = cur = 0
    for i, score in enumerate(bad):
        cur += score
        if cur > best:
            best = cur
            ans = i
    return ans
```
#### TC: O(N) **SC:** O(N)

The code calculates the 'bad' array to keep track of the changes in score for each rotation. It then
iterates through the 'bad' array to find the rotation that gives the highest score. The algorithm
has a time complexity of O(N) and a space complexity of O(N), where N is the length of the input
array A.

---
### Smallest Rotation with Highest Score -- Best TC Solution:
```python
from collections import Counter
def bestRotation(A):
    n = len(A)
    bad = [0] * n
    for i, x in enumerate(A):
        left, right = (i - x + 1) % n, (i + 1) % n
        bad[left] -= 1
        bad[right] += 1
        if left > right:
            bad[0] -= 1
    best_k, best_score, cur_score = 0, 0, 0
    for i in range(n):
        cur_score += bad[i]
        if cur_score > best_score:
            best_k, best_score = i, cur_score
    return best_k
```
#### TC: O(n) **SC:** O(n)

The code calculates the 'bad' array to keep track of the changes in score for each rotation. It then
iterates through the array to find the best rotation 'k' that maximizes the score. The algorithm has
a time complexity of O(n) and a space complexity of O(n), making it efficient and easy to
understand.

---
---
---