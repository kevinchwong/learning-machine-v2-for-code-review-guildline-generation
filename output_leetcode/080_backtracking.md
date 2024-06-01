# Backtracking
## Frequent score for this algorithmic framework: 80 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Word Search](#word-search) | 70 | 40 |
| 2 | [Sudoku Solver](#sudoku-solver) | 65 | 60 |
| 3 | [Generate Parentheses](#generate-parentheses) | 60 | 40 |
| 4 | [N-Queens](#n-queens) | 60 | 50 |
| 5 | [Word Search II](#word-search-ii) | 60 | 50 |
| 6 | [Letter Combinations of a Phone Number](#letter-combinations-of-a-phone-number) | 55 | 35 |
| 7 | [Combination Sum](#combination-sum) | 55 | 45 |
| 8 | [Partition to K Equal Sum Subsets](#partition-to-k-equal-sum-subsets) | 55 | 55 |
| 9 | [Expression Add Operators](#expression-add-operators) | 55 | 60 |
| 10 | [Permutations](#permutations) | 50 | 35 |
| 11 | [Palindrome Partitioning](#palindrome-partitioning) | 50 | 40 |
| 12 | [Restore IP Addresses](#restore-ip-addresses) | 50 | 40 |
| 13 | [Combination Sum II](#combination-sum-ii) | 50 | 45 |
| 14 | [Beautiful Arrangement](#beautiful-arrangement) | 50 | 45 |
| 15 | [Matchsticks to Square](#matchsticks-to-square) | 50 | 50 |
| 16 | [Remove Invalid Parentheses](#remove-invalid-parentheses) | 50 | 55 |
| 17 | [Subsets](#subsets) | 45 | 30 |
| 18 | [Combinations](#combinations) | 45 | 35 |
| 19 | [Factor Combinations](#factor-combinations) | 45 | 50 |
---
Backtracking is a recursive algorithm used to solve problems by trying to build a solution incrementally. It abandons a solution as soon as it determines that the solution cannot be completed. This method is often used for solving constraint satisfaction problems like puzzles, pathfinding, and combinatorial optimization problems.
```python
def is_safe(board, row, col):
    for i in range(col):
        if board[row][i] == 1:
            return False
    for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    for i, j in zip(range(row, len(board), 1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    return True

def solve_n_queens(board, col):
    if col >= len(board):
        return True
    for i in range(len(board)):
        if is_safe(board, i, col):
            board[i][col] = 1
            if solve_n_queens(board, col + 1):
                return True
            board[i][col] = 0
    return False

def print_board(board):
    for row in board:
        print(" ".join(str(x) for x in row))

n = 4
board = [[0 for _ in range(n)] for _ in range(n)]
if solve_n_queens(board, 0):
    print_board(board)
else:
    print("No solution found")
```
### Insight:
1. Backtracking is useful for problems with a large search space.
2. The provided code solves the N-Queens problem, a classic example of backtracking.
3. The `is_safe` function checks if a queen can be placed on the board without conflicts.
4. The `solve_n_queens` function tries to place queens column by column and backtracks if a placement leads to no solution.
5. The time complexity is factorial because it tries every possible configuration.
6. The space complexity is linear due to the recursion stack.
---
 --- 
# Word Search
>Given a 2D board and a word, find if the word exists in the grid.

>Input: board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = 'ABCCED'
Output: true
- https://leetcode.com/problems/word-search/
- Difficulty: 40
- Frequent: 70
- Tags: ['Backtracking']

### Word Search -- Shortest Solution:
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        def backtrack(i, j, k):
            if k == len(word):
                return True
            if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]) or board[i][j] != word[k]:
                return False
            tmp, board[i][j] = board[i][j], '#'
            res = (backtrack(i+1, j, k+1) or backtrack(i-1, j, k+1) or backtrack(i, j+1, k+1) or backtrack(i, j-1, k+1))
            board[i][j] = tmp
            return res
        for i in range(len(board)):
            for j in range(len(board[0])):
                if backtrack(i, j, 0):
                    return True
        return False
```
#### TC: O(N * 3^L) **SC:** O(L)

The solution uses a backtracking approach to explore all possible paths in the board to find the
given word. The `backtrack` function is called recursively to check each cell and its neighbors. If
the current cell matches the corresponding character in the word, it proceeds to the next character.
The cell is temporarily marked to avoid revisiting. The process continues until the word is found or
all possibilities are exhausted.

---
### Word Search -- Best TC Solution:
```python
from typing import List

class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        def backtrack(row, col, index):
            if index == len(word):
                return True
            if row < 0 or row >= len(board) or col < 0 or col >= len(board[0]) or board[row][col] != word[index]:
                return False
            temp = board[row][col]
            board[row][col] = ''
            found = backtrack(row + 1, col, index + 1) or backtrack(row - 1, col, index + 1) or \
                    backtrack(row, col + 1, index + 1) or backtrack(row, col - 1, index + 1)
            board[row][col] = temp
            return found
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == word[0] and backtrack(i, j, 0):
                    return True
        return False
```
#### TC: O(M * N * 4^L) **SC:** O(L)

The code uses backtracking to explore all possible paths in the board to find the given word. It
checks each cell in the board to see if it matches the first character of the word and then
recursively explores the neighboring cells to find the complete word. The time complexity is O(M * N
* 4^L) where M and N are the dimensions of the board and L is the length of the word. The space
complexity is O(L) where L is the length of the word, representing the recursive call stack depth.

---
---
---
 --- 
# Sudoku Solver
>Write a program to solve a Sudoku puzzle by filling the empty cells.

>Input: board = [['5','3','.','.','7','.','.','.','.'],['6','.','.','1','9','5','.','.','.'],['.','9','8','.','.','.','.','6','.'],['8','.','.','.','6','.','.','.','3'],['4','.','.','8','.','3','.','.','1'],['7','.','.','.','2','.','.','.','6'],['.','6','.','.','.','.','2','8','.'],['.','.','.','4','1','9','.','.','5'],['.','.','.','.','8','.','.','7','9']]
Output: [['5','3','4','6','7','8','9','1','2'],['6','7','2','1','9','5','3','4','8'],['1','9','8','3','4','2','5','6','7'],['8','5','9','7','6','1','4','2','3'],['4','2','6','8','5','3','7','9','1'],['7','1','3','9','2','4','8','5','6'],['9','6','1','5','3','7','2','8','4'],['2','8','7','4','1','9','6','3','5'],['3','4','5','2','8','6','1','7','9']]
- https://leetcode.com/problems/sudoku-solver/
- Difficulty: 60
- Frequent: 65
- Tags: ['Backtracking']

### Sudoku Solver -- Shortest Solution:
```python
def solveSudoku(board):
    def is_valid(board, row, col, num):
        for i in range(9):
            if board[row][i] == num or board[i][col] == num or board[row//3*3 + i//3][col//3*3 + i%3] == num:
                return False
        return True

    def solve(board):
        for row in range(9):
            for col in range(9):
                if board[row][col] == '.':
                    for num in '123456789':
                        if is_valid(board, row, col, num):
                            board[row][col] = num
                            if solve(board):
                                return True
                            board[row][col] = '.'
                    return False
        return True

    solve(board)
```
#### TC: O(9^(n*n)) **SC:** O(n*n)

The solution uses a backtracking approach to fill the Sudoku board. The `is_valid` function checks
if placing a number in a specific cell is valid according to Sudoku rules. The `solve` function
attempts to fill each cell with a valid number and recursively solves the board. If a solution is
found, it returns True; otherwise, it backtracks and tries the next number. The process continues
until the board is completely and correctly filled.

---
---
---
 --- 
# Generate Parentheses
>Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

>Input: n = 3
Output: ['((()))','(()())','(())()','()(())','()()()']
- https://leetcode.com/problems/generate-parentheses/
- Difficulty: 40
- Frequent: 60
- Tags: ['Backtracking']

### Generate Parentheses -- Shortest Solution:
```python
def generateParenthesis(n):
    def backtrack(s='', left=0, right=0):
        if len(s) == 2 * n:
            res.append(s)
            return
        if left < n:
            backtrack(s+'(', left+1, right)
        if right < left:
            backtrack(s+')', left, right+1)
    res = []
    backtrack()
    return res
```
#### TC: O(4^n / √n) **SC:** O(4^n / √n)

The solution uses backtracking to generate all valid combinations of n pairs of parentheses. The
`backtrack` function builds the string `s` by adding '(' or ')' based on the current counts of left
and right parentheses. It ensures that at any point, the number of ')' does not exceed the number of
'('. The base case is when the length of `s` is 2*n, meaning a valid combination is formed.

---
### Generate Parentheses -- Best TC Solution:
```python
from typing import List

def generateParenthesis(n: int) -> List[str]:
    def backtrack(s='', left=0, right=0):
        if len(s) == 2 * n:
            res.append(s)
            return
        if left < n:
            backtrack(s + '(', left + 1, right)
        if right < left:
            backtrack(s + ')', left, right + 1)
    res = []
    backtrack()
    return res

# Example usage
n = 3
print(generateParenthesis(n))
```
#### TC: O(4^n / sqrt(n)) **SC:** O(4^n / sqrt(n))

The code uses backtracking to generate all valid combinations of parentheses. It recursively
explores all possible choices and prunes the invalid ones. The backtrack function keeps track of the
current string, the count of left and right parentheses, and appends the valid combinations to the
result list. The time and space complexity are both O(4^n / sqrt(n)), where n is the given input
value. The code elegantly handles the generation of valid parentheses by ensuring that the number of
left parentheses is always greater than or equal to the number of right parentheses at each step.

---
---
---
 --- 
# N-Queens
>The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

>Input: n = 4
Output: [['.Q..','...Q','Q...','..Q.'],['..Q.','Q...','...Q','.Q..']]
- https://leetcode.com/problems/n-queens/
- Difficulty: 50
- Frequent: 60
- Tags: ['Backtracking']

### N-Queens -- Shortest Solution:
```python
def solveNQueens(n):
    def backtrack(row, diagonals, anti_diagonals, cols, state):
        if row == n:
            result.append(state)
            return
        for col in range(n):
            curr_diag = row - col
            curr_anti_diag = row + col
            if col in cols or curr_diag in diagonals or curr_anti_diag in anti_diagonals:
                continue
            cols.add(col)
            diagonals.add(curr_diag)
            anti_diagonals.add(curr_anti_diag)
            backtrack(row + 1, diagonals, anti_diagonals, cols, state + [col])
            cols.remove(col)
            diagonals.remove(curr_diag)
            anti_diagonals.remove(curr_anti_diag)
    result = []
    backtrack(0, set(), set(), set(), [])
    return [["." * i + "Q" + "." * (n - i - 1) for i in sol] for sol in result]
```
#### TC: O(N!) **SC:** O(N)

The solution uses backtracking to place queens on the board one row at a time. It keeps track of
columns and diagonals that are under attack to ensure no two queens threaten each other. The state
is updated recursively, and valid solutions are added to the result list. The final result is
formatted to represent the board visually.

---
---
---
 --- 
# Word Search II
>Given a 2D board and a list of words from the dictionary, find all words in the board.

>Input: board = [['o','a','a','n'],['e','t','a','e'],['i','h','k','r'],['i','f','l','v']], words = ['oath','pea','eat','rain']
Output: ['eat','oath']
- https://leetcode.com/problems/word-search-ii/
- Difficulty: 50
- Frequent: 60
- Tags: ['Backtracking']

### Word Search II -- Shortest Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
        self.word = None

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
        node.word = word

class Solution:
    def findWords(self, board, words):
        def backtrack(node, x, y):
            char = board[x][y]
            curr_node = node.children[char]
            if curr_node.is_end_of_word:
                result.add(curr_node.word)
            board[x][y] = '#'
            for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                nx, ny = x + dx, y + dy
                if 0 <= nx < len(board) and 0 <= ny < len(board[0]) and board[nx][ny] in curr_node.children:
                    backtrack(curr_node, nx, ny)
            board[x][y] = char

        trie = Trie()
        for word in words:
            trie.insert(word)
        result = set()
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] in trie.root.children:
                    backtrack(trie.root, i, j)
        return list(result)
```
#### TC: O(m * n * 4^l) **SC:** O(k)

1. **Trie Construction**: A Trie (prefix tree) is built from the list of words. Each node in the
Trie represents a character in a word. 2. **Backtracking**: The algorithm uses backtracking to
explore all possible paths in the board. It starts from each cell and explores all 4 possible
directions (up, down, left, right). 3. **Marking Visited Cells**: During the backtracking process,
cells are temporarily marked as visited by changing their value to '#'. This prevents revisiting the
same cell within the same word path. 4. **Word Matching**: When a word is found in the Trie (i.e.,
the end of a word is reached), it is added to the result set. 5. **Result Compilation**: Finally,
the set of found words is converted to a list and returned as the result.

---
### Word Search II -- Best TC Solution:
```python
# TrieNode class for Trie implementation

class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None


class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        def build_trie(words):
            root = TrieNode()
            for word in words:
                node = root
                for char in word:
                    if char not in node.children:
                        node.children[char] = TrieNode()
                    node = node.children[char]
                node.word = word
            return root

        def backtrack(node, i, j):
            char = board[i][j]
            curr_node = node.children.get(char)
            if curr_node and curr_node.word:
                result.append(curr_node.word)
                curr_node.word = None

            board[i][j] = '#'
            for dx, dy in directions:
                x, y = i + dx, j + dy
                if 0 <= x < m and 0 <= y < n and board[x][y] in curr_node.children:
                    backtrack(curr_node, x, y)
            board[i][j] = char

        if not board or not board[0]:
            return []

        m, n = len(board), len(board[0])
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        result = []
        root = build_trie(words)

        for i in range(m):
            for j in range(n):
                if board[i][j] in root.children:
                    backtrack(root, i, j)

        return result
```
#### TC: O(M(4⋅3^(L−1))), where M is the number of cells in the board, L is the maximum length of words, and N is the number of words in the list. **SC:** O(N), where N is the total number of characters in the words list.

The code implements a Trie data structure to efficiently search for words in a 2D board. It uses
backtracking to explore all possible paths starting from each cell on the board. The Trie structure
helps in pruning unnecessary paths by checking if the current path forms a valid word. The time
complexity is optimized by early termination of backtracking when a word is found. The space
complexity is determined by the Trie structure storing the words. Overall, the code is clean, easy
to understand, and provides an efficient solution to the Word Search II problem.

---
### Word Search II -- Best SC Solution:
```python
# TrieNode class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None


class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        def build_trie(words):
            root = TrieNode()
            for word in words:
                node = root
                for char in word:
                    if char not in node.children:
                        node.children[char] = TrieNode()
                    node = node.children[char]
                node.word = word
            return root

        def backtrack(node, i, j):
            char = board[i][j]
            curr_node = node.children.get(char)
            if curr_node and curr_node.word:
                found_words.add(curr_node.word)
            board[i][j] = '#'
            for dx, dy in directions:
                x, y = i + dx, j + dy
                if 0 <= x < m and 0 <= y < n and board[x][y] != '#' and board[x][y] in curr_node.children:
                    backtrack(curr_node, x, y)
            board[i][j] = char

        if not board or not board[0] or not words:
            return []

        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        m, n = len(board), len(board[0])
        found_words = set()
        trie_root = build_trie(words)

        for i in range(m):
            for j in range(n):
                if board[i][j] in trie_root.children:
                    backtrack(trie_root, i, j)

        return list(found_words)
```
#### TC: O(M(4⋅3^(L−1))), where M is the number of cells in the board, N is the number of words, and L is the maximum length of words **SC:** O(N), where N is the total number of letters in all words combined

The code implements a Trie data structure to efficiently search for words in a 2D board. It uses
backtracking to explore all possible paths starting from each cell in the board. The Trie structure
helps in pruning unnecessary paths by checking if the current path matches any word in the
dictionary. The time complexity is determined by the number of cells in the board and the maximum
length of words, while the space complexity is determined by the total number of letters in all
words combined. Overall, the code is clean, efficient, and easy to understand.

---
---
---
 --- 
# Letter Combinations of a Phone Number
>Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

>Input: digits = '23'
Output: ['ad','ae','af','bd','be','bf','cd','ce','cf']
- https://leetcode.com/problems/letter-combinations-of-a-phone-number/
- Difficulty: 35
- Frequent: 55
- Tags: ['Backtracking']

### Letter Combinations of a Phone Number -- Shortest Solution:
```python
from itertools import product

def letterCombinations(digits):
    if not digits:
        return []
    mapping = {'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl', '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'}
    return [''.join(combo) for combo in product(*(mapping[d] for d in digits))]
```
#### TC: O(4^n) **SC:** O(4^n)

1. The code uses the `product` function from the `itertools` module to generate all possible
combinations of letters for the given digits. 2. The `mapping` dictionary maps each digit to its
corresponding letters. 3. The `product` function takes the letters for each digit and computes the
Cartesian product, which gives all possible combinations. 4. The list comprehension constructs the
final list of letter combinations by joining each tuple of letters into a string. 5. The time and
space complexity are both O(4^n), where n is the length of the input digits, because each digit can
map to up to 4 letters.

---
### Letter Combinations of a Phone Number -- Best TC Solution:
```python
from typing import List

class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        phone = {
            '2': 'abc',
            '3': 'def',
            '4': 'ghi',
            '5': 'jkl',
            '6': 'mno',
            '7': 'pqrs',
            '8': 'tuv',
            '9': 'wxyz'
        }
        def backtrack(index, path):
            if index == len(digits):
                combinations.append(''.join(path))
                return
            for char in phone[digits[index]]:
                path.append(char)
                backtrack(index + 1, path)
                path.pop()
        combinations = []
        backtrack(0, [])
        return combinations
```
#### TC: O(3^N * 4^M) **SC:** O(3^N * 4^M)

The code uses backtracking to generate all possible letter combinations of a phone number. It
recursively explores all possible combinations by appending characters from the phone dictionary.
The time complexity is O(3^N * 4^M) where N is the number of digits with 3 letters and M is the
number of digits with 4 letters. The space complexity is also O(3^N * 4^M) due to the recursive
calls and storing the combinations.

---
---
---
 --- 
# Combination Sum
>Find all unique combinations in candidates where the candidate numbers sum to target.

>Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
- https://leetcode.com/problems/combination-sum/
- Difficulty: 45
- Frequent: 55
- Tags: ['Backtracking']

### Combination Sum -- Shortest Solution:
```python
class Solution:
    def combinationSum(self, candidates, target):
        def backtrack(start, path, target):
            if target == 0:
                res.append(path)
                return
            for i in range(start, len(candidates)):
                if candidates[i] > target:
                    continue
                backtrack(i, path + [candidates[i]], target - candidates[i])
        res = []
        candidates.sort()
        backtrack(0, [], target)
        return res
```
#### TC: O(2^t) where t is the target value **SC:** O(t) where t is the target value

The solution uses a backtracking approach to explore all possible combinations of candidates that
sum up to the target. The candidates are first sorted to facilitate early stopping in the loop when
the current candidate exceeds the remaining target. The `backtrack` function is called recursively,
building the path and reducing the target until it reaches zero, at which point the current path is
added to the result list.

---
### Combination Sum -- Best TC Solution:
```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def backtrack(remain, path, start):
            if remain == 0:
                res.append(path[:])
                return
            for i in range(start, len(candidates)):
                if remain - candidates[i] >= 0:
                    path.append(candidates[i])
                    backtrack(remain - candidates[i], path, i)
                    path.pop()
        res = []
        backtrack(target, [], 0)
        return res
```
#### TC: O(N^target) **SC:** O(target)

The code uses backtracking to find all combinations of candidates that sum up to the target. It
explores all possible combinations by recursively trying different candidates. The time complexity
is O(N^target) where N is the number of candidates and target is the target sum. The space
complexity is O(target) to store the path during backtracking.

---
---
---
 --- 
# Partition to K Equal Sum Subsets
>Given an array of integers nums and a positive integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are all equal.

>Input: nums = [4,3,2,3,5,2,1], k = 4
Output: true
- https://leetcode.com/problems/partition-to-k-equal-sum-subsets/
- Difficulty: 55
- Frequent: 55
- Tags: ['Backtracking']

### Partition to K Equal Sum Subsets -- Shortest Solution:
```python
from functools import lru_cache

def canPartitionKSubsets(nums, k):
    total_sum = sum(nums)
    if total_sum % k != 0:
        return False
    target = total_sum // k
    nums.sort(reverse=True)
    if nums[0] > target:
        return False

    @lru_cache(None)
    def search(groups):
        if not nums:
            return all(g == target for g in groups)
        v = nums.pop()
        for i in range(k):
            if groups[i] + v <= target:
                groups[i] += v
                if search(groups):
                    return True
                groups[i] -= v
            if groups[i] == 0:
                break
        nums.append(v)
        return False

    return search(tuple([0] * k))
```
#### TC: O(k^n) **SC:** O(n)

1. The function first checks if the total sum of the array is divisible by k. If not, it's
impossible to partition. 2. It sorts the array in descending order to optimize the backtracking
process. 3. The `search` function uses memoization to store previously computed results, reducing
redundant calculations. 4. The backtracking approach tries to place each number in one of the k
groups and checks if a valid partition is possible. 5. The use of `lru_cache` helps in optimizing
the recursive calls by caching the results.

---
### Partition to K Equal Sum Subsets -- Best TC Solution:
```python
from typing import List

class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        target, rem = divmod(sum(nums), k)
        if rem or max(nums) > target:
            return False
        nums.sort(reverse=True)
        dp = [0] * k
        def dfs(index):
            if index == len(nums):
                return len(set(dp)) == 1
            for i in range(k):
                if dp[i] + nums[index] <= target:
                    dp[i] += nums[index]
                    if dfs(index + 1):
                        return True
                    dp[i] -= nums[index]
                if dp[i] == 0:
                    break
            return False
        return dfs(0)
```
#### TC: O(k * 2^n) **SC:** O(n)

The code uses backtracking to explore all possible combinations of subsets. It optimizes the process
by sorting the input array in descending order and using a target sum to check if a subset can be
formed. The code efficiently tracks the sum of elements in each subset using a dynamic programming
array. The backtracking function recursively tries to assign each element to a subset until all
elements are processed. If a valid partition is found, it returns True. The time complexity is O(k *
2^n) where k is the number of subsets and n is the number of elements in the input array. The space
complexity is O(n) for the dynamic programming array.

---
---
---
 --- 
# Expression Add Operators
>Given a string num that contains only digits and an integer target, return all possibilities to add the binary operators '+', '-', or '*' between the digits of num so that the resultant expression evaluates to the target value.

>Input: num = '123', target = 6
Output: ['1+2+3','1*2*3']
- https://leetcode.com/problems/expression-add-operators/
- Difficulty: 60
- Frequent: 55
- Tags: ['Backtracking']

### Expression Add Operators -- Shortest Solution:
```python
class Solution:
    def addOperators(self, num: str, target: int) -> List[str]:
        def backtrack(index, path, value, last):
            if index == len(num):
                if value == target:
                    result.append(path)
                return
            for i in range(index + 1, len(num) + 1):
                temp = num[index:i]
                n = int(temp)
                if index == 0:
                    backtrack(i, temp, n, n)
                else:
                    backtrack(i, path + '+' + temp, value + n, n)
                    backtrack(i, path + '-' + temp, value - n, -n)
                    backtrack(i, path + '*' + temp, value - last + last * n, last * n)
                if num[index] == '0':
                    break
        result = []
        backtrack(0, '', 0, 0)
        return result
```
#### TC: O(4^n) **SC:** O(n)

The solution uses backtracking to explore all possible ways to insert operators between the digits
of the input string `num`. The `backtrack` function is called recursively to build expressions and
evaluate their values. If the current expression evaluates to the target value when all digits are
used, it is added to the result list. The function handles addition, subtraction, and multiplication
by keeping track of the current value and the last operand used. The check `if num[index] == '0'`
ensures that numbers with leading zeros are not considered, except for the number '0' itself.

---
### Expression Add Operators -- Best SC Solution:
```python
def addOperators(self, num: str, target: int) -> List[str]:
        def backtrack(index, path, value, prev):
            if index == len(num):
                if value == target:
                    res.append(path)
                return
            for i in range(index + 1, len(num) + 1):
                if i == index + 1 or (i > index + 1 and num[index] != '0'):
                    cur = int(num[index:i])
                    if index == 0:
                        backtrack(i, str(cur), cur, cur)
                    else:
                        backtrack(i, path + '+' + str(cur), value + cur, cur)
                        backtrack(i, path + '-' + str(cur), value - cur, -cur)
                        backtrack(i, path + '*' + str(cur), value - prev + prev * cur, prev * cur)
        res = []
        backtrack(0, '', 0, 0)
        return res
```
#### TC: O(4^N) **SC:** O(N)

The code uses backtracking to explore all possible combinations of operators (+, -, *) inserted
between the digits of the input number. It recursively builds expressions and evaluates them to
check if they equal the target value. The backtracking function tracks the current index, current
path, current value, and the previous operand. By exploring all possible combinations, it generates
valid expressions that evaluate to the target value. The time complexity is O(4^N) because each
digit can have 4 choices (+, -, *, or no operator). The space complexity is O(N) to store the
current path during backtracking.

---
---
---
 --- 
# Permutations
>Given an array nums of distinct integers, return all possible permutations.

>Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
- https://leetcode.com/problems/permutations/
- Difficulty: 35
- Frequent: 50
- Tags: ['Backtracking']

### Permutations -- Shortest Solution:
```python
from itertools import permutations

def permute(nums):
    return list(permutations(nums))
```
#### TC: O(n * n!) **SC:** O(n * n!)

The code leverages Python's built-in `itertools.permutations` function to generate all possible
permutations of the input list `nums`. This approach is both concise and efficient for generating
permutations. The time and space complexity are both O(n * n!) because there are n! permutations,
and each permutation takes O(n) space.

---
### Permutations -- Best TC Solution:
```python
from typing import List

def permute(nums: List[int]) -> List[List[int]]:
    def backtrack(start):
        if start == len(nums):
            res.append(nums[:])
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]
            backtrack(start + 1)
            nums[start], nums[i] = nums[i], nums[start]
    res = []
    backtrack(0)
    return res

# Example
nums = [1, 2, 3]
print(permute(nums))
```
#### TC: O(N * N!) **SC:** O(N * N!)

The code uses backtracking to generate all possible permutations of the input array. It swaps
elements to create different permutations and backtracks to explore all possibilities. The time
complexity is O(N * N!) where N is the number of elements in the input array. The space complexity
is also O(N * N!) due to the storage of all permutations. This approach efficiently generates all
permutations without duplicates.

---
---
---
 --- 
# Palindrome Partitioning
>Given a string s, partition s such that every substring of the partition is a palindrome.

>Input: s = 'aab'
Output: [['a','a','b'],['aa','b']]
- https://leetcode.com/problems/palindrome-partitioning/
- Difficulty: 40
- Frequent: 50
- Tags: ['Backtracking']

### Palindrome Partitioning -- Shortest Solution:
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        def is_palindrome(sub):
            return sub == sub[::-1]
        def backtrack(start, path):
            if start == len(s):
                res.append(path)
                return
            for end in range(start + 1, len(s) + 1):
                if is_palindrome(s[start:end]):
                    backtrack(end, path + [s[start:end]])
        res = []
        backtrack(0, [])
        return res
```
#### TC: O(N * 2^N) **SC:** O(N)

1. **Palindrome Check**: The `is_palindrome` function checks if a substring is a palindrome by
comparing it to its reverse. 2. **Backtracking**: The `backtrack` function explores all possible
partitions starting from a given index. If the current substring is a palindrome, it recursively
partitions the remaining string. 3. **Base Case**: When the start index reaches the end of the
string, the current partition path is added to the result list. 4. **Result Storage**: The `res`
list stores all valid palindrome partitions, which is returned at the end.

---
### Palindrome Partitioning -- Best TC Solution:
```python
from typing import List

class Solution:
    def partition(self, s: str) -> List[List[str]]:
        def is_palindrome(s):
            return s == s[::-1]
        
        def backtrack(start, path):
            if start == len(s):
                res.append(path[:])
                return
            
            for i in range(start, len(s)):
                if is_palindrome(s[start:i+1]):
                    path.append(s[start:i+1])
                    backtrack(i+1, path)
                    path.pop()
        
        res = []
        backtrack(0, [])
        return res
```
#### TC: O(n * 2^n) **SC:** O(n)

The code uses backtracking to generate all possible palindrome partitions of the input string. It
defines a helper function to check if a string is a palindrome. The backtrack function recursively
explores all possible partitions by considering each substring starting from the current index. The
time complexity is O(n * 2^n) as there are 2^n possible partitions, and each partition takes O(n)
time to check if it is a palindrome. The space complexity is O(n) to store the current partition
path during backtracking.

---
---
---
 --- 
# Restore IP Addresses
>Given a string s containing only digits, return all possible valid IP addresses that can be obtained from s.

>Input: s = '25525511135'
Output: ['255.255.11.135','255.255.111.35']
- https://leetcode.com/problems/restore-ip-addresses/
- Difficulty: 40
- Frequent: 50
- Tags: ['Backtracking']

### Restore IP Addresses -- Shortest Solution:
```python
def restoreIpAddresses(s):
    def backtrack(start=0, parts=[]):
        if len(parts) == 4:
            if start == len(s):
                result.append(".".join(parts))
            return
        for i in range(1, 4):
            if start + i <= len(s):
                part = s[start:start + i]
                if (part[0] != '0' or i == 1) and int(part) <= 255:
                    backtrack(start + i, parts + [part])
    result = []
    backtrack()
    return result
```
#### TC: O(3^4) **SC:** O(3^4)

The solution uses a backtracking approach to explore all possible ways to split the input string `s`
into four parts, each representing a segment of an IP address. The `backtrack` function is called
recursively, building parts of the IP address and adding valid combinations to the result list. The
constraints ensure that each part is between 1 and 3 digits and does not exceed the value 255. The
base case checks if four parts have been created and if the entire string has been used, in which
case the combination is added to the result.

---
### Restore IP Addresses -- Best TC Solution:
```python
from typing import List

class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        def backtrack(start, path):
            if len(path) == 4 and start == len(s):
                res.append('.'.join(path))
                return
            if len(path) == 4 or start == len(s):
                return
            for i in range(1, 4):
                if start + i > len(s):
                    break
                if i > 1 and s[start] == '0':
                    break
                num = int(s[start:start + i])
                if num <= 255:
                    backtrack(start + i, path + [s[start:start + i]])
        res = []
        backtrack(0, [])
        return res
```
#### TC: O(1) **SC:** O(1)

The code defines a class Solution with a method restoreIpAddresses that takes a string s as input
and returns a list of valid IP addresses. It uses backtracking to explore all possible combinations
of valid IP addresses. The backtrack function recursively builds the IP address by considering each
segment of the address. The code efficiently handles constraints such as the length of the string
and the range of each segment. The time complexity and space complexity are both O(1) as they do not
depend on the input size.

---
---
---
 --- 
# Combination Sum II
>Find all unique combinations in candidates where the candidate numbers sum to target, each number in candidates may only be used once.

>Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: [[1,1,6],[1,2,5],[1,7],[2,6]]
- https://leetcode.com/problems/combination-sum-ii/
- Difficulty: 45
- Frequent: 50
- Tags: ['Backtracking']

### Combination Sum II -- Shortest Solution:
```python
class Solution:
    def combinationSum2(self, candidates, target):
        candidates.sort()
        res = []
        def backtrack(start, path, target):
            if target == 0:
                res.append(path)
                return
            for i in range(start, len(candidates)):
                if i > start and candidates[i] == candidates[i - 1]:
                    continue
                if candidates[i] > target:
                    break
                backtrack(i + 1, path + [candidates[i]], target - candidates[i])
        backtrack(0, [], target)
        return res
```
#### TC: O(2^n) **SC:** O(n)

1. **Sorting**: The candidates are sorted to facilitate the skipping of duplicates. 2.
**Backtracking**: The `backtrack` function is used to explore all possible combinations. 3.
**Pruning**: If a candidate exceeds the target, the loop breaks early to avoid unnecessary work. 4.
**Skipping Duplicates**: The condition `if i > start and candidates[i] == candidates[i - 1]` ensures
that duplicates are skipped, preventing redundant combinations.

---
### Combination Sum II -- Best TC Solution:
```python
from typing import List

class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def backtrack(start, target, path):
            if target == 0:
                res.append(path[:])
                return
            for i in range(start, len(candidates)):
                if i > start and candidates[i] == candidates[i - 1]:
                    continue
                if candidates[i] > target:
                    break
                path.append(candidates[i])
                backtrack(i + 1, target - candidates[i], path)
                path.pop()
        candidates.sort()
        res = []
        backtrack(0, target, [])
        return res
```
#### TC: O(2^N) **SC:** O(N)

The code uses backtracking to find all unique combinations that sum up to the target value. It sorts
the candidates array to handle duplicates efficiently. The backtracking function explores all
possible combinations by considering each candidate element once. The time complexity is O(2^N)
because in the worst case, all combinations are considered. The space complexity is O(N) to store
the path during backtracking.

---
---
---
 --- 
# Beautiful Arrangement
>Suppose you have n integers from 1 to n. We define a beautiful arrangement as an array that is constructed by these n numbers successfully if one of the following is true for the ith position.

>Input: n = 2
Output: 2
- https://leetcode.com/problems/beautiful-arrangement/
- Difficulty: 45
- Frequent: 50
- Tags: ['Backtracking']

### Beautiful Arrangement -- Shortest Solution:
```python
class Solution:
    def countArrangement(self, n: int) -> int:
        def count(i, X):
            if i == 1:
                return 1
            return sum(count(i - 1, X - {x}) for x in X if x % i == 0 or i % x == 0)
        return count(n, set(range(1, n + 1)))
```
#### TC: O(n!) **SC:** O(n)

The solution uses a backtracking approach to count the number of valid permutations. The `count`
function recursively tries to place each number in the set `X` at position `i` if it satisfies the
condition `x % i == 0 or i % x == 0`. The base case is when `i` is 1, which means all positions have
been successfully filled, so it returns 1. The sum of all valid permutations is returned as the
final result.

---
### Beautiful Arrangement -- Best TC Solution:
```python
from typing import List
class Solution:
    def countArrangement(self, n: int) -> int:
        def backtrack(index):
            nonlocal count
            if index == n + 1:
                count += 1
                return
            for i in range(1, n + 1):
                if not visited[i] and (i % index == 0 or index % i == 0):
                    visited[i] = True
                    backtrack(index + 1)
                    visited[i] = False
        count = 0
        visited = [False] * (n + 1)
        backtrack(1)
        return count
```
#### TC: O(k), where k is the number of valid permutations **SC:** O(n)

The code uses backtracking to find all valid permutations of numbers from 1 to n. It increments the
count whenever a valid arrangement is found. The backtracking function checks if a number can be
placed at a certain index based on the given conditions. The time complexity is O(k) where k is the
number of valid permutations, and the space complexity is O(n) for the visited array.

---
---
---
 --- 
# Matchsticks to Square
>You are given an integer array matchsticks where matchsticks[i] is the length of the ith matchstick. You want to use all the matchsticks to make one square.

>Input: matchsticks = [1,1,2,2,2]
Output: true
- https://leetcode.com/problems/matchsticks-to-square/
- Difficulty: 50
- Frequent: 50
- Tags: ['Backtracking']

### Matchsticks to Square -- Shortest Solution:
```python
def makesquare(matchsticks):
    if not matchsticks or sum(matchsticks) % 4 != 0:
        return False
    side_length = sum(matchsticks) // 4
    matchsticks.sort(reverse=True)
    sides = [0] * 4
    def backtrack(index):
        if index == len(matchsticks):
            return len(set(sides)) == 1
        for i in range(4):
            if sides[i] + matchsticks[index] <= side_length:
                sides[i] += matchsticks[index]
                if backtrack(index + 1):
                    return True
                sides[i] -= matchsticks[index]
        return False
    return backtrack(0)
```
#### TC: O(4^N) **SC:** O(N)

1. The problem is to partition the matchsticks into 4 equal sides of a square. 2. First, check if
the total length of matchsticks is divisible by 4. If not, return False. 3. Sort the matchsticks in
descending order to optimize the backtracking process. 4. Use a backtracking function to try to
build each side of the square. 5. The backtracking function attempts to place each matchstick into
one of the 4 sides, ensuring the side length does not exceed the target length. 6. If all
matchsticks are used and all 4 sides are equal, return True.

---
---
---
 --- 
# Remove Invalid Parentheses
>Remove the minimum number of invalid parentheses in order to make the input string valid.

>Input: s = '(a)())()'
Output: ['(a)()()','(a())()']
- https://leetcode.com/problems/remove-invalid-parentheses/
- Difficulty: 55
- Frequent: 50
- Tags: ['Backtracking']

### Remove Invalid Parentheses -- Shortest Solution:
```python
def removeInvalidParentheses(s):
    def isValid(s):
        count = 0
        for char in s:
            if char == '(': count += 1
            if char == ')': count -= 1
            if count < 0: return False
        return count == 0

    level = {s}
    while True:
        valid = list(filter(isValid, level))
        if valid: return valid
        level = {s[:i] + s[i+1:] for s in level for i in range(len(s))}
```
#### TC: O(2^n) **SC:** O(n)

1. The function `isValid` checks if a string has valid parentheses by ensuring the count of '(' and
')' is balanced. 2. The main function uses a BFS approach to generate all possible strings by
removing one character at a time. 3. It filters out the valid strings at each level and returns them
if found. 4. The process continues until valid strings are found, ensuring the minimum number of
removals.

---
### Remove Invalid Parentheses -- Best TC Solution:
```python
# Function to remove invalid parentheses

def removeInvalidParentheses(s):
    def is_valid(s):
        count = 0
        for char in s:
            if char == '(': count += 1
            elif char == ')': count -= 1
            if count < 0: return False
        return count == 0
    
    level = {s}
    while True:
        valid = list(filter(is_valid, level))
        if valid: return valid
        new_level = set()
        for item in level:
            for i in range(len(item)):
                new_level.add(item[:i] + item[i+1:])
        level = new_level
    
# Test the function
input_string = "()())()"
print(removeInvalidParentheses(input_string))
```
#### TC: O(2^N) **SC:** O(N)

The code uses a backtracking approach to remove invalid parentheses from the input string. It
iterates through all possible combinations of removing parentheses and checks if the resulting
string is valid. The function is_valid checks if a string has valid parentheses. The algorithm has a
time complexity of O(2^N) as it explores all possible combinations, and a space complexity of O(N)
to store the intermediate strings during the process.

---
---
---
 --- 
# Subsets
>Given an integer array nums of unique elements, return all possible subsets (the power set).

>Input: nums = [1,2,3]
Output: [[3],[1],[2],[1,2,3],[1,3],[2,3],[1,2],[]]
- https://leetcode.com/problems/subsets/
- Difficulty: 30
- Frequent: 45
- Tags: ['Backtracking']

### Subsets -- Shortest Solution:
```python
class Solution:
    def subsets(self, nums):
        res = []
        def backtrack(start, path):
            res.append(path)
            for i in range(start, len(nums)):
                backtrack(i + 1, path + [nums[i]])
        backtrack(0, [])
        return res
```
#### TC: O(2^n) **SC:** O(2^n)

The code uses a backtracking approach to generate all possible subsets of a given list of numbers.
The `backtrack` function is called recursively, building subsets by including or excluding each
element. The `res` list stores all the subsets. The time and space complexity are both O(2^n)
because each element can either be included or excluded, leading to 2^n possible subsets.

---
### Subsets -- Best TC Solution:
```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def backtrack(start, path):
            res.append(path[:])
            for i in range(start, len(nums)):
                path.append(nums[i])
                backtrack(i + 1, path)
                path.pop()
        res = []
        backtrack(0, [])
        return res

# Example usage
solution = Solution()
nums = [1, 2, 3]
print(solution.subsets(nums))
```
#### TC: O(N * 2^N) **SC:** O(N * 2^N)

The code uses backtracking to generate all possible subsets of the given array. The backtrack
function recursively explores all possible combinations by including or excluding each element. The
time complexity is O(N * 2^N) because there are 2^N subsets and each subset can have up to N
elements. The space complexity is also O(N * 2^N) because we store all subsets in the result list.

---
---
---
 --- 
# Combinations
>Given two integers n and k, return all possible combinations of k numbers out of the range [1, n].

>Input: n = 4, k = 2
Output: [[2,4],[3,4],[2,3],[1,2],[1,3],[1,4]]
- https://leetcode.com/problems/combinations/
- Difficulty: 35
- Frequent: 45
- Tags: ['Backtracking']

### Combinations -- Shortest Solution:
```python
from itertools import combinations

def combine(n, k):
    return list(combinations(range(1, n+1), k))
```
#### TC: O(C(n, k) * k) **SC:** O(C(n, k) * k)

The code uses Python's built-in itertools.combinations to generate all possible combinations of k
numbers out of the range from 1 to n. The function returns these combinations as a list of tuples.
The time and space complexity is determined by the number of combinations, which is C(n, k) = n! /
(k! * (n-k)!), and each combination has a length of k.

---
### Combinations -- Best TC Solution:
```python
from typing import List

class Solution:
    def combine(self, n: int, k: int) -> List[List[int]:
        def backtrack(start, path):
            if len(path) == k:
                res.append(path[:])
                return
            for i in range(start, n + 1):
                path.append(i)
                backtrack(i + 1, path)
                path.pop()
        res = []
        backtrack(1, [])
        return res
```
#### TC: O((n choose k) * k) **SC:** O((n choose k) * k)

The code uses backtracking to generate all possible combinations of k numbers out of the numbers 1
to n. The backtrack function recursively builds the combinations by adding numbers to the path and
backtracking when the path reaches the desired length. The time complexity is O((n choose k) * k) as
there are (n choose k) combinations and each combination has k elements. The space complexity is
also O((n choose k) * k) to store all the combinations.

---
### Combinations -- Best SC Solution:
```python
from typing import List

class Solution:
    def combine(self, n: int, k: int) -> List[List[int]:
        def backtrack(start, path):
            if len(path) == k:
                res.append(path[:])
                return
            for i in range(start, n + 1):
                path.append(i)
                backtrack(i + 1, path)
                path.pop()
        res = []
        backtrack(1, [])
        return res
```
#### TC: O(k * C(n, k)) **SC:** O(C(n, k))

The code uses backtracking to generate all possible combinations of k numbers from 1 to n. The
backtrack function recursively builds the combinations by adding numbers to the path and
backtracking when the path reaches the desired length. The time complexity is O(k * C(n, k)), where
C(n, k) is the number of combinations, and the space complexity is O(C(n, k)) to store all the
combinations.

---
---
---
 --- 
# Factor Combinations
>Write a function that takes an integer n and returns all possible combinations of its factors.

>Input: n = 12
Output: [[2,6],[2,2,3],[3,4]]
- https://leetcode.com/problems/factor-combinations/
- Difficulty: 50
- Frequent: 45
- Tags: ['Backtracking']

### Factor Combinations -- Shortest Solution:
```python
def get_factors(n):
    def backtrack(start, product, path):
        if product == n:
            res.append(path)
            return
        for i in range(start, n // product + 1):
            if n % i == 0:
                backtrack(i, product * i, path + [i])
    res = []
    backtrack(2, 1, [])
    return res
```
#### TC: O(n log n) **SC:** O(log n)

The solution uses a backtracking approach to explore all possible factor combinations. The
`backtrack` function recursively tries to build combinations of factors starting from a given
number. If the product of the current combination equals `n`, it adds the combination to the result
list. The loop ensures that we only consider valid factors and avoid duplicates by starting from the
current factor. The time complexity is O(n log n) due to the nature of factorization, and the space
complexity is O(log n) due to the recursion depth.

---
### Factor Combinations -- Best TC Solution:
```python
from math import sqrt
def getFactors(n):
    def backtrack(start, path, res, n):
        if n == 1:
            if len(path) > 1:
                res.append(path[:])
            return
        for i in range(start, int(sqrt(n)) + 1):
            if n % i == 0:
                path.append(i)
                backtrack(i, path, res, n // i)
                path.pop()
        if n >= start:
            path.append(n)
            backtrack(n, path, res, 1)
            path.pop()
    res = []
    backtrack(2, [], res, n)
    return res

n = 12
print(getFactors(n))
```
#### TC: O(nlogn) **SC:** O(n)

The code uses backtracking to find all possible factor combinations of a given number. It
recursively explores all possible factors by dividing the number and adding factors to the path. The
algorithm ensures that factors are added in ascending order to avoid duplicates. The time complexity
is O(nlogn) due to the recursive nature of the backtracking algorithm, and the space complexity is
O(n) to store the factor combinations.

---
---
---