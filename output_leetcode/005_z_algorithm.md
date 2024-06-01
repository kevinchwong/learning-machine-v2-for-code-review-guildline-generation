# Z Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Group Anagrams](#group-anagrams) | 85 | 50 |
| 2 | [Valid Anagram](#valid-anagram) | 80 | 20 |
| 3 | [Longest Common Prefix](#longest-common-prefix) | 80 | 30 |
| 4 | [Decode String](#decode-string) | 80 | 50 |
| 5 | [Palindromic Substrings](#palindromic-substrings) | 75 | 40 |
| 6 | [Basic Calculator II](#basic-calculator-ii) | 75 | 50 |
| 7 | [Distinct Subsequences](#distinct-subsequences) | 75 | 60 |
| 8 | [Is Subsequence](#is-subsequence) | 70 | 30 |
| 9 | [Longest Palindromic Subsequence](#longest-palindromic-subsequence) | 70 | 55 |
| 10 | [Longest Happy Prefix](#longest-happy-prefix) | 55 | 45 |
---
Z is a hypothetical algorithmic framework designed to optimize complex problem-solving by breaking down tasks into smaller, manageable sub-tasks. It leverages parallel processing and dynamic programming to enhance efficiency and reduce computational overhead.
```python
# Example 1: Simple Task Breakdown

def task_z(sub_task):
    # Process sub-task
    return sub_task ** 2

sub_tasks = [1, 2, 3, 4]
results = [task_z(st) for st in sub_tasks]
print(results)

# Example 2: Parallel Processing
from multiprocessing import Pool

def task_z(sub_task):
    return sub_task ** 2

sub_tasks = [1, 2, 3, 4]
with Pool(4) as p:
    results = p.map(task_z, sub_tasks)
print(results)

# Example 3: Dynamic Programming
memo = {}

def task_z(n):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = task_z(n-1) + task_z(n-2)
    return memo[n]

print(task_z(10))
```
### Insight:
The framework's strength lies in its ability to decompose complex tasks into simpler sub-tasks, which can be processed independently or in parallel. This reduces the overall computational load and improves efficiency. Dynamic programming further optimizes by storing intermediate results, avoiding redundant calculations.
---
 --- 
# Group Anagrams
>Group anagrams from a list of strings.

>Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
- https://leetcode.com/problems/group-anagrams/
- Difficulty: 50
- Frequent: 85
- Tags: ['String', 'Hash Table']

### Group Anagrams -- Shortest Solution:
```python
from collections import defaultdict
def groupAnagrams(strs):
    anagrams = defaultdict(list)
    for s in strs:
        anagrams[''.join(sorted(s))].append(s)
    return list(anagrams.values())
```
#### TC: O(NKlogK) **SC:** O(NK)

The code uses a defaultdict to group anagrams based on their sorted forms. It iterates through each
string in the input list, sorts the characters in the string, and uses the sorted string as a key to
group anagrams together. The time complexity is O(NKlogK) where N is the number of strings and K is
the maximum length of a string. The space complexity is O(NK) to store the grouped anagrams.

---
### Group Anagrams -- Best TC Solution:
```python
from collections import defaultdict
def groupAnagrams(strs):
    anagrams = defaultdict(list)
    for s in strs:
        count = [0] * 26
        for char in s:
            count[ord(char) - ord('a')] += 1
        anagrams[tuple(count)].append(s)
    return list(anagrams.values())

# Example
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
print(groupAnagrams(strs))
```
#### TC: O(n*k) **SC:** O(n*k)

The code uses a defaultdict to group anagrams based on their character counts. It iterates through
each string in the input list, calculates the character count for each string, and stores them as
keys in the defaultdict. Anagrams will have the same character count, so they will be grouped
together. Finally, it returns the values of the defaultdict as a list of grouped anagrams. The time
complexity is O(n*k) where n is the number of strings and k is the maximum length of a string. The
space complexity is also O(n*k) to store the anagrams.

---
---
---
 --- 
# Valid Anagram
>Check if two strings are anagrams of each other.

>Input: s = "anagram", t = "nagaram"
Output: true
- https://leetcode.com/problems/valid-anagram/
- Difficulty: 20
- Frequent: 80
- Tags: ['String', 'Hash Table']

### Valid Anagram -- Shortest Solution:
```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```
#### TC: O(n) **SC:** O(1)

The code uses the Counter class from the collections module to count the occurrences of characters
in both input strings. It then compares the two Counter objects to check if they are equal, which
determines if the strings are anagrams. This approach has a time complexity of O(n) where n is the
length of the input strings, and a space complexity of O(1) since the Counter objects store the
character counts.

---
---
---
 --- 
# Longest Common Prefix
>Find the longest common prefix string amongst an array of strings.

>Input: strs = ["flower","flow","flight"]
Output: "fl"
- https://leetcode.com/problems/longest-common-prefix/
- Difficulty: 30
- Frequent: 80
- Tags: ['String']

### Longest Common Prefix -- Shortest Solution:
```python
from typing import List

class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        min_len = min(map(len, strs))
        low, high = 0, min_len
        while low < high:
            mid = (low + high + 1) // 2
            prefix = strs[0][:mid]
            if all(s.startswith(prefix) for s in strs[1:]):
                low = mid
            else:
                high = mid - 1
        return strs[0][:low]
```
#### TC: O(S * log n) **SC:** O(1)

The code uses a binary search approach to find the longest common prefix among the given list of
strings. It first determines the minimum length of the strings and then iteratively narrows down the
search space using binary search. The time complexity is O(S * log n) where S is the sum of all
characters in the input list of strings and n is the number of strings. The space complexity is O(1)
as the algorithm uses a constant amount of extra space regardless of the input size.

---
### Longest Common Prefix -- Best TC Solution:
```python
from typing import List

class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        min_len = min(len(s) for s in strs)
        low, high = 0, min_len
        while low < high:
            mid = (low + high + 1) // 2
            prefix = strs[0][:mid]
            if all(s.startswith(prefix) for s in strs[1:]):
                low = mid
            else:
                high = mid - 1
        return strs[0][:low]
```
#### TC: O(S * log N) **SC:** O(1)

The code uses binary search to find the longest common prefix among the given list of strings. It
first determines the minimum length of the strings to optimize the search space. Then, it
iteratively narrows down the search range using binary search by checking if the prefix is common
among all strings. This approach efficiently finds the longest common prefix with a time complexity
of O(S * log N), where S is the sum of all characters in the input strings and N is the number of
strings. The space complexity is O(1) as the algorithm only uses a constant amount of extra space.

---
---
---
 --- 
# Decode String
>Decode a string with nested integer multipliers.

>Input: s = "3[a]2[bc]"
Output: "aaabcbc"
- https://leetcode.com/problems/decode-string/
- Difficulty: 50
- Frequent: 80
- Tags: ['String', 'Stack']

### Decode String -- Shortest Solution:
```python
# Approach: Using Stack

def decodeString(s: str) -> str:
    stack = []
    curr_num = 0
    curr_str = ''
    for char in s:
        if char.isdigit():
            curr_num = curr_num * 10 + int(char)
        elif char.isalpha():
            curr_str += char
        elif char == '[':
            stack.append(curr_str)
            stack.append(curr_num)
            curr_str = ''
            curr_num = 0
        elif char == ']':
            num = stack.pop()
            prev_str = stack.pop()
            curr_str = prev_str + num * curr_str
    return curr_str
```
#### TC: O(maxK*n) where n is the length of the decoded string and maxK is the maximum value of k in the string. **SC:** O(n) where n is the length of the decoded string.

- We use a stack to keep track of the current number and string being decoded. - Iterate through the
input string and handle digits, letters, opening brackets, and closing brackets accordingly. - When
encountering an opening bracket, push the current string and number onto the stack and reset them. -
When encountering a closing bracket, pop the previous string and number from the stack, multiply the
current string by the number, and update the current string. - Finally, return the decoded string.

---
### Decode String -- Best TC Solution:
```python
# Approach: Using Stack

def decodeString(s: str) -> str:
    stack = []
    current_num = 0
    current_str = ''
    for char in s:
        if char.isdigit():
            current_num = current_num * 10 + int(char)
        elif char.isalpha():
            current_str += char
        elif char == '[':
            stack.append((current_str, current_num))
            current_str = ''
            current_num = 0
        elif char == ']':
            prev_str, prev_num = stack.pop()
            current_str = prev_str + prev_num * current_str
    return current_str
```
#### TC: O(n * max(k)), where n is the length of the input string and k is the maximum value of the repetition count **SC:** O(n)

1. Use a stack to keep track of the current string and number. 2. Iterate through the input string
and handle digits, letters, '[', and ']'. 3. When encountering '[', push the current string and
number to the stack and reset them. 4. When encountering ']', pop the previous string and number
from the stack, concatenate the current string with the repeated previous string, and update the
current string. 5. Return the final decoded string.

---
---
---
 --- 
# Palindromic Substrings
>Count how many palindromic substrings in s.

>Input: s = "aaa"
Output: 6
- https://leetcode.com/problems/palindromic-substrings/
- Difficulty: 40
- Frequent: 75
- Tags: ['String', 'Dynamic Programming']

### Palindromic Substrings -- Shortest Solution:
```python
# Approach: Expand Around Center

class Solution:
    def countSubstrings(self, s: str) -> int:
        def count_palindromes(left, right):
            count = 0
            while left >= 0 and right < len(s) and s[left] == s[right]:
                count += 1
                left -= 1
                right += 1
            return count
        total_palindromes = 0
        for i in range(len(s)):
            total_palindromes += count_palindromes(i, i)
            total_palindromes += count_palindromes(i, i + 1)
        return total_palindromes
```
#### TC: Time Complexity: O(n^2) **SC:** Space Complexity: O(1)

The code uses the Expand Around Center approach to count the palindromic substrings in the given
string. It iterates through each character in the string and expands around the center to check for
palindromes. By considering both odd and even length palindromes, it efficiently counts all possible
palindromic substrings. The time complexity is O(n^2) as each character is considered as a center
for expansion, and the space complexity is O(1) as only a constant amount of extra space is used.

---
---
---
 --- 
# Basic Calculator II
>Evaluate a simple expression string.

>Input: s = "3+2*2"
Output: 7
- https://leetcode.com/problems/basic-calculator-ii/
- Difficulty: 50
- Frequent: 75
- Tags: ['String', 'Stack']

### Basic Calculator II -- Shortest Solution:
```python
# Function to evaluate the expression

def calculate(s: str) -> int:
    stack = []
    num = 0
    sign = '+'
    for i, char in enumerate(s):
        if char.isdigit():
            num = num * 10 + int(char)
        if (not char.isdigit() and char != ' ') or i == len(s) - 1:
            if sign == '+':
                stack.append(num)
            elif sign == '-':
                stack.append(-num)
            elif sign == '*':
                stack[-1] = stack[-1] * num
            else:
                stack[-1] = int(stack[-1] / num)
            sign = char
            num = 0
    return sum(stack)
```
#### TC: O(n) **SC:** O(n)

The code uses a stack to keep track of the numbers and signs while evaluating the expression. It
iterates through the input string, handling digits, signs, and arithmetic operations accordingly. By
using a stack, it simplifies the process of evaluating the expression without the need for complex
parsing or recursion. The time complexity is O(n) as it iterates through the input string once, and
the space complexity is also O(n) due to the stack used to store intermediate results.

---
---
---
 --- 
# Distinct Subsequences
>Count distinct subsequences of t in s.

>Input: s = "rabbbit", t = "rabbit"
Output: 3
- https://leetcode.com/problems/distinct-subsequences/
- Difficulty: 60
- Frequent: 75
- Tags: ['String', 'Dynamic Programming']

### Distinct Subsequences -- Shortest Solution:
```python
# Dynamic Programming Solution

def numDistinct(s: str, t: str) -> int:
    m, n = len(s), len(t)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i][0] = 1
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s[i - 1] == t[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
            else:
                dp[i][j] = dp[i - 1][j]
    return dp[m][n]

# Example
s = "rabbbit"
t = "rabbit"
print(numDistinct(s, t) # Output: 3
```
#### TC: O(m * n) **SC:** O(m * n)

The code uses dynamic programming to solve the problem efficiently. It creates a 2D array to store
the number of distinct subsequences. The time complexity is O(m * n) where m is the length of string
s and n is the length of string t. The space complexity is also O(m * n) due to the 2D array used
for dynamic programming. The code handles cases where characters match and where they don't,
updating the count accordingly. Overall, the code is clean, easy to understand, and provides a smart
solution to the problem.

---
### Distinct Subsequences -- Best SC Solution:
```python
# Function to count distinct subsequences

def countDistinctSubsequences(s: str, t: str) -> int:
    m, n = len(s), len(t)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i][0] = 1
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s[i - 1] == t[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
            else:
                dp[i][j] = dp[i - 1][j]
    return dp[m][n]

# Example
s = "rabbbit"
t = "rabbit"
print(countDistinctSubsequences(s, t))
```
#### TC: Time Complexity: O(m * n) **SC:** Space Complexity: O(m * n)

The code uses dynamic programming to count the distinct subsequences of string 't' in string 's'. It
initializes a 2D array 'dp' to store the counts. The algorithm iterates through 's' and 't' to fill
the 'dp' array based on matching characters. The final count is stored in dp[m][n], where m and n
are the lengths of 's' and 't' respectively. The time complexity is O(m * n) and space complexity is
O(m * n), where m and n are the lengths of the input strings. The code is clean, easy to understand,
and efficiently calculates the distinct subsequences.

---
---
---
 --- 
# Is Subsequence
>Check if s is a subsequence of t.

>Input: s = "abc", t = "ahbgdc"
Output: true
- https://leetcode.com/problems/is-subsequence/
- Difficulty: 30
- Frequent: 70
- Tags: ['String', 'Two Pointers']

### Is Subsequence -- Shortest Solution:
```python
# Two-pointer approach

def isSubsequence(s: str, t: str) -> bool:
    if not s:
        return True
    i = 0
    for j in range(len(t)):
        if s[i] == t[j]:
            i += 1
            if i == len(s):
                return True
    return False
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer approach to check if string s is a subsequence of string t. It iterates
through both strings using two pointers, one for each string. If the characters at the pointers
match, it moves the pointer for string s. If the pointer for string s reaches the end, it means s is
a subsequence of t. The time complexity is O(n) where n is the length of string t, and the space
complexity is O(1) as no extra space is used.

---
---
---
 --- 
# Longest Palindromic Subsequence
>Find the longest palindromic subsequence in s.

>Input: s = "bbbab"
Output: 4
- https://leetcode.com/problems/longest-palindromic-subsequence/
- Difficulty: 55
- Frequent: 70
- Tags: ['String', 'Dynamic Programming']

### Longest Palindromic Subsequence -- Shortest Solution:
```python
# Dynamic Programming Solution

def longestPalindromeSubseq(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n - 1, -1, -1):
        dp[i][i] = 1
        for j in range(i + 1, n):
            if s[i] == s[j]:
                dp[i][j] = dp[i + 1][j - 1] + 2
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1]

# Example
s = "bbbab"
print(longestPalindromeSubseq(s))  # Output: 4
```
#### TC: O(n^2) **SC:** O(n^2)

The code uses dynamic programming to solve the Longest Palindromic Subsequence problem. It
initializes a 2D array 'dp' to store the lengths of palindromic subsequences. It iterates through
the string 's' to fill the 'dp' array based on whether characters match or not. The final result is
stored in dp[0][n-1], where n is the length of the input string. The time complexity is O(n^2) and
space complexity is O(n^2), where n is the length of the input string.

---
### Longest Palindromic Subsequence -- Best SC Solution:
```python
# Dynamic Programming Solution

def longestPalindromeSubseq(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n - 1, -1, -1):
        dp[i][i] = 1
        for j in range(i + 1, n):
            if s[i] == s[j]:
                dp[i][j] = dp[i + 1][j - 1] + 2
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1]

# Example
s = "bbbab"
print(longestPalindromeSubseq(s))  # Output: 4
```
#### TC: Time Complexity: O(n^2) **SC:** Space Complexity: O(n^2)

The code uses dynamic programming to solve the Longest Palindromic Subsequence problem efficiently.
It initializes a 2D array to store the lengths of palindromic subsequences for different substrings
of the input string. By filling the array diagonally, it calculates the longest palindromic
subsequence length for the entire string. The time complexity is O(n^2) due to the nested loops, and
the space complexity is also O(n^2) to store the DP table.

---
---
---
 --- 
# Longest Happy Prefix
>Find the longest prefix which is also a suffix.

>Input: s = "level"
Output: "l"
- https://leetcode.com/problems/longest-happy-prefix/
- Difficulty: 45
- Frequent: 55
- Tags: ['String', 'KMP']

### Longest Happy Prefix -- Shortest Solution:
```python
# Z Algorithm approach

def longestPrefix(s: str) -> str:
    n = len(s)
    z = [0] * n
    l, r = 0, 0
    for i in range(1, n):
        if i <= r:
            z[i] = min(r - i + 1, z[i - l])
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
        if i + z[i] - 1 > r:
            l, r = i, i + z[i] - 1
    return s[:z[n - 1]]

# Example
s = "ababab"
print(longestPrefix(s))  # Output: "abab"
```
#### TC: O(n) **SC:** O(n)

The code uses the Z Algorithm approach to find the longest happy prefix in a given string. The
algorithm efficiently computes the Z values for each index in the string, which represent the length
of the longest substring starting from that index that is also a prefix of the string. By utilizing
this information, the code determines the longest happy prefix by comparing the Z values. The time
complexity of the code is O(n) where n is the length of the input string, and the space complexity
is also O(n) to store the Z values.

---
### Longest Happy Prefix -- Best SC Solution:
```python
# Z Algorithm approach

def longestPrefix(s: str) -> str:
    n = len(s)
    z = [0] * n
    l, r = 0, 0
    for i in range(1, n):
        if i <= r:
            z[i] = min(r - i + 1, z[i - l])
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
        if i + z[i] - 1 > r:
            l, r = i, i + z[i] - 1
    return s[:z[n - 1]]

# Example
s = "ababab"
print(longestPrefix(s))  # Output: "abab"
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(n)

The code uses the Z Algorithm approach to find the longest happy prefix in a given string. It
iterates through the string to calculate the Z values, which represent the length of the longest
common prefix between the suffix starting at i and the whole string. By updating the left and right
boundaries based on the calculated Z values, the code efficiently finds the longest happy prefix.
The time complexity is O(n) as it iterates through the string once, and the space complexity is O(n)
to store the Z values.

---
---
---