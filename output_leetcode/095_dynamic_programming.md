# Dynamic Programming
## Frequent score for this algorithmic framework: 95 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Maximum Subarray](#maximum-subarray) | 95 | 45 |
| 2 | [Coin Change](#coin-change) | 90 | 60 |
| 3 | [House Robber](#house-robber) | 85 | 40 |
| 4 | [Minimum Path Sum](#minimum-path-sum) | 85 | 45 |
| 5 | [Unique Paths](#unique-paths) | 80 | 35 |
| 6 | [Longest Increasing Subsequence](#longest-increasing-subsequence) | 80 | 50 |
| 7 | [Decode Ways](#decode-ways) | 80 | 55 |
| 8 | [Word Break](#word-break) | 75 | 50 |
| 9 | [Longest Common Subsequence](#longest-common-subsequence) | 75 | 55 |
| 10 | [Edit Distance](#edit-distance) | 75 | 65 |
| 11 | [Climbing Stairs](#climbing-stairs) | 70 | 30 |
| 12 | [Triangle](#triangle) | 70 | 50 |
| 13 | [Partition Equal Subset Sum](#partition-equal-subset-sum) | 70 | 55 |
| 14 | [Longest Palindromic Subsequence](#longest-palindromic-subsequence) | 65 | 60 |
| 15 | [Regular Expression Matching](#regular-expression-matching) | 65 | 70 |
| 16 | [Palindrome Partitioning II](#palindrome-partitioning-ii) | 60 | 65 |
| 17 | [Wildcard Matching](#wildcard-matching) | 60 | 65 |
| 18 | [Burst Balloons](#burst-balloons) | 60 | 70 |
| 19 | [Interleaving String](#interleaving-string) | 55 | 65 |
| 20 | [Scramble String](#scramble-string) | 50 | 70 |
---
Dynamic Programming (DP) is an optimization technique used to solve complex problems by breaking them down into simpler subproblems. It stores the results of subproblems to avoid redundant computations, making it efficient for problems with overlapping subproblems and optimal substructure properties. DP can be implemented using either a top-down (memoization) or bottom-up (tabulation) approach.
```python
# Example 1: Fibonacci Sequence (Top-Down)
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 2:
        return 1
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]

# Example 2: Fibonacci Sequence (Bottom-Up)
def fib_tab(n):
    if n <= 2:
        return 1
    dp = [0] * (n + 1)
    dp[1], dp[2] = 1, 1
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

# Example 3: 0/1 Knapsack Problem
def knapsack(weights, values, W):
    n = len(weights)
    dp = [[0 for _ in range(W + 1)] for _ in range(n + 1)]
    for i in range(1, n + 1):
        for w in range(1, W + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i-1][w], dp[i-1][w-weights[i-1]] + values[i-1])
            else:
                dp[i][w] = dp[i-1][w]
    return dp[n][W]
```
### Insight:
1. Dynamic Programming is useful for problems with overlapping subproblems and optimal substructure.
2. Memoization (top-down) stores results of subproblems in a dictionary to avoid redundant calculations.
3. Tabulation (bottom-up) builds a table iteratively to store results of subproblems.
4. The choice between memoization and tabulation depends on the problem and implementation preference.
5. DP can significantly reduce time complexity at the cost of increased space complexity.
---
 --- 
# Maximum Subarray
>Find the contiguous subarray with the largest sum.

>Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
- https://leetcode.com/problems/maximum-subarray/
- Difficulty: 45
- Frequent: 95
- Tags: ['Dynamic Programming']

### Maximum Subarray -- Shortest Solution:
```python
def maxSubArray(nums):
    for i in range(1, len(nums)):
        nums[i] = max(nums[i], nums[i] + nums[i - 1])
    return max(nums)
```
#### TC: O(n) **SC:** O(1)

The solution uses a dynamic programming approach where we iterate through the array and update each
element to be the maximum of itself or the sum of itself and the previous element. This way, each
element represents the maximum subarray sum ending at that position. Finally, we return the maximum
value in the updated array.

---
---
---
 --- 
# Coin Change
>Find the minimum number of coins needed to make up a given amount.

>Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
- https://leetcode.com/problems/coin-change/
- Difficulty: 60
- Frequent: 90
- Tags: ['Dynamic Programming']

### Coin Change -- Shortest Solution:
```python
from typing import List

def coinChange(coins: List[int], amount: int) -> int:
    dp = [0] + [float('inf')] * amount
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```
#### TC: O(n * m) where n is the number of coins and m is the amount **SC:** O(m) where m is the amount

The code uses dynamic programming to build up a solution for the minimum number of coins needed to
make each amount from 0 to the target amount. The `dp` array is initialized with `float('inf')` to
represent unreachable states, except for `dp[0]` which is 0 because no coins are needed to make
amount 0. For each coin, the code updates the `dp` array by considering if using the coin would
result in a smaller number of coins for each amount from the coin's value to the target amount. The
final answer is in `dp[amount]`, unless it remains `float('inf')`, in which case it's impossible to
form the amount with the given coins.

---
### Coin Change -- Best TC Solution:
```python
# Dynamic Programming Solution

def coinChange(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1

# Example
coins = [1, 2, 5]
amount = 11
print(coinChange(coins, amount))
```
#### TC: O(S*n) **SC:** O(S)

The code uses dynamic programming to solve the Coin Change problem efficiently. It initializes a DP
array to store the minimum number of coins needed to make up each amount from 0 to the target
amount. By iterating through each coin denomination and updating the DP array, the code calculates
the minimum number of coins required for each amount. Finally, it returns the result for the target
amount. The time complexity is O(S*n) where S is the amount and n is the number of coins, and the
space complexity is O(S) where S is the amount.

---
---
---
 --- 
# House Robber
>Determine the maximum amount of money you can rob without robbing two adjacent houses.

>Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2) and then rob house 3 (money = 9) and then rob house 5 (money = 1). Total amount you can rob = 2 + 9 + 1 = 12.
- https://leetcode.com/problems/house-robber/
- Difficulty: 40
- Frequent: 85
- Tags: ['Dynamic Programming']

### House Robber -- Shortest Solution:
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        prev, curr = 0, 0
        for num in nums:
            prev, curr = curr, max(curr, prev + num)
        return curr
```
#### TC: O(n) **SC:** O(1)

The solution uses a dynamic programming approach to keep track of the maximum amount of money that
can be robbed up to the current house without alerting the police. It maintains two variables,
`prev` and `curr`, which represent the maximum amounts robbed up to the previous house and the
current house, respectively. For each house, it updates these variables to reflect the maximum
amount that can be robbed either by skipping the current house or by robbing it. This approach
ensures that the solution is both time and space efficient.

---
---
---
 --- 
# Minimum Path Sum
>Find the path from top-left to bottom-right of a grid with the minimum sum of values.

>Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: The path 1→3→1→1→1 minimizes the sum.
- https://leetcode.com/problems/minimum-path-sum/
- Difficulty: 45
- Frequent: 85
- Tags: ['Dynamic Programming']

### Minimum Path Sum -- Shortest Solution:
```python
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    for i in range(1, m):
        grid[i][0] += grid[i-1][0]
    for j in range(1, n):
        grid[0][j] += grid[0][j-1]
    for i in range(1, m):
        for j in range(1, n):
            grid[i][j] += min(grid[i-1][j], grid[i][j-1])
    return grid[-1][-1]
```
#### TC: O(m * n) **SC:** O(1)

The code modifies the input grid to store the minimum path sum at each cell. It first updates the
first column and first row to represent the minimum path sums to reach those cells. Then, it
iterates through the rest of the grid, updating each cell to be the sum of its value and the minimum
of the values from the cell above or to the left. The final cell contains the minimum path sum for
the entire grid.

---
### Minimum Path Sum -- Best TC Solution:
```python
from typing import List

class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        dp = [[0] * n for _ in range(m)]
        dp[0][0] = grid[0][0]
        for i in range(1, m):
            dp[i][0] = dp[i - 1][0] + grid[i][0]
        for j in range(1, n):
            dp[0][j] = dp[0][j - 1] + grid[0][j]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]
        return dp[-1][-1]
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses dynamic programming to find the minimum path sum. It initializes a 2D array 'dp' to
store the minimum path sum at each cell. It iterates through the grid to calculate the minimum path
sum for each cell based on the previous cells. The final result is stored in dp[-1][-1], which
represents the minimum path sum to reach the bottom right cell. The time complexity is O(m*n) where
m is the number of rows and n is the number of columns in the grid. The space complexity is also
O(m*n) to store the dp array.

---
---
---
 --- 
# Unique Paths
>Count the number of unique paths from the top-left corner to the bottom-right corner of a grid.

>Input: m = 3, n = 7
Output: 28
Explanation: There are 28 unique paths from top-left to bottom-right.
- https://leetcode.com/problems/unique-paths/
- Difficulty: 35
- Frequent: 80
- Tags: ['Dynamic Programming']

### Unique Paths -- Shortest Solution:
```python
from math import comb

def uniquePaths(m: int, n: int) -> int:
    return comb(m + n - 2, n - 1)
```
#### TC: O(1) **SC:** O(1)

The problem of finding unique paths in a grid can be reduced to a combinatorial problem. The number
of unique paths from the top-left to the bottom-right of an m x n grid is equivalent to choosing
(m-1) movements down from a total of (m-1) + (n-1) movements (down + right). This can be computed
using the binomial coefficient, which is efficiently calculated using Python's math.comb function.

---
### Unique Paths -- Best TC Solution:
```python
# Dynamic Programming Approach

def uniquePaths(m, n):
    dp = [[1] * n for _ in range(m)]
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    return dp[m - 1][n - 1]

m = 3
n = 7
result = uniquePaths(m, n)
print(result)
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a dynamic programming approach to solve the Unique Paths problem. It creates a 2D
array 'dp' to store the number of unique paths to reach each cell. The value at dp[i][j] represents
the number of unique paths to reach cell (i, j). By iteratively updating the values in the dp array
based on the number of paths from the top and left cells, the final result is obtained at
dp[m-1][n-1]. The time complexity is O(m*n) as we iterate through the entire 2D array, and the space
complexity is also O(m*n) to store the dp array.

---
### Unique Paths -- Best SC Solution:
```python
# Dynamic Programming Approach

def uniquePaths(m, n):
    dp = [[1] * n for _ in range(m)]
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    return dp[-1][-1]

m = 3
n = 7
result = uniquePaths(m, n)
print(result)
```
#### TC: Time Complexity: O(m*n) **SC:** Space Complexity: O(m*n)

The code uses a dynamic programming approach to solve the Unique Paths problem. It creates a 2D
array 'dp' to store the number of unique paths to reach each cell. By iterating through the array
and updating the values based on the sum of paths from the top and left cells, the code efficiently
calculates the total unique paths to reach the bottom-right cell. The time complexity is O(m*n) as
we iterate through an m x n grid, and the space complexity is also O(m*n) to store the DP array.

---
---
---
 --- 
# Longest Increasing Subsequence
>Find the length of the longest increasing subsequence in an array.

>Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
- https://leetcode.com/problems/longest-increasing-subsequence/
- Difficulty: 50
- Frequent: 80
- Tags: ['Dynamic Programming']

### Longest Increasing Subsequence -- Shortest Solution:
```python
from bisect import bisect_left

def lengthOfLIS(nums):
    sub = []
    for x in nums:
        if len(sub) == 0 or x > sub[-1]:
            sub.append(x)
        else:
            sub[bisect_left(sub, x)] = x
    return len(sub)
```
#### TC: O(n log n) **SC:** O(n)

1. **Binary Search with `bisect_left`**: The `bisect_left` function is used to find the position
where the current element should be placed in the `sub` list to maintain the increasing order. 2.
**Dynamic Array `sub`**: The `sub` list is dynamically updated to keep track of the smallest
possible tail value for all increasing subsequences of different lengths. 3. **Efficient Update**:
If the current element is greater than the last element in `sub`, it is appended. Otherwise, it
replaces the first element in `sub` that is greater than or equal to it, ensuring `sub` remains
sorted and optimal. 4. **Result**: The length of the `sub` list at the end of the iteration is the
length of the longest increasing subsequence.

---
### Longest Increasing Subsequence -- Best TC Solution:
```python
# Dynamic Programming Approach

def lengthOfLIS(nums):
    if not nums:
        return 0
    dp = [1] * len(nums)
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp)

# Example
nums = [10, 9, 2, 5, 3, 7, 101, 18]
print(lengthOfLIS(nums))  # Output: 4
```
#### TC: O(n^2) **SC:** O(n)

The code uses a dynamic programming approach to find the length of the longest increasing
subsequence. It iterates through the input array and maintains a dynamic programming array to store
the length of the longest increasing subsequence ending at each index. By comparing each element
with previous elements, it updates the dp array accordingly. The final result is the maximum value
in the dp array, which represents the length of the longest increasing subsequence. The time
complexity is O(n^2) due to the nested loops, and the space complexity is O(n) for the dp array.

---
---
---
 --- 
# Decode Ways
>Count the number of ways to decode a given string of digits.

>Input: s = "226"
Output: 3
Explanation: "226" can be decoded as "BZ" (2 26), "VF" (22 6), and "BBF" (2 2 6).
- https://leetcode.com/problems/decode-ways/
- Difficulty: 55
- Frequent: 80
- Tags: ['Dynamic Programming']

### Decode Ways -- Shortest Solution:
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if not s or s[0] == '0':
            return 0
        n = len(s)
        dp = [0] * (n + 1)
        dp[0], dp[1] = 1, 1
        for i in range(2, n + 1):
            if s[i-1] != '0':
                dp[i] += dp[i-1]
            if 10 <= int(s[i-2:i]) <= 26:
                dp[i] += dp[i-2]
        return dp[n]
```
#### TC: O(n) **SC:** O(n)

The solution uses dynamic programming to keep track of the number of ways to decode the string up to
each character. The `dp` array is used where `dp[i]` represents the number of ways to decode the
substring `s[:i]`. The algorithm iterates through the string and updates the `dp` array based on
whether the current character and the previous character can form valid decodings.

---
### Decode Ways -- Best TC Solution:
```python
# Dynamic Programming Approach

def numDecodings(s: str) -> int:
    if not s or s[0] == '0':
        return 0
    n = len(s)
    dp = [0] * (n + 1)
    dp[0], dp[1] = 1, 1
    for i in range(2, n + 1):
        if 1 <= int(s[i - 1]) <= 9:
            dp[i] += dp[i - 1]
        if 10 <= int(s[i - 2:i]) <= 26:
            dp[i] += dp[i - 2]
    return dp[n]

# Example
s = "226"
print(numDecodings(s))  # Output: 3
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(n)

The code uses a dynamic programming approach to solve the Decode Ways problem efficiently. It
iterates through the input string and calculates the number of ways to decode the string based on
the previous results. The use of a dynamic programming array 'dp' helps in storing and reusing the
subproblem solutions, leading to an optimal solution. The time complexity is O(n) as it iterates
through the input string once, and the space complexity is O(n) to store the dynamic programming
array.

---
---
---
 --- 
# Word Break
>Determine if a string can be segmented into a space-separated sequence of dictionary words.

>Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
- https://leetcode.com/problems/word-break/
- Difficulty: 50
- Frequent: 75
- Tags: ['Dynamic Programming']

### Word Break -- Shortest Solution:
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [False] * (len(s) + 1)
        dp[0] = True
        wordSet = set(wordDict)
        for i in range(1, len(s) + 1):
            for j in range(i):
                if dp[j] and s[j:i] in wordSet:
                    dp[i] = True
                    break
        return dp[-1]
```
#### TC: O(n^2) **SC:** O(n)

The solution uses dynamic programming to determine if the string `s` can be segmented into a space-
separated sequence of one or more dictionary words. The `dp` array keeps track of whether a
substring can be segmented. The outer loop iterates over the length of the string, and the inner
loop checks all possible substrings ending at the current position. If a valid segmentation is
found, the `dp` value is updated.

---
---
---
 --- 
# Longest Common Subsequence
>Find the length of the longest common subsequence between two strings.

>Input: text1 = "abcde", text2 = "ace"
Output: 3
Explanation: The longest common subsequence is "ace" and its length is 3.
- https://leetcode.com/problems/longest-common-subsequence/
- Difficulty: 55
- Frequent: 75
- Tags: ['Dynamic Programming']

### Longest Common Subsequence -- Shortest Solution:
```python
def longestCommonSubsequence(text1, text2):
    dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
    for i in range(1, len(text1) + 1):
        for j in range(1, len(text2) + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[-1][-1]
```
#### TC: O(m * n) **SC:** O(m * n)

The code uses a 2D list `dp` to store the lengths of the longest common subsequence at each point.
It iterates through each character of `text1` and `text2`, updating the `dp` table based on whether
the characters match or not. The final value in `dp` represents the length of the longest common
subsequence.

---
### Longest Common Subsequence -- Best TC Solution:
```python
# Dynamic Programming Approach

def longestCommonSubsequence(text1: str, text2: str) -> int:
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[m][n]

# Example
text1 = "abcde"
text2 = "ace"
print(longestCommonSubsequence(text1, text2) # Output: 3
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a dynamic programming approach to find the longest common subsequence between two
strings. It creates a 2D array 'dp' to store the lengths of the common subsequences. By iterating
through the strings and updating the values in the 'dp' array based on matching characters, it
efficiently calculates the length of the longest common subsequence. The time complexity is O(m*n)
where m and n are the lengths of the input strings, and the space complexity is also O(m*n) due to
the 2D array used for dynamic programming.

---
---
---
 --- 
# Edit Distance
>Find the minimum number of operations required to convert one word to another.

>Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: horse -> rorse (replace 'h' with 'r') -> rose (remove 'r') -> ros (remove 'e')
- https://leetcode.com/problems/edit-distance/
- Difficulty: 65
- Frequent: 75
- Tags: ['Dynamic Programming']

### Edit Distance -- Shortest Solution:
```python
def minDistance(word1, word2):
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        for j in range(n + 1):
            if i == 0:
                dp[i][j] = j
            elif j == 0:
                dp[i][j] = i
            elif word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[m][n]
```
#### TC: O(m * n) **SC:** O(m * n)

The solution uses dynamic programming to build a 2D table `dp` where `dp[i][j]` represents the
minimum edit distance between the first `i` characters of `word1` and the first `j` characters of
`word2`. The table is filled by comparing characters of both words and considering the operations:
insert, delete, and replace. The final answer is found at `dp[m][n]` where `m` and `n` are the
lengths of `word1` and `word2` respectively.

---
### Edit Distance -- Best TC Solution:
```python
# Dynamic Programming Solution

def minDistance(word1: str, word2: str) -> int:
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]) + 1
    return dp[m][n]

# Example
word1 = "horse"
word2 = "ros"
print(minDistance(word1, word2))  # Output: 3
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a dynamic programming approach to solve the Edit Distance problem efficiently. It
creates a 2D array 'dp' to store the minimum number of operations required to convert one word into
another. The time complexity of the solution is O(m*n) where m and n are the lengths of the input
words. The space complexity is also O(m*n) due to the 2D array used for dynamic programming. The
code iterates through the words and fills the 'dp' array based on the matching characters and
previous operations. The final value in 'dp' at position dp[m][n] represents the minimum edit
distance between the two words.

---
### Edit Distance -- Best SC Solution:
```python
# Dynamic Programming Approach

def minDistance(word1: str, word2: str) -> int:
    m = len(word1)
    n = len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    
    return dp[m][n]

# Example
word1 = "horse"
word2 = "ros"
print(minDistance(word1, word2))  # Output: 3
```
#### TC: Time Complexity: O(m*n) **SC:** Space Complexity: O(m*n)

The code uses a dynamic programming approach to solve the Edit Distance problem efficiently. It
creates a 2D array 'dp' to store the minimum number of operations required to convert one word into
another. The code iterates through the words and fills the 'dp' array based on the matching
characters or insertions, deletions, or substitutions needed. The final value in dp[m][n] represents
the minimum edit distance between the two words. The time complexity is O(m*n) where m and n are the
lengths of the input words, and the space complexity is also O(m*n) due to the 2D array 'dp'.

---
---
---
 --- 
# Climbing Stairs
>Count the number of ways to climb a staircase with n steps, taking 1 or 2 steps at a time.

>Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
- https://leetcode.com/problems/climbing-stairs/
- Difficulty: 30
- Frequent: 70
- Tags: ['Dynamic Programming']

### Climbing Stairs -- Shortest Solution:
```python
def climbStairs(n):
    if n <= 2:
        return n
    a, b = 1, 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b
```
#### TC: O(n) **SC:** O(1)

The problem is essentially about finding the nth Fibonacci number, where the first two numbers are 1
and 2. The solution uses two variables to keep track of the last two numbers in the sequence,
updating them iteratively. This approach ensures that the space complexity remains constant (O(1)),
while the time complexity is linear (O(n)) because we only need to iterate through the sequence
once.

---
---
---
 --- 
# Triangle
>Find the minimum path sum from top to bottom in a triangle array.

>Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The minimum path sum from top to bottom is 11 (2 + 3 + 5 + 1 = 11).
- https://leetcode.com/problems/triangle/
- Difficulty: 50
- Frequent: 70
- Tags: ['Dynamic Programming']

### Triangle -- Shortest Solution:
```python
def minimumTotal(triangle):
    if not triangle:
        return 0
    for i in range(len(triangle) - 2, -1, -1):
        for j in range(len(triangle[i])):
            triangle[i][j] += min(triangle[i + 1][j], triangle[i + 1][j + 1])
    return triangle[0][0]
```
#### TC: O(n^2) **SC:** O(1)

The code modifies the input triangle in place, starting from the second to last row and moving
upwards. For each element, it adds the minimum of the two elements directly below it. This way, the
top element of the triangle will eventually contain the minimum path sum.

---
---
---
 --- 
# Partition Equal Subset Sum
>Determine if a given array can be partitioned into two subsets with equal sum.

>Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
- https://leetcode.com/problems/partition-equal-subset-sum/
- Difficulty: 55
- Frequent: 70
- Tags: ['Dynamic Programming']

### Partition Equal Subset Sum -- Shortest Solution:
```python
from functools import lru_cache

def canPartition(nums):
    total = sum(nums)
    if total % 2 != 0:
        return False
    target = total // 2
    
    @lru_cache(None)
    def dfs(i, current_sum):
        if current_sum == target:
            return True
        if i >= len(nums) or current_sum > target:
            return False
        return dfs(i + 1, current_sum + nums[i]) or dfs(i + 1, current_sum)
    
    return dfs(0, 0)
```
#### TC: O(n * sum(nums) / 2) **SC:** O(n * sum(nums) / 2)

1. The problem is essentially about finding a subset of the given list of numbers that sums up to
half of the total sum of the list. 2. If the total sum is odd, it's impossible to partition it into
two equal subsets, so we return False immediately. 3. We use a depth-first search (DFS) approach
with memoization (via `lru_cache`) to explore possible subsets and their sums. 4. The `dfs` function
checks if we can reach the target sum by either including or excluding the current number. 5. The
memoization helps in storing the results of subproblems to avoid redundant calculations, making the
solution efficient.

---
### Partition Equal Subset Sum -- Best TC Solution:
```python
from typing import List

def canPartition(nums: List[int]) -> bool:
    total_sum = sum(nums)
    if total_sum % 2 != 0:
        return False
    target = total_sum // 2
    dp = [False] * (target + 1)
    dp[0] = True
    for num in nums:
        for i in range(target, num - 1, -1):
            dp[i] = dp[i] or dp[i - num]
    return dp[target]

# Example
nums = [1, 5, 11, 5]
print(canPartition(nums))  # Output: True
```
#### TC: O(n * target) **SC:** O(target)

The code uses dynamic programming to solve the problem efficiently. It calculates the total sum of
the input array and checks if it can be divided into two equal subsets. The time complexity is O(n *
target) where n is the number of elements in the input array and target is half of the total sum.
The space complexity is O(target) to store the DP array. The code iterates through the elements and
updates the DP array to determine if a subset with sum equal to target can be formed.

---
---
---
 --- 
# Longest Palindromic Subsequence
>Find the length of the longest palindromic subsequence in a string.

>Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".
- https://leetcode.com/problems/longest-palindromic-subsequence/
- Difficulty: 60
- Frequent: 65
- Tags: ['Dynamic Programming']

### Longest Palindromic Subsequence -- Shortest Solution:
```python
def longestPalindromeSubseq(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n-1, -1, -1):
        dp[i][i] = 1
        for j in range(i+1, n):
            if s[i] == s[j]:
                dp[i][j] = dp[i+1][j-1] + 2
            else:
                dp[i][j] = max(dp[i+1][j], dp[i][j+1])
    return dp[0][n-1]
```
#### TC: O(n^2) **SC:** O(n^2)

The code uses dynamic programming to find the longest palindromic subsequence in a given string. It
initializes a 2D list `dp` where `dp[i][j]` represents the length of the longest palindromic
subsequence in the substring `s[i:j+1]`. The code iterates over the string in reverse order to fill
the `dp` table. If the characters at positions `i` and `j` are the same, it adds 2 to the value of
`dp[i+1][j-1]`. Otherwise, it takes the maximum value between `dp[i+1][j]` and `dp[i][j+1]`. The
final result is stored in `dp[0][n-1]`, which represents the length of the longest palindromic
subsequence in the entire string.

---
### Longest Palindromic Subsequence -- Best TC Solution:
```python
# Dynamic Programming Approach

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

The code uses a dynamic programming approach to find the length of the longest palindromic
subsequence in a given string. It initializes a 2D array 'dp' to store the lengths of palindromic
subsequences. The code iterates through the string 's' and fills the 'dp' array based on whether the
characters at the current indices match or not. By considering all possible substrings, it
calculates the length of the longest palindromic subsequence. The time complexity of the code is
O(n^2) due to the nested loops, and the space complexity is also O(n^2) to store the DP array.

---
---
---
 --- 
# Regular Expression Matching
>Implement regular expression matching with support for '.' and '*'.

>Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
- https://leetcode.com/problems/regular-expression-matching/
- Difficulty: 70
- Frequent: 65
- Tags: ['Dynamic Programming']

### Regular Expression Matching -- Shortest Solution:
```python
import re
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        return re.fullmatch(p, s) is not None
```
#### TC: O(n) **SC:** O(1)

The solution leverages Python's built-in `re` module to perform regular expression matching. The
`re.fullmatch` function checks if the entire string `s` matches the pattern `p`. This approach is
both concise and efficient for this problem.

---
### Regular Expression Matching -- Best TC Solution:
```python
# Dynamic Programming Approach

def isMatch(s: str, p: str) -> bool:
    dp = [[False] * (len(p) + 1) for _ in range(len(s) + 1)]
    dp[0][0] = True
    for i in range(2, len(p) + 1):
        if p[i - 1] == '*':
            dp[0][i] = dp[0][i - 2]
    for i in range(1, len(s) + 1):
        for j in range(1, len(p) + 1):
            if p[j - 1] == '.' or p[j - 1] == s[i - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            elif p[j - 1] == '*':
                dp[i][j] = dp[i][j - 2] or (dp[i - 1][j] and (s[i - 1] == p[j - 2] or p[j - 2] == '.'))
    return dp[len(s)][len(p)]

# Test the function
s = "aa"
p = "a*"
print(isMatch(s, p))  # Output: True
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a dynamic programming approach to solve the Regular Expression Matching problem
efficiently. It creates a 2D DP table to store the matching status between substrings of the input
strings. The time complexity of the code is O(m*n) where m is the length of string s and n is the
length of string p. The space complexity is also O(m*n) to store the DP table. The code handles
cases where '.' matches any single character and '*' matches zero or more of the preceding element.
Overall, the code is clean, smart, and easy to understand, making it an optimal solution for the
problem.

---
---
---
 --- 
# Palindrome Partitioning II
>Find the minimum cuts needed for a palindrome partitioning of a string.

>Input: s = "aab"
Output: 1
Explanation: The palindrome partitioning is ["aa","b"].
- https://leetcode.com/problems/palindrome-partitioning-ii/
- Difficulty: 65
- Frequent: 60
- Tags: ['Dynamic Programming']

### Palindrome Partitioning II -- Shortest Solution:
```python
s = input()
n = len(s)
cut = [x for x in range(-1, n)]
for i in range(n):
    for j in range(i, n):
        if s[i:j+1] == s[i:j+1][::-1]:
            cut[j+1] = min(cut[j+1], cut[i]+1)
print(cut[-1])
```
#### TC: O(n^2) **SC:** O(n)

1. The `cut` array is initialized to store the maximum cuts needed for each prefix of the string. 2.
The outer loop iterates over each character in the string, while the inner loop checks for
palindromes starting from the current character. 3. If a palindrome is found, the minimum cuts
needed to partition the string up to the end of the palindrome are updated. 4. The final value in
the `cut` array represents the minimum cuts needed for the entire string.

---
### Palindrome Partitioning II -- Best SC Solution:
```python
# Dynamic Programming Approach

def minCut(s: str) -> int:
    n = len(s)
    dp = [i - 1 for i in range(n + 1)]
    for i in range(n):
        for j in range(i, n):
            if s[i:j + 1] == s[i:j + 1][::-1]:
                dp[j + 1] = min(dp[j + 1], dp[i] + 1)
    return dp[n]

# Example
s = "aab"
print(minCut(s) # Output: 1
```
#### TC: Time Complexity: O(n^2) **SC:** Space Complexity: O(n)

The code uses dynamic programming to find the minimum number of cuts needed to partition a string
into palindromes. It initializes a DP array to store the minimum cuts at each index. By iterating
through the string and checking if substrings are palindromes, it updates the DP array with the
minimum cuts required. The final element in the DP array represents the minimum cuts needed for the
entire string. The time complexity is O(n^2) due to the nested loops, and the space complexity is
O(n) for the DP array.

---
---
---
 --- 
# Wildcard Matching
>Implement wildcard pattern matching with support for '?' and '*'.

>Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
- https://leetcode.com/problems/wildcard-matching/
- Difficulty: 65
- Frequent: 60
- Tags: ['Dynamic Programming']

### Wildcard Matching -- Shortest Solution:
```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        dp = [[False] * (len(p) + 1) for _ in range(len(s) + 1)]
        dp[0][0] = True
        for j in range(1, len(p) + 1):
            if p[j - 1] == '*':
                dp[0][j] = dp[0][j - 1]
        for i in range(1, len(s) + 1):
            for j in range(1, len(p) + 1):
                if p[j - 1] == '*':
                    dp[i][j] = dp[i - 1][j] or dp[i][j - 1]
                else:
                    dp[i][j] = dp[i - 1][j - 1] and (s[i - 1] == p[j - 1] or p[j - 1] == '?')
        return dp[-1][-1]
```
#### TC: O(m * n) **SC:** O(m * n)

The solution uses dynamic programming to solve the problem. The `dp` table is created where
`dp[i][j]` represents if the first `i` characters in `s` can match the first `j` characters in `p`.
The table is filled based on the characters in `s` and `p`, considering the wildcard characters `*`
and `?`. The final answer is found in `dp[len(s)][len(p)]`.

---
### Wildcard Matching -- Best TC Solution:
```python
# Dynamic Programming Approach

def isMatch(s: str, p: str) -> bool:
    m, n = len(s), len(p)
    dp = [[False] * (n + 1) for _ in range(m + 1)]
    dp[0][0] = True
    for j in range(1, n + 1):
        if p[j - 1] == '*':
            dp[0][j] = dp[0][j - 1]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if p[j - 1] == '*':
                dp[i][j] = dp[i - 1][j] or dp[i][j - 1]
            elif p[j - 1] == '?' or s[i - 1] == p[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
    return dp[m][n]

# Example
s = "adceb"
p = "*a*b"
print(isMatch(s, p))  # Output: True
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a dynamic programming approach to solve the Wildcard Matching problem efficiently. It
creates a 2D DP array to store the matching status between substrings of s and p. The time
complexity of the solution is O(m*n) where m is the length of string s and n is the length of string
p. The space complexity is also O(m*n) due to the DP array. The code handles wildcard characters '*'
and '?' appropriately to determine if the strings match. Overall, the code is clean, easy to
understand, and provides an optimal solution for the problem.

---
---
---
 --- 
# Burst Balloons
>Find the maximum coins you can collect by bursting balloons wisely.

>Input: nums = [3,1,5,8]
Output: 167
Explanation: Burst balloons in the order [1, 5, 3, 8] for maximum coins.
- https://leetcode.com/problems/burst-balloons/
- Difficulty: 70
- Frequent: 60
- Tags: ['Dynamic Programming']

### Burst Balloons -- Shortest Solution:
```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + nums + [1]
        n = len(nums)
        dp = [[0] * n for _ in range(n)]
        for length in range(2, n):
            for left in range(n - length):
                right = left + length
                for i in range(left + 1, right):
                    dp[left][right] = max(dp[left][right], nums[left] * nums[i] * nums[right] + dp[left][i] + dp[i][right])
        return dp[0][n - 1]
```
#### TC: O(n^3) **SC:** O(n^2)

The solution uses dynamic programming to solve the problem. The idea is to consider the problem as
bursting balloons in an optimal order. By adding two virtual balloons with value 1 at both ends of
the array, we can simplify the problem. The `dp` table is used to store the maximum coins that can
be obtained for each subarray. The outer loop iterates over the length of the subarray, and the
inner loops iterate over the possible positions to burst the balloons. The state transition is
defined by considering the coins obtained by bursting the current balloon and adding the results
from the subproblems.

---
---
---
 --- 
# Interleaving String
>Determine if a string is an interleaving of two other strings.

>Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
Explanation: s3 can be formed by interleaving s1 and s2.
- https://leetcode.com/problems/interleaving-string/
- Difficulty: 65
- Frequent: 55
- Tags: ['Dynamic Programming']

### Interleaving String -- Shortest Solution:
```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        if len(s1) + len(s2) != len(s3):
            return False
        dp = [[False] * (len(s2) + 1) for _ in range(len(s1) + 1)]
        dp[0][0] = True
        for i in range(len(s1) + 1):
            for j in range(len(s2) + 1):
                if i > 0:
                    dp[i][j] = dp[i][j] or (dp[i-1][j] and s1[i-1] == s3[i+j-1])
                if j > 0:
                    dp[i][j] = dp[i][j] or (dp[i][j-1] and s2[j-1] == s3[i+j-1])
        return dp[-1][-1]
```
#### TC: O(n * m) **SC:** O(n * m)

The solution uses dynamic programming to determine if s3 is an interleaving of s1 and s2. The dp
table is used to store the results of subproblems, where dp[i][j] represents whether s3[:i+j] is an
interleaving of s1[:i] and s2[:j]. The table is filled by checking characters of s1 and s2 against
s3 and updating the table based on previous results.

---
### Interleaving String -- Best TC Solution:
```python
# Dynamic Programming Solution

def isInterleave(s1: str, s2: str, s3: str) -> bool:
    if len(s1) + len(s2) != len(s3):
        return False
    dp = [[False] * (len(s2) + 1) for _ in range(len(s1) + 1)]
    dp[0][0] = True
    for i in range(1, len(s1) + 1):
        dp[i][0] = dp[i - 1][0] and s1[i - 1] == s3[i - 1]
    for j in range(1, len(s2) + 1):
        dp[0][j] = dp[0][j - 1] and s2[j - 1] == s3[j - 1]
    for i in range(1, len(s1) + 1):
        for j in range(1, len(s2) + 1):
            dp[i][j] = (dp[i - 1][j] and s1[i - 1] == s3[i + j - 1]) or (dp[i][j - 1] and s2[j - 1] == s3[i + j - 1])
    return dp[-1][-1]

# Example
s1 = "aabcc"
s2 = "dbbca"
s3 = "aadbbcbcac"
print(isInterleave(s1, s2, s3))  # Output: True
```
#### TC: Time Complexity: O(m*n) **SC:** Space Complexity: O(m*n)

The code uses a dynamic programming approach to solve the problem efficiently. It creates a 2D DP
array to store the intermediate results. The time complexity is O(m*n) where m is the length of s1
and n is the length of s2. The space complexity is also O(m*n) due to the DP array. The code checks
if the interleaving of s1 and s2 can form s3 by iterating through the characters of s1, s2, and s3.

---
### Interleaving String -- Best SC Solution:
```python
# Dynamic Programming Approach

def isInterleave(s1: str, s2: str, s3: str) -> bool:
    if len(s1) + len(s2) != len(s3):
        return False
    dp = [False] * (len(s2) + 1)
    dp[0] = True
    for i in range(1, len(dp)):
        dp[i] = dp[i - 1] and s2[i - 1] == s3[i - 1]
    for i in range(1, len(s1) + 1):
        dp[0] = dp[0] and s1[i - 1] == s3[i - 1]
        for j in range(1, len(dp)):
            dp[j] = (dp[j] and s1[i - 1] == s3[i + j - 1]) or (dp[j - 1] and s2[j - 1] == s3[i + j - 1])
    return dp[-1]
```
#### TC: O(m*n) **SC:** O(n)

The code uses a dynamic programming approach to solve the problem of interleaving strings. It
creates a 1D DP array to store the intermediate results. The time complexity is O(m*n) where m and n
are the lengths of the input strings, and the space complexity is O(n) where n is the length of the
second string. The code efficiently checks if the third string can be formed by interleaving the
first and second strings.

---
---
---
 --- 
# Scramble String
>Determine if one string is a scrambled version of another string.

>Input: s1 = "great", s2 = "rgeat"
Output: true
Explanation: s1 can be transformed into s2 by swapping the substrings "gr" and "eat".
- https://leetcode.com/problems/scramble-string/
- Difficulty: 70
- Frequent: 50
- Tags: ['Dynamic Programming']

### Scramble String -- Shortest Solution:
```python
from functools import lru_cache

@lru_cache(None)
def isScramble(s1: str, s2: str) -> bool:
    if len(s1) != len(s2) or sorted(s1) != sorted(s2):
        return False
    if s1 == s2:
        return True
    n = len(s1)
    for i in range(1, n):
        if (isScramble(s1[:i], s2[:i]) and isScramble(s1[i:], s2[i:])) or \
           (isScramble(s1[:i], s2[-i:]) and isScramble(s1[i:], s2[:-i])):
            return True
    return False
```
#### TC: O(N^4) **SC:** O(N^3)

1. The function uses memoization to store previously computed results, which helps in reducing
redundant calculations. 2. The base cases check if the strings are of different lengths or if their
sorted characters do not match, in which case they cannot be scrambled versions of each other. 3.
The function recursively checks all possible partitions of the strings to see if any valid scramble
configuration exists. 4. The use of `lru_cache` helps in optimizing the recursive calls by caching
the results of previously computed states.

---
### Scramble String -- Best TC Solution:
```python
# Dynamic Programming Approach

def isScramble(s1: str, s2: str) -> bool:
    if s1 == s2:
        return True
    if len(s1) != len(s2) or sorted(s1) != sorted(s2):
        return False
    n = len(s1)
    dp = [[[False] * n for _ in range(n)] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            dp[0][i][j] = s1[i] == s2[j]
    for l in range(2, n + 1):
        for i in range(n - l + 1):
            for j in range(n - l + 1):
                for k in range(1, l):
                    if (dp[k - 1][i][j] and dp[l - k - 1][i + k][j + k]) or (dp[k - 1][i][j + l - k] and dp[l - k - 1][i + k][j]):
                        dp[l - 1][i][j] = True
                        break
    return dp[n - 1][0][0]
```
#### TC: O(n^4) **SC:** O(n^3)

The code uses a dynamic programming approach to determine if two strings are scrambled versions of
each other. It creates a 3D DP array to store the results of subproblems. By iterating through the
lengths of substrings and checking all possible splits, it efficiently computes the result. The time
complexity is O(n^4) due to the nested loops, and the space complexity is O(n^3) for the DP array.
The code is clean, easy to understand, and provides a smart solution to the problem.

---
---
---