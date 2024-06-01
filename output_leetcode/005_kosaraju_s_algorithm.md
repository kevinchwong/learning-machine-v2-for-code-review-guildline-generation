# Kosaraju's Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Clone Graph](#clone-graph) | 65 | 50 |
| 2 | [Pacific Atlantic Water Flow](#pacific-atlantic-water-flow) | 60 | 55 |
---
Kosaraju's algorithm is used to find the strongly connected components (SCCs) of a directed graph. It involves two main steps: first, perform a depth-first search (DFS) to compute the finishing times of vertices, then reverse the graph and perform DFS in the order of decreasing finishing times to identify SCCs.
```python
from collections import defaultdict

class Graph:
    def __init__(self, vertices):
        self.graph = defaultdict(list)
        self.V = vertices

    def add_edge(self, u, v):
        self.graph[u].append(v)

    def _dfs(self, v, visited, stack):
        visited[v] = True
        for i in self.graph[v]:
            if not visited[i]:
                self._dfs(i, visited, stack)
        stack.append(v)

    def _reverse_graph(self):
        g = Graph(self.V)
        for i in self.graph:
            for j in self.graph[i]:
                g.add_edge(j, i)
        return g

    def _fill_order(self, v, visited, stack):
        visited[v] = True
        for i in self.graph[v]:
            if not visited[i]:
                self._fill_order(i, visited, stack)
        stack.append(v)

    def _dfs_util(self, v, visited):
        visited[v] = True
        print(v, end=' ')
        for i in self.graph[v]:
            if not visited[i]:
                self._dfs_util(i, visited)

    def print_sccs(self):
        stack = []
        visited = [False] * self.V
        for i in range(self.V):
            if not visited[i]:
                self._fill_order(i, visited, stack)
        gr = self._reverse_graph()
        visited = [False] * self.V
        while stack:
            i = stack.pop()
            if not visited[i]:
                gr._dfs_util(i, visited)
                print("")

g = Graph(5)
g.add_edge(1, 0)
g.add_edge(0, 2)
g.add_edge(2, 1)
g.add_edge(0, 3)
g.add_edge(3, 4)

print("Strongly Connected Components:")
g.print_sccs()
```
### Insight:
Kosaraju's algorithm is efficient for finding SCCs because it leverages two depth-first searches (DFS). The first DFS is used to determine the finishing times of vertices, and the second DFS is performed on the reversed graph in the order of decreasing finishing times. This ensures that each SCC is identified correctly. The algorithm's time complexity is linear, making it suitable for large graphs. The space complexity is also linear, primarily due to the storage required for the graph representation and the visited array.
---
 --- 
# Clone Graph
>Clone a graph.

>Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
- https://leetcode.com/problems/clone-graph/
- Difficulty: 50
- Frequent: 65
- Tags: ['Graph', 'DFS', "Kosaraju's Algorithm"]

### Clone Graph -- Shortest Solution:
```python
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = []):
        self.val = val
        self.neighbors = neighbors

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node:
            return None
        visited = {}
        def dfs(node):
            if node in visited:
                return visited[node]
            clone_node = Node(node.val, [])
            visited[node] = clone_node
            for neighbor in node.neighbors:
                clone_node.neighbors.append(dfs(neighbor))
            return clone_node
        return dfs(node)
```
#### TC: O(V + E) **SC:** O(V)

The code defines a Node class and a Solution class with a cloneGraph method. It uses depth-first
search (DFS) to clone the graph. The visited dictionary is used to keep track of visited nodes to
avoid infinite loops. The DFS function recursively clones each node and its neighbors. The time
complexity is O(V + E) where V is the number of vertices and E is the number of edges, and the space
complexity is O(V) where V is the number of vertices in the graph.

---
---
---
 --- 
# Pacific Atlantic Water Flow
>Find all cells that can flow to both the Pacific and Atlantic ocean.

>Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
- https://leetcode.com/problems/pacific-atlantic-water-flow/
- Difficulty: 55
- Frequent: 60
- Tags: ['Graph', 'DFS', "Kosaraju's Algorithm"]

### Pacific Atlantic Water Flow -- Shortest Solution:
```python
from typing import List
class Solution:
    def pacificAtlantic(self, matrix: List[List[int]]) -> List[List[int]]:
        if not matrix or not matrix[0]:
            return []
        rows, cols = len(matrix), len(matrix[0])
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        def dfs(r, c, ocean):
            ocean.add((r, c))
            for dr, dc in directions:
                new_r, new_c = r + dr, c + dc
                if 0 <= new_r < rows and 0 <= new_c < cols and (new_r, new_c) not in ocean and matrix[new_r][new_c] >= matrix[r][c]:
                    dfs(new_r, new_c, ocean)
        pacific = set()
        atlantic = set()
        for i in range(rows):
            dfs(i, 0, pacific)
            dfs(i, cols - 1, atlantic)
        for j in range(cols):
            dfs(0, j, pacific)
            dfs(rows - 1, j, atlantic)
        return list(pacific & atlantic)
```
#### TC: O(m*n) **SC:** O(m*n)

The code defines a Solution class with a pacificAtlantic method that takes a matrix as input. It
uses a depth-first search (DFS) approach to traverse the matrix from the Pacific and Atlantic
oceans. The DFS function recursively explores neighboring cells to determine if water can flow from
the current cell to the ocean. The code maintains two sets, pacific and atlantic, to store the
reachable cells from each ocean. Finally, it returns the intersection of cells reachable from both
oceans. The time complexity is O(m*n) where m is the number of rows and n is the number of columns
in the matrix. The space complexity is also O(m*n) to store the visited cells in the oceans.

---
---
---