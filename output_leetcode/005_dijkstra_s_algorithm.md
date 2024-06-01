# Dijkstra's Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Path With Minimum Effort](#path-with-minimum-effort) | 60 | 55 |
| 2 | [Reachable Nodes In Subdivided Graph](#reachable-nodes-in-subdivided-graph) | 40 | 55 |
| 3 | [Find the Safest Path in a Grid](#find-the-safest-path-in-a-grid) | 35 | 50 |
| 4 | [Minimum Number of Days to Disconnect Island](#minimum-number-of-days-to-disconnect-island) | 25 | 60 |
| 5 | [Minimum Cost to Connect All Points](#minimum-cost-to-connect-all-points) | 20 | 50 |
| 6 | [Find the Longest Path in a Matrix](#find-the-longest-path-in-a-matrix) | 15 | 55 |
| 7 | [Find the Shortest Path in a Grid with Obstacles Elimination](#find-the-shortest-path-in-a-grid-with-obstacles-elimination) | 10 | 60 |
| 8 | [Find the Shortest Path in a Grid with Teleportation](#find-the-shortest-path-in-a-grid-with-teleportation) | 5 | 55 |
| 9 | [Find the Shortest Path in a Grid with Portals](#find-the-shortest-path-in-a-grid-with-portals) | 5 | 60 |
| 10 | [Find the Shortest Path in a Grid with Keys and Doors](#find-the-shortest-path-in-a-grid-with-keys-and-doors) | 5 | 65 |
| 11 | [Find the Shortest Path in a Grid with Traps](#find-the-shortest-path-in-a-grid-with-traps) | 5 | 65 |
---
Dijkstra's algorithm finds the shortest path from a source node to all other nodes in a weighted graph with non-negative weights. It uses a priority queue to greedily select the closest unvisited node, updating the shortest path to each neighbor. The process repeats until all nodes are visited.
```python
import heapq

def dijkstra(graph, start):
    queue = [(0, start)]
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    while queue:
        current_distance, current_node = heapq.heappop(queue)
        if current_distance > distances[current_node]:
            continue
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(queue, (distance, neighbor))
    return distances

graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2, 'D': 5},
    'C': {'A': 4, 'B': 2, 'D': 1},
    'D': {'B': 5, 'C': 1}
}

print(dijkstra(graph, 'A'))
```
### Insight:
1. The algorithm uses a priority queue (min-heap) to efficiently fetch the next node with the smallest tentative distance.
2. The distances dictionary keeps track of the shortest known distance to each node.
3. The algorithm updates the shortest path estimates for each neighbor of the current node.
4. The process continues until all nodes have been visited and the shortest paths from the source to all other nodes are determined.
5. The graph is represented as an adjacency list, where each node points to a dictionary of its neighbors and their edge weights.
---
 --- 
# Path With Minimum Effort
>Find a path with the minimum effort required to travel from the top-left to the bottom-right corner.

>Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
- https://leetcode.com/problems/path-with-minimum-effort/
- Difficulty: 55
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Path With Minimum Effort -- Shortest Solution:
```python
import heapq
def minimumEffortPath(heights):
    m, n = len(heights), len(heights[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    heap = [(0, 0, 0)]
    efforts = [[float('inf')] * n for _ in range(m)]
    efforts[0][0] = 0
    while heap:
        effort, x, y = heapq.heappop(heap)
        if x == m - 1 and y == n - 1:
            return effort
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < m and 0 <= ny < n:
                new_effort = max(effort, abs(heights[x][y] - heights[nx][ny]))
                if new_effort < efforts[nx][ny]:
                    efforts[nx][ny] = new_effort
                    heapq.heappush(heap, (new_effort, nx, ny))
```
#### TC: O(m * n * log(m*n)) **SC:** O(m * n)

The code uses Dijkstra's Algorithm to find the minimum effort path in a grid of heights. It
initializes a heap with the starting point and iterates through the grid, updating the effort
required to reach each cell. The algorithm prioritizes cells with lower effort, ensuring the minimum
effort path is found. The time complexity is O(m * n * log(m*n)) due to the heap operations, and the
space complexity is O(m * n) for the efforts array.

---
---
---
 --- 
# Reachable Nodes In Subdivided Graph
>Find the maximum number of nodes reachable in a subdivided graph within a given number of moves.

>Input: edges = [[0,1,10],[0,2,1],[1,2,2]], maxMoves = 6, n = 3
Output: 13
- https://leetcode.com/problems/reachable-nodes-in-subdivided-graph/
- Difficulty: 55
- Frequent: 40
- Tags: ['Graph', 'Shortest Path']

### Reachable Nodes In Subdivided Graph -- Shortest Solution:
```python
from heapq import heappop, heappush
from collections import defaultdict

def reachableNodes(edges, M, N):
    graph = defaultdict(dict)
    for u, v, w in edges:
        graph[u][v] = graph[v][u] = w
    pq = [(-M, 0)]
    dist = {}
    while pq:
        moves, node = heappop(pq)
        if node not in dist:
            dist[node] = -moves
            for nei, weight in graph[node].items():
                if moves + weight + 1 > 0:
                    if nei not in dist:
                        heappush(pq, (moves + weight + 1, nei))
    ans = len(dist)
    for u, v, w in edges:
        ans += min(dist.get(u, 0) + dist.get(v, 0), w)
    return ans
```
#### TC: O(ElogN) **SC:** O(N)

The code uses Dijkstra's Algorithm to find the reachable nodes in a subdivided graph. It maintains a
priority queue to keep track of the nodes to visit based on the remaining moves. The algorithm
efficiently calculates the reachable nodes and updates the distances accordingly. The space
complexity is O(N) due to the usage of a dictionary to store distances, and the time complexity is
O(ElogN) where E is the number of edges and N is the number of nodes in the graph.

---
---
---
 --- 
# Find the Safest Path in a Grid
>Find the safest path in a grid where some cells are blocked.

>Input: grid = [[1,0,0],[1,1,0],[1,1,1]]
Output: 4
- https://leetcode.com/problems/find-the-safest-path-in-a-grid/
- Difficulty: 50
- Frequent: 35
- Tags: ['Graph', 'Shortest Path']

### Find the Safest Path in a Grid -- Shortest Solution:
```python
from heapq import heappop, heappush

def safest_path(grid):
    rows, cols = len(grid), len(grid[0])
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    pq = [(0, 0, 0)]  # (safety level, row, col)
    visited = set()
    while pq:
        safety, r, c = heappop(pq)
        if (r, c) == (rows - 1, cols - 1):
            return safety
        if (r, c) in visited:
            continue
        visited.add((r, c))
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols:
                nsafety = min(safety, grid[nr][nc])
                heappush(pq, (nsafety, nr, nc))
    return -1
```
#### TC: O(m * n * log(m * n)) **SC:** O(m * n)

The code implements a Dijkstra's Algorithm approach to find the safest path in a grid. It uses a
priority queue to explore cells with the highest safety level first. The algorithm iterates through
the grid, updating the safety level based on the minimum of the current safety level and the safety
level of the next cell. This ensures that the path chosen is the safest possible. The time
complexity is O(m * n * log(m * n)) where m is the number of rows and n is the number of columns in
the grid. The space complexity is O(m * n) to store the visited cells.

---
### Find the Safest Path in a Grid -- Best SC Solution:
```python
from heapq import heappush, heappop

def safest_path(grid):
    rows, cols = len(grid), len(grid[0])
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    pq = [(0, 0, 0)]
    visited = set()
    while pq:
        dist, x, y = heappop(pq)
        if (x, y) == (rows - 1, cols - 1):
            return dist
        if (x, y) in visited:
            continue
        visited.add((x, y))
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < rows and 0 <= ny < cols and (nx, ny) not in visited:
                ndist = max(dist, grid[nx][ny])
                heappush(pq, (ndist, nx, ny))
    return -1
```
#### TC: O(N * M * log(N * M)) **SC:** O(N * M)

The code defines a function `safest_path` that finds the safest path in a grid using Dijkstra's
Algorithm. It initializes a priority queue `pq` with the starting point (0, 0, 0) representing
(distance, x-coordinate, y-coordinate). It then iterates through the grid while updating the
distance and checking for the destination. The code efficiently handles the grid traversal and keeps
track of visited cells to avoid revisiting them. The time complexity is O(N * M * log(N * M)) where
N is the number of rows and M is the number of columns, and the space complexity is O(N * M) to
store the visited cells.

---
---
---
 --- 
# Minimum Number of Days to Disconnect Island
>Find the minimum number of days to disconnect an island in a grid.

>Input: grid = [[1,1,0,0,0],[1,1,0,0,0],[1,1,0,0,0],[0,0,0,0,0],[0,0,0,0,0]]
Output: 2
- https://leetcode.com/problems/minimum-number-of-days-to-disconnect-island/
- Difficulty: 60
- Frequent: 25
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Days to Disconnect Island -- Shortest Solution:
```python
from collections import deque

def minDays(grid):
    def bfs(grid):
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        visited = set()
        queue = deque()
        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0]):
                if grid[i][j] == 1:
                    queue.append((i, j))
                    visited.add((i, j))
                    count += 1
                    break
            if count > 0:
                break
        days = 0
        while queue:
            for _ in range(len(queue)):
                x, y = queue.popleft()
                for dx, dy in directions:
                    nx, ny = x + dx, y + dy
                    if 0 <= nx < len(grid) and 0 <= ny < len(grid[0]) and (nx, ny) not in visited and grid[nx][ny] == 1:
                        queue.append((nx, ny))
                        visited.add((nx, ny))
                        count += 1
            days += 1
        return count, days
    island_count, min_days = bfs(grid)
    if island_count == 0 or island_count > 1:
        return 0
    for i in range(len(grid)):
        for j in range(len(grid[0]):
            if grid[i][j] == 1:
                grid[i][j] = 0
                new_island_count, new_min_days = bfs(grid)
                if new_island_count != 0 and new_island_count <= 1:
                    return new_min_days
                grid[i][j] = 1
    return -1
```
#### TC: O(N^2) **SC:** O(N^2)

The code uses a breadth-first search (BFS) approach to find the minimum number of days required to
disconnect an island. It iterates through the grid to find the first island, then performs BFS to
count the number of connected cells and the days required to disconnect them. It then checks if
there is only one island in the grid and tries disconnecting each cell to find the minimum days
needed. The time complexity is O(N^2) where N is the size of the grid, and the space complexity is
also O(N^2) due to the visited set storing visited cells during BFS.

---
---
---
 --- 
# Minimum Cost to Connect All Points
>Find the minimum cost to connect all points in a 2D plane.

>Input: points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
Output: 20
- https://leetcode.com/problems/minimum-cost-to-connect-all-points/
- Difficulty: 50
- Frequent: 20
- Tags: ['Graph', 'Shortest Path']

### Minimum Cost to Connect All Points -- Shortest Solution:
```python
import heapq

def minCostConnectPoints(points):
    def dist(p1, p2):
        return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])
    n = len(points)
    visited = set()
    min_cost = 0
    pq = [(0, 0)]
    while len(visited) < n:
        cost, cur = heapq.heappop(pq)
        if cur not in visited:
            visited.add(cur)
            min_cost += cost
            for i in range(n):
                if i not in visited:
                    heapq.heappush(pq, (dist(points[cur], points[i]), i))
    return min_cost
```
#### TC: O(n^2 * log(n)) **SC:** O(n)

The code uses a greedy approach with Dijkstra's Algorithm to find the minimum cost to connect all
points. It calculates the distance between points using Manhattan distance. The code maintains a
priority queue to select the next point to visit based on the minimum distance. The time complexity
is O(n^2 * log(n)) due to the nested loops and heap operations, and the space complexity is O(n) for
the visited set and priority queue.

---
---
---
 --- 
# Find the Longest Path in a Matrix
>Find the longest path in a matrix where you can move in four directions.

>Input: matrix = [[1,2,3],[6,5,4],[7,8,9]]
Output: 9
- https://leetcode.com/problems/find-the-longest-path-in-a-matrix/
- Difficulty: 55
- Frequent: 15
- Tags: ['Graph', 'Shortest Path']

### Find the Longest Path in a Matrix -- Shortest Solution:
```python
from collections import deque

def longest_path(matrix):
    if not matrix:
        return 0
    rows, cols = len(matrix), len(matrix[0])
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    dp = [[0] * cols for _ in range(rows)]
    longest = 0
    queue = deque()
    for i in range(rows):
        for j in range(cols):
            queue.append((i, j, 1))
    while queue:
        x, y, length = queue.popleft()
        longest = max(longest, length)
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < rows and 0 <= ny < cols and matrix[nx][ny] > matrix[x][y]:
                queue.append((nx, ny, length + 1))
    return longest

# Example
matrix = [
    [9, 9, 4],
    [6, 6, 8],
    [2, 1, 1]
]
print(longest_path(matrix) # Output: 4
```
#### TC: O(m*n) **SC:** O(m*n)

The code defines a function `longest_path` that calculates the length of the longest increasing path
in a matrix. It uses a dynamic programming approach to keep track of the longest path ending at each
cell. The function iterates through each cell in the matrix and performs a breadth-first search to
find the longest increasing path starting from that cell. The time complexity of the solution is
O(m*n) where m is the number of rows and n is the number of columns in the matrix. The space
complexity is also O(m*n) to store the dynamic programming table.

---
### Find the Longest Path in a Matrix -- Best SC Solution:
```python
from collections import deque

def longestIncreasingPath(matrix):
    if not matrix:
        return 0
    rows, cols = len(matrix), len(matrix[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    indegrees = [[0] * cols for _ in range(rows)]
    queue = deque()
    for i in range(rows):
        for j in range(cols):
            for dx, dy in directions:
                x, y = i + dx, j + dy
                if 0 <= x < rows and 0 <= y < cols and matrix[x][y] > matrix[i][j]:
                    indegrees[i][j] += 1
            if indegrees[i][j] == 0:
                queue.append((i, j))
    while queue:
        for _ in range(len(queue)):
            x, y = queue.popleft()
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < rows and 0 <= ny < cols and matrix[nx][ny] < matrix[x][y]:
                    indegrees[nx][ny] -= 1
                    if indegrees[nx][ny] == 0:
                        queue.append((nx, ny))
    return max(max(row) for row in indegrees) + 1

# Example
matrix = [
    [9, 9, 4],
    [6, 6, 8],
    [2, 1, 1]
]
print(longestIncreasingPath(matrix) # Output: 4
```
#### TC: O(m * n) **SC:** O(m * n)

The code uses a topological sorting approach to find the longest increasing path in a matrix. It
calculates the indegrees of each cell by comparing it with its neighboring cells. The algorithm then
iterates through the cells with zero indegree, updating the indegrees of their neighbors. Finally,
it returns the maximum value in the indegrees matrix plus 1, which represents the length of the
longest increasing path. The time complexity is O(m * n) where m is the number of rows and n is the
number of columns, and the space complexity is also O(m * n) to store the indegrees matrix.

---
---
---
 --- 
# Find the Shortest Path in a Grid with Obstacles Elimination
>Find the shortest path in a grid where you can eliminate a limited number of obstacles.

>Input: grid = [[0,1,1],[1,1,0],[1,1,0]], k = 1
Output: 4
- https://leetcode.com/problems/find-the-shortest-path-in-a-grid-with-obstacles-elimination/
- Difficulty: 60
- Frequent: 10
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Grid with Obstacles Elimination -- Shortest Solution:
```python
from collections import deque
def shortestPath(grid, k):
    m, n = len(grid), len(grid[0])
    queue = deque([(0, 0, k, 0)])
    visited = set([(0, 0, k)])
    while queue:
        x, y, remain, steps = queue.popleft()
        if x == m - 1 and y == n - 1:
            return steps
        for dx, dy in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
            nx, ny = x + dx, y + dy
            if 0 <= nx < m and 0 <= ny < n:
                nk = remain - grid[nx][ny]
                if nk >= 0 and (nx, ny, nk) not in visited:
                    visited.add((nx, ny, nk))
                    queue.append((nx, ny, nk, steps + 1))
    return -1
```
#### TC: O(m * n * k) **SC:** O(m * n * k)

The code uses a breadth-first search (BFS) approach to find the shortest path in a grid with
obstacles elimination. It maintains a queue to explore possible paths while keeping track of the
remaining obstacles that can be eliminated. The visited set helps avoid revisiting the same cell
with the same remaining obstacles. The algorithm iterates through all possible directions (up, down,
left, right) from each cell and updates the queue with valid paths. The time complexity is O(m * n *
k) where m and n are the dimensions of the grid, and k is the maximum number of obstacles that can
be eliminated. The space complexity is also O(m * n * k) due to the queue and visited set storing
information for each cell with remaining obstacles.

---
### Find the Shortest Path in a Grid with Obstacles Elimination -- Best SC Solution:
```python
from collections import deque
def shortestPath(grid, k):
    m, n = len(grid), len(grid[0])
    queue = deque([(0, 0, k, 0)])
    visited = set([(0, 0, k)])
    while queue:
        i, j, remain, steps = queue.popleft()
        if i == m - 1 and j == n - 1:
            return steps
        for x, y in [(i + 1, j), (i - 1, j), (i, j + 1), (i, j - 1)]:
            if 0 <= x < m and 0 <= y < n:
                if grid[x][y] == 1 and remain > 0 and (x, y, remain - 1) not in visited:
                    visited.add((x, y, remain - 1))
                    queue.append((x, y, remain - 1, steps + 1))
                elif grid[x][y] == 0 and (x, y, remain) not in visited:
                    visited.add((x, y, remain))
                    queue.append((x, y, remain, steps + 1))
    return -1
```
#### TC: O(m*n*k) **SC:** O(m*n*k)

The code uses a breadth-first search (BFS) approach to find the shortest path in a grid with
obstacles elimination. It maintains a queue to explore all possible paths while keeping track of the
remaining obstacles that can be eliminated. The visited set helps avoid revisiting the same cell
with the same remaining obstacles. The algorithm iterates through the grid cells and explores valid
neighboring cells based on the obstacle elimination constraint. The time complexity is O(m*n*k)
where m and n are the dimensions of the grid, and k is the maximum number of obstacles that can be
eliminated. The space complexity is also O(m*n*k) due to the queue and visited set storing the
states of each cell with remaining obstacles.

---
---
---
 --- 
# Find the Shortest Path in a Grid with Teleportation
>Find the shortest path in a grid where you can use teleportation.

>Input: grid = [[0,1,2],[3,4,5],[6,7,8]]
Output: 10
- https://leetcode.com/problems/find-the-shortest-path-in-a-grid-with-teleportation/
- Difficulty: 55
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Grid with Teleportation -- Shortest Solution:
```python
from heapq import heappush, heappop

def shortestPath(grid):
    rows, cols = len(grid), len(grid[0])
    visited = set()
    heap = [(0, 0, 0)]
    while heap:
        dist, r, c = heappop(heap)
        if (r, c) == (rows - 1, cols - 1):
            return dist
        if (r, c) in visited:
            continue
        visited.add((r, c))
        for dr, dc in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols:
                heappush(heap, (dist + 1, nr, nc))
        if grid[r][c] > 0:
            teleport = grid[r][c] - 1
            tr, tc = teleport // cols, teleport % cols
            if (tr, tc) not in visited:
                heappush(heap, (dist + abs(tr - r) + abs(tc - c), tr, tc))
    return -1
```
#### TC: O(N * M * log(N * M)) **SC:** O(N * M)

The code uses Dijkstra's Algorithm to find the shortest path in a grid with teleportation. It
maintains a heap to store the distances and explores the grid cells based on the current position.
Teleportation is handled by calculating the distance to the teleportation cell and adding it to the
heap. The code efficiently tracks visited cells to avoid revisiting them. The time complexity is O(N
* M * log(N * M)), where N is the number of rows and M is the number of columns in the grid. The
space complexity is O(N * M) to store the visited cells.

---
---
---
 --- 
# Find the Shortest Path in a Grid with Portals
>Find the shortest path in a grid where you can use portals.

>Input: grid = [[0,1,2],[3,4,5],[6,7,8]]
Output: 8
- https://leetcode.com/problems/find-the-shortest-path-in-a-grid-with-portals/
- Difficulty: 60
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Grid with Portals -- Shortest Solution:
```python
# Optimal Python Solution

def findShortestPath(grid):
    
    
    
    return result
```
#### TC: O(M * N * log(M * N)) **SC:** O(M * N)

To be provided.

---
### Find the Shortest Path in a Grid with Portals -- Best TC Solution:
```python
# Function to find the shortest path in a grid with portals

def shortestPath(grid):
    # Your code here
    pass

# Example usage
grid = [
    ['S', '.', '.', 'P'],
    ['.', '.', '.', '.'],
    ['P', '.', '.', 'E']
]
result = shortestPath(grid)
print(result)
```
#### TC: O(N^2 * log(N)) **SC:** O(N^2)

To find the shortest path in a grid with portals, we can use Dijkstra's Algorithm approach. The code
initializes a priority queue to store the nodes to be visited based on their distances. It then
iterates through the grid, updating the distances and paths to reach each node. The algorithm
continues until the end node is reached or all possible paths are explored. The time complexity is
O(N^2 * log(N)) due to the priority queue operations, and the space complexity is O(N^2) to store
the distances and paths for each node.

---
### Find the Shortest Path in a Grid with Portals -- Best SC Solution:
```python
from collections import deque

def shortestPath(grid):
    rows, cols = len(grid), len(grid[0])
    start, end = None, None
    portals = {}
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 'S':
                start = (i, j)
            elif grid[i][j] == 'E':
                end = (i, j)
            elif grid[i][j].isdigit():
                portals[int(grid[i][j])] = portals.get(int(grid[i][j]), []) + [(i, j)]
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    queue = deque([(start, 0)])
    visited = set()
    while queue:
        (x, y), steps = queue.popleft()
        if (x, y) == end:
            return steps
        if (x, y) in visited:
            continue
        visited.add((x, y))
        for dx, dy in directions:
            new_x, new_y = x + dx, y + dy
            if 0 <= new_x < rows and 0 <= new_y < cols and grid[new_x][new_y] != '#':
                queue.append(((new_x, new_y), steps + 1))
        if grid[x][y].isdigit():
            for portal_x, portal_y in portals[int(grid[x][y])]:
                if (portal_x, portal_y) != (x, y):
                    queue.append(((portal_x, portal_y), steps + 1))
    return -1
```
#### TC: O(rows * cols) **SC:** O(rows * cols)

The code defines a function `shortestPath` that finds the shortest path in a grid with portals using
a breadth-first search approach. It iterates through the grid to find the start, end, and portal
locations. It then uses a queue to explore neighboring cells and portals while keeping track of
visited cells. The function returns the minimum steps required to reach the end position. The time
complexity is O(rows * cols) where rows and cols are the dimensions of the grid, and the space
complexity is also O(rows * cols) due to the queue and visited set.

---
---
---
 --- 
# Find the Shortest Path in a Grid with Keys and Doors
>Find the shortest path in a grid where you need to collect keys to open doors.

>Input: grid = [[0,1,2],[3,4,5],[6,7,8]]
Output: 12
- https://leetcode.com/problems/find-the-shortest-path-in-a-grid-with-keys-and-doors/
- Difficulty: 65
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Grid with Keys and Doors -- Shortest Solution:
```python
from collections import deque
def shortestPathAllKeys(grid):
    m, n = len(grid), len(grid[0])
    target = 0
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '@':
                start = (i, j, 0)
            elif 'a' <= grid[i][j] <= 'f':
                target |= 1 << (ord(grid[i][j]) - ord('a'))
    queue = deque([(start, 0)])
    visited = set(start)
    moves = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    steps = 0
    while queue:
        for _ in range(len(queue)):
            (i, j, keys), collected = queue.popleft()
            if keys == target:
                return steps
            for dx, dy in moves:
                x, y = i + dx, j + dy
                if 0 <= x < m and 0 <= y < n:
                    cell = grid[x][y]
                    if cell == '#':
                        continue
                    if 'A' <= cell <= 'F' and not keys & 1 << (ord(cell) - ord('A')):
                        continue
                    if 'a' <= cell <= 'f':
                        new_keys = keys | 1 << (ord(cell) - ord('a'))
                    else:
                        new_keys = keys
                    if (x, y, new_keys) not in visited:
                        visited.add((x, y, new_keys))
                        queue.append(((x, y, new_keys), collected + (1 if 'a' <= cell <= 'f' else 0)))
        steps += 1
    return -1
```
#### TC: O(m * n * 2^k) **SC:** O(m * n * 2^k)

The code uses a breadth-first search approach to find the shortest path in a grid with keys and
doors. It iterates through the grid cells, collects keys, and updates the queue with possible moves.
The 'target' variable is used to track all the keys that need to be collected. The 'visited' set is
used to keep track of visited cells with specific keys. The code efficiently handles obstacles,
doors, and keys to find the shortest path to collect all keys. The time complexity is O(m * n * 2^k)
where m and n are the dimensions of the grid and k is the number of keys, and the space complexity
is O(m * n * 2^k) to store visited cells with keys.

---
---
---
 --- 
# Find the Shortest Path in a Grid with Traps
>Find the shortest path in a grid where you need to avoid traps.

>Input: grid = [[0,1,2],[3,4,5],[6,7,8]]
Output: 9
- https://leetcode.com/problems/find-the-shortest-path-in-a-grid-with-traps/
- Difficulty: 65
- Frequent: 5
- Tags: ['Graph', 'Shortest Path']

### Find the Shortest Path in a Grid with Traps -- Shortest Solution:
```python
from heapq import heappop, heappush

def shortestPath(grid):
    rows, cols = len(grid), len(grid[0])
    traps = set()
    start, end = None, None
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 'S':
                start = (i, j)
            elif grid[i][j] == 'E':
                end = (i, j)
            elif grid[i][j] == 'T':
                traps.add((i, j))
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    heap = [(0, start, frozenset())]
    while heap:
        cost, curr, state = heappop(heap)
        if curr == end:
            return cost
        if curr in traps:
            state ^= {curr}
        for dx, dy in directions:
            x, y = curr[0] + dx, curr[1] + dy
            if 0 <= x < rows and 0 <= y < cols and (x, y) not in state:
                new_cost = cost + 1
                new_state = state
                if (x, y) in traps:
                    new_state ^= {(x, y)}
                heappush(heap, (new_cost, (x, y), new_state))
    return -1
```
#### TC: O((R * C) * 2^(R * C)) **SC:** O((R * C) * 2^(R * C))

The code implements a Dijkstra's Algorithm approach to find the shortest path in a grid with traps.
It uses a heap to prioritize the nodes to explore based on the cost. The algorithm considers the
traps as toggleable obstacles, changing their state when visited. The use of a set to track the
state of traps ensures that the algorithm explores all possible paths. The time and space complexity
are both exponential due to the number of possible states in the grid with traps.

---
### Find the Shortest Path in a Grid with Traps -- Best TC Solution:
```python
# Function to find the shortest path in a grid with traps
from heapq import heappop, heappush

def shortestPath(grid):
    rows, cols = len(grid), len(grid[0])
    traps = set()
    start, end = None, None
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 'S':
                start = (i, j)
            elif grid[i][j] == 'E':
                end = (i, j)
            elif grid[i][j] == 'T':
                traps.add((i, j))
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    def is_valid(x, y):
        return 0 <= x < rows and 0 <= y < cols
    def get_neighbors(x, y):
        neighbors = []
        for dx, dy in directions:
            new_x, new_y = x + dx, y + dy
            if is_valid(new_x, new_y):
                neighbors.append((new_x, new_y))
        return neighbors
    def is_trap(x, y):
        return (x, y) in traps
    heap = [(0, start, frozenset())]
    visited = set()
    while heap:
        cost, current, state = heappop(heap)
        if current == end:
            return cost
        if (current, state) in visited:
            continue
        visited.add((current, state))
        for neighbor in get_neighbors(*current):
            new_state = state
            if neighbor in traps:
                new_state ^= {neighbor}
            if (neighbor, new_state) not in visited:
                new_cost = cost + 1
                heappush(heap, (new_cost, neighbor, new_state))
    return -1

# Example
grid = [
    ['S', 'E', 'T', 'T'],
    ['T', 'T', 'T', 'T'],
    ['T', 'T', 'T', 'T'],
    ['T', 'T', 'T', 'T']
]
result = shortestPath(grid)
print(result)
```
#### TC: O((V + E)logV) **SC:** O(V)

The code defines a function to find the shortest path in a grid with traps using Dijkstra's
Algorithm. It initializes the start, end, and traps positions, and then iterates through the grid to
find the shortest path. The function uses a heap to store the cost, current position, and state. It
explores neighbors, considering traps and updating the cost accordingly. The code efficiently
handles traps and visited states to find the shortest path. The time complexity is O((V + E)logV)
and space complexity is O(V), where V is the number of vertices and E is the number of edges in the
graph.

---
---
---