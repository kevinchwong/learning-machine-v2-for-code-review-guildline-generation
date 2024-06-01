# Depth-First Search (DFS)
## Frequent score for this algorithmic framework: 75 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Number of Islands](#number-of-islands) | 80 | 40 |
| 2 | [Course Schedule](#course-schedule) | 75 | 45 |
| 3 | [Word Ladder](#word-ladder) | 70 | 60 |
| 4 | [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum) | 65 | 60 |
| 5 | [Surrounded Regions](#surrounded-regions) | 60 | 50 |
| 6 | [Path Sum II](#path-sum-ii) | 60 | 50 |
| 7 | [Clone Graph](#clone-graph) | 55 | 40 |
| 8 | [Number of Connected Components in an Undirected Graph](#number-of-connected-components-in-an-undirected-graph) | 55 | 45 |
| 9 | [Graph Valid Tree](#graph-valid-tree) | 50 | 50 |
| 10 | [Longest Increasing Path in a Matrix](#longest-increasing-path-in-a-matrix) | 50 | 55 |
| 11 | [Pacific Atlantic Water Flow](#pacific-atlantic-water-flow) | 45 | 60 |
---
Depth-First Search (DFS) is an algorithm for traversing or searching tree or graph data structures. It starts at the root (or an arbitrary node) and explores as far as possible along each branch before backtracking. This approach uses a stack to remember the next vertex to visit, either via recursion or an explicit stack.
```python
# Recursive DFS

def dfs_recursive(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start)
    for next in graph[start] - visited:
        dfs_recursive(graph, next, visited)
    return visited

# Iterative DFS

def dfs_iterative(graph, start):
    visited, stack = set(), [start]
    while stack:
        vertex = stack.pop()
        if vertex not in visited:
            visited.add(vertex)
            print(vertex)
            stack.extend(graph[vertex] - visited)
    return visited

# Example usage

graph = {'A': {'B', 'C'}, 'B': {'A', 'D', 'E'}, 'C': {'A', 'F'}, 'D': {'B'}, 'E': {'B', 'F'}, 'F': {'C', 'E'}}
print("Recursive DFS:")
dfs_recursive(graph, 'A')
print("\nIterative DFS:")
dfs_iterative(graph, 'A')
```
### Insight:
1. **Recursive vs Iterative**: Recursive DFS is simpler and more elegant but can lead to stack overflow for deep graphs. Iterative DFS using an explicit stack avoids this issue.
2. **Graph Representation**: The graph is represented as an adjacency list, which is efficient for sparse graphs.
3. **Visited Set**: A set is used to keep track of visited nodes to avoid cycles and redundant processing.
4. **Applications**: DFS is useful for pathfinding, topological sorting, and detecting cycles in graphs.
---
 --- 
# Number of Islands
>Count the number of islands in a given grid.

>Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
- https://leetcode.com/problems/number-of-islands/
- Difficulty: 40
- Frequent: 80
- Tags: ['Graph', 'DFS', 'BFS']

### Number of Islands -- Shortest Solution:
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0
        def dfs(i, j):
            if i < 0 or i >= len(grid) or j < 0 or j >= len(grid[0]) or grid[i][j] == '0':
                return
            grid[i][j] = '0'
            dfs(i + 1, j)
            dfs(i - 1, j)
            dfs(i, j + 1)
            dfs(i, j - 1)
        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    dfs(i, j)
                    count += 1
        return count
```
#### TC: O(M * N) **SC:** O(M * N)

The code uses Depth-First Search (DFS) to traverse the grid. When a '1' is found, it triggers a DFS
to mark all connected '1's as '0's, effectively sinking the island. The outer loops ensure every
cell is checked, and the DFS handles the traversal of each island. The time complexity is O(M * N)
because each cell is visited once. The space complexity is also O(M * N) due to the recursion stack
in the worst case.

---
### Number of Islands -- Best TC Solution:
```python
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0
        rows, cols = len(grid), len(grid[0])
        def dfs(i, j):
            if i < 0 or i >= rows or j < 0 or j >= cols or grid[i][j] == '0':
                return
            grid[i][j] = '0'
            dfs(i + 1, j)
            dfs(i - 1, j)
            dfs(i, j + 1)
            dfs(i, j - 1)
        islands = 0
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == '1':
                    islands += 1
                    dfs(i, j)
        return islands
```
#### TC: O(M*N) **SC:** O(M*N)

The code uses Depth-First Search (DFS) to traverse the grid and count the number of islands. It
marks visited land cells as '0' to avoid revisiting them. The time complexity is O(M*N) where M is
the number of rows and N is the number of columns in the grid. The space complexity is also O(M*N)
due to the recursive calls in the DFS function.

---
---
---
 --- 
# Course Schedule
>Determine if you can finish all courses given the prerequisites.

>Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
- https://leetcode.com/problems/course-schedule/
- Difficulty: 45
- Frequent: 75
- Tags: ['Graph', 'DFS', 'BFS']

### Course Schedule -- Shortest Solution:
```python
from collections import defaultdict

def canFinish(numCourses, prerequisites):
    graph = defaultdict(list)
    for course, prereq in prerequisites:
        graph[course].append(prereq)
    visited = [0] * numCourses
    def dfs(course):
        if visited[course] == 1:
            return False
        if visited[course] == 2:
            return True
        visited[course] = 1
        for prereq in graph[course]:
            if not dfs(prereq):
                return False
        visited[course] = 2
        return True
    for course in range(numCourses):
        if not dfs(course):
            return False
    return True
```
#### TC: O(V + E) **SC:** O(V + E)

1. **Graph Representation**: The courses and their prerequisites are represented as a directed graph
using an adjacency list. 2. **DFS for Cycle Detection**: The core of the solution is a DFS function
that detects cycles in the graph. If a cycle is detected, it means it's impossible to complete all
courses. 3. **Visited States**: The `visited` list keeps track of the state of each course: 0
(unvisited), 1 (visiting), and 2 (visited). This helps in detecting cycles efficiently. 4. **Main
Loop**: The main loop iterates through each course and applies the DFS function. If any course leads
to a cycle, the function returns `False`. 5. **Return Value**: If no cycles are detected, the
function returns `True`, indicating that all courses can be completed.

---
---
---
 --- 
# Word Ladder
>Find the length of the shortest transformation sequence from beginWord to endWord.

>Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
- https://leetcode.com/problems/word-ladder/
- Difficulty: 60
- Frequent: 70
- Tags: ['DFS', 'BFS']

### Word Ladder -- Shortest Solution:
```python
from collections import deque

def ladderLength(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return 0
    queue = deque([(beginWord, 1)])
    while queue:
        word, length = queue.popleft()
        if word == endWord:
            return length
        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                next_word = word[:i] + c + word[i+1:]
                if next_word in wordSet:
                    wordSet.remove(next_word)
                    queue.append((next_word, length + 1))
    return 0
```
#### TC: O(M^2 * N) **SC:** O(M * N)

1. **Initialization**: We use a set for the word list for O(1) lookups and a queue for BFS. 2. **BFS
Loop**: We process each word level by level, transforming it one character at a time. 3.
**Transformation**: For each character in the word, we try all possible replacements from 'a' to
'z'. 4. **Early Exit**: If we find the end word, we return the current transformation length. 5.
**Avoid Reprocessing**: Words are removed from the set once processed to avoid cycles. 6.
**Complexity**: The time complexity is O(M^2 * N) where M is the word length and N is the number of
words. The space complexity is O(M * N) due to the queue and set.

---
### Word Ladder -- Best TC Solution:
```python
from collections import deque
def ladderLength(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return 0
    queue = deque([(beginWord, 1)])
    while queue:
        word, length = queue.popleft()
        if word == endWord:
            return length
        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                nextWord = word[:i] + c + word[i + 1:]
                if nextWord in wordSet:
                    wordSet.remove(nextWord)
                    queue.append((nextWord, length + 1))
    return 0
```
#### TC: O(M*N) **SC:** O(M*N)

The code uses a Breadth-First Search (BFS) approach to find the shortest transformation sequence
from the beginWord to the endWord. It iterates through each character of the word and tries all
possible combinations by replacing one character at a time with all lowercase letters. The wordSet
is used to keep track of valid words, and the queue is used to store the current word and its
length. The code efficiently finds the shortest transformation sequence by exploring all possible
valid words in a breadth-first manner.

---
---
---
 --- 
# Binary Tree Maximum Path Sum
>Find the maximum path sum in a binary tree.

>Input: root = [-10,9,20,null,null,15,7]
Output: 42
- https://leetcode.com/problems/binary-tree-maximum-path-sum/
- Difficulty: 60
- Frequent: 65
- Tags: ['Tree', 'DFS']

### Binary Tree Maximum Path Sum -- Shortest Solution:
```python
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        def dfs(node):
            if not node:
                return 0
            left = max(dfs(node.left), 0)
            right = max(dfs(node.right), 0)
            self.max_sum = max(self.max_sum, node.val + left + right)
            return node.val + max(left, right)
        self.max_sum = float('-inf')
        dfs(root)
        return self.max_sum
```
#### TC: O(N) **SC:** O(H)

The solution uses a Depth-First Search (DFS) approach to traverse the tree. For each node, it
calculates the maximum path sum including the node and updates the global maximum path sum. The
function returns the maximum path sum of the subtree rooted at the current node. The use of `max`
ensures that only positive contributions are considered for the path sum, effectively ignoring any
negative paths.

---
---
---
 --- 
# Surrounded Regions
>Capture all regions surrounded by 'X'.

>Input: board = [['X','X','X','X'],['X','O','O','X'],['X','X','O','X'],['X','O','X','X']]
Output: [['X','X','X','X'],['X','X','X','X'],['X','X','X','X'],['X','O','X','X']]
- https://leetcode.com/problems/surrounded-regions/
- Difficulty: 50
- Frequent: 60
- Tags: ['DFS', 'BFS']

### Surrounded Regions -- Shortest Solution:
```python
class Solution:
    def solve(self, board):
        if not board: return
        rows, cols = len(board), len(board[0])
        def dfs(r, c):
            if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != 'O':
                return
            board[r][c] = 'E'
            directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
            for dr, dc in directions:
                dfs(r + dr, c + dc)
        for r in range(rows):
            for c in range(cols):
                if (r in [0, rows-1] or c in [0, cols-1]) and board[r][c] == 'O':
                    dfs(r, c)
        for r in range(rows):
            for c in range(cols):
                if board[r][c] == 'O':
                    board[r][c] = 'X'
                elif board[r][c] == 'E':
                    board[r][c] = 'O'
```
#### TC: O(m * n) **SC:** O(m * n)

The code uses Depth-First Search (DFS) to mark 'O's that are connected to the border and should not
be flipped. It first marks these 'O's as 'E'. Then, it flips all remaining 'O's to 'X' and finally
converts 'E's back to 'O'. This ensures that only the 'O's surrounded by 'X's are flipped.

---
### Surrounded Regions -- Best TC Solution:
```python
from typing import List

class Solution:
    def solve(self, board: List[List[str]]) -> None:
        if not board:
            return
        rows, cols = len(board), len(board[0])
        def dfs(i, j):
            if 0 <= i < rows and 0 <= j < cols and board[i][j] == 'O':
                board[i][j] = 'T'
                dfs(i + 1, j)
                dfs(i - 1, j)
                dfs(i, j + 1)
                dfs(i, j - 1)
        for i in range(rows):
            dfs(i, 0)
            dfs(i, cols - 1)
        for j in range(cols):
            dfs(0, j)
            dfs(rows - 1, j)
        for i in range(rows):
            for j in range(cols):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                elif board[i][j] == 'T':
                    board[i][j] = 'O'
```
#### TC: O(rows * cols) **SC:** O(1)

The code uses Depth-First Search (DFS) to traverse the board and mark 'O's that are not surrounded
by 'X's. It first marks all 'O's connected to the borders as 'T', then iterates through the board to
update 'O's to 'X' and 'T's to 'O'. The time complexity is O(rows * cols) as it visits each cell
once, and the space complexity is O(1) as it does not use any additional data structures.

---
---
---
 --- 
# Path Sum II
>Find all root-to-leaf paths where each path's sum equals the given sum.

>Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
- https://leetcode.com/problems/path-sum-ii/
- Difficulty: 50
- Frequent: 60
- Tags: ['Tree', 'DFS']

### Path Sum II -- Shortest Solution:
```python
class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> List[List[int]]:
        def dfs(node, current_path, current_sum):
            if not node:
                return
            current_path.append(node.val)
            current_sum += node.val
            if not node.left and not node.right and current_sum == targetSum:
                result.append(list(current_path))
            else:
                dfs(node.left, current_path, current_sum)
                dfs(node.right, current_path, current_sum)
            current_path.pop()
        result = []
        dfs(root, [], 0)
        return result
```
#### TC: O(N) **SC:** O(N)

The solution uses Depth-First Search (DFS) to explore all paths from the root to the leaves. It
keeps track of the current path and the sum of the values along that path. When a leaf node is
reached, it checks if the current sum equals the target sum. If it does, the current path is added
to the result list. The use of DFS ensures that all paths are explored, and the backtracking step
(current_path.pop()) ensures that the algorithm can explore new paths without interference from
previously explored paths.

---
---
---
 --- 
# Clone Graph
>Clone a graph given its adjacency list.

>Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
- https://leetcode.com/problems/clone-graph/
- Difficulty: 40
- Frequent: 55
- Tags: ['Graph', 'DFS', 'BFS']

### Clone Graph -- Shortest Solution:
```python
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def cloneGraph(node):
    if not node:
        return node
    old_to_new = {}
    def dfs(node):
        if node in old_to_new:
            return old_to_new[node]
        copy = Node(node.val)
        old_to_new[node] = copy
        for neighbor in node.neighbors:
            copy.neighbors.append(dfs(neighbor))
        return copy
    return dfs(node)
```
#### TC: O(V + E) **SC:** O(V)

1. **Node Class**: The `Node` class is defined to represent each node in the graph. Each node has a
value (`val`) and a list of neighbors (`neighbors`).  2. **Base Case**: The function `cloneGraph`
first checks if the input node is `None`. If it is, the function returns `None` immediately.  3.
**HashMap for Cloning**: A dictionary `old_to_new` is used to keep track of already cloned nodes to
avoid infinite loops and redundant work.  4. **DFS Function**: The `dfs` function is defined to
perform a depth-first search. It clones the current node and recursively clones all its neighbors.
5. **Return Cloned Graph**: Finally, the `dfs` function is called with the input node to start the
cloning process, and the cloned graph is returned.

---
---
---
 --- 
# Number of Connected Components in an Undirected Graph
>Count the number of connected components in an undirected graph.

>Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
- https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/
- Difficulty: 45
- Frequent: 55
- Tags: ['Graph', 'DFS', 'BFS']

### Number of Connected Components in an Undirected Graph -- Shortest Solution:
```python
def countComponents(n, edges):
    parent = list(range(n))
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY
    for x, y in edges:
        union(x, y)
    return len(set(find(x) for x in range(n)))
```
#### TC: O(n + e) **SC:** O(n)

The code uses the Union-Find algorithm to count the number of connected components in an undirected
graph. The `parent` list keeps track of the parent or root of each node. The `find` function finds
the root of a node and performs path compression to flatten the structure for efficiency. The
`union` function connects two nodes by linking their roots. After processing all edges, the number
of unique roots (connected components) is determined by applying the `find` function to all nodes
and converting the results to a set.

---
### Number of Connected Components in an Undirected Graph -- Best TC Solution:
```python
from collections import defaultdict

def countComponents(n, edges):
    graph = defaultdict(list)
    visited = [False] * n
    components = 0
    
    def dfs(node):
        if visited[node]:
            return
        visited[node] = True
        for neighbor in graph[node]:
            dfs(neighbor)
    
    for edge in edges:
        graph[edge[0]].append(edge[1])
        graph[edge[1]].append(edge[0])
    
    for i in range(n):
        if not visited[i]:
            components += 1
            dfs(i)
    
    return components
```
#### TC: O(V + E) **SC:** O(V + E)

The code uses Depth-First Search (DFS) to traverse the graph and count the number of connected
components. It initializes a graph using a defaultdict, tracks visited nodes, and counts the
components. The time complexity is O(V + E) where V is the number of vertices and E is the number of
edges. The space complexity is also O(V + E) due to the graph and visited array.

---
---
---
 --- 
# Graph Valid Tree
>Determine if a given graph is a valid tree.

>Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true
- https://leetcode.com/problems/graph-valid-tree/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'DFS', 'BFS']

### Graph Valid Tree -- Shortest Solution:
```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if len(edges) != n - 1:
            return False
        graph = {i: [] for i in range(n)}
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        visited = set()
        def dfs(node, parent):
            visited.add(node)
            for neighbor in graph[node]:
                if neighbor == parent:
                    continue
                if neighbor in visited or not dfs(neighbor, node):
                    return False
            return True
        return dfs(0, -1) and len(visited) == n
```
#### TC: O(N + E) **SC:** O(N + E)

1. The code first checks if the number of edges is exactly one less than the number of nodes. If
not, it returns False immediately because a valid tree must have exactly n-1 edges. 2. It then
constructs an adjacency list to represent the graph. 3. A Depth-First Search (DFS) is performed
starting from node 0. The DFS checks for cycles and ensures all nodes are connected. 4. The function
returns True if all nodes are visited and no cycles are detected, otherwise it returns False.

---
### Graph Valid Tree -- Best TC Solution:
```python
from collections import defaultdict
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if len(edges) != n - 1:
            return False
        graph = defaultdict(list)
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        visited = [False] * n
        def dfs(node, parent):
            visited[node] = True
            for neighbor in graph[node]:
                if not visited[neighbor]:
                    if not dfs(neighbor, node):
                        return False
                elif neighbor != parent:
                    return False
            return True
        if not dfs(0, -1):
            return False
        return all(visited)
```
#### TC: O(V + E) **SC:** O(V + E)

The code uses a Depth-First Search (DFS) approach to check if the given graph is a valid tree. It
first checks if the number of edges is equal to (n - 1), where n is the number of nodes. Then, it
creates a graph using a defaultdict to store the connections between nodes. The DFS function
recursively traverses the graph, marking visited nodes and checking for cycles. If a cycle is found
or not all nodes are visited, the function returns False. Finally, it ensures that all nodes are
visited to confirm a valid tree structure. The time complexity is O(V + E) where V is the number of
vertices and E is the number of edges, and the space complexity is also O(V + E) to store the graph
and visited nodes.

---
### Graph Valid Tree -- Best SC Solution:
```python
# Approach: Union Find

class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if len(edges) != n - 1:
            return False
        parent = list(range(n))
        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
        def union(x, y):
            root_x, root_y = find(x), find(y)
            if root_x == root_y:
                return False
            parent[root_x] = root_y
            return True
        for edge in edges:
            if not union(*edge):
                return False
        return True
```
#### TC: O(N * alpha(N)) **SC:** O(N)

The code uses the Union Find algorithm to determine if the given graph is a valid tree. It checks if
the number of edges is equal to n-1, where n is the number of nodes. It then performs union
operations on the edges to connect the nodes. The find function recursively finds the root of a
node, and the union function merges two sets. The code efficiently handles cycles in the graph and
returns true if the graph is a valid tree, false otherwise.

---
---
---
 --- 
# Longest Increasing Path in a Matrix
>Find the longest increasing path in a matrix.

>Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
- https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
- Difficulty: 55
- Frequent: 50
- Tags: ['DFS', 'Topological Sort']

### Longest Increasing Path in a Matrix -- Shortest Solution:
```python
from functools import lru_cache

def longestIncreasingPath(matrix):
    if not matrix or not matrix[0]:
        return 0
    rows, cols = len(matrix), len(matrix[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    
    @lru_cache(None)
    def dfs(r, c):
        return 1 + max(
            (dfs(r + dr, c + dc) for dr, dc in directions 
             if 0 <= r + dr < rows and 0 <= c + dc < cols and matrix[r + dr][c + dc] > matrix[r][c]),
            default=0
        )
    
    return max(dfs(r, c) for r in range(rows) for c in range(cols))
```
#### TC: O(m * n) **SC:** O(m * n)

The solution uses Depth-First Search (DFS) with memoization to explore all possible increasing paths
in the matrix. The `lru_cache` decorator is used to store the results of subproblems to avoid
redundant calculations, making the solution efficient. The `dfs` function computes the longest
increasing path starting from any cell by exploring all four possible directions (up, down, left,
right) and recursively finding the longest path. The main function iterates over all cells to find
the maximum path length.

---
### Longest Increasing Path in a Matrix -- Best TC Solution:
```python
from functools import lru_cache
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        if not matrix:
            return 0
        rows, cols = len(matrix), len(matrix[0])
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        @lru_cache(None)
        def dfs(row, col):
            val = matrix[row][col]
            max_path = 1
            for dx, dy in directions:
                new_row, new_col = row + dx, col + dy
                if 0 <= new_row < rows and 0 <= new_col < cols and matrix[new_row][new_col] > val:
                    max_path = max(max_path, 1 + dfs(new_row, new_col))
            return max_path
        return max(dfs(i, j) for i in range(rows) for j in range(cols))
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a Depth-First Search (DFS) approach to find the longest increasing path in a matrix.
It utilizes memoization with the @lru_cache decorator to avoid redundant calculations. The DFS
function recursively explores all possible paths from each cell, considering only cells with values
greater than the current cell. The maximum path length is updated and returned for each cell. The
time complexity is O(m*n) where m is the number of rows and n is the number of columns in the
matrix. The space complexity is also O(m*n) due to the memoization cache.

---
---
---
 --- 
# Pacific Atlantic Water Flow
>Find all cells that can flow to both the Pacific and Atlantic ocean.

>Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
- https://leetcode.com/problems/pacific-atlantic-water-flow/
- Difficulty: 60
- Frequent: 45
- Tags: ['DFS', 'BFS']

### Pacific Atlantic Water Flow -- Shortest Solution:
```python
class Solution:
    def pacificAtlantic(self, heights):
        if not heights: return []
        rows, cols = len(heights), len(heights[0])
        pacific, atlantic = set(), set()
        def dfs(r, c, visit, prevHeight):
            if ((r, c) in visit or r < 0 or c < 0 or r == rows or c == cols or heights[r][c] < prevHeight):
                return
            visit.add((r, c))
            directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
            for dr, dc in directions:
                dfs(r + dr, c + dc, visit, heights[r][c])
        for c in range(cols):
            dfs(0, c, pacific, heights[0][c])
            dfs(rows - 1, c, atlantic, heights[rows - 1][c])
        for r in range(rows):
            dfs(r, 0, pacific, heights[r][0])
            dfs(r, cols - 1, atlantic, heights[r][cols - 1])
        return list(pacific & atlantic)
```
#### TC: O(M * N) **SC:** O(M * N)

1. **Initialization**: We start by checking if the input `heights` is empty. If it is, we return an
empty list. We then initialize the number of rows and columns, and two sets (`pacific` and
`atlantic`) to keep track of cells that can reach each ocean.  2. **DFS Function**: The `dfs`
function is defined to perform depth-first search. It takes the current cell's row and column
indices, the set of visited cells, and the height of the previous cell. If the current cell is out
of bounds, already visited, or its height is less than the previous cell's height, we return.
Otherwise, we add the current cell to the visited set and recursively call `dfs` for all four
possible directions (up, down, left, right).  3. **Populating Reachable Cells**: We iterate over
each column of the first and last rows to populate the `pacific` and `atlantic` sets using the `dfs`
function. Similarly, we iterate over each row of the first and last columns to do the same.  4.
**Result**: Finally, we return the intersection of the `pacific` and `atlantic` sets, which gives us
the cells that can reach both oceans.

---
### Pacific Atlantic Water Flow -- Best TC Solution:
```python
from typing import List
class Solution:
    def pacificAtlantic(self, matrix: List[List[int]]) -> List[List[int]]:
        if not matrix:
            return []
        rows, cols = len(matrix), len(matrix[0])
        pacific_reachable = [[False] * cols for _ in range(rows)]
        atlantic_reachable = [[False] * cols for _ in range(rows)]
        def dfs(r, c, reachable):
            reachable[r][c] = True
            directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
            for dr, dc in directions:
                new_r, new_c = r + dr, c + dc
                if 0 <= new_r < rows and 0 <= new_c < cols and not reachable[new_r][new_c] and matrix[new_r][new_c] >= matrix[r][c]:
                    dfs(new_r, new_c, reachable)
        for i in range(rows):
            dfs(i, 0, pacific_reachable)
            dfs(i, cols - 1, atlantic_reachable)
        for j in range(cols):
            dfs(0, j, pacific_reachable)
            dfs(rows - 1, j, atlantic_reachable)
        result = []
        for i in range(rows):
            for j in range(cols):
                if pacific_reachable[i][j] and atlantic_reachable[i][j]:
                    result.append([i, j])
        return result
```
#### TC: O(m*n) **SC:** O(m*n)

The code defines a Solution class with a pacificAtlantic method that takes a matrix as input. It
uses Depth-First Search (DFS) to traverse the matrix and mark cells that are reachable from the
Pacific and Atlantic oceans. The algorithm iterates through the matrix to find cells that are
reachable from both oceans and adds them to the result list. The time complexity of the code is
O(m*n) where m is the number of rows and n is the number of columns in the matrix. The space
complexity is also O(m*n) due to the creation of two matrices to track reachable cells from each
ocean.

---
---
---