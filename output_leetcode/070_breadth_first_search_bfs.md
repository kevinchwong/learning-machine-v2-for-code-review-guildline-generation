# Breadth-First Search (BFS)
## Frequent score for this algorithmic framework: 70 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal) | 80 | 30 |
| 2 | [Binary Tree Zigzag Level Order Traversal](#binary-tree-zigzag-level-order-traversal) | 70 | 35 |
| 3 | [Rotting Oranges](#rotting-oranges) | 70 | 40 |
| 4 | [Binary Tree Right Side View](#binary-tree-right-side-view) | 65 | 35 |
| 5 | [Minimum Depth of Binary Tree](#minimum-depth-of-binary-tree) | 60 | 20 |
| 6 | [Jump Game III](#jump-game-iii) | 60 | 35 |
| 7 | [Walls and Gates](#walls-and-gates) | 60 | 40 |
| 8 | [Shortest Path in Binary Matrix](#shortest-path-in-binary-matrix) | 60 | 45 |
| 9 | [Word Ladder II](#word-ladder-ii) | 60 | 50 |
| 10 | [Perfect Squares](#perfect-squares) | 55 | 35 |
| 11 | [Open the Lock](#open-the-lock) | 55 | 45 |
| 12 | [Shortest Bridge](#shortest-bridge) | 55 | 45 |
| 13 | [Sliding Puzzle](#sliding-puzzle) | 50 | 50 |
| 14 | [Bus Routes](#bus-routes) | 50 | 50 |
---
Breadth-First Search (BFS) is an algorithm for traversing or searching tree or graph data structures. It starts at the root (or an arbitrary node) and explores the neighbor nodes at the present depth prior to moving on to nodes at the next depth level. BFS uses a queue to keep track of the next location to visit.
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        vertex = queue.popleft()
        print(vertex, end=' ')
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# Example usage
# Graph represented as an adjacency list
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

bfs(graph, 'A')
```
### Insight:
1. BFS is useful for finding the shortest path in unweighted graphs.
2. It uses a queue to explore nodes level by level.
3. Marking nodes as visited is crucial to avoid processing the same node multiple times.
4. The algorithm is complete and guarantees the shortest path in unweighted graphs.
5. BFS can be adapted to work with weighted graphs using priority queues.
---
 --- 
# Binary Tree Level Order Traversal
>Given the root of a binary tree, return the level order traversal of its nodes' values.

>Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
- https://leetcode.com/problems/binary-tree-level-order-traversal/
- Difficulty: 30
- Frequent: 80
- Tags: ['Tree', 'Breadth-First Search']

### Binary Tree Level Order Traversal -- Shortest Solution:
```python
from collections import deque

def levelOrder(root):
    if not root:
        return []
    result, queue = [], deque([root])
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        result.append(level)
    return result
```
#### TC: O(n) **SC:** O(n)

The code uses a Breadth-First Search (BFS) approach to traverse the binary tree level by level. It
utilizes a queue to keep track of nodes at the current level. For each level, it processes all
nodes, appends their values to the current level list, and adds their children to the queue for the
next level. This ensures that nodes are processed in the correct order. The time complexity is O(n)
because each node is processed once, and the space complexity is O(n) due to the storage
requirements of the queue and result list.

---
### Binary Tree Level Order Traversal -- Best TC Solution:
```python
from collections import deque

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]:
        if not root:
            return []
        result = []
        queue = deque([root])
        while queue:
            level_size = len(queue)
            current_level = []
            for _ in range(level_size):
                node = queue.popleft()
                current_level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            result.append(current_level)
        return result
```
#### TC: O(N) **SC:** O(N)

The code implements a Breadth-First Search (BFS) approach to perform level order traversal of a
binary tree. It uses a queue to keep track of nodes at each level. The algorithm iterates through
each level, adding the node values to the current level list and updating the queue with the child
nodes. This process continues until all levels are traversed, and the result list contains the node
values at each level. The time complexity is O(N) as each node is visited once, and the space
complexity is also O(N) to store the nodes in the queue.

---
---
---
 --- 
# Binary Tree Zigzag Level Order Traversal
>Given the root of a binary tree, return the zigzag level order traversal of its nodes' values.

>Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
- https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
- Difficulty: 35
- Frequent: 70
- Tags: ['Tree', 'Breadth-First Search']

### Binary Tree Zigzag Level Order Traversal -- Shortest Solution:
```python
from collections import deque

def zigzagLevelOrder(root):
    if not root:
        return []
    result, temp, queue, flag = [], [], deque([root]), 1
    while queue:
        for i in range(len(queue)):
            node = queue.popleft()
            temp.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        result.append(temp[::flag])
        temp = []
        flag *= -1
    return result
```
#### TC: O(N) **SC:** O(N)

1. The code uses a deque for efficient popping from the left. 2. A flag is used to control the order
of appending values (left-to-right or right-to-left). 3. The BFS approach ensures that nodes are
processed level by level.

---
### Binary Tree Zigzag Level Order Traversal -- Best SC Solution:
```python
from collections import deque

class Solution:
    def zigzagLevelOrder(self, root):
        if not root:
            return []
        result = []
        queue = deque([root])
        level = 0
        while queue:
            level_size = len(queue)
            current_level = []
            for _ in range(level_size):
                node = queue.popleft()
                current_level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            if level % 2 == 1:
                current_level = current_level[::-1]
            result.append(current_level)
            level += 1
        return result
```
#### TC: O(n) **SC:** O(n)

The code uses a Breadth-First Search (BFS) approach to traverse the binary tree in a zigzag level
order. It maintains a queue to keep track of the nodes at each level and processes them level by
level. The algorithm iterates through each level, storing the values in a current_level list. If the
level is odd, it reverses the current_level list to achieve the zigzag order. The result list stores
the values of each level in the zigzag order. The time complexity is O(n) as each node is visited
once, and the space complexity is also O(n) to store the nodes in the queue and the result list.

---
---
---
 --- 
# Rotting Oranges
>Given a grid of oranges, determine the minimum time required for all oranges to rot.

>Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
- https://leetcode.com/problems/rotting-oranges/
- Difficulty: 40
- Frequent: 70
- Tags: ['Breadth-First Search']

### Rotting Oranges -- Shortest Solution:
```python
from collections import deque

def orangesRotting(grid):
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_oranges = 0
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))
            elif grid[r][c] == 1:
                fresh_oranges += 1
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    minutes = 0
    while queue:
        r, c, minutes = queue.popleft()
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2
                fresh_oranges -= 1
                queue.append((nr, nc, minutes + 1))
    return minutes if fresh_oranges == 0 else -1
```
#### TC: O(n * m) **SC:** O(n * m)

1. **Initialization**: We initialize a queue to keep track of the rotten oranges and a counter for
fresh oranges. 2. **Queue Population**: We populate the queue with the initial positions of rotten
oranges and count the fresh oranges. 3. **BFS Traversal**: We use BFS to traverse the grid. For each
rotten orange, we rot its adjacent fresh oranges and add them to the queue with an incremented
minute count. 4. **Termination**: The process continues until the queue is empty. If there are no
fresh oranges left, we return the minutes passed; otherwise, we return -1.

---
### Rotting Oranges -- Best TC Solution:
```python
from collections import deque
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        fresh_count = 0
        queue = deque()
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        minutes = 0
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 1:
                    fresh_count += 1
                elif grid[i][j] == 2:
                    queue.append((i, j))
        while queue and fresh_count:
            minutes += 1
            for _ in range(len(queue)):
                x, y = queue.popleft()
                for dx, dy in directions:
                    new_x, new_y = x + dx, y + dy
                    if 0 <= new_x < rows and 0 <= new_y < cols and grid[new_x][new_y] == 1:
                        grid[new_x][new_y] = 2
                        fresh_count -= 1
                        queue.append((new_x, new_y))
        return -1 if fresh_count != 0 else max(0, minutes)
```
#### TC: O(rows * cols) **SC:** O(rows * cols)

The code uses BFS to simulate the rotting process of oranges. It iterates through the grid to count
fresh oranges and initialize the queue with rotten oranges. It then performs BFS by checking
neighboring cells and updating the grid accordingly. The process continues until all fresh oranges
are rotten or no more rotten oranges can infect fresh ones. The time complexity is O(rows * cols) as
each cell is visited once, and the space complexity is also O(rows * cols) due to the queue storing
cell coordinates.

---
---
---
 --- 
# Binary Tree Right Side View
>Given the root of a binary tree, return the right side view of it.

>Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
- https://leetcode.com/problems/binary-tree-right-side-view/
- Difficulty: 35
- Frequent: 65
- Tags: ['Tree', 'Breadth-First Search']

### Binary Tree Right Side View -- Shortest Solution:
```python
from collections import deque

def rightSideView(root):
    if not root:
        return []
    view = []
    queue = deque([root])
    while queue:
        level_length = len(queue)
        for i in range(level_length):
            node = queue.popleft()
            if i == level_length - 1:
                view.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    return view
```
#### TC: O(n) **SC:** O(n)

The code uses a Breadth-First Search (BFS) approach to traverse the binary tree level by level. It
keeps track of the last node at each level, which is the rightmost node visible from the right side
view. The `deque` is used for efficient popping from the left side of the queue. The time complexity
is O(n) because each node is processed once, and the space complexity is O(n) due to the queue and
the output list.

---
---
---
 --- 
# Minimum Depth of Binary Tree
>Given a binary tree, find its minimum depth.

>Input: root = [3,9,20,null,null,15,7]
Output: 2
- https://leetcode.com/problems/minimum-depth-of-binary-tree/
- Difficulty: 20
- Frequent: 60
- Tags: ['Tree', 'Breadth-First Search']

### Minimum Depth of Binary Tree -- Shortest Solution:
```python
from collections import deque

def minDepth(root):
    if not root:
        return 0
    queue = deque([(root, 1)])
    while queue:
        node, depth = queue.popleft()
        if not node.left and not node.right:
            return depth
        if node.left:
            queue.append((node.left, depth + 1))
        if node.right:
            queue.append((node.right, depth + 1))
```
#### TC: O(n) **SC:** O(n)

The code uses a Breadth-First Search (BFS) approach to find the minimum depth of a binary tree. BFS
is ideal for this problem because it explores nodes level by level, ensuring that the first leaf
node encountered is at the minimum depth. The `deque` is used for efficient popping from the front
of the queue. The algorithm starts by checking if the root is `None`, in which case the depth is 0.
It then initializes a queue with the root node and a depth of 1. As it processes each node, it
checks if the node is a leaf (i.e., has no left or right children). If a leaf is found, the current
depth is returned as the minimum depth. If not, the algorithm adds the node's children to the queue
with an incremented depth. This process continues until a leaf node is found.

---
### Minimum Depth of Binary Tree -- Best TC Solution:
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        queue = [(root, 1)]
        while queue:
            node, depth = queue.pop(0)
            if not node.left and not node.right:
                return depth
            if node.left:
                queue.append((node.left, depth + 1))
            if node.right:
                queue.append((node.right, depth + 1))
        return 0
```
#### TC: O(N) **SC:** O(N)

The code defines a TreeNode class and a Solution class with a method minDepth to find the minimum
depth of a binary tree. It uses a Breadth-First Search (BFS) approach by maintaining a queue of
nodes along with their depths. The algorithm traverses the tree level by level, updating the depth
as it goes. The time complexity is O(N) as it visits each node once, and the space complexity is
also O(N) in the worst case scenario where the tree is skewed. The code is clean, easy to
understand, and efficiently solves the problem.

---
---
---
 --- 
# Jump Game III
>Given an array of non-negative integers, determine if you can reach any index with value 0.

>Input: arr = [4,2,3,0,3,1,2], start = 5
Output: true
- https://leetcode.com/problems/jump-game-iii/
- Difficulty: 35
- Frequent: 60
- Tags: ['Breadth-First Search']

### Jump Game III -- Shortest Solution:
```python
from collections import deque

def canReach(arr, start):
    n = len(arr)
    queue = deque([start])
    visited = set()
    while queue:
        idx = queue.popleft()
        if arr[idx] == 0:
            return True
        visited.add(idx)
        for next_idx in [idx + arr[idx], idx - arr[idx]]:
            if 0 <= next_idx < n and next_idx not in visited:
                queue.append(next_idx)
    return False
```
#### TC: O(n) **SC:** O(n)

1. **Initialization**: We use a queue to facilitate the BFS and a set to keep track of visited
indices. 2. **BFS Loop**: We process each index in the queue. If the value at the current index is
0, we return True. 3. **Visit Tracking**: We mark the current index as visited. 4. **Next Indices**:
We calculate the next possible indices to jump to and add them to the queue if they haven't been
visited and are within bounds. 5. **Termination**: If the queue is exhausted without finding a 0, we
return False.

---
### Jump Game III -- Best TC Solution:
```python
from collections import deque
def canReach(arr, start):
    queue = deque([start])
    visited = set()
    while queue:
        idx = queue.popleft()
        if arr[idx] == 0:
            return True
        visited.add(idx)
        left = idx - arr[idx]
        right = idx + arr[idx]
        if 0 <= left < len(arr) and left not in visited:
            queue.append(left)
        if 0 <= right < len(arr) and right not in visited:
            queue.append(right)
    return False
```
#### TC: O(N) **SC:** O(N)

The code uses a Breadth-First Search (BFS) approach to traverse the array starting from the given
index. It checks if the value at the current index is 0, indicating that the destination is reached.
It maintains a queue to store the indices to visit next and a set to keep track of visited indices.
The algorithm continues until either the destination is reached or all possible indices are visited.
The time complexity is O(N) where N is the number of elements in the array, and the space complexity
is O(N) to store the visited indices.

---
---
---
 --- 
# Walls and Gates
>Given a 2D grid of rooms, fill each empty room with the distance to its nearest gate.

>Input: rooms = [[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]
Output: [[3,0,1,2],[2,2,3,-1],[1,-1,2,-1],[0,-1,3,4]]
- https://leetcode.com/problems/walls-and-gates/
- Difficulty: 40
- Frequent: 60
- Tags: ['Breadth-First Search']

### Walls and Gates -- Shortest Solution:
```python
from collections import deque

def wallsAndGates(rooms):
    if not rooms:
        return
    rows, cols = len(rooms), len(rooms[0])
    queue = deque()
    for r in range(rows):
        for c in range(cols):
            if rooms[r][c] == 0:
                queue.append((r, c))
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    while queue:
        r, c = queue.popleft()
        for dr, dc in directions:
            rr, cc = r + dr, c + dc
            if 0 <= rr < rows and 0 <= cc < cols and rooms[rr][cc] == 2147483647:
                rooms[rr][cc] = rooms[r][c] + 1
                queue.append((rr, cc))
```
#### TC: O(m * n) **SC:** O(m * n)

The solution uses Breadth-First Search (BFS) to propagate the distance from each gate (0) to the
nearest empty room (INF). The BFS starts from all gates simultaneously, ensuring that each room is
filled with the shortest distance to a gate. The queue is initialized with all gate positions, and
for each position, the algorithm checks its four possible directions (up, down, left, right). If an
adjacent room is empty (INF), it updates the room's distance and adds the room to the queue for
further processing. This ensures that each room is processed in the order of its distance from the
nearest gate.

---
### Walls and Gates -- Best TC Solution:
```python
from collections import deque

def wallsAndGates(rooms):
    if not rooms:
        return
    m, n = len(rooms), len(rooms[0])
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    queue = deque()
    for i in range(m):
        for j in range(n):
            if rooms[i][j] == 0:
                queue.append((i, j))
    while queue:
        x, y = queue.popleft()
        for dx, dy in directions:
            new_x, new_y = x + dx, y + dy
            if 0 <= new_x < m and 0 <= new_y < n and rooms[new_x][new_y] == 2147483647:
                rooms[new_x][new_y] = rooms[x][y] + 1
                queue.append((new_x, new_y))

# Example
rooms = [
    [2147483647, -1, 0, 2147483647],
    [2147483647, 2147483647, 2147483647, -1],
    [2147483647, -1, 2147483647, -1],
    [0, -1, 2147483647, 2147483647]
]
wallsAndGates(rooms)
print(rooms)
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses Breadth-First Search (BFS) to traverse the rooms starting from the gates (value 0). It
iterates through each room and updates the distance to the nearest gate using a queue. The algorithm
ensures that each room is visited only once, leading to a time complexity of O(m*n) where m is the
number of rows and n is the number of columns in the rooms matrix. The space complexity is also
O(m*n) due to the queue storing the coordinates of each room to be processed.

---
---
---
 --- 
# Shortest Path in Binary Matrix
>Given an n x n binary matrix, return the length of the shortest clear path in the matrix.

>Input: grid = [[0,1],[1,0]]
Output: 2
- https://leetcode.com/problems/shortest-path-in-binary-matrix/
- Difficulty: 45
- Frequent: 60
- Tags: ['Graph', 'Breadth-First Search']

### Shortest Path in Binary Matrix -- Shortest Solution:
```python
from collections import deque

def shortestPathBinaryMatrix(grid):
    if grid[0][0] or grid[-1][-1]:
        return -1
    n = len(grid)
    directions = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]
    queue = deque([(0, 0, 1)])
    while queue:
        x, y, dist = queue.popleft()
        if x == n - 1 and y == n - 1:
            return dist
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and not grid[nx][ny]:
                grid[nx][ny] = 1
                queue.append((nx, ny, dist + 1))
    return -1
```
#### TC: O(n^2) **SC:** O(n^2)

The code uses Breadth-First Search (BFS) to find the shortest path in a binary matrix. It starts
from the top-left corner and explores all 8 possible directions (up, down, left, right, and the 4
diagonals). The BFS ensures that the shortest path is found by exploring all nodes level by level.
The grid is updated to mark visited cells, preventing re-visitation and infinite loops. The time and
space complexity are both O(n^2) due to the need to potentially explore and store all cells in the
grid.

---
### Shortest Path in Binary Matrix -- Best TC Solution:
```python
from collections import deque
def shortestPathBinaryMatrix(grid):
    if grid[0][0] == 1 or grid[-1][-1] == 1:
        return -1
    n = len(grid)
    queue = deque([(0, 0, 1)])
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1),
                  (1, 1), (-1, -1), (1, -1), (-1, 1)]
    while queue:
        i, j, steps = queue.popleft()
        if i == n - 1 and j == n - 1:
            return steps
        for di, dj in directions:
            ni, nj = i + di, j + dj
            if 0 <= ni < n and 0 <= nj < n and grid[ni][nj] == 0:
                queue.append((ni, nj, steps + 1))
                grid[ni][nj] = 1
    return -1
```
#### TC: O(N) **SC:** O(N)

The code uses Breadth-First Search (BFS) to find the shortest path in a binary matrix. It starts
from the top-left corner and explores all possible paths by moving in all directions. The algorithm
keeps track of the steps taken to reach each cell and updates the grid to mark visited cells. If the
destination cell is reached, the algorithm returns the minimum steps taken. The time complexity is
O(N) where N is the number of cells in the matrix, and the space complexity is also O(N) due to the
queue used to store the cells to be visited.

---
---
---
 --- 
# Word Ladder II
>Given two words and a dictionary, return all the shortest transformation sequences from beginWord to endWord.

>Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
- https://leetcode.com/problems/word-ladder-ii/
- Difficulty: 50
- Frequent: 60
- Tags: ['Breadth-First Search']

### Word Ladder II -- Shortest Solution:
```python
from collections import defaultdict, deque

def findLadders(beginWord, endWord, wordList):
    wordList = set(wordList)
    if endWord not in wordList:
        return []
    layer = {}
    layer[beginWord] = [[beginWord]]
    while layer:
        newlayer = defaultdict(list)
        for word in layer:
            if word == endWord:
                return layer[word]
            for i in range(len(word)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    newWord = word[:i] + c + word[i+1:]
                    if newWord in wordList:
                        newlayer[newWord] += [j + [newWord] for j in layer[word]]
        wordList -= set(newlayer.keys())
        layer = newlayer
    return []
```
#### TC: O(N * M^2) **SC:** O(N * M)

1. **Initialization**: The function initializes a set from the word list for O(1) lookups and checks
if the end word is in the word list. 2. **Layer Tracking**: It uses a dictionary to keep track of
the current layer of words and their transformation sequences. 3. **BFS Traversal**: The while loop
performs a BFS, where each word in the current layer is transformed by changing each character to
every letter from 'a' to 'z'. 4. **New Layer Formation**: For each valid transformation (i.e., the
new word is in the word list), it appends the new word to the transformation sequences and forms a
new layer. 5. **Early Exit**: If the end word is found in the current layer, it returns all
transformation sequences leading to it. 6. **Layer Update**: The word list is updated to remove
words that have already been used in the current layer to prevent cycles, and the layer is updated
to the new layer. 7. **Return**: If the end word is not found, it returns an empty list.

---
### Word Ladder II -- Best TC Solution:
```python
from collections import deque, defaultdict
def findLadders(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return []
    graph = defaultdict(list)
    level = {beginWord: 0}
    queue = deque([beginWord])
    while queue:
        word = queue.popleft()
        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                newWord = word[:i] + c + word[i + 1:]
                if newWord in wordSet:
                    if newWord not in level:
                        level[newWord] = level[word] + 1
                        queue.append(newWord)
                    graph[newWord].append(word)
    res = []
    def backtrack(path, word):
        if word == beginWord:
            res.append([beginWord] + path)
        else:
            for prevWord in graph[word]:
                backtrack([word] + path, prevWord)
    backtrack([], endWord)
    return res
```
#### TC: O(M^2 * N), where M is the length of each word and N is the total number of words in the word list **SC:** O(N)

The code uses a BFS approach to find the shortest transformation sequence from beginWord to endWord.
It builds a graph of valid transformations and uses backtracking to find all possible transformation
sequences. The use of a queue for BFS traversal and a defaultdict for the graph representation helps
in efficient traversal and storage of intermediate results. The time complexity is O(M^2 * N) due to
the nested loops for word transformations, and the space complexity is O(N) for storing the word
list and graph data structures.

---
### Word Ladder II -- Best SC Solution:
```python
from collections import defaultdict, deque
def findLadders(beginWord, endWord, wordList):
    wordList = set(wordList)
    if endWord not in wordList:
        return []
    layer = defaultdict(list)
    layer[beginWord] = [[beginWord]]
    while layer:
        new_layer = defaultdict(list)
        for word in layer:
            if word == endWord:
                return layer[word]
            for i in range(len(word)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    new_word = word[:i] + c + word[i + 1:]
                    if new_word in wordList:
                        new_layer[new_word] += [j + [new_word] for j in layer[word]]
        wordList -= set(new_layer.keys())
        layer = new_layer
    return []
```
#### TC: O(M*N), where M is the length of words and N is the total number of words in the wordList **SC:** O(M*N)

The code uses a BFS approach to find the shortest transformation sequence from beginWord to endWord.
It maintains a layer dictionary to store the paths at each level of transformation. The algorithm
iterates through each word in the layer, generates new words by changing one character at a time,
and checks if the new word is in the wordList. It continues this process until the endWord is
reached or no more transformations are possible. The space complexity is optimized by using sets to
store words and paths, reducing redundant calculations.

---
---
---
 --- 
# Perfect Squares
>Given a positive integer n, find the least number of perfect square numbers which sum to n.

>Input: n = 12
Output: 3
- https://leetcode.com/problems/perfect-squares/
- Difficulty: 35
- Frequent: 55
- Tags: ['Breadth-First Search']

### Perfect Squares -- Shortest Solution:
```python
from collections import deque
import math

def numSquares(n: int) -> int:
    if n < 2:
        return n
    squares = [i * i for i in range(1, int(math.sqrt(n)) + 1)]
    level = 0
    queue = {n}
    while queue:
        level += 1
        next_queue = set()
        for remainder in queue:
            for square in squares:
                if remainder == square:
                    return level
                if remainder < square:
                    break
                next_queue.add(remainder - square)
        queue = next_queue
    return level
```
#### TC: O(n) **SC:** O(n)

1. **Initialization**: The function starts by checking if `n` is less than 2, in which case it
returns `n` directly. This is a base case for very small values of `n`.  2. **Precompute Squares**:
It precomputes all perfect squares less than or equal to `n` and stores them in a list called
`squares`. This helps in quickly accessing the square numbers during the BFS traversal.  3. **BFS
Setup**: The BFS is initialized with a queue containing the initial number `n`. The `level` variable
is used to keep track of the number of levels (or steps) in the BFS.  4. **BFS Traversal**: The BFS
loop continues until the queue is empty. For each level, it iterates through all the remainders in
the current queue. For each remainder, it checks all the precomputed squares:    - If the remainder
is exactly equal to a square, it returns the current level as the answer.    - If the remainder is
less than a square, it breaks out of the loop to avoid unnecessary checks.    - Otherwise, it adds
the difference (remainder - square) to the next level's queue.  5. **Return Result**: The function
returns the level when it finds a perfect square that matches the remainder, indicating the minimum
number of perfect squares that sum up to `n`.

---
### Perfect Squares -- Best TC Solution:
```python
from math import sqrt

def numSquares(n: int) -> int:
    squares = [i*i for i in range(1, int(sqrt(n)) + 1)]
    level = 0
    queue = {n}
    while queue:
        level += 1
        next_queue = set()
        for remainder in queue:
            for square in squares:
                if remainder == square:
                    return level
                elif remainder < square:
                    break
                else:
                    next_queue.add(remainder - square)
        queue = next_queue
    return level

# Example usage
n = 12
print(numSquares(n))  # Output: 3
```
#### TC: O(n*sqrt(n)) **SC:** O(sqrt(n))

The code defines a function numSquares that calculates the least number of perfect square numbers
that sum up to n. It uses a BFS approach to explore all possible combinations of perfect squares to
reach the target number. The code efficiently utilizes a set to store the remainders at each level
of the BFS traversal. By iterating through the squares and updating the next level's remainders, it
finds the minimum number of squares required. The time complexity is O(n*sqrt(n)) due to the nested
loops, and the space complexity is O(sqrt(n)) for storing the squares.

---
---
---
 --- 
# Open the Lock
>Given a lock represented by a string, return the minimum total number of turns required to open the lock.

>Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
- https://leetcode.com/problems/open-the-lock/
- Difficulty: 45
- Frequent: 55
- Tags: ['Breadth-First Search']

### Open the Lock -- Shortest Solution:
```python
from collections import deque

def openLock(deadends, target):
    dead = set(deadends)
    if '0000' in dead:
        return -1
    queue = deque([('0000', 0)])
    visited = {'0000'}
    while queue:
        node, depth = queue.popleft()
        if node == target:
            return depth
        for i in range(4):
            for d in (-1, 1):
                new_digit = (int(node[i]) + d) % 10
                new_node = node[:i] + str(new_digit) + node[i+1:]
                if new_node not in dead and new_node not in visited:
                    visited.add(new_node)
                    queue.append((new_node, depth + 1))
    return -1
```
#### TC: O(10^4 * 4^2) **SC:** O(10^4)

1. **Initialization**: The code initializes a set of deadends and a queue for BFS. It also keeps
track of visited nodes to avoid cycles. 2. **Early Exit**: If '0000' is a deadend, it immediately
returns -1. 3. **BFS Loop**: The BFS loop processes each node, checking if it matches the target. If
not, it generates all possible next states by incrementing or decrementing each digit. 4. **State
Generation**: For each digit in the current node, the code generates two new states (one incremented
and one decremented) and adds them to the queue if they haven't been visited and aren't deadends. 5.
**Termination**: The loop continues until the queue is empty or the target is found. If the target
is found, it returns the depth (number of moves). If the queue is exhausted, it returns -1.

---
### Open the Lock -- Best TC Solution:
```python
from collections import deque
def openLock(deadends, target):
    deadends = set(deadends)
    if '0000' in deadends:
        return -1
    if target == '0000':
        return 0
    queue = deque([('0000', 0)])
    visited = set('0000')
    while queue:
        current, steps = queue.popleft()
        for i in range(4):
            for move in [-1, 1]:
                new_digit = str((int(current[i]) + move) % 10)
                new_lock = current[:i] + new_digit + current[i + 1:]
                if new_lock not in visited and new_lock not in deadends:
                    if new_lock == target:
                        return steps + 1
                    queue.append((new_lock, steps + 1))
                    visited.add(new_lock)
    return -1
```
#### TC: O(N^2 * A^N + D) **SC:** O(A^N + D)

The code uses a Breadth-First Search (BFS) approach to find the shortest path to reach the target
combination. It avoids visiting deadends and already visited combinations to optimize the search.
The time complexity is O(N^2 * A^N + D), where N is the number of digits in the lock, A is the
number of possible values for each digit (10 in this case), and D is the number of deadends. The
space complexity is O(A^N + D) due to the queue and visited set storing combinations and deadends.

---
---
---
 --- 
# Shortest Bridge
>Given a binary matrix, return the smallest number of 0s you must flip to connect two islands.

>Input: grid = [[0,1],[1,0]]
Output: 1
- https://leetcode.com/problems/shortest-bridge/
- Difficulty: 45
- Frequent: 55
- Tags: ['Graph', 'Breadth-First Search']

### Shortest Bridge -- Shortest Solution:
```python
from collections import deque

def shortestBridge(A):
    def neighbors(x, y):
        for nx, ny in ((x-1, y), (x+1, y), (x, y-1), (x, y+1)):
            if 0 <= nx < len(A) and 0 <= ny < len(A[0]):
                yield nx, ny

    def bfs(queue):
        while queue:
            x, y, d = queue.popleft()
            for nx, ny in neighbors(x, y):
                if (nx, ny) not in visited:
                    if A[nx][ny] == 1:
                        return d
                    queue.append((nx, ny, d+1))
                    visited.add((nx, ny))

    def find_first_island():
        for i in range(len(A)):
            for j in range(len(A[0])):
                if A[i][j] == 1:
                    return i, j

    def mark_island(x, y):
        queue = deque([(x, y)])
        while queue:
            cx, cy = queue.popleft()
            for nx, ny in neighbors(cx, cy):
                if A[nx][ny] == 1 and (nx, ny) not in visited:
                    visited.add((nx, ny))
                    queue.append((nx, ny))
                    border.append((nx, ny, 0))

    visited = set()
    border = deque()
    x, y = find_first_island()
    visited.add((x, y))
    mark_island(x, y)
    return bfs(border)
```
#### TC: O(N^2) **SC:** O(N^2)

1. **Finding the First Island**: The `find_first_island` function scans the grid to locate the first
cell that is part of an island. 2. **Marking the Island**: The `mark_island` function uses BFS to
traverse and mark all cells of the first island, adding its border cells to the `border` queue. 3.
**Breadth-First Search (BFS)**: The `bfs` function then performs a BFS starting from the border
cells of the first island, expanding outwards until it reaches a cell of the second island. 4.
**Neighbors Function**: The `neighbors` function generates valid neighboring cells for a given cell,
ensuring we stay within grid bounds. 5. **Visited Set**: The `visited` set keeps track of all
visited cells to avoid re-processing and infinite loops. 6. **Queue for BFS**: The `deque` (double-
ended queue) is used for efficient popping from the front and appending to the back during BFS
traversal.

---
### Shortest Bridge -- Best TC Solution:
```python
from collections import deque

class Solution:
    def shortestBridge(self, A: List[List[int]]) -> int:
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        m, n = len(A), len(A[0])
        visited = set()
        queue = deque()
        found = False
        
        def dfs(i, j):
            if i < 0 or i >= m or j < 0 or j >= n or A[i][j] == 0 or (i, j) in visited:
                return
            visited.add((i, j))
            queue.append((i, j))
            for dx, dy in directions:
                dfs(i + dx, j + dy)
        
        for i in range(m):
            if found:
                break
            for j in range(n):
                if A[i][j] == 1:
                    dfs(i, j)
                    found = True
                    break
        
        steps = 0
        while queue:
            size = len(queue)
            for _ in range(size):
                x, y = queue.popleft()
                for dx, dy in directions:
                    nx, ny = x + dx, y + dy
                    if nx < 0 or nx >= m or ny < 0 or ny >= n or (nx, ny) in visited:
                        continue
                    if A[nx][ny] == 1:
                        return steps
                    visited.add((nx, ny))
                    queue.append((nx, ny))
            steps += 1
        
        return -1
```
#### TC: O(M*N) **SC:** O(M*N)

The code uses a Breadth-First Search (BFS) approach to find the shortest bridge between two islands
represented by 1s in the input matrix. It first identifies the two islands using a Depth-First
Search (DFS) approach and then uses BFS to find the shortest path between them. The code efficiently
tracks visited cells and uses a queue to explore neighboring cells. The time complexity is O(M*N)
where M is the number of rows and N is the number of columns in the input matrix, and the space
complexity is also O(M*N) to store visited cells and the queue.

---
---
---
 --- 
# Sliding Puzzle
>Given a 2x3 board, return the least number of moves required to solve the puzzle.

>Input: board = [[1,2,3],[4,0,5]]
Output: 1
- https://leetcode.com/problems/sliding-puzzle/
- Difficulty: 50
- Frequent: 50
- Tags: ['Breadth-First Search']

### Sliding Puzzle -- Shortest Solution:
```python
from collections import deque

def slidingPuzzle(board):
    target = '123450'
    start = ''.join(str(num) for row in board for num in row)
    neighbors = {0: [1, 3], 1: [0, 2, 4], 2: [1, 5], 3: [0, 4], 4: [1, 3, 5], 5: [2, 4]}
    queue = deque([(start, start.index('0'), 0)])
    visited = {start}
    while queue:
        state, zero, depth = queue.popleft()
        if state == target:
            return depth
        for neighbor in neighbors[zero]:
            new_state = list(state)
            new_state[zero], new_state[neighbor] = new_state[neighbor], new_state[zero]
            new_state = ''.join(new_state)
            if new_state not in visited:
                visited.add(new_state)
                queue.append((new_state, neighbor, depth + 1))
    return -1
```
#### TC: O((m*n)!) **SC:** O((m*n)!)

The solution uses Breadth-First Search (BFS) to explore all possible states of the board. The board
is represented as a string, and the BFS explores all possible moves by swapping the '0' tile with
its neighbors. The `neighbors` dictionary defines the possible moves for each position of the '0'
tile. The algorithm keeps track of visited states to avoid redundant work. The BFS ensures that the
shortest path to the target state is found.

---
### Sliding Puzzle -- Best TC Solution:
```python
from collections import deque
def slidingPuzzle(board):
    target = '123450'
    start = ''.join(str(num) for row in board for num in row)
    moves = {
        0: [1, 3],
        1: [0, 2, 4],
        2: [1, 5],
        3: [0, 4],
        4: [1, 3, 5],
        5: [2, 4]
    }
    queue = deque([(start, start.index('0'))])
    visited = set([start])
    steps = 0
    while queue:
        for _ in range(len(queue)):
            node, zero = queue.popleft()
            if node == target:
                return steps
            for move in moves[zero]:
                new_node = list(node)
                new_node[zero], new_node[move] = new_node[move], new_node[zero]
                new_node_str = ''.join(new_node)
                if new_node_str not in visited:
                    visited.add(new_node_str)
                    queue.append((new_node_str, move))
        steps += 1
    return -1
```
#### TC: O(1) **SC:** O(1)

The code uses a Breadth-First Search (BFS) approach to solve the sliding puzzle problem efficiently.
It converts the board configuration into a string representation for easier manipulation. The
algorithm explores all possible moves from the current state and keeps track of visited states to
avoid revisiting them. The queue data structure helps in processing states level by level, ensuring
the shortest path to the target state. The code is clean, easy to understand, and has a time and
space complexity of O(1), making it an optimal solution for the sliding puzzle problem.

---
---
---
 --- 
# Bus Routes
>Given a list of bus routes, return the least number of buses needed to travel from the source to the target.

>Input: routes = [[1,2,7],[3,6,7]], source = 1, target = 6
Output: 2
- https://leetcode.com/problems/bus-routes/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'Breadth-First Search']

### Bus Routes -- Shortest Solution:
```python
from collections import deque, defaultdict

def numBusesToDestination(routes, source, target):
    if source == target:
        return 0
    graph = defaultdict(set)
    for i, route in enumerate(routes):
        for stop in route:
            graph[stop].add(i)
    visited_stops = set([source])
    visited_buses = set()
    queue = deque([(source, 0)])
    while queue:
        stop, bus_count = queue.popleft()
        for bus in graph[stop]:
            if bus not in visited_buses:
                visited_buses.add(bus)
                for next_stop in routes[bus]:
                    if next_stop == target:
                        return bus_count + 1
                    if next_stop not in visited_stops:
                        visited_stops.add(next_stop)
                        queue.append((next_stop, bus_count + 1))
    return -1
```
#### TC: O(N + E) **SC:** O(N + E)

1. **Graph Construction**: The code first constructs a graph where each stop points to the buses
that visit that stop. This helps in quickly finding which buses can be taken from any given stop. 2.
**BFS Initialization**: The BFS is initialized with the source stop. The queue stores tuples of the
current stop and the number of buses taken to reach there. 3. **BFS Execution**: The BFS explores
each stop, checking all buses that visit that stop. For each bus, it checks all subsequent stops. If
the target stop is found, it returns the bus count incremented by one. 4. **Visited Sets**: Two sets
are maintained to keep track of visited stops and buses to avoid redundant processing and infinite
loops. 5. **Early Exit**: If the source is the same as the target, the function immediately returns
0, indicating no buses are needed.

---
### Bus Routes -- Best TC Solution:
```python
from collections import defaultdict, deque
def numBusesToDestination(routes, S, T):
    if S == T:
        return 0
    stop_to_routes = defaultdict(set)
    for i, route in enumerate(routes):
        for stop in route:
            stop_to_routes[stop].add(i)
    queue = deque([(S, 0)])
    visited_routes = set()
    while queue:
        stop, buses = queue.popleft()
        if stop == T:
            return buses
        for route_idx in stop_to_routes[stop]:
            if route_idx not in visited_routes:
                visited_routes.add(route_idx)
                for next_stop in routes[route_idx]:
                    queue.append((next_stop, buses + 1))
    return -1
```
#### TC: O(N + R^2) **SC:** O(N + R^2)

The code uses a BFS approach to find the minimum number of buses needed to reach the destination. It
creates a mapping of stops to routes using a defaultdict. It then iterates through the routes and
stops to build this mapping. The BFS algorithm starts from the source stop and explores all possible
routes to reach the destination stop. The visited_routes set ensures that a route is not visited
multiple times. The time complexity is O(N + R^2) where N is the total number of stops and R is the
total number of routes. The space complexity is O(N + R^2) due to the mapping and visited_routes
set.

---
---
---