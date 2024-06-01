# Prim's Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Minimum Spanning Tree](#minimum-spanning-tree) | 70 | 50 |
---
Prim's algorithm is a greedy algorithm that finds a minimum spanning tree (MST) for a weighted undirected graph. It starts with a single vertex and grows the MST by repeatedly adding the smallest edge that connects a vertex in the MST to a vertex outside the MST.
```python
import heapq

def prim(graph, start):
    mst = []
    visited = set([start])
    edges = [(cost, start, to) for to, cost in graph[start].items()]
    heapq.heapify(edges)

    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in visited:
            visited.add(to)
            mst.append((frm, to, cost))
            for to_next, cost in graph[to].items():
                if to_next not in visited:
                    heapq.heappush(edges, (cost, to, to_next))
    return mst

graph = {
    'A': {'B': 1, 'C': 3},
    'B': {'A': 1, 'C': 3, 'D': 6},
    'C': {'A': 3, 'B': 3, 'D': 4},
    'D': {'B': 6, 'C': 4}
}

print(prim(graph, 'A'))
```
### Insight:
1. The algorithm uses a priority queue to select the smallest edge at each step.
2. The graph is represented as an adjacency list.
3. The 'visited' set keeps track of vertices that are already included in the MST.
4. The 'edges' list is a min-heap that stores the edges to be processed.
5. The algorithm continues until all vertices are included in the MST.
---
 --- 
# Minimum Spanning Tree
>Find the minimum spanning tree of a graph.

>Input: n = 4, edges = [[0,1,1],[1,2,2],[2,3,1],[0,3,4]]
Output: 4
- https://leetcode.com/problems/minimum-spanning-tree/
- Difficulty: 50
- Frequent: 70
- Tags: ['Graph', 'Greedy']

### Minimum Spanning Tree -- Shortest Solution:
```python
# Prim's Algorithm

def min_spanning_tree(n, edges):
    graph = defaultdict(list)
    for u, v, w in edges:
        graph[u].append((v, w))
        graph[v].append((u, w))
    mst = set()
    mst_weight = 0
    pq = [(0, 0)]
    while pq:
        weight, node = heappop(pq)
        if node not in mst:
            mst.add(node)
            mst_weight += weight
            for neighbor, neighbor_weight in graph[node]:
                if neighbor not in mst:
                    heappush(pq, (neighbor_weight, neighbor))
    return mst_weight
```
#### TC: O(E log V) **SC:** O(V + E)

The code implements Prim's Algorithm to find the minimum spanning tree of a graph. It uses a
priority queue to efficiently select the next edge with the smallest weight to add to the spanning
tree. The time complexity is O(E log V) where E is the number of edges and V is the number of
vertices. The space complexity is O(V + E) to store the graph and the priority queue. The algorithm
ensures that the minimum spanning tree is constructed by selecting the edges with the smallest
weights while avoiding cycles.

---
### Minimum Spanning Tree -- Best SC Solution:
```python
# Prim's Algorithm

def min_spanning_tree(n, edges):
    parent = [i for i in range(n)]
    rank = [0] * n
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    def union(x, y):
        root_x = find(x)
        root_y = find(y)
        if root_x != root_y:
            if rank[root_x] > rank[root_y]:
                parent[root_y] = root_x
            else:
                parent[root_x] = root_y
                if rank[root_x] == rank[root_y]:
                    rank[root_y] += 1
    edges.sort(key=lambda x: x[2])
    min_cost = 0
    for edge in edges:
        u, v, cost = edge
        if find(u) != find(v):
            union(u, v)
            min_cost += cost
    return min_cost

# Example
n = 4
edges = [[0, 1, 1], [0, 2, 3], [0, 3, 4], [1, 3, 2], [2, 3, 5]]
result = min_spanning_tree(n, edges)
print(result)
```
#### TC: O(E log E) **SC:** O(V)

The code implements Prim's Algorithm to find the minimum spanning tree of a graph. It uses the
concepts of disjoint sets and union-find to efficiently find the minimum cost spanning tree. The
time complexity is O(E log E) where E is the number of edges, and the space complexity is O(V) where
V is the number of vertices. The code sorts the edges based on their weights and then iterates
through them, adding edges to the minimum spanning tree if they do not create a cycle. The union-
find data structure helps in detecting cycles and maintaining the connectivity of the graph.

---
---
---