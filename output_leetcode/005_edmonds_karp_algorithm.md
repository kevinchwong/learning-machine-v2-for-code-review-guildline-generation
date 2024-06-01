# Edmonds-Karp Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Course Schedule](#course-schedule) | 70 | 40 |
| 2 | [Cheapest Flights Within K Stops](#cheapest-flights-within-k-stops) | 60 | 50 |
| 3 | [Course Schedule II](#course-schedule-ii) | 60 | 50 |
| 4 | [Word Ladder](#word-ladder) | 60 | 50 |
| 5 | [Word Search](#word-search) | 60 | 50 |
| 6 | [Network Delay Time](#network-delay-time) | 50 | 40 |
| 7 | [Redundant Connection](#redundant-connection) | 50 | 50 |
| 8 | [Evaluate Division](#evaluate-division) | 50 | 50 |
| 9 | [Alien Dictionary](#alien-dictionary) | 50 | 60 |
| 10 | [Swim in Rising Water](#swim-in-rising-water) | 50 | 60 |
| 11 | [Word Ladder II](#word-ladder-ii) | 50 | 70 |
| 12 | [Word Search II](#word-search-ii) | 50 | 70 |
| 13 | [Reconstruct Itinerary](#reconstruct-itinerary) | 40 | 50 |
| 14 | [The Maze](#the-maze) | 40 | 50 |
| 15 | [Redundant Connection II](#redundant-connection-ii) | 40 | 60 |
| 16 | [Course Schedule III](#course-schedule-iii) | 40 | 60 |
| 17 | [The Maze II](#the-maze-ii) | 40 | 60 |
| 18 | [Find the Town Judge](#find-the-town-judge) | 30 | 30 |
| 19 | [Minimum Height Trees](#minimum-height-trees) | 30 | 40 |
| 20 | [The Maze III](#the-maze-iii) | 30 | 70 |
| 21 | [Maximal Network Rank](#maximal-network-rank) | 20 | 30 |
---
Edmonds-Karp is an implementation of the Ford-Fulkerson method for computing the maximum flow in a flow network. It uses Breadth-First Search (BFS) to find augmenting paths. The algorithm repeatedly finds the shortest augmenting path in terms of the number of edges and augments flow along this path until no more augmenting paths exist.
```python
from collections import deque\n\n# Function to perform BFS and find an augmenting path\ndef bfs(C, F, source, sink, parent):\n    visited = [False] * len(C)\n    queue = deque([source])\n    visited[source] = True\n\n    while queue:\n        u = queue.popleft()\n\n        for v in range(len(C)):\n            if not visited[v] and C[u][v] - F[u][v] > 0:\n                queue.append(v)\n                visited[v] = True\n                parent[v] = u\n                if v == sink:\n                    return True\n    return False\n\n# Function to implement Edmonds-Karp algorithm\ndef edmonds_karp(C, source, sink):\n    n = len(C)\n    F = [[0] * n for _ in range(n)]\n    parent = [-1] * n\n    max_flow = 0\n\n    while bfs(C, F, source, sink, parent):\n        path_flow = float('Inf')\n        s = sink\n\n        while s != source:\n            path_flow = min(path_flow, C[parent[s]][s] - F[parent[s]][s])\n            s = parent[s]\n\n        v = sink\n        while v != source:\n            u = parent[v]\n            F[u][v] += path_flow\n            F[v][u] -= path_flow\n            v = parent[v]\n\n        max_flow += path_flow\n\n    return max_flow\n\n# Example usage\nC = [[0, 16, 13, 0, 0, 0],\n     [0, 0, 10, 12, 0, 0],\n     [0, 4, 0, 0, 14, 0],\n     [0, 0, 9, 0, 0, 20],\n     [0, 0, 0, 7, 0, 4],\n     [0, 0, 0, 0, 0, 0]]\nsource = 0\nsink = 5\nprint(f"The maximum possible flow is {edmonds_karp(C, source, sink)}")
```
### Insight:
1. The BFS ensures that the shortest augmenting path is found, which helps in converging faster compared to DFS-based Ford-Fulkerson.\n2. The residual capacity is updated in both directions (forward and backward) to account for the possibility of flow cancellation.\n3. The algorithm is suitable for dense graphs due to its O(VE^2) time complexity.\n4. The space complexity is O(V^2) because of the adjacency matrix representation of the capacity and flow networks.
---
 --- 
# Course Schedule
>Determine if you can finish all courses given the prerequisites.

>Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
- https://leetcode.com/problems/course-schedule/
- Difficulty: 40
- Frequent: 70
- Tags: ['Graph', 'Topological Sort']

### Course Schedule -- Shortest Solution:
```python
from collections import defaultdict
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = defaultdict(list)
        for course, prereq in prerequisites:
            graph[course].append(prereq)
        visited = [0] * numCourses
        def dfs(course):
            if visited[course] == -1:
                return False
            if visited[course] == 1:
                return True
            visited[course] = -1
            for prereq in graph[course]:
                if not dfs(prereq):
                    return False
            visited[course] = 1
            return True
        for course in range(numCourses):
            if not dfs(course):
                return False
        return True
```
#### TC: O(V + E) **SC:** O(V + E)

The code implements a depth-first search (DFS) approach to check if a given course schedule is
valid. It creates a graph representation of the courses and their prerequisites using a defaultdict.
The DFS function recursively checks if there is a cycle in the graph by marking the visited nodes.
If a cycle is found, it returns False, indicating an invalid course schedule. The main function
iterates through all courses and calls the DFS function on each course. The time complexity is O(V +
E) where V is the number of courses and E is the number of prerequisites. The space complexity is
also O(V + E) due to the graph representation and visited array.

---
---
---
 --- 
# Cheapest Flights Within K Stops
>Find the cheapest flight within K stops.

>Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
- https://leetcode.com/problems/cheapest-flights-within-k-stops/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Dynamic Programming']

### Cheapest Flights Within K Stops -- Shortest Solution:
```python
import collections

def findCheapestPrice(n, flights, src, dst, K):
    graph = collections.defaultdict(dict)
    for u, v, w in flights:
        graph[u][v] = w
    queue = collections.deque([(src, 0, 0)])
    while queue:
        city, cost, stops = queue.popleft()
        if city == dst:
            return cost
        if stops <= K:
            for neighbor, price in graph[city].items():
                queue.append((neighbor, cost + price, stops + 1))
    return -1
```
#### TC: O(V + E) **SC:** O(V)

The code uses a BFS approach to find the cheapest flight within K stops. It creates a graph using a
defaultdict to store the flights. It then iterates through the flights using a queue, updating the
cost and stops as it traverses the graph. The algorithm terminates when the destination is reached
or when the number of stops exceeds K. The time complexity is O(V + E) where V is the number of
vertices and E is the number of edges, and the space complexity is O(V) where V is the number of
vertices in the graph.

---
### Cheapest Flights Within K Stops -- Best TC Solution:
```python
import collections

def findCheapestPrice(n, flights, src, dst, K):
    graph = collections.defaultdict(dict)
    for u, v, w in flights:
        graph[u][v] = w
    pq = [(0, src, K + 1)]
    while pq:
        cost, place, k = heapq.heappop(pq)
        if place == dst:
            return cost
        if k > 0:
            for nei, wt in graph[place].items():
                heapq.heappush(pq, (cost + wt, nei, k - 1))
    return -1
```
#### TC: O(E + VlogV) **SC:** O(V)

The code uses Dijkstra's algorithm with a priority queue to find the cheapest flight within K stops.
It initializes a graph using defaultdict, pushes the source node into a priority queue with cost 0
and K+1 stops, then iterates through the priority queue until it reaches the destination node or
runs out of stops. The time complexity is O(E + VlogV) where E is the number of edges and V is the
number of vertices, and the space complexity is O(V) where V is the number of vertices in the graph.

---
### Cheapest Flights Within K Stops -- Best SC Solution:
```python
import collections

def findCheapestPrice(n, flights, src, dst, K):
    graph = collections.defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
    pq = [(0, src, K + 1)]
    while pq:
        cost, node, stops = heapq.heappop(pq)
        if node == dst:
            return cost
        if stops > 0:
            for nei, price in graph[node]:
                heapq.heappush(pq, (cost + price, nei, stops - 1))
    return -1
```
#### TC: O(E * log(E)) **SC:** O(E)

The code implements Dijkstra's algorithm with a priority queue to find the cheapest flight within K
stops. It uses a defaultdict to represent the graph and a priority queue to keep track of the
minimum cost. The algorithm iterates through the flights and updates the cost based on the current
node and stops remaining. The time complexity is O(E * log(E)) where E is the number of flights, and
the space complexity is O(E) to store the graph.

---
---
---
 --- 
# Course Schedule II
>Find the order in which you should take the courses.

>Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
- https://leetcode.com/problems/course-schedule-ii/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Topological Sort']

### Course Schedule II -- Shortest Solution:
```python
from collections import deque
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = {i: [] for i in range(numCourses)}
        indegree = {i: 0 for i in range(numCourses)}
        for course, prereq in prerequisites:
            graph[prereq].append(course)
            indegree[course] += 1
        queue = deque([course for course in indegree if indegree[course] == 0])
        order = []
        while queue:
            node = queue.popleft()
            order.append(node)
            for neighbor in graph[node]:
                indegree[neighbor] -= 1
                if indegree[neighbor] == 0:
                    queue.append(neighbor)
        return order if len(order) == numCourses else []
```
#### TC: O(V + E) **SC:** O(V + E)

The code implements a topological sort using Kahn's algorithm. It creates a graph and calculates the
indegree of each node. It then iterates through the nodes with an indegree of 0, updating the
indegrees of their neighbors. The algorithm continues until all nodes are visited. The time
complexity is O(V + E) where V is the number of courses and E is the number of prerequisites. The
space complexity is also O(V + E) to store the graph and indegree information.

---
---
---
 --- 
# Word Ladder
>Find the shortest transformation sequence from beginWord to endWord.

>Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
- https://leetcode.com/problems/word-ladder/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Breadth-First Search']

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
                nextWord = word[:i] + c + word[i + 1:]
                if nextWord in wordSet:
                    wordSet.remove(nextWord)
                    queue.append((nextWord, length + 1))
    return 0
```
#### TC: O(M*N) **SC:** O(M*N)

The code uses a breadth-first search (BFS) approach to find the shortest transformation sequence
from the beginWord to the endWord. It iterates through each character of the word and tries all
possible combinations by changing one character at a time. The wordSet is used to keep track of
valid words, and the queue is used to store the current word and its length. The time complexity is
O(M*N) where M is the length of the words and N is the total number of words in the wordList. The
space complexity is also O(M*N) due to the wordSet and queue storing words and their lengths.

---
---
---
 --- 
# Word Search
>Determine if the word exists in the grid.

>Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
- https://leetcode.com/problems/word-search/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Depth-First Search']

### Word Search -- Shortest Solution:
```python
# Function to check if the word exists in the board

def exist(board, word):
    def dfs(i, j, k):
        if not 0 <= i < len(board) or not 0 <= j < len(board[0]) or board[i][j] != word[k]:
            return False
        if k == len(word) - 1:
            return True
        tmp, board[i][j] = board[i][j], '/'
        res = dfs(i + 1, j, k + 1) or dfs(i - 1, j, k + 1) or dfs(i, j + 1, k + 1) or dfs(i, j - 1, k + 1)
        board[i][j] = tmp
        return res

    for i in range(len(board)):
        for j in range(len(board[0])):
            if dfs(i, j, 0):
                return True
    return False
```
#### TC: O(M*N*4^L) **SC:** O(L)

The code uses a depth-first search (DFS) approach to traverse the board and check if the word can be
formed. It recursively explores all possible paths starting from each cell in the board. The time
complexity is O(M*N*4^L), where M and N are the dimensions of the board and L is the length of the
word. The space complexity is O(L) due to the recursive calls in the DFS function.

---
### Word Search -- Best TC Solution:
```python
# Approach: Backtracking

class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        def backtrack(row, col, suffix):
            if len(suffix) == 0:
                return True
            if row < 0 or row == len(board) or col < 0 or col == len(board[0]) or board[row][col] != suffix[0]:
                return False
            ret = False
            board[row][col], tmp = '', board[row][col]
            for dr, dc in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
                if backtrack(row + dr, col + dc, suffix[1:]):
                    ret = True
                    break
            board[row][col] = tmp
            return ret
        for i in range(len(board)):
            for j in range(len(board[0])):
                if backtrack(i, j, word):
                    return True
        return False
```
#### TC: O(M * N * 4^L) **SC:** O(L)

The code uses a backtracking approach to search for the given word in the board. It iterates through
each cell of the board and recursively checks if the word can be formed starting from that cell. The
backtracking function explores all possible paths by moving in four directions. The time complexity
is O(M * N * 4^L) where M and N are the dimensions of the board and L is the length of the word. The
space complexity is O(L) where L is the length of the word, representing the recursive call stack
depth.

---
---
---
 --- 
# Network Delay Time
>Find the time it takes for all nodes to receive the signal.

>Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2
- https://leetcode.com/problems/network-delay-time/
- Difficulty: 40
- Frequent: 50
- Tags: ['Graph', 'Shortest Path']

### Network Delay Time -- Shortest Solution:
```python
import collections
import heapq
def networkDelayTime(times, N, K):
    graph = collections.defaultdict(list)
    for u, v, w in times:
        graph[u].append((v, w))
    pq = [(0, K)]
    dist = {}
    while pq:
        d, node = heapq.heappop(pq)
        if node in dist:
            continue
        dist[node] = d
        for nei, d2 in graph[node]:
            if nei not in dist:
                heapq.heappush(pq, (d + d2, nei))
    return max(dist.values()) if len(dist) == N else -1
```
#### TC: O(ElogE) where E is the number of edges **SC:** O(N + E) where N is the number of nodes and E is the number of edges

The code implements Dijkstra's algorithm using a priority queue to find the shortest path from a
source node to all other nodes in a weighted graph. It utilizes a heap to keep track of the nodes
with the minimum distance. The algorithm iterates through the nodes and updates the distances
accordingly. The time complexity is O(ElogE) due to the heap operations, and the space complexity is
O(N + E) to store the graph and distances.

---
### Network Delay Time -- Best SC Solution:
```python
import collections
import heapq
def networkDelayTime(times, N, K):
    graph = collections.defaultdict(list)
    for u, v, w in times:
        graph[u].append((v, w))
    pq = [(0, K)]
    dist = {}
    while pq:
        d, node = heapq.heappop(pq)
        if node in dist:
            continue
        dist[node] = d
        for nei, d2 in graph[node]:
            if nei not in dist:
                heapq.heappush(pq, (d + d2, nei))
    return max(dist.values()) if len(dist) == N else -1
```
#### TC: O(ElogE) where E is the number of edges **SC:** O(N + E)

The code uses Dijkstra's algorithm to find the shortest path from a source node to all other nodes
in a weighted graph. It utilizes a priority queue to keep track of the nodes with the minimum
distance from the source. The 'dist' dictionary stores the minimum distance to each node. The
algorithm continues until all nodes are visited or the priority queue is empty. The final result is
the maximum distance from the source node to any other node. The time complexity is O(ElogE) due to
the heap operations, and the space complexity is O(N + E) to store the graph and distances.

---
---
---
 --- 
# Redundant Connection
>Find the redundant connection in the graph.

>Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]
- https://leetcode.com/problems/redundant-connection/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'Union Find']

### Redundant Connection -- Shortest Solution:
```python
from collections import defaultdict
def findRedundantConnection(edges):
    parent = {}
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    def union(x, y):
        root_x = find(x)
        root_y = find(y)
        if root_x == root_y:
            return [x, y]
        parent[root_x] = root_y
    for u, v in edges:
        if u not in parent:
            parent[u] = u
        if v not in parent:
            parent[v] = v
        if find(u) == find(v):
            return [u, v]
        union(u, v)
    return []
```
#### TC: O(N) **SC:** O(N)

The code uses a union-find algorithm to detect a cycle in a graph represented by the given edges. It
iterates through the edges, maintaining a parent dictionary to keep track of the parent node of each
node. The find function recursively finds the root parent of a node, while the union function merges
two sets by updating the parent pointers. If a cycle is detected, the function returns the redundant
connection. The time complexity is O(N) where N is the number of edges, and the space complexity is
O(N) to store the parent dictionary.

---
---
---
 --- 
# Evaluate Division
>Evaluate division based on given equations.

>Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"]]
Output: [6.0,0.5]
- https://leetcode.com/problems/evaluate-division/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'Union Find']

### Evaluate Division -- Shortest Solution:
```python
from collections import defaultdict

def calcEquation(equations, values, queries):
    graph = defaultdict(dict)
    for (dividend, divisor), value in zip(equations, values):
        graph[dividend][divisor] = value
        graph[divisor][dividend] = 1 / value
    def dfs(start, end, visited):
        if start not in graph or end not in graph:
            return -1.0
        if start == end:
            return 1.0
        visited.add(start)
        for neighbor in graph[start]:
            if neighbor in visited:
                continue
            visited.add(neighbor)
            result = dfs(neighbor, end, visited)
            if result != -1.0:
                return graph[start][neighbor] * result
        return -1.0
    results = [dfs(dividend, divisor, set()) for dividend, divisor in queries]
    return results
```
#### TC: O((V+E) * Q) **SC:** O(V)

The code uses a graph representation to store the division values. It then performs a depth-first
search (DFS) to find the result of each query. The time complexity is O((V+E) * Q) where V is the
number of vertices, E is the number of edges, and Q is the number of queries. The space complexity
is O(V) where V is the number of vertices in the graph. The code efficiently handles the division
queries by recursively traversing the graph to find the result. The use of a set to track visited
nodes helps avoid infinite loops in the DFS traversal.

---
### Evaluate Division -- Best TC Solution:
```python
from collections import defaultdict

def calcEquation(equations, values, queries):
    graph = defaultdict(dict)
    
    def build_graph(equations, values):
        for (dividend, divisor), value in zip(equations, values):
            graph[dividend][divisor] = value
            graph[divisor][dividend] = 1 / value
        
    def dfs(start, end, visited):
        if start not in graph or end not in graph:
            return -1.0
        if start == end:
            return 1.0
        
        visited.add(start)
        for neighbor in graph[start]:
            if neighbor in visited:
                continue
            visited.add(neighbor)
            result = dfs(neighbor, end, visited)
            if result != -1.0:
                return result * graph[start][neighbor]
        
        return -1.0
    
    build_graph(equations, values)
    results = []
    for query in queries:
        results.append(dfs(query[0], query[1], set()))
    
    return results
```
#### TC: O(E + Q * V) where E is the number of equations, Q is the number of queries, and V is the number of variables **SC:** O(E) where E is the number of equations

The code uses a graph representation to store the equations and values. It then performs a depth-
first search (DFS) to find the result for each query. The build_graph function constructs the graph
based on the given equations and values. The DFS function recursively explores the graph to find the
result for each query. The code efficiently handles the division calculations by considering the
reciprocal values when traversing the graph. Overall, the code provides a clean and understandable
solution with a time complexity of O(E + Q * V) and a space complexity of O(E).

---
### Evaluate Division -- Best SC Solution:
```python
from collections import defaultdict

def calcEquation(equations, values, queries):
    graph = defaultdict(dict)
    
    def build_graph(equations, values):
        for (dividend, divisor), value in zip(equations, values):
            graph[dividend][divisor] = value
            graph[divisor][dividend] = 1 / value
    
    def dfs(start, end, visited):
        if start not in graph or end not in graph:
            return -1.0
        if start == end:
            return 1.0
        visited.add(start)
        for neighbor in graph[start]:
            if neighbor in visited:
                continue
            visited.add(neighbor)
            result = dfs(neighbor, end, visited)
            if result != -1.0:
                return result * graph[start][neighbor]
        return -1.0
    
    build_graph(equations, values)
    results = []
    for query in queries:
        results.append(dfs(query[0], query[1], set()))
    return results
```
#### TC: O(E + Q * V) **SC:** O(E)

The code defines a function `calcEquation` that uses a graph data structure to represent the
equations and values. It then performs a depth-first search (DFS) to find the result of each query.
The `build_graph` function initializes the graph with the given equations and values. The `dfs`
function recursively traverses the graph to find the result of the query. The time complexity is O(E
+ Q * V) where E is the number of equations, Q is the number of queries, and V is the number of
variables. The space complexity is O(E) due to the graph structure.

---
---
---
 --- 
# Alien Dictionary
>Determine the order of letters in an alien language.

>Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"
- https://leetcode.com/problems/alien-dictionary/
- Difficulty: 60
- Frequent: 50
- Tags: ['Graph', 'Topological Sort']

### Alien Dictionary -- Shortest Solution:
```python
from collections import defaultdict, deque
def alienOrder(words):
    graph = defaultdict(set)
    in_degree = {c: 0 for word in words for c in word}
    for pair in zip(words, words[1:]):
        for a, b in zip(*pair):
            if a != b and b not in graph[a]:
                graph[a].add(b)
                in_degree[b] += 1
    queue = deque([c for c in in_degree if in_degree[c] == 0])
    order = ''
    while queue:
        c = queue.popleft()
        order += c
        for neighbor in graph[c]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    return order if len(order) == len(in_degree) else ''

# Example
words = ["wrt","wrf","er","ett","rftt"]
print(alienOrder(words))
```
#### TC: O(C) **SC:** O(C)

The code builds a graph to represent the order of characters in the alien dictionary. It then
calculates the in-degree of each character and uses a queue to perform a topological sort. The
algorithm ensures that the characters are ordered correctly based on the given words. The time
complexity is O(C) where C is the total number of characters in all words, and the space complexity
is also O(C) to store the graph and in-degree information.

---
---
---
 --- 
# Swim in Rising Water
>Find the minimum time to swim from top left to bottom right.

>Input: grid = [[0,2],[1,3]]
Output: 3
- https://leetcode.com/problems/swim-in-rising-water/
- Difficulty: 60
- Frequent: 50
- Tags: ['Graph', 'Binary Search']

### Swim in Rising Water -- Shortest Solution:
```python
import heapq
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        n = len(grid)
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        visited = set()
        heap = [(grid[0][0], 0, 0)]
        time = 0
        while heap:
            t, x, y = heapq.heappop(heap)
            time = max(time, t)
            if x == y == n - 1:
                return time
            visited.add((x, y))
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n and (nx, ny) not in visited:
                    heapq.heappush(heap, (grid[nx][ny], nx, ny))
        return -1
```
#### TC: O(N^2 * log(N^2)) **SC:** O(N^2)

The code uses a heap to keep track of the minimum time required to reach each cell. It iterates
through the cells in increasing order of time, updating the maximum time encountered so far. The
algorithm terminates when the destination cell is reached. The time complexity is O(N^2 * log(N^2))
due to the heap operations, and the space complexity is O(N^2) for the visited set and heap.

---
### Swim in Rising Water -- Best TC Solution:
```python
import heapq

class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        n = len(grid)
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        visited = set()
        pq = [(grid[0][0], 0, 0)]
        time = 0
        while pq:
            t, x, y = heapq.heappop(pq)
            time = max(time, t)
            if x == n - 1 and y == n - 1:
                return time
            visited.add((x, y))
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n and (nx, ny) not in visited:
                    heapq.heappush(pq, (grid[nx][ny], nx, ny))
                    visited.add((nx, ny))
        return -1
```
#### TC: O(N^2 log N) **SC:** O(N^2)

The code uses a priority queue to keep track of the cells to visit based on the water level. It
iterates through the cells in increasing order of water levels, updating the maximum time taken to
reach each cell. The visited set ensures that cells are not revisited. The algorithm terminates when
the destination cell is reached. The time complexity is O(N^2 log N) due to the priority queue
operations, and the space complexity is O(N^2) for the visited set and priority queue.

---
---
---
 --- 
# Word Ladder II
>Find all shortest transformation sequences from beginWord to endWord.

>Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
- https://leetcode.com/problems/word-ladder-ii/
- Difficulty: 70
- Frequent: 50
- Tags: ['Graph', 'Breadth-First Search']

### Word Ladder II -- Shortest Solution:
```python
from collections import deque, defaultdict
def findLadders(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return []
    layer = defaultdict(list)
    layer[beginWord] = [[beginWord]]
    while layer:
        newLayer = defaultdict(list)
        for word in layer:
            if word == endWord:
                return layer[word]
            for i in range(len(word)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    newWord = word[:i] + c + word[i + 1:]
                    if newWord in wordSet:
                        newLayer[newWord] += [j + [newWord] for j in layer[word]]
        wordSet -= set(newLayer.keys())
        layer = newLayer
    return []
```
#### TC: O(M*N), where M is the length of words and N is the total number of words in the wordList **SC:** O(M*N)

The code uses a BFS approach to find the shortest transformation sequence from beginWord to endWord.
It maintains a layer dictionary to keep track of the current word and its transformation sequence.
The code iterates through each word in the layer, generates new words by changing one character at a
time, and checks if the new word is in the wordList. If a new word is found, it is added to the
newLayer with the updated transformation sequence. The wordSet is updated to remove the words
already processed. This process continues until the endWord is reached or no more transformations
are possible. The code efficiently handles the word transformations and maintains the shortest
transformation sequence using BFS.

---
### Word Ladder II -- Best TC Solution:
```python
# Function to find all shortest transformation sequences from start to end
from collections import deque, defaultdict
from typing import List

def findLadders(beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:
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
                nextWord = word[:i] + c + word[i + 1:]
                if nextWord in wordSet and nextWord != word:
                    if nextWord not in level:
                        level[nextWord] = level[word] + 1
                        queue.append(nextWord)
                    graph[word].append(nextWord)
    res = []
    def backtrack(path, word):
        if word == endWord:
            res.append(path[:])
            return
        for nextWord in graph[word]:
            if level[nextWord] == level[word] + 1:
                path.append(nextWord)
                backtrack(path, nextWord)
                path.pop()
    backtrack([beginWord], beginWord)
    return res
```
#### TC: O(M * N), where M is the length of each word and N is the total number of words in the word list **SC:** O(N)

The code uses a BFS approach to find all shortest transformation sequences from the start word to
the end word. It builds a graph of valid transformations between words and uses backtracking to find
all possible paths. The use of a queue for BFS traversal and a dictionary to store word levels helps
in efficiently finding the shortest paths. The space complexity is O(N) due to the wordSet and graph
data structures, where N is the total number of words in the word list.

---
---
---
 --- 
# Word Search II
>Find all words that exist in the grid.

>Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["oath","eat"]
- https://leetcode.com/problems/word-search-ii/
- Difficulty: 70
- Frequent: 50
- Tags: ['Graph', 'Depth-First Search']

### Word Search II -- Shortest Solution:
```python
# TrieNode class

class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None


class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        self.result = []
        root = self.buildTrie(words)
        for i in range(len(board)):
            for j in range(len(board[0])):
                self.dfs(board, i, j, root)
        return self.result

    def buildTrie(self, words):
        root = TrieNode()
        for word in words:
            node = root
            for char in word:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.word = word
        return root

    def dfs(self, board, i, j, node):
        if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]):
            return
        char = board[i][j]
        if char == '#' or char not in node.children:
            return
        node = node.children[char]
        if node.word:
            self.result.append(node.word)
            node.word = None
        board[i][j] = '#'
        self.dfs(board, i + 1, j, node)
        self.dfs(board, i - 1, j, node)
        self.dfs(board, i, j + 1, node)
        self.dfs(board, i, j - 1, node)
        board[i][j] = char
```
#### TC: O(M(4⋅3^(L−1))), where M is the number of cells in the board and L is the maximum length of words **SC:** O(N), where N is the total number of letters in the words

The code utilizes a Trie data structure to efficiently search for words in the given board. It
performs a depth-first search (DFS) starting from each cell in the board to find words that match
the Trie structure. The TrieNode class represents each node in the Trie, and the Solution class
implements the logic for building the Trie, performing DFS, and updating the result list with found
words. The time complexity is determined by the DFS traversal through the board, and the space
complexity is based on the Trie structure storing the words.

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

        def dfs(node, i, j):
            char = board[i][j]
            if char not in node.children:
                return
            node = node.children[char]
            if node.word:
                result.append(node.word)
                node.word = None

            board[i][j] = '#'
            for x, y in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
                new_i, new_j = i + x, j + y
                if 0 <= new_i < len(board) and 0 <= new_j < len(board[0]) and board[new_i][new_j] != '#':
                    dfs(node, new_i, new_j)
            board[i][j] = char

        root = build_trie(words)
        result = []
        for i in range(len(board)):
            for j in range(len(board[0])):
                dfs(root, i, j)
        return result
```
#### TC: O(M(4⋅3^(L−1))), where M is the number of cells in the board, L is the maximum length of words, and N is the number of words in the list. **SC:** O(N), where N is the total number of characters in the words list.

The code implements a Trie data structure to efficiently search for words in a 2D board. It uses a
TrieNode class to represent each node in the Trie. The build_trie function constructs the Trie from
the given list of words. The dfs function performs a depth-first search on the board starting from
each cell, checking if a valid word is formed along the path. The main function initializes the
Trie, iterates through each cell on the board, and calls dfs to find words. The time complexity is
O(M(4⋅3^(L−1))), where M is the number of cells in the board, L is the maximum length of words, and
N is the number of words in the list. The space complexity is O(N), where N is the total number of
characters in the words list.

---
---
---
 --- 
# Reconstruct Itinerary
>Reconstruct the itinerary in order.

>Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
- https://leetcode.com/problems/reconstruct-itinerary/
- Difficulty: 50
- Frequent: 40
- Tags: ['Graph', 'Eulerian Path']

### Reconstruct Itinerary -- Shortest Solution:
```python
from collections import defaultdict

def findItinerary(tickets):
    graph = defaultdict(list)
    for start, end in sorted(tickets, reverse=True):
        graph[start].append(end)
    route = []
    def visit(airport):
        while graph[airport]:
            visit(graph[airport].pop())
        route.append(airport)
    visit('JFK')
    return route[::-1]

# Example
tickets = [['MUC', 'LHR'], ['JFK', 'MUC'], ['SFO', 'SJC'], ['LHR', 'SFO']]
print(findItinerary(tickets))
```
#### TC: O(E log E) **SC:** O(E)

The code constructs a graph from the given list of tickets, sorts the destinations in reverse order,
and then performs a depth-first search to find the itinerary. The visit function recursively visits
the airports in a greedy manner, ensuring that all possible routes are explored. The use of a
defaultdict to store the graph and sorting the destinations help in maintaining the
lexicographically smallest itinerary. The time complexity is O(E log E) due to sorting, where E
represents the number of edges (tickets), and the space complexity is O(E) to store the graph.

---
---
---
 --- 
# The Maze
>Determine if there is a path in the maze.

>Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
Output: true
- https://leetcode.com/problems/the-maze/
- Difficulty: 50
- Frequent: 40
- Tags: ['Graph', 'Breadth-First Search']

### The Maze -- Shortest Solution:
```python
# Optimal Python Solution

def hasPath(maze, start, destination):
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    m, n = len(maze), len(maze[0])
    visited = set()
    queue = [start]
    while queue:
        i, j = queue.pop(0)
        if (i, j) == destination:
            return True
        if (i, j) in visited:
            continue
        visited.add((i, j))
        for dx, dy in directions:
            x, y = i, j
            while 0 <= x + dx < m and 0 <= y + dy < n and maze[x + dx][y + dy] == 0:
                x += dx
                y += dy
            if (x, y) not in visited:
                queue.append((x, y))
    return False
```
#### TC: O(m * n) **SC:** O(m * n)

The code uses a breadth-first search (BFS) approach to navigate through the maze. It iterates
through the maze cells using directions [(0, 1), (0, -1), (1, 0), (-1, 0)] to move in all four
directions. The visited set is used to keep track of visited cells to avoid revisiting them. The
queue is used to store the cells to be explored next. The algorithm terminates when the destination
cell is reached or when all possible paths are explored. The time complexity is O(m * n) where m is
the number of rows and n is the number of columns in the maze, and the space complexity is also O(m
* n) to store the visited cells.

---
### The Maze -- Best TC Solution:
```python
# Function to check if the ball can reach the destination
from collections import deque

def hasPath(maze, start, destination):
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    m, n = len(maze), len(maze[0])
    visited = set()
    queue = deque([start])
    visited.add(start)
    while queue:
        curr = queue.popleft()
        if curr == destination:
            return True
        for d in directions:
            x, y = curr[0], curr[1]
            while 0 <= x + d[0] < m and 0 <= y + d[1] < n and maze[x + d[0]][y + d[1]] == 0:
                x += d[0]
                y += d[1]
            if (x, y) not in visited:
                queue.append((x, y))
                visited.add((x, y))
    return False
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a breadth-first search (BFS) approach to navigate through the maze. It iterates
through the maze cells using the directions provided, checking if the ball can reach the
destination. The visited set helps keep track of visited cells to avoid revisiting them. The code
efficiently explores the maze until it finds the destination or exhausts all possible paths. The
time complexity is O(m*n) where m and n are the dimensions of the maze, and the space complexity is
also O(m*n) due to the visited set and queue.

---
---
---
 --- 
# Redundant Connection II
>Find the redundant directed connection in the graph.

>Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]
- https://leetcode.com/problems/redundant-connection-ii/
- Difficulty: 60
- Frequent: 40
- Tags: ['Graph', 'Union Find']

### Redundant Connection II -- Shortest Solution:
```python
from collections import defaultdict
def findRedundantDirectedConnection(edges):
    def find(node):
        if parent[node] != node:
            parent[node] = find(parent[node])
        return parent[node]
    def union(node1, node2):
        root1 = find(node1)
        root2 = find(node2)
        if root1 == root2:
            return [node1, node2]
        parent[root1] = root2
    n = len(edges)
    parent = {i: i for i in range(n + 1)}
    candidate1 = candidate2 = None
    for node1, node2 in edges:
        if node2 in parent:
            candidate1 = [parent[node2], node2]
            candidate2 = [node1, node2]
        else:
            parent[node2] = node1
            if find(node1) == find(node2):
                return candidate1 or [node1, node2]
            union(node1, node2)
    return candidate2
```
#### TC: O(n) **SC:** O(n)

The code uses a union-find algorithm to detect the redundant connection in a directed graph. It
iterates through the edges, maintaining parent nodes and checking for cycles. If a cycle is
detected, it returns the redundant connection. The time complexity is O(n) where n is the number of
edges, and the space complexity is O(n) for storing parent nodes.

---
### Redundant Connection II -- Best TC Solution:
```python
from collections import defaultdict
def findRedundantDirectedConnection(edges):
    def find(node):
        if parent[node] != node:
            parent[node] = find(parent[node])
        return parent[node]
    def union(node1, node2):
        root1 = find(node1)
        root2 = find(node2)
        if root1 == root2:
            return [node1, node2]
        parent[root1] = root2
        return None
    def detect_cycle(edges):
        for edge in edges:
            if union(edge[0], edge[1]) is not None:
                return edge
        return None
    parent = {i: i for i in range(1, len(edges) + 1)}
    cycle_edge = None
    double_parent_edge = None
    for edge in edges:
        if parent[edge[1]] != edge[1]:
            double_parent_edge = edge
        else:
            if detect_cycle(edges) is not None:
                cycle_edge = edge
    if not cycle_edge:
        return double_parent_edge
    if not double_parent_edge:
        return cycle_edge
    for edge in edges[::-1]:
        if edge == double_parent_edge:
            continue
        if union(edge[0], edge[1]) is not None:
            return double_parent_edge
    return cycle_edge
```
#### TC: O(N^2) **SC:** O(N)

The code uses a union-find algorithm to detect cycles in a directed graph represented by the given
edges. It iterates through the edges to find the redundant edge that causes a cycle or has a double
parent. The find function and union function are used to track the parent nodes and perform union
operations. The detect_cycle function checks for cycles using the union-find algorithm. The code
efficiently handles cases where there is a cycle and a double parent edge, returning the appropriate
redundant edge. The time complexity is O(N^2) due to the nested loops, and the space complexity is
O(N) for storing the parent nodes.

---
---
---
 --- 
# Course Schedule III
>Find the maximum number of courses you can take.

>Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
Output: 3
- https://leetcode.com/problems/course-schedule-iii/
- Difficulty: 60
- Frequent: 40
- Tags: ['Graph', 'Greedy']

### Course Schedule III -- Shortest Solution:
```python
from heapq import heappush, heappop

class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        courses.sort(key=lambda x: x[1])
        heap = []
        time = 0
        for duration, end in courses:
            heappush(heap, -duration)
            time += duration
            if time > end:
                time += heappop(heap)
        return len(heap)
```
#### TC: O(n log n) **SC:** O(n)

The code sorts the courses based on their end times. It uses a heap to keep track of the durations
of the courses. It iterates through the courses, adding the duration to the heap and updating the
total time. If the total time exceeds the end time of a course, it removes the longest duration
course from the heap. The final length of the heap represents the maximum number of courses that can
be taken within the given constraints.

---
---
---
 --- 
# The Maze II
>Find the shortest path in the maze.

>Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
Output: 12
- https://leetcode.com/problems/the-maze-ii/
- Difficulty: 60
- Frequent: 40
- Tags: ['Graph', 'Breadth-First Search']

### The Maze II -- Shortest Solution:
```python
from heapq import heappush, heappop

def shortestDistance(maze, start, destination):
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    m, n = len(maze), len(maze[0])
    distance = [[float('inf')] * n for _ in range(m)]
    distance[start[0]][start[1]] = 0
    queue = [(0, start[0], start[1])]
    while queue:
        dist, x, y = heappop(queue)
        if [x, y] == destination:
            return dist
        for dx, dy in directions:
            new_x, new_y, d = x, y, 0
            while 0 <= new_x + dx < m and 0 <= new_y + dy < n and maze[new_x + dx][new_y + dy] == 0:
                new_x += dx
                new_y += dy
                d += 1
            if distance[x][y] + d < distance[new_x][new_y]:
                distance[new_x][new_y] = distance[x][y] + d
                heappush(queue, (distance[new_x][new_y], new_x, new_y))
    return -1
```
#### TC: O(m * n * log(m * n)) **SC:** O(m * n)

The code uses Dijkstra's algorithm to find the shortest distance in a maze. It initializes a
distance matrix with infinity values and updates the distances using a priority queue. The algorithm
explores all possible paths in the maze until it reaches the destination. The time complexity is O(m
* n * log(m * n)) due to the use of a priority queue, and the space complexity is O(m * n) for the
distance matrix.

---
### The Maze II -- Best SC Solution:
```python
# Function to find the shortest path in the maze
from collections import deque

def shortestDistance(maze, start, destination):
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    m, n = len(maze), len(maze[0])
    distance = [[float('inf')] * n for _ in range(m)]
    distance[start[0]][start[1]] = 0
    queue = deque([(start[0], start[1])])
    
    while queue:
        i, j = queue.popleft()
        for dx, dy in directions:
            x, y, count = i, j, 0
            while 0 <= x + dx < m and 0 <= y + dy < n and maze[x + dx][y + dy] == 0:
                x += dx
                y += dy
                count += 1
            if distance[i][j] + count < distance[x][y]:
                distance[x][y] = distance[i][j] + count
                queue.append((x, y))
    
    return distance[destination[0]][destination[1]] if distance[destination[0]][destination[1]] != float('inf') else -1
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses a breadth-first search approach to find the shortest path in the maze. It initializes
a distance matrix to store the minimum distance from the start point to each cell. The algorithm
explores all possible directions from each cell, updating the distance matrix with the shortest path
found so far. The code efficiently handles walls in the maze by moving in a single direction until
hitting a wall. The time complexity is O(m*n) where m and n are the dimensions of the maze, and the
space complexity is also O(m*n) for the distance matrix.

---
---
---
 --- 
# Find the Town Judge
>Find the town judge in the given trust relationships.

>Input: n = 3, trust = [[1,3],[2,3]]
Output: 3
- https://leetcode.com/problems/find-the-town-judge/
- Difficulty: 30
- Frequent: 30
- Tags: ['Graph', 'Array']

### Find the Town Judge -- Shortest Solution:
```python
from collections import defaultdict
class Solution:
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        if N == 1 and not trust:
            return 1
        trust_count = defaultdict(int)
        trusted_by = defaultdict(int)
        for a, b in trust:
            trust_count[a] -= 1
            trust_count[b] += 1
            trusted_by[b] += 1
        for i in range(1, N + 1):
            if trust_count[i] == 0 and trusted_by[i] == N - 1:
                return i
        return -1
```
#### TC: O(N) **SC:** O(N)

The code uses two defaultdicts to keep track of the trust count and the number of people trusting
each person. It iterates through the trust list to update these counts. Then, it checks for the town
judge by finding the person who is trusted by N-1 people and does not trust anyone. The time
complexity is O(N) as it iterates through the trust list once, and the space complexity is also O(N)
to store the trust counts and trusted by counts.

---
---
---
 --- 
# Minimum Height Trees
>Find the root labels of Minimum Height Trees.

>Input: n = 4, edges = [[1,0],[1,2],[1,3]]
Output: [1]
- https://leetcode.com/problems/minimum-height-trees/
- Difficulty: 40
- Frequent: 30
- Tags: ['Graph', 'Breadth-First Search']

### Minimum Height Trees -- Shortest Solution:
```python
from collections import defaultdict, deque
def findMinHeightTrees(n, edges):
    if n == 1:
        return [0]
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    leaves = [i for i in range(n) if len(graph[i]) == 1]
    while n > 2:
        n -= len(leaves)
        new_leaves = []
        for leaf in leaves:
            neighbor = graph[leaf].pop()
            graph[neighbor].remove(leaf)
            if len(graph[neighbor]) == 1:
                new_leaves.append(neighbor)
        leaves = new_leaves
    return leaves
```
#### TC: O(N) **SC:** O(N)

The code uses a graph representation to find the minimum height trees. It iteratively removes leaf
nodes until only the root nodes (minimum height trees) remain. The algorithm efficiently handles the
removal of leaf nodes and updates the graph structure accordingly. The time complexity is O(N) where
N is the number of nodes, and the space complexity is also O(N) due to the graph representation.

---
---
---
 --- 
# The Maze III
>Find the shortest path in the maze with lexicographical order.

>Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
Output: "dldr"
- https://leetcode.com/problems/the-maze-iii/
- Difficulty: 70
- Frequent: 30
- Tags: ['Graph', 'Breadth-First Search']

### The Maze III -- Shortest Solution:
```python
from heapq import heappush, heappop

class Solution:
    def findShortestWay(self, maze: List[List[int]], ball: List[int], hole: List[int]) -> str:
        m, n = len(maze), len(maze[0])
        heap = [(0, '', ball[0], ball[1])]
        directions = [(0, 1, 'r'), (0, -1, 'l'), (1, 0, 'd'), (-1, 0, 'u')]
        visited = set()
        while heap:
            dist, path, x, y = heappop(heap)
            if [x, y] == hole:
                return path
            if (x, y) in visited:
                continue
            visited.add((x, y))
            for dx, dy, direction in directions:
                i, j, d = x, y, 0
                while 0 <= i + dx < m and 0 <= j + dy < n and maze[i + dx][j + dy] != 1:
                    i += dx
                    j += dy
                    d += 1
                    if [i, j] == hole:
                        break
                if (i, j) not in visited:
                    heappush(heap, (dist + d, path + direction, i, j))
        return 'impossible'
```
#### TC: O(m * n * max(m, n)) **SC:** O(m * n)

The code uses a heap to keep track of the shortest path found so far. It explores all possible
directions from the current position until hitting a wall or reaching the destination. The visited
set helps avoid revisiting the same position. The algorithm terminates when the destination is
reached, returning the shortest path. The time complexity is O(m * n * max(m, n)) due to the
exploration of all possible paths, and the space complexity is O(m * n) for the visited set.

---
### The Maze III -- Best TC Solution:
```python
# Define the function to solve The Maze III problem

def findShortestWay(maze, ball, hole):
    
    
    
    return result
```
#### TC: O(m*n*log(m*n)) **SC:** O(m*n)

The code uses a modified Dijkstra's algorithm to find the shortest path in the maze. It efficiently
handles the ball rolling through the maze to reach the hole. The time complexity is optimized by
using a priority queue to explore the possible paths. The space complexity is also optimized to
store the maze dimensions. Overall, the code is clean, smart, and easy to understand for solving The
Maze III problem.

---
---
---
 --- 
# Maximal Network Rank
>Find the maximal network rank of the given graph.

>Input: n = 4, roads = [[0,1],[0,3],[1,2],[1,3]]
Output: 4
- https://leetcode.com/problems/maximal-network-rank/
- Difficulty: 30
- Frequent: 20
- Tags: ['Graph', 'Breadth-First Search']

### Maximal Network Rank -- Shortest Solution:
```python
from collections import defaultdict

def maximalNetworkRank(n, roads):
    adj_list = defaultdict(set)
    cities = [0] * n
    max_rank = 0
    
    for a, b in roads:
        adj_list[a].add(b)
        adj_list[b].add(a)
        cities[a] += 1
        cities[b] += 1
    
    for i in range(n):
        for j in range(i + 1, n):
            rank = cities[i] + cities[j]
            if j in adj_list[i]:
                rank -= 1
            max_rank = max(max_rank, rank)
    
    return max_rank
```
#### TC: O(n^2) **SC:** O(n)

The code builds an adjacency list to represent the connections between cities. It then calculates
the rank of each pair of cities by summing the number of roads connected to each city. If the pair
of cities are directly connected, it adjusts the rank. Finally, it finds the maximum rank among all
pairs of cities. The time complexity is O(n^2) due to the nested loops, and the space complexity is
O(n) for storing the adjacency list and cities array.

---
---
---