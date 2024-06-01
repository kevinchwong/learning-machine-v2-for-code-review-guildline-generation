# Graph Algorithms
## Frequent score for this algorithmic framework: 20 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Cheapest Flights Within K Stops](#cheapest-flights-within-k-stops) | 75 | 65 |
| 2 | [Network Delay Time](#network-delay-time) | 70 | 60 |
| 3 | [Critical Connections in a Network](#critical-connections-in-a-network) | 70 | 70 |
---
Graph algorithms are a set of instructions that traverse (or search) through a graph data structure. They are used to solve problems like finding the shortest path, detecting cycles, and finding connected components. Common graph algorithms include Depth-First Search (DFS), Breadth-First Search (BFS), Dijkstra's algorithm, and Kruskal's algorithm.
```python
# Depth-First Search (DFS) in Python

def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start)
    for next in graph[start] - visited:
        dfs(graph, next, visited)
    return visited

# Example usage:
graph = {'A': {'B', 'C'}, 'B': {'A', 'D', 'E'}, 'C': {'A', 'F'}, 'D': {'B'}, 'E': {'B', 'F'}, 'F': {'C', 'E'}}
dfs(graph, 'A')

# Breadth-First Search (BFS) in Python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    while queue:
        vertex = queue.popleft()
        print(vertex)
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# Example usage:
bfs(graph, 'A')
```
### Insight:
1. DFS uses a stack (can be the call stack via recursion) and is good for problems involving backtracking.
2. BFS uses a queue and is optimal for finding the shortest path in an unweighted graph.
3. Both algorithms visit each vertex and edge once, leading to their O(V + E) time complexity.
4. Space complexity is O(V) due to the storage of visited nodes.
5. Understanding the structure of the graph (e.g., adjacency list) is crucial for implementing these algorithms efficiently.
---
 --- 
# Cheapest Flights Within K Stops
>Find the cheapest flight within K stops.

>Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
- https://leetcode.com/problems/cheapest-flights-within-k-stops/
- Difficulty: 65
- Frequent: 75
- Tags: ['Graph', 'Shortest Path']

### Cheapest Flights Within K Stops -- Shortest Solution:
```python
import heapq
from collections import defaultdict, deque

def findCheapestPrice(n, flights, src, dst, K):
    graph = defaultdict(list)
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
#### TC: O(E + K * log(V)) **SC:** O(V + E)

1. **Graph Representation**: The graph is represented using an adjacency list where each node points
to a list of tuples representing its neighbors and the corresponding edge weights. 2. **Priority
Queue**: A priority queue (min-heap) is used to always expand the least costly node first, ensuring
that we find the cheapest path. 3. **Stops Management**: The number of stops is managed by
decrementing the allowed stops each time we push a new node onto the priority queue. This ensures
that we do not exceed the allowed number of stops. 4. **Early Termination**: The algorithm
terminates early if the destination node is reached, returning the cost immediately. 5. **Edge
Cases**: If the destination cannot be reached within the allowed stops, the function returns -1.

---
### Cheapest Flights Within K Stops -- Best TC Solution:
```python
import heapq
from collections import defaultdict

def findCheapestPrice(n, flights, src, dst, k):
    graph = defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
    pq = [(0, src, k + 1)]
    while pq:
        cost, node, stops = heapq.heappop(pq)
        if node == dst:
            return cost
        if stops > 0:
            for nei, price in graph[node]:
                heapq.heappush(pq, (cost + price, nei, stops - 1))
    return -1
```
#### TC: O(E + VlogV) **SC:** O(V)

The code uses Dijkstra's algorithm with a priority queue to find the cheapest flight within K stops.
It initializes a graph using defaultdict to store the flights. It then uses a priority queue to
explore the flights with the lowest cost. The algorithm terminates when the destination is reached
or when the number of stops exceeds K. The time complexity is O(E + VlogV) where E is the number of
flights and V is the number of vertices. The space complexity is O(V) where V is the number of
vertices in the graph.

---
---
---
 --- 
# Network Delay Time
>Find the time it takes for all nodes to receive a signal from a given node.

>Input: times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2
Output: 2
- https://leetcode.com/problems/network-delay-time/
- Difficulty: 60
- Frequent: 70
- Tags: ['Graph', 'Shortest Path']

### Network Delay Time -- Shortest Solution:
```python
import heapq

def networkDelayTime(times, n, k):
    graph = {i: [] for i in range(1, n + 1)}
    for u, v, w in times:
        graph[u].append((w, v))
    min_heap = [(0, k)]
    visited = set()
    t = 0
    while min_heap:
        w1, n1 = heapq.heappop(min_heap)
        if n1 in visited:
            continue
        visited.add(n1)
        t = max(t, w1)
        for w2, n2 in graph[n1]:
            if n2 not in visited:
                heapq.heappush(min_heap, (w1 + w2, n2))
    return t if len(visited) == n else -1
```
#### TC: O((N + E) log N) **SC:** O(N + E)

The solution uses Dijkstra's algorithm to find the shortest path from the starting node `k` to all
other nodes in the graph. The graph is represented as an adjacency list. A min-heap (priority queue)
is used to always extend the shortest known path. The algorithm keeps track of visited nodes to
avoid processing a node more than once. The maximum time taken to reach any node is tracked and
returned if all nodes are visited; otherwise, -1 is returned.

---
### Network Delay Time -- Best TC Solution:
```python
import heapq
from collections import defaultdict

class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        graph = defaultdict(list)
        for u, v, w in times:
            graph[u].append((v, w))
        pq = [(0, k)]
        dist = {}
        while pq:
            d, node = heapq.heappop(pq)
            if node in dist:
                continue
            dist[node] = d
            for nei, d2 in graph[node]:
                if nei not in dist:
                    heapq.heappush(pq, (d + d2, nei))
        return max(dist.values()) if len(dist) == n else -1
```
#### TC: O(ElogE) where E is the number of edges **SC:** O(N + E) where N is the number of nodes and E is the number of edges

The code uses Dijkstra's algorithm to find the shortest path from a source node to all other nodes
in a weighted graph. It initializes a priority queue to keep track of the nodes to be visited next
based on their distances. The algorithm iterates through the priority queue, updating the distances
and nodes until all nodes are visited. The final result is the maximum distance from the source node
to any other node. The time complexity is O(ElogE) due to the heap operations, and the space
complexity is O(N + E) to store the graph and distances.

---
---
---
 --- 
# Critical Connections in a Network
>Find all critical connections in a network.

>Input: n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
Output: [[1,3]]
- https://leetcode.com/problems/critical-connections-in-a-network/
- Difficulty: 70
- Frequent: 70
- Tags: ['Graph', 'Depth-First Search']

### Critical Connections in a Network -- Shortest Solution:
```python
from collections import defaultdict

def criticalConnections(n, connections):
    def dfs(node, discovery_time):
        disc[node] = low[node] = discovery_time
        discovery_time += 1
        for neighbor in graph[node]:
            if disc[neighbor] == -1:
                parent[neighbor] = node
                dfs(neighbor, discovery_time)
                low[node] = min(low[node], low[neighbor])
                if low[neighbor] > disc[node]:
                    result.append([node, neighbor])
            elif neighbor != parent[node]:
                low[node] = min(low[node], disc[neighbor])

    graph = defaultdict(list)
    for u, v in connections:
        graph[u].append(v)
        graph[v].append(u)

    disc = [-1] * n
    low = [-1] * n
    parent = [-1] * n
    result = []
    for i in range(n):
        if disc[i] == -1:
            dfs(i, 0)
    return result
```
#### TC: O(V + E) **SC:** O(V + E)

The solution uses Tarjan's algorithm to find bridges in an undirected graph. The `dfs` function
performs a Depth-First Search to discover nodes and update their discovery and low times. If a
node's low time is greater than its discovery time, it is a critical connection. The graph is
represented using an adjacency list, and the algorithm runs in linear time relative to the number of
vertices and edges.

---
---
---