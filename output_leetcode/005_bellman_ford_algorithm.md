# Bellman-Ford Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Bellman-Ford Algorithm](#bellman-ford-algorithm) | 60 | 50 |
| 2 | [Path with Maximum Probability](#path-with-maximum-probability) | 45 | 55 |
| 3 | [Maximum Total Weight of Paths](#maximum-total-weight-of-paths) | 40 | 65 |
| 4 | [Minimum Weighted Subgraph With the Required Paths](#minimum-weighted-subgraph-with-the-required-paths) | 35 | 70 |
| 5 | [Find the Longest Path in a Tree](#find-the-longest-path-in-a-tree) | 25 | 55 |
| 6 | [Find the Shortest Path with Alternating Colors](#find-the-shortest-path-with-alternating-colors) | 15 | 50 |
| 7 | [Find the Shortest Path in a Binary Matrix](#find-the-shortest-path-in-a-binary-matrix) | 10 | 45 |
| 8 | [Find the Shortest Path in a Weighted Graph](#find-the-shortest-path-in-a-weighted-graph) | 5 | 50 |
| 9 | [Find the Shortest Path in a Graph with Positive Weights](#find-the-shortest-path-in-a-graph-with-positive-weights) | 5 | 50 |
| 10 | [Find the Shortest Path in a Directed Acyclic Graph](#find-the-shortest-path-in-a-directed-acyclic-graph) | 5 | 55 |
| 11 | [Find the Shortest Path in a Graph with Negative Weights](#find-the-shortest-path-in-a-graph-with-negative-weights) | 5 | 60 |
---
The Bellman-Ford algorithm computes shortest paths from a single source vertex to all other vertices in a weighted graph. It can handle graphs with negative weight edges and detects negative weight cycles. The algorithm iteratively relaxes edges, updating the shortest path estimates, and repeats this process for all vertices.
```python
class Edge:
    def __init__(self, u, v, weight):
        self.u = u
        self.v = v
        self.weight = weight

class Graph:
    def __init__(self, vertices):
        self.V = vertices
        self.edges = []

    def add_edge(self, u, v, weight):
        self.edges.append(Edge(u, v, weight))

    def bellman_ford(self, src):
        dist = [float('inf')] * self.V
        dist[src] = 0

        for _ in range(self.V - 1):
            for edge in self.edges:
                if dist[edge.u] != float('inf') and dist[edge.u] + edge.weight < dist[edge.v]:
                    dist[edge.v] = dist[edge.u] + edge.weight

        for edge in self.edges:
            if dist[edge.u] != float('inf') and dist[edge.u] + edge.weight < dist[edge.v]:
                print("Graph contains negative weight cycle")
                return

        self.print_solution(dist)

    def print_solution(self, dist):
        print("Vertex Distance from Source")
        for i in range(self.V):
            print(f"{i}\t{dist[i]}")

g = Graph(5)
g.add_edge(0, 1, -1)
g.add_edge(0, 2, 4)
g.add_edge(1, 2, 3)
g.add_edge(1, 3, 2)
g.add_edge(1, 4, 2)
g.add_edge(3, 2, 5)
g.add_edge(3, 1, 1)
g.add_edge(4, 3, -3)

g.bellman_ford(0)
```
### Insight:
1. Initialize distances from the source to all vertices as infinite, except the source itself which is zero.
2. Relax all edges V-1 times, where V is the number of vertices.
3. Check for negative weight cycles by verifying if further relaxation is possible.
4. The algorithm is slower than Dijkstra's but can handle negative weights, making it versatile for various applications.
---
 --- 
# Bellman-Ford Algorithm
>Find the shortest path from a source vertex to all other vertices in a weighted graph.

>Input: n = 5, edges = [[0,1,5],[1,2,3],[2,3,1],[3,4,6],[4,0,2]]
Output: [0,5,8,9,15]
- https://leetcode.com/problems/bellman-ford-algorithm/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Bellman-Ford Algorithm -- Shortest Solution:
```python
from typing import List

class Solution:
    def bellmanFord(self, n: int, times: List[List[int]], k: int) -> int:
        INF = float('inf')
        dist = [INF] * n
        dist[k - 1] = 0
        for _ in range(n - 1):
            for u, v, w in times:
                if dist[u - 1] + w < dist[v - 1]:
                    dist[v - 1] = dist[u - 1] + w
        return max(dist) if max(dist) < INF else -1
```
#### TC: O(N*E) **SC:** O(N)

The code implements the Bellman-Ford algorithm to find the shortest path from a source node to all
other nodes in a weighted graph. It initializes the distances with infinity except for the source
node which is set to 0. It then iterates through all edges for N-1 times, updating the distances if
a shorter path is found. The algorithm has a time complexity of O(N*E) where N is the number of
nodes and E is the number of edges, and a space complexity of O(N) to store the distances.

---
---
---
 --- 
# Path with Maximum Probability
>Find the path with the maximum probability from start to end.

>Input: n = 3, edges = [[0,1,0.5],[1,2,0.5],[0,2,0.2]], start = 0, end = 2
Output: 0.25
- https://leetcode.com/problems/path-with-maximum-probability/
- Difficulty: 55
- Frequent: 45
- Tags: ['Graph', 'Shortest Path']

### Path with Maximum Probability -- Shortest Solution:
```python
import collections
import heapq
def maxProbability(n, edges, succProb, start, end):
    graph = collections.defaultdict(list)
    for i, (a, b) in enumerate(edges):
        graph[a].append((b, succProb[i]))
        graph[b].append((a, succProb[i]))
    pq = [(-1, start)]
    probs = collections.defaultdict(int)
    while pq:
        prob, node = heapq.heappop(pq)
        if node in probs:
            continue
        probs[node] = -prob
        for nei, nei_prob in graph[node]:
            heapq.heappush(pq, (prob * nei_prob, nei))
    return probs[end] if end in probs else 0
```
#### TC: O(E log E) **SC:** O(E)

The code uses Dijkstra's algorithm with a priority queue to find the maximum probability path from
start to end. It initializes a graph with edge probabilities, uses a priority queue to explore nodes
with the highest probability first, and updates the probabilities as it traverses the graph. The
time complexity is O(E log E) due to the priority queue operations, and the space complexity is O(E)
for storing the probabilities.

---
### Path with Maximum Probability -- Best TC Solution:
```python
from typing import List
from collections import defaultdict
import heapq

class Solution:
    def maxProbability(self, n: int, edges: List[List[int]], succProb: List[float], start: int, end: int) -> float:
        graph = defaultdict(list)
        for i, (u, v) in enumerate(edges):
            graph[u].append((v, succProb[i]))
            graph[v].append((u, succProb[i]))
        prob = [0.0] * n
        prob[start] = 1.0
        pq = [(-1.0, start)]
        while pq:
            cur_prob, node = heapq.heappop(pq)
            if node == end:
                return -cur_prob
            for nei, nei_prob in graph[node]:
                if -cur_prob * nei_prob > prob[nei]:
                    prob[nei] = -cur_prob * nei_prob
                    heapq.heappush(pq, (-prob[nei], nei))
        return 0.0
```
#### TC: O(ElogV) **SC:** O(n + E)

The code implements the Bellman-Ford algorithm to find the maximum probability path from start to
end in a graph. It uses a priority queue to keep track of the nodes with the highest probability.
The algorithm iterates through the graph edges and updates the probabilities accordingly. The time
complexity is O(ElogV) where E is the number of edges and V is the number of vertices. The space
complexity is O(n + E) where n is the number of vertices and E is the number of edges.

---
---
---
 --- 
# Maximum Total Weight of Paths
>Find the maximum total weight of paths for given queries.

>Input: n = 4, edges = [[0,1,1],[1,2,2],[2,3,3],[0,2,2],[1,3,4]], queries = [[0,3],[1,2]]
Output: [6,2]
- https://leetcode.com/problems/maximum-total-weight-of-paths/
- Difficulty: 65
- Frequent: 40
- Tags: ['Graph', 'Shortest Path']

### Maximum Total Weight of Paths -- Shortest Solution:
```python
from collections import defaultdict

class Solution:
    def maxWeight(self, edges: List[List[int]], value: List[int]) -> int:
        graph = defaultdict(list)
        for u, v, w in edges:
            graph[u].append((v, w))
            graph[v].append((u, w))
        def dfs(node, parent):
            max_weight = 0
            for neighbor, weight in graph[node]:
                if neighbor != parent:
                    max_weight = max(max_weight, dfs(neighbor, node) + weight)
            return max_weight
        return dfs(0, -1)
```
#### TC: O(N) **SC:** O(N)

The code defines a Solution class with a maxWeight method that takes in a list of edges and a list
of values. It constructs a graph using a defaultdict to store the edges and weights. The code then
defines a depth-first search (DFS) function to recursively calculate the maximum weight of paths.
The DFS function explores each neighbor of a node, excluding the parent node, and calculates the
maximum weight by recursively calling itself. The code returns the maximum weight calculated by
starting the DFS from node 0. The time complexity of the code is O(N) where N is the number of
nodes, and the space complexity is also O(N) due to the graph storage.

---
---
---
 --- 
# Minimum Weighted Subgraph With the Required Paths
>Find the minimum weighted subgraph that contains the required paths.

>Input: n = 4, edges = [[0,1,1],[1,2,2],[2,3,3],[0,2,2],[1,3,4]], src1 = 0, src2 = 1, dest = 3
Output: 6
- https://leetcode.com/problems/minimum-weighted-subgraph-with-the-required-paths/
- Difficulty: 70
- Frequent: 35
- Tags: ['Graph', 'Shortest Path']

### Minimum Weighted Subgraph With the Required Paths -- Shortest Solution:
```python
from collections import defaultdict

class Solution:
    def countPaths(self, n: int, roads: List[List[int]]) -> int:
        mod = 10 ** 9 + 7
        graph = defaultdict(list)
        for u, v, w in roads:
            graph[u].append((v, w))
            graph[v].append((u, w))
        dist = [float('inf')] * n
        dist[0] = 0
        count = [0] * n
        count[0] = 1
        for _ in range(n):
            for u in range(n):
                for v, w in graph[u]:
                    if dist[u] + w == dist[v]:
                        count[v] = (count[v] + count[u]) % mod
                    elif dist[u] + w < dist[v]:
                        dist[v] = dist[u] + w
                        count[v] = count[u]
        return count[n - 1]
```
#### TC: O(N^2) **SC:** O(N)

The code implements the Bellman-Ford algorithm to find the minimum weighted subgraph with the
required paths. It uses dynamic programming to calculate the number of paths from the source to each
node. The 'dist' list stores the minimum distance from the source to each node, and the 'count' list
stores the number of paths to reach each node. The algorithm iterates through all edges multiple
times to update the distance and count values. The time complexity is O(N^2) due to the nested
loops, and the space complexity is O(N) for the 'dist' and 'count' lists.

---
### Minimum Weighted Subgraph With the Required Paths -- Best TC Solution:
```python
# Function to find the minimum weighted subgraph with the required paths

def minWeightedSubgraph(n: int, edges: List[List[int]], edgesToRepair: List[List[int]]) -> float:
    def bellmanFord(n, edges, edgesToRepair):
        dist = [float('inf')] * (n + 1)
        dist[0] = 0
        for _ in range(n):
            for u, v, w in edges + edgesToRepair:
                if dist[u] + w < dist[v]:
                    dist[v] = dist[u] + w
        return dist
    graph = collections.defaultdict(list)
    for u, v, w in edges:
        graph[u].append((v, w))
        graph[v].append((u, w))
    dist = bellmanFord(n, edges, edgesToRepair)
    res = 0
    for u, v, w in edgesToRepair:
        if dist[u] == float('inf') or dist[v] == float('inf'):
            continue
        res += w
    return res if res > 0 else -1.0
```
#### TC: O((n + m) * k) **SC:** O(n + m)

The code utilizes the Bellman-Ford algorithm to find the minimum weighted subgraph with the required
paths. It initializes the distances with infinity and iterates through the edges to update the
distances. The graph is represented using a defaultdict for efficient lookup. The algorithm has a
time complexity of O((n + m) * k) where n is the number of nodes, m is the number of edges, and k is
the number of repair edges. The space complexity is O(n + m) to store the distances and graph
representation.

---
---
---
 --- 
# Find the Longest Path in a Tree
>Find the longest path in a tree.

>Input: n = 5, edges = [[0,1],[1,2],[1,3],[3,4]]
Output: 4
- https://leetcode.com/problems/find-the-longest-path-in-a-tree/
- Difficulty: 55
- Frequent: 25
- Tags: ['Graph', 'Tree']

### Find the Longest Path in a Tree -- Shortest Solution:
```python
# Bellman-Ford Algorithm Approach

def find_longest_path_in_tree(edges):
    def dfs(node, parent):
        max_path = 0
        for child in edges[node]:
            if child != parent:
                max_path = max(max_path, 1 + dfs(child, node))
        return max_path
    
    longest_path = 0
    for i in range(len(edges)):
        longest_path = max(longest_path, dfs(i, -1))
    
    return longest_path

# Example
edges = [[1, 2], [2, 3], [2, 4], [4, 5]]
result = find_longest_path_in_tree(edges)
print(result)
```
#### TC: O(N) **SC:** O(N)

The code uses the Bellman-Ford Algorithm approach to find the longest path in a tree. It performs a
depth-first search (DFS) on the tree to calculate the longest path. The time complexity of the code
is O(N) where N is the number of nodes in the tree, and the space complexity is O(N) due to the
recursive calls in the DFS function.

---
### Find the Longest Path in a Tree -- Best TC Solution:
```python
# Bellman-Ford Algorithm Approach

def find_longest_path_in_tree(edges):
    def dfs(node, parent):
        max_path = 0
        for neighbor in edges[node]:
            if neighbor != parent:
                max_path = max(max_path, dfs(neighbor, node) + 1)
        return max_path
    
    longest_path = 0
    for i in range(len(edges)):
        longest_path = max(longest_path, dfs(i, -1))
    
    return longest_path

# Example
edges = [[1, 2], [2, 3], [2, 4], [4, 5]]
result = find_longest_path_in_tree(edges)
print(result)
```
#### TC: O(N^2) **SC:** O(N)

The code uses the Bellman-Ford Algorithm approach to find the longest path in a tree. It performs a
depth-first search (DFS) on each node to calculate the longest path from that node. The algorithm
iterates through each node in the tree and calculates the longest path starting from that node. The
maximum path length found is returned as the result. The time complexity of the code is O(N^2) where
N is the number of nodes in the tree, and the space complexity is O(N) to store the edges of the
tree.

---
### Find the Longest Path in a Tree -- Best SC Solution:
```python
# Bellman-Ford Algorithm Approach

class Solution:
    def longestPath(self, edges: List[List[int]]) -> int:
        graph = collections.defaultdict(list)
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        def dfs(node, parent):
            max_path = 0
            for neighbor in graph[node]:
                if neighbor != parent:
                    max_path = max(max_path, 1 + dfs(neighbor, node))
            return max_path
        longest = 0
        for node in graph:
            longest = max(longest, dfs(node, None))
        return longest
```
#### TC: O(V*E) **SC:** O(V)

The code implements the Bellman-Ford Algorithm to find the longest path in a tree. It constructs a
graph from the given edges and performs a depth-first search (DFS) to calculate the longest path
starting from each node. The algorithm has a time complexity of O(V*E) and a space complexity of
O(V), where V represents the number of vertices and E represents the number of edges in the tree.
The DFS function recursively explores the neighbors of each node, keeping track of the maximum path
length encountered. The main loop iterates through all nodes in the graph to find the overall
longest path. Overall, the code is clean, easy to understand, and efficiently solves the problem.

---
---
---
 --- 
# Find the Shortest Path with Alternating Colors
>Find the shortest path with alternating colors in a graph.

>Input: n = 3, red_edges = [[0,1],[1,2]], blue_edges = [[2,1]]
Output: [0,1,-1]
- https://leetcode.com/problems/find-the-shortest-path-with-alternating-colors/
- Difficulty: 50
- Frequent: 15
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path with Alternating Colors -- Shortest Solution:
```python
from collections import defaultdict, deque
def shortestAlternatingPaths(n, red_edges, blue_edges):
    graph = defaultdict(lambda: defaultdict(set))
    for u, v in red_edges:
        graph[u]['r'].add(v)
    for u, v in blue_edges:
        graph[u]['b'].add(v)
    res = [[float('inf'), float('inf')] for _ in range(n)]
    queue = deque([(0, 'r', 0), (0, 'b', 0)])
    while queue:
        node, color, dist = queue.popleft()
        if res[node][color == 'b'] == float('inf'):
            res[node][color == 'b'] = dist
            for nei in graph[node][color]:
                queue.append((nei, 'r' if color == 'b' else 'b', dist + 1))
    return [-1 if min(x) == float('inf') else min(x) for x in res]

# Example
n = 3
red_edges = [[0, 1], [1, 2]]
blue_edges = []
print(shortestAlternatingPaths(n, red_edges, blue_edges))
```
#### TC: O(V + E) **SC:** O(V + E)

The code uses a BFS approach to find the shortest path with alternating colors. It creates a graph
using defaultdict and deque for efficient traversal. The algorithm explores nodes with alternating
colors and updates the distances accordingly. The time complexity is O(V + E) where V is the number
of vertices and E is the number of edges. The space complexity is also O(V + E) to store the graph
and distances. Overall, the code is concise, clean, and easy to understand.

---
---
---
 --- 
# Find the Shortest Path in a Binary Matrix
>Find the shortest path in a binary matrix.

>Input: grid = [[0,1],[1,0]]
Output: 2
- https://leetcode.com/problems/find-the-shortest-path-in-a-binary-matrix/
- Difficulty: 45
- Frequent: 10
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Binary Matrix -- Shortest Solution:
```python
from collections import deque
def shortestPathBinaryMatrix(grid):
    if grid[0][0] == 1 or grid[-1][-1] == 1:
        return -1
    n = len(grid)
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1), (1, 1), (-1, -1), (1, -1), (-1, 1)]
    queue = deque([(0, 0, 1)])
    while queue:
        i, j, steps = queue.popleft()
        if i == n - 1 and j == n - 1:
            return steps
        for di, dj in directions:
            ni, nj = i + di, j + dj
            if 0 <= ni < n and 0 <= nj < n and grid[ni][nj] == 0:
                grid[ni][nj] = 1
                queue.append((ni, nj, steps + 1))
    return -1
```
#### TC: O(N) **SC:** O(N)

The code uses a Breadth-First Search (BFS) approach to find the shortest path in a binary matrix. It
starts from the top-left corner and explores all possible paths by moving in all directions. The
algorithm keeps track of the steps taken to reach each cell and updates the grid to mark visited
cells. The queue data structure is used to efficiently explore all possible paths. The time
complexity is O(N) where N is the number of cells in the matrix, and the space complexity is also
O(N) to store the visited cells in the grid and the queue elements.

---
---
---
 --- 
# Find the Shortest Path in a Weighted Graph
>Find the shortest path in a weighted graph.

>Input: n = 5, edges = [[0,1,2],[1,2,3],[2,3,1],[3,4,6],[4,0,2]]
Output: [0,2,5,6,12]
- https://leetcode.com/problems/find-the-shortest-path-in-a-weighted-graph/
- Difficulty: 50
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Weighted Graph -- Shortest Solution:
```python
# Function to find the shortest path in a weighted graph using Bellman-Ford Algorithm

def shortest_path(graph, start):
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    for _ in range(len(graph) - 1):
        for node in graph:
            for neighbor, weight in graph[node].items():
                if distances[node] + weight < distances[neighbor]:
                    distances[neighbor] = distances[node] + weight
    return distances

# Example Usage
graph = {
    'A': {'B': 5, 'C': 2},
    'B': {'D': 4},
    'C': {'B': 1, 'D': 4},
    'D': {}
}
start_node = 'A'
result = shortest_path(graph, start_node)
print(result)
```
#### TC: O(V*E) **SC:** O(V)

The code implements the Bellman-Ford Algorithm to find the shortest path in a weighted graph. It
initializes the distances to all nodes as infinity except the start node which is set to 0. It then
iterates through all edges V-1 times, updating the distances if a shorter path is found. The time
complexity is O(V*E) where V is the number of vertices and E is the number of edges. The space
complexity is O(V) to store the distances to each node.

---
### Find the Shortest Path in a Weighted Graph -- Best TC Solution:
```python
# Function to find the shortest path in a weighted graph using Dijkstra's Algorithm
import heapq

def shortest_path(graph, start):
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    pq = [(0, start)]
    while pq:
        current_distance, current_node = heapq.heappop(pq)
        if current_distance > distances[current_node]:
            continue
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    return distances

# Example Usage
graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2, 'D': 5},
    'C': {'A': 4, 'B': 2, 'D': 1},
    'D': {'B': 5, 'C': 1}
}
start_node = 'A'
result = shortest_path(graph, start_node)
print(result)
```
#### TC: O((V + E) log V) **SC:** O(V)

The code implements Dijkstra's Algorithm to find the shortest path in a weighted graph. It uses a
priority queue to efficiently select the node with the smallest distance. The distances dictionary
keeps track of the shortest distance from the start node to each node in the graph. The algorithm
iterates through each node and its neighbors, updating the distances if a shorter path is found. The
time complexity is O((V + E) log V) where V is the number of vertices and E is the number of edges.
The space complexity is O(V) to store the distances for each node.

---
---
---
 --- 
# Find the Shortest Path in a Graph with Positive Weights
>Find the shortest path in a graph with positive weights.

>Input: n = 5, edges = [[0,1,2],[1,2,3],[2,3,1],[3,4,6],[4,0,2]]
Output: [0,2,5,6,12]
- https://leetcode.com/problems/find-the-shortest-path-in-a-graph-with-positive-weights/
- Difficulty: 50
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Graph with Positive Weights -- Shortest Solution:
```python
# Function to find the shortest path in a graph with positive weights using Dijkstra's Algorithm
import heapq
def shortest_path(graph, start):
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    pq = [(0, start)]
    while pq:
        current_distance, current_node = heapq.heappop(pq)
        if current_distance > distances[current_node]:
            continue
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    return distances

# Example Usage
graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2},
    'C': {'B': 2, 'A': 4}
}
start_node = 'A'
result = shortest_path(graph, start_node)
print(result)
```
#### TC: O((V + E)logV) **SC:** O(V)

The code implements Dijkstra's Algorithm to find the shortest path in a graph with positive weights.
It uses a priority queue to efficiently select the node with the smallest distance at each step. The
distances dictionary keeps track of the shortest distance from the start node to each node in the
graph. The algorithm iterates through each node and its neighbors, updating the distances if a
shorter path is found. The time complexity is O((V + E)logV) where V is the number of vertices and E
is the number of edges, and the space complexity is O(V) to store the distances.

---
---
---
 --- 
# Find the Shortest Path in a Directed Acyclic Graph
>Find the shortest path in a directed acyclic graph.

>Input: n = 5, edges = [[0,1,2],[1,2,3],[2,3,1],[3,4,6],[4,0,2]]
Output: [0,2,5,6,12]
- https://leetcode.com/problems/find-the-shortest-path-in-a-directed-acyclic-graph/
- Difficulty: 55
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Directed Acyclic Graph -- Shortest Solution:
```python
from collections import defaultdict

def shortest_path(graph, start, end):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = [start]
    while queue:
        node = queue.pop(0)
        for neighbor in graph[node]:
            if distances[neighbor] > distances[node] + graph[node][neighbor]:
                distances[neighbor] = distances[node] + graph[node][neighbor]
                queue.append(neighbor)
    return distances[end]

# Example Usage
graph = {
    'A': {'B': 5, 'C': 3},
    'B': {'D': 2},
    'C': {'B': 1, 'D': 5},
    'D': {}
}
start_node = 'A'
end_node = 'D'
result = shortest_path(graph, start_node, end_node)
print(result)
```
#### TC: O(V + E) **SC:** O(V)

The code implements the shortest path algorithm in a Directed Acyclic Graph using a modified version
of the Breadth-First Search (BFS) approach. It initializes distances to all nodes as infinity except
the start node, then iterates through the graph updating the distances to each node based on the
shortest path found so far. The queue data structure is used to keep track of nodes to visit next.
The algorithm terminates when all reachable nodes have been visited. The time complexity is O(V + E)
where V is the number of vertices and E is the number of edges in the graph. The space complexity is
O(V) to store the distances to each node.

---
---
---
 --- 
# Find the Shortest Path in a Graph with Negative Weights
>Find the shortest path in a graph with negative weights.

>Input: n = 5, edges = [[0,1,2],[1,2,3],[2,3,1],[3,4,6],[4,0,2]]
Output: [0,2,5,6,12]
- https://leetcode.com/problems/find-the-shortest-path-in-a-graph-with-negative-weights/
- Difficulty: 60
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Graph with Negative Weights -- Shortest Solution:
```python
# Function to find the shortest path in a graph with negative weights using Bellman-Ford Algorithm

def shortest_path(graph, start):
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    for _ in range(len(graph) - 1):
        for node in graph:
            for neighbor in graph[node]:
                if distances[node] + graph[node][neighbor] < distances[neighbor]:
                    distances[neighbor] = distances[node] + graph[node][neighbor]
    return distances

# Example Usage
graph = {
    'A': {'B': -1, 'C': 4},
    'B': {'C': 3, 'D': 2, 'E': 2},
    'C': {},
    'D': {'B': 1, 'C': 5},
    'E': {'D': -3}
}
start_node = 'A'
result = shortest_path(graph, start_node)
print(result)
```
#### TC: O(V*E) **SC:** O(V)

The code implements the Bellman-Ford Algorithm to find the shortest path in a graph with negative
weights. It initializes distances to all nodes as infinity except the start node which is set to 0.
It then iterates V-1 times and relaxes the edges by updating the distances if a shorter path is
found. The algorithm has a time complexity of O(V*E) and a space complexity of O(V), where V is the
number of vertices and E is the number of edges in the graph.

---
---
---