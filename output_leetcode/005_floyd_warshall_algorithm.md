# Floyd-Warshall Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Minimum Cost to Connect Two Groups of Points](#minimum-cost-to-connect-two-groups-of-points) | 75 | 65 |
| 2 | [Minimum Cost to Reach Destination in Time](#minimum-cost-to-reach-destination-in-time) | 70 | 60 |
| 3 | [Minimum Cost to Make at Least One Valid Path in a Grid](#minimum-cost-to-make-at-least-one-valid-path-in-a-grid) | 65 | 55 |
| 4 | [Floyd-Warshall Algorithm](#floyd-warshall-algorithm) | 60 | 50 |
| 5 | [All Pairs Shortest Path](#all-pairs-shortest-path) | 60 | 50 |
| 6 | [City With the Smallest Number of Neighbors at a Threshold Distance](#city-with-the-smallest-number-of-neighbors-at-a-threshold-distance) | 45 | 50 |
---
The Floyd-Warshall algorithm is used to find the shortest paths between all pairs of vertices in a weighted graph. It works by iteratively improving the estimated shortest paths through a series of updates, considering each vertex as an intermediate point. The algorithm is particularly useful for dense graphs and can handle negative weights, but not negative cycles.
```python
def floyd_warshall(graph):
    num_vertices = len(graph)
    dist = list(map(lambda i: list(map(lambda j: j, i)), graph))

    for k in range(num_vertices):
        for i in range(num_vertices):
            for j in range(num_vertices):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])

    return dist

# Example usage
inf = float('inf')
graph = [
    [0, 3, inf, 5],
    [2, 0, inf, 4],
    [inf, 1, 0, inf],
    [inf, inf, 2, 0]
]

shortest_paths = floyd_warshall(graph)
for row in shortest_paths:
    print(row)
```
### Insight:
1. The algorithm uses dynamic programming to iteratively improve the solution.
2. It initializes the distance matrix with direct edge weights and updates it by considering each vertex as an intermediate point.
3. The algorithm can detect negative cycles by checking if the distance from a vertex to itself becomes negative.
4. It is most efficient for dense graphs where the number of edges is close to the square of the number of vertices.
5. The algorithm is simple to implement but may not be suitable for sparse graphs due to its cubic time complexity.
---
 --- 
# Minimum Cost to Connect Two Groups of Points
>Find the minimum cost to connect two groups of points.

>Input: cost = [[1,3,5],[4,1,1],[1,5,3]]
Output: 4
- https://leetcode.com/problems/minimum-cost-to-connect-two-groups-of-points/
- Difficulty: 65
- Frequent: 75
- Tags: ['Graph', 'Dynamic Programming']

### Minimum Cost to Connect Two Groups of Points -- Shortest Solution:
```python
from functools import lru_cache
class Solution:
    def connectTwoGroups(self, cost: List[List[int]]) -> int:
        m, n = len(cost), len(cost[0])
        dp = [[float('inf')] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                dp[i][j] = cost[i][j]
        for j in range(1, n):
            dp[0][j] += dp[0][j - 1]
        for i in range(1, m):
            dp[i][0] += dp[i - 1][0]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] += min(dp[i - 1][j], dp[i][j - 1])
        @lru_cache(None)
        def dfs(i, mask):
            if i == m:
                return sum(dp[i - 1][j] for j in range(n) if not mask & (1 << j))
            res = float('inf')
            for j in range(n):
                res = min(res, dp[i][j] + dfs(i + 1, mask | (1 << j)))
            return res
        return dfs(0, 0)
```
#### TC: O(2^m * n) **SC:** O(2^m * n)

The code uses dynamic programming and backtracking to find the minimum cost to connect two groups of
points. The dynamic programming part calculates the minimum cost to reach each point in the cost
matrix. The backtracking part explores all possible combinations of connecting points to find the
minimum total cost. The @lru_cache decorator is used to memoize the results of the backtracking
function for efficiency. Overall, the code efficiently solves the problem with a time complexity of
O(2^m * n) and a space complexity of O(2^m * n).

---
---
---
 --- 
# Minimum Cost to Reach Destination in Time
>Find the minimum cost to reach a destination within a given time.

>Input: maxTime = 30, edges = [[0,1,10],[1,2,10],[2,3,10],[3,4,10]], passingFees = [5,1,2,20,1]
Output: 11
- https://leetcode.com/problems/minimum-cost-to-reach-destination-in-time/
- Difficulty: 60
- Frequent: 70
- Tags: ['Graph', 'Dynamic Programming']

### Minimum Cost to Reach Destination in Time -- Shortest Solution:
```python
from collections import defaultdict

def minCost(self, maxTime: int, edges: List[List[int]], passingFees: List[int]) -> int:
    graph = defaultdict(list)
    for u, v, t in edges:
        graph[u].append((v, t))
        graph[v].append((u, t))
    n = len(passingFees)
    dp = [[float('inf')] * n for _ in range(maxTime + 1)]
    dp[0][0] = passingFees[0]
    for t in range(1, maxTime + 1):
        for i in range(n):
            dp[t][i] = passingFees[i] + min(dp[t - t2][j] for j, t2 in graph[i] if t2 <= t)
    res = min(dp[maxTime])
    return res if res != float('inf') else -1
```
#### TC: O(N^2 * T) **SC:** O(N * T)

The code uses dynamic programming to find the minimum cost to reach the destination within the given
maximum time. It creates a graph based on the edges provided and calculates the minimum cost at each
time step for each node. The time complexity is O(N^2 * T) where N is the number of nodes and T is
the maximum time. The space complexity is O(N * T) to store the dynamic programming table. The code
efficiently handles the constraints and provides the optimal solution.

---
### Minimum Cost to Reach Destination in Time -- Best TC Solution:
```python
from heapq import heappush, heappop

def minCost(maxTime: int, edges: List[List[int]], passingFees: List[int]) -> int:
    n = len(passingFees)
    graph = defaultdict(list)
    for u, v, t in edges:
        graph[u].append((v, t))
        graph[v].append((u, t))
    dp = [[float('inf')] * n for _ in range(maxTime + 1)]
    dp[0][0] = passingFees[0]
    heap = [(passingFees[0], 0, 0)]
    while heap:
        cost, time, node = heappop(heap)
        if time > maxTime:
            continue
        for nei, t in graph[node]:
            if newTime := time + t <= maxTime and cost + passingFees[nei] < dp[newTime][nei]:
                dp[newTime][nei] = cost + passingFees[nei]
                heappush(heap, (dp[newTime][nei], newTime, nei))
    res = min(dp[maxTime])
    return res if res != float('inf') else -1
```
#### TC: O(N^2 * T + E * log(E)) **SC:** O(N * T)

The code uses Dijkstra's algorithm with a priority queue to find the minimum cost to reach the
destination within the given time. It maintains a 2D DP array to store the minimum cost at each node
and time. The priority queue helps in exploring the nodes efficiently based on the cost. The code
efficiently handles the constraints and provides the minimum cost to reach the destination in time.

---
### Minimum Cost to Reach Destination in Time -- Best SC Solution:
```python
from heapq import heappush, heappop

def minCost(maxTime: int, edges: List[List[int]], passingFees: List[int]) -> int:
    n = len(passingFees)
    graph = defaultdict(list)
    for u, v, t in edges:
        graph[u].append((v, t))
        graph[v].append((u, t))
    pq = [(passingFees[0], 0, 0)]
    dist = {0: passingFees[0]}
    while pq:
        cost, node, time = heappop(pq)
        if node == n - 1:
            return cost
        if time > maxTime:
            continue
        for nei, t in graph[node]:
            if time + t <= maxTime:
                new_cost = cost + passingFees[nei]
                if nei not in dist or new_cost < dist[nei]:
                    dist[nei] = new_cost
                    heappush(pq, (new_cost, nei, time + t))
    return -1
```
#### TC: O(NlogN + ElogN) **SC:** O(N)

The code uses Dijkstra's algorithm to find the minimum cost to reach the destination within the
given maximum time. It maintains a priority queue to explore the nodes with the minimum cost. The
algorithm iterates through the edges and updates the cost to reach each node based on the passing
fees. The time complexity is O(NlogN + ElogN) where N is the number of nodes and E is the number of
edges. The space complexity is O(N) to store the distances to each node.

---
---
---
 --- 
# Minimum Cost to Make at Least One Valid Path in a Grid
>Find the minimum cost to make at least one valid path in a grid.

>Input: grid = [[1,2,3],[4,5,6],[7,8,9]]
Output: 12
- https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/
- Difficulty: 55
- Frequent: 65
- Tags: ['Graph', 'Dynamic Programming']

### Minimum Cost to Make at Least One Valid Path in a Grid -- Shortest Solution:
```python
from collections import deque
class Solution:
    def minCost(self, grid: List[List[int]]) -> int:
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        m, n = len(grid), len(grid[0])
        queue = deque([(0, 0, 0)])
        visited = set()
        cost = 0
        while queue:
            for _ in range(len(queue)):
                i, j, c = queue.popleft()
                if (i, j) == (m - 1, n - 1):
                    return cost
                if (i, j) in visited:
                    continue
                visited.add((i, j))
                for k, (di, dj) in enumerate(directions):
                    ni, nj = i + di, j + dj
                    if 0 <= ni < m and 0 <= nj < n:
                        if k + 1 != grid[i][j]:
                            queue.append((ni, nj, c + 1))
                        else:
                            queue.appendleft((ni, nj, c))
            cost += 1
        return -1
```
#### TC: O(m * n) **SC:** O(m * n)

The code uses a Breadth-First Search (BFS) approach to find the minimum cost to reach the bottom-
right cell of the grid. It maintains a queue to explore the grid cells in a level-by-level manner.
The visited set is used to keep track of visited cells to avoid revisiting them. The code adjusts
the cost based on the direction of movement and the value of the current cell. The time complexity
is O(m * n) where m and n are the dimensions of the grid, and the space complexity is also O(m * n)
due to the visited set and the queue.

---
---
---
 --- 
# Floyd-Warshall Algorithm
>Find the shortest paths between all pairs of nodes in a graph.

>Input: n = 4, edges = [[0,1,1],[0,2,5],[1,2,2],[1,3,1],[2,3,2]]
Output: [[0,1,3,4],[1,0,2,3],[2,1,0,2],[3,2,1,0]]
- https://leetcode.com/problems/floyd-warshall-algorithm/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Dynamic Programming']

### Floyd-Warshall Algorithm -- Shortest Solution:
```python
# Floyd-Warshall Algorithm

def floyd_warshall(n, connections):
    dist = [[float('inf')] * n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0
    for u, v, w in connections:
        dist[u][v] = w
    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist

# Example Usage
n = 4
connections = [[0, 1, 1], [0, 2, 4], [1, 2, 2], [1, 3, 5], [2, 3, 1]]
result = floyd_warshall(n, connections)
print(result)
```
#### TC: O(n^3) **SC:** O(n^2)

The Floyd-Warshall Algorithm is used to find the shortest paths between all pairs of vertices in a
weighted graph. The code initializes a distance matrix with infinite distances and updates it by
considering all intermediate vertices one by one. The algorithm has a time complexity of O(n^3) and
a space complexity of O(n^2), where n is the number of vertices in the graph. It is a dynamic
programming approach that guarantees finding the shortest paths between all pairs of vertices.

---
---
---
 --- 
# All Pairs Shortest Path
>Find the shortest paths between all pairs of nodes in a graph.

>Input: n = 4, edges = [[0,1,1],[0,2,5],[1,2,2],[1,3,1],[2,3,2]]
Output: [[0,1,3,4],[1,0,2,3],[2,1,0,2],[3,2,1,0]]
- https://leetcode.com/problems/all-pairs-shortest-path/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Dynamic Programming']

### All Pairs Shortest Path -- Shortest Solution:
```python
# Floyd-Warshall Algorithm

def allPairsShortestPath(n, edges):
    dist = [[float('inf')] * n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0
    for u, v, w in edges:
        dist[u][v] = w
    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist

# Example
n = 4
edges = [[0, 1, 1], [0, 2, 5], [1, 2, 2], [1, 3, 7], [2, 3, 3]]
result = allPairsShortestPath(n, edges)
print(result)
```
#### TC: O(n^3) **SC:** O(n^2)

The code implements the Floyd-Warshall Algorithm to find the shortest paths between all pairs of
vertices in a graph. It initializes a distance matrix with infinite values and updates it by
considering all possible intermediate vertices. The algorithm has a time complexity of O(n^3) and a
space complexity of O(n^2), where n is the number of vertices in the graph. The code is concise,
clean, and easy to understand, making it an optimal solution for the All Pairs Shortest Path
problem.

---
---
---
 --- 
# City With the Smallest Number of Neighbors at a Threshold Distance
>Find the city with the smallest number of neighbors within a threshold distance.

>Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3
- https://leetcode.com/problems/city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/
- Difficulty: 50
- Frequent: 45
- Tags: ['Graph', 'Shortest Path']

### City With the Smallest Number of Neighbors at a Threshold Distance -- Shortest Solution:
```python
from typing import List

def findTheCity(n: int, edges: List[List[int]], distanceThreshold: int) -> int:
    INF = float('inf')
    dist = [[INF] * n for _ in range(n)]
    for i, j, w in edges:
        dist[i][j] = dist[j][i] = w
    for i in range(n):
        dist[i][i] = 0
    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    res = {sum(d <= distanceThreshold for d in dist[i]): i for i in range(n)}
    return res[min(res.values())]
```
#### TC: O(n^3) **SC:** O(n^2)

The code uses the Floyd-Warshall algorithm to find the shortest path between all pairs of nodes in
the graph. It initializes the distance matrix with infinity values and updates it with the given
edge weights. Then, it iterates through all nodes to find the shortest path using dynamic
programming. Finally, it calculates the number of reachable cities within the threshold distance for
each city and returns the city with the smallest number of neighbors. The time complexity is O(n^3)
due to the triple nested loops, and the space complexity is O(n^2) for the distance matrix.

---
---
---