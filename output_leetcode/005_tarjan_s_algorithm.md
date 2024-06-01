# Tarjan's Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Graph Valid Tree](#graph-valid-tree) | 70 | 50 |
| 2 | [Critical Connections in a Network](#critical-connections-in-a-network) | 70 | 60 |
| 3 | [Accounts Merge](#accounts-merge) | 65 | 60 |
| 4 | [Strongly Connected Components](#strongly-connected-components) | 65 | 70 |
| 5 | [Longest Increasing Path in a Matrix](#longest-increasing-path-in-a-matrix) | 65 | 70 |
| 6 | [Number of Connected Components in an Undirected Graph](#number-of-connected-components-in-an-undirected-graph) | 60 | 55 |
| 7 | [Bridges in Graph](#bridges-in-graph) | 60 | 65 |
| 8 | [Articulation Points in Graph](#articulation-points-in-graph) | 55 | 70 |
| 9 | [Longest Cycle in a Graph](#longest-cycle-in-a-graph) | 55 | 70 |
| 10 | [Find Eventual Safe States](#find-eventual-safe-states) | 50 | 60 |
| 11 | [Biconnected Components](#biconnected-components) | 50 | 75 |
---
Tarjan's Algorithm is used to find strongly connected components (SCCs) in a directed graph. It uses depth-first search (DFS) to traverse the graph and employs a stack to keep track of the visited nodes. The algorithm assigns each node a unique index and a low-link value, which helps in identifying the SCCs.
```python
class Graph:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = defaultdict(list)
        self.Time = 0

    def addEdge(self, u, v):
        self.graph[u].append(v)

    def SCCUtil(self, u, low, disc, stackMember, st):
        disc[u] = self.Time
        low[u] = self.Time
        self.Time += 1
        stackMember[u] = True
        st.append(u)

        for v in self.graph[u]:
            if disc[v] == -1:
                self.SCCUtil(v, low, disc, stackMember, st)
                low[u] = min(low[u], low[v])
            elif stackMember[v] == True:
                low[u] = min(low[u], disc[v])

        w = -1
        if low[u] == disc[u]:
            while w != u:
                w = st.pop()
                print(w, end=' ')
                stackMember[w] = False
            print()

    def SCC(self):
        disc = [-1] * self.V
        low = [-1] * self.V
        stackMember = [False] * self.V
        st = []

        for i in range(self.V):
            if disc[i] == -1:
                self.SCCUtil(i, low, disc, stackMember, st)

# Example usage
if __name__ == "__main__":
    g1 = Graph(5)
    g1.addEdge(1, 0)
    g1.addEdge(0, 2)
    g1.addEdge(2, 1)
    g1.addEdge(0, 3)
    g1.addEdge(3, 4)

    print("SSC in first graph ")
    g1.SCC()
```
### Insight:
1. The algorithm uses a stack to keep track of the nodes visited during the DFS traversal.
2. Each node is assigned a discovery time and a low-link value, which helps in identifying the root of an SCC.
3. The low-link value of a node is the smallest discovery time reachable from that node, including itself.
4. When the discovery time of a node is equal to its low-link value, it indicates the root of an SCC.
5. The algorithm ensures that each node and edge is processed only once, leading to its linear time complexity.
---
 --- 
# Graph Valid Tree
>Determine if a given graph is a valid tree.

>Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true
- https://leetcode.com/problems/graph-valid-tree/
- Difficulty: 50
- Frequent: 70
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Graph Valid Tree -- Shortest Solution:
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
#### TC: O(V + E) **SC:** O(V)

The code checks if the given graph is a valid tree by ensuring it has n-1 edges and is connected. It
uses a depth-first search (DFS) approach to traverse the graph and detect cycles. The time
complexity is O(V + E) where V is the number of vertices and E is the number of edges. The space
complexity is O(V) for the visited array. The code is clean, concise, and easy to understand, making
it a smart solution for the Graph Valid Tree problem.

---
---
---
 --- 
# Critical Connections in a Network
>Find all critical connections in a network.

>Input: n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
Output: [[1,3]]
- https://leetcode.com/problems/critical-connections-in-a-network/
- Difficulty: 60
- Frequent: 70
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Critical Connections in a Network -- Shortest Solution:
```python
from collections import defaultdict
def criticalConnections(n, connections):
    graph = defaultdict(list)
    for u, v in connections:
        graph[u].append(v)
        graph[v].append(u)
    rank = [-2] * n
    def dfs(node, depth):
        if rank[node] >= 0:
            return rank[node]
        rank[node] = depth
        min_back_depth = n
        for neighbor in graph[node]:
            if rank[neighbor] == depth - 1:
                continue
            back_depth = dfs(neighbor, depth + 1)
            if back_depth <= depth:
                connections.remove([node, neighbor])
            min_back_depth = min(min_back_depth, back_depth)
        return min_back_depth
    dfs(0, 0)
    return connections
```
#### TC: O(V + E) **SC:** O(V + E)

The code implements the Tarjan's Algorithm approach to find critical connections in a network. It
uses depth-first search (DFS) to traverse the graph and identify the critical connections. The time
complexity is O(V + E) where V is the number of vertices and E is the number of edges in the graph.
The space complexity is also O(V + E) to store the graph and rank array. The code efficiently
removes non-critical connections during the DFS traversal, resulting in a clean and concise
solution.

---
---
---
 --- 
# Accounts Merge
>Merge accounts with common email addresses.

>Input: accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
Output: [["John", "john00@mail.com", "john_newyork@mail.com", "johnsmith@mail.com"],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
- https://leetcode.com/problems/accounts-merge/
- Difficulty: 60
- Frequent: 65
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Accounts Merge -- Shortest Solution:
```python
from collections import defaultdict
class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
        def union(x, y):
            parent[find(x)] = find(y)
        parent = {}
        owner = {}
        for acc in accounts:
            for email in acc[1:]:
                parent[email] = email
                owner[email] = acc[0]
        for acc in accounts:
            p = find(acc[1])
            for email in acc[2:]:
                union(email, p)
        merged = defaultdict(list)
        for email in parent:
            merged[find(email)].append(email)
        return [[owner[k]] + sorted(v) for k, v in merged.items()]
```
#### TC: O(n log n) **SC:** O(n)

The code uses a union-find approach to merge accounts efficiently. It first initializes parent and
owner dictionaries to store email and account owner mappings. Then, it iterates through the accounts
to union emails within the same account. Finally, it creates a merged dictionary to group emails by
their root parent and returns the merged accounts. The time complexity is O(n log n) due to the
sorting operation, and the space complexity is O(n) for the parent and owner dictionaries.

---
---
---
 --- 
# Strongly Connected Components
>Find all strongly connected components in a directed graph.

>Input: n = 5, edges = [[1,0],[0,2],[2,1],[0,3],[3,4]]
Output: [[0,1,2],[3],[4]]
- https://leetcode.com/problems/strongly-connected-components/
- Difficulty: 70
- Frequent: 65
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Strongly Connected Components -- Shortest Solution:
```python
# Tarjan's Algorithm

def strongly_connected_components(self, n: int, edges: List[List[int]]) -> List[List[int]]:
    def tarjan(node):
        nonlocal index, stack, low_link, on_stack, result
        index += 1
        stack.append(node)
        low_link[node] = index
        on_stack[node] = True
        for neighbor in graph[node]:
            if low_link[neighbor] == -1:
                tarjan(neighbor)
                low_link[node] = min(low_link[node], low_link[neighbor])
            elif on_stack[neighbor]:
                low_link[node] = min(low_link[node], low_link[neighbor])
        if low_link[node] == index:
            component = []
            while True:
                top = stack.pop()
                component.append(top)
                on_stack[top] = False
                if top == node:
                    break
            result.append(component)

    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)

    index = -1
    stack = []
    low_link = [-1] * n
    on_stack = [False] * n
    result = []

    for i in range(n):
        if low_link[i] == -1:
            tarjan(i)

    return result
```
#### TC: O(V + E) **SC:** O(V)

The code implements Tarjan's Algorithm to find the strongly connected components in a directed
graph. The algorithm uses a depth-first search approach to identify the components efficiently. The
time complexity is O(V + E) where V is the number of vertices and E is the number of edges. The
space complexity is O(V) to store the components. The code is clean, concise, and easy to
understand, making it a smart solution for finding strongly connected components in a graph.

---
---
---
 --- 
# Longest Increasing Path in a Matrix
>Find the length of the longest increasing path in a matrix.

>Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
- https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
- Difficulty: 70
- Frequent: 65
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Longest Increasing Path in a Matrix -- Shortest Solution:
```python
from functools import lru_cache
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        if not matrix:
            return 0
        rows, cols = len(matrix), len(matrix[0])
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
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

The code uses a depth-first search (DFS) approach with memoization to find the longest increasing
path in a matrix. It recursively explores all possible paths starting from each cell and keeps track
of the maximum path length. The @lru_cache decorator is used to cache the results of subproblems to
avoid redundant calculations. The time complexity is O(m*n) where m is the number of rows and n is
the number of columns in the matrix. The space complexity is also O(m*n) to store the memoization
results. Overall, the code is concise, efficient, and easy to understand.

---
---
---
 --- 
# Number of Connected Components in an Undirected Graph
>Find the number of connected components in an undirected graph.

>Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
- https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/
- Difficulty: 55
- Frequent: 60
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Number of Connected Components in an Undirected Graph -- Shortest Solution:
```python
from collections import defaultdict
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
        def union(x, y):
            root_x, root_y = find(x), find(y)
            if root_x != root_y:
                parent[root_x] = root_y
        parent = {i: i for i in range(n)}
        for x, y in edges:
            union(x, y)
        components = set()
        for i in range(n):
            components.add(find(i))
        return len(components)
```
#### TC: O(n + m * alpha(n)) **SC:** O(n)

The code defines a Solution class with a countComponents method that takes the number of nodes 'n'
and a list of edges 'edges'. It uses the union-find algorithm to find connected components in an
undirected graph. The find function recursively finds the parent of a node, while the union function
merges two components. The parent dictionary stores the parent of each node. The code iterates
through the edges to perform unions and then counts the unique components to determine the number of
connected components. The time complexity is O(n + m * alpha(n)), where n is the number of nodes, m
is the number of edges, and alpha(n) is the inverse Ackermann function. The space complexity is O(n)
to store the parent dictionary.

---
### Number of Connected Components in an Undirected Graph -- Best TC Solution:
```python
from collections import defaultdict

class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        def find(parent, i):
            if parent[i] == -1:
                return i
            if parent[i] != i:
                parent[i] = find(parent, parent[i])
            return parent[i]
        def union(parent, rank, x, y):
            x_root = find(parent, x)
            y_root = find(parent, y)
            if x_root != y_root:
                if rank[x_root] < rank[y_root]:
                    parent[x_root] = y_root
                elif rank[x_root] > rank[y_root]:
                    parent[y_root] = x_root
                else:
                    parent[y_root] = x_root
                    rank[x_root] += 1
        parent = [-1] * n
        rank = [0] * n
        for edge in edges:
            union(parent, rank, edge[0], edge[1])
        components = set()
        for i in range(n):
            components.add(find(parent, i))
        return len(components)
```
#### TC: O(N + E) **SC:** O(N)

The code uses the Union-Find algorithm to find the number of connected components in an undirected
graph efficiently. It iterates through the edges to perform union operations and then counts the
unique parent nodes to determine the number of components. The time complexity is O(N + E) where N
is the number of nodes and E is the number of edges. The space complexity is O(N) to store the
parent and rank arrays.

---
### Number of Connected Components in an Undirected Graph -- Best SC Solution:
```python
from collections import defaultdict
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        def dfs(node):
            if visited[node]:
                return
            visited[node] = True
            for neighbor in graph[node]:
                dfs(neighbor)
        graph = defaultdict(list)
        for edge in edges:
            graph[edge[0]].append(edge[1])
            graph[edge[1]].append(edge[0])
        visited = [False] * n
        components = 0
        for i in range(n):
            if not visited[i]:
                dfs(i)
                components += 1
        return components
```
#### TC: O(V + E) **SC:** O(V + E)

The code uses a depth-first search (DFS) approach to find the number of connected components in an
undirected graph. It initializes a graph using a defaultdict to store the edges. Then, it performs
DFS on each node to mark all connected nodes as visited. The components variable is incremented each
time a new connected component is found. The time complexity is O(V + E) where V is the number of
vertices and E is the number of edges, and the space complexity is also O(V + E) to store the graph
and visited nodes.

---
---
---
 --- 
# Bridges in Graph
>Find all bridges in an undirected graph.

>Input: n = 5, edges = [[0,1],[1,2],[2,0],[1,3],[3,4]]
Output: [[1,3],[3,4]]
- https://leetcode.com/problems/bridges-in-graph/
- Difficulty: 65
- Frequent: 60
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Bridges in Graph -- Shortest Solution:
```python
from typing import List

def criticalConnections(n: int, connections: List[List[int]]) -> List[List[int]]:
    graph = [[] for _ in range(n)]
    for u, v in connections:
        graph[u].append(v)
        graph[v].append(u)
    rank = [-2] * n
    res = []
    def dfs(node, depth):
        if rank[node] >= 0:
            return rank[node]
        rank[node] = depth
        min_back_depth = n
        for neighbor in graph[node]:
            if rank[neighbor] == depth - 1:
                continue
            back_depth = dfs(neighbor, depth + 1)
            if back_depth <= depth:
                res.append([node, neighbor])
            min_back_depth = min(min_back_depth, back_depth)
        return min_back_depth
    dfs(0, 0)
    return res
```
#### TC: O(V + E) **SC:** O(V + E)

The code implements Tarjan's Algorithm to find critical connections in a graph. It uses depth-first
search (DFS) to traverse the graph and identify bridges. The rank array is used to keep track of the
depth of each node during the DFS traversal. The algorithm identifies critical connections by
comparing the back depth of each neighbor with the current depth. If the back depth is less than or
equal to the current depth, it indicates a critical connection. The time complexity is O(V + E)
where V is the number of vertices and E is the number of edges, and the space complexity is also O(V
+ E) to store the graph and rank array.

---
---
---
 --- 
# Articulation Points in Graph
>Find all articulation points in an undirected graph.

>Input: n = 5, edges = [[0,1],[1,2],[2,0],[1,3],[3,4]]
Output: [1,3]
- https://leetcode.com/problems/articulation-points-in-graph/
- Difficulty: 70
- Frequent: 55
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Articulation Points in Graph -- Shortest Solution:
```python
from collections import defaultdict
def articulation_points(graph):
    def dfs(node, parent, time):
        nonlocal result
        visited[node] = True
        discovery_time[node] = time
        low_time[node] = time
        children = 0
        for neighbor in graph[node]:
            if not visited[neighbor]:
                children += 1
                dfs(neighbor, node, time + 1)
                low_time[node] = min(low_time[node], low_time[neighbor])
                if parent != -1 and low_time[neighbor] >= discovery_time[node]:
                    result.add(node)
                if parent == -1 and children > 1:
                    result.add(node)
            elif neighbor != parent:
                low_time[node] = min(low_time[node], discovery_time[neighbor])
    result = set()
    visited = defaultdict(bool)
    discovery_time = defaultdict(int)
    low_time = defaultdict(int)
    for node in graph:
        if not visited[node]:
            dfs(node, -1, 0)
    return result

# Example Usage
graph = {
    0: [1, 2],
    1: [0, 2],
    2: [0, 1, 3],
    3: [2, 4],
    4: [3]
}
print(articulation_points(graph))
```
#### TC: O(V + E) **SC:** O(V)

The code defines a function to find articulation points in a graph using Tarjan's Algorithm. It
performs a depth-first search (DFS) traversal to identify the articulation points based on the low
time and discovery time of each node. The algorithm keeps track of visited nodes, discovery time,
and low time for each node. The time complexity is O(V + E) where V is the number of vertices and E
is the number of edges in the graph. The space complexity is O(V) to store the visited, discovery
time, and low time information for each node.

---
---
---
 --- 
# Longest Cycle in a Graph
>Find the longest cycle in a directed graph.

>Input: edges = [3,3,4,2,3]
Output: 3
- https://leetcode.com/problems/longest-cycle-in-a-graph/
- Difficulty: 70
- Frequent: 55
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Longest Cycle in a Graph -- Shortest Solution:
```python
# Function to find the longest cycle in a graph
from collections import defaultdict

def longest_cycle(graph):
    def dfs(node, visited, path):
        visited.add(node)
        path.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor, visited, path)
            elif neighbor == path[0] and len(path) > 2:
                cycles.append(path[:])
        path.pop()
        visited.remove(node)
    
    cycles = []
    for node in graph:
        dfs(node, set(), [])
    
    if not cycles:
        return 0
    
    return max(len(cycle) for cycle in cycles)

# Example
graph = {
    0: [1, 2],
    1: [2],
    2: [0, 3],
    3: [3]
}
result = longest_cycle(graph)
print(result)
```
#### TC: O(V + E) **SC:** O(V)

The code defines a function to find the longest cycle in a graph using a depth-first search (DFS)
approach. It iterates through each node in the graph and performs a DFS to find cycles starting from
that node. If a cycle is found, it is added to the list of cycles. The function then returns the
length of the longest cycle found. The time complexity is O(V + E) where V is the number of vertices
and E is the number of edges in the graph. The space complexity is O(V) due to the recursive calls
in the DFS function.

---
### Longest Cycle in a Graph -- Best TC Solution:
```python
# Function to find the longest cycle in a graph
from collections import defaultdict

def longest_cycle(graph):
    def dfs(node, visited, path):
        visited[node] = True
        path.append(node)
        for neighbor in graph[node]:
            if neighbor not in path:
                if neighbor in path[:-2]:
                    return len(path[path.index(neighbor):])
                if not visited[neighbor]:
                    cycle_length = dfs(neighbor, visited, path)
                    if cycle_length:
                        return cycle_length
        path.pop()
        return 0

    longest = 0
    for node in graph:
        visited = defaultdict(bool)
        cycle_length = dfs(node, visited, [])
        longest = max(longest, cycle_length)
    return longest

# Example usage
graph = {
    0: [1, 2],
    1: [2],
    2: [0, 3],
    3: [3]
}
result = longest_cycle(graph)
print(result)
```
#### TC: O(V*(V+E)) **SC:** O(V)

The code defines a function `longest_cycle` that uses a depth-first search (DFS) approach to find
the longest cycle in a graph. It iterates through each node in the graph and performs a DFS to
detect cycles starting from that node. The function keeps track of visited nodes and the current
path to avoid revisiting nodes. The cycle length is calculated by finding the index of the neighbor
in the path and returning the length of the cycle. The longest cycle length found is updated and
returned as the result. The time complexity is O(V*(V+E)), where V is the number of vertices and E
is the number of edges, and the space complexity is O(V) for the visited dictionary.

---
---
---
 --- 
# Find Eventual Safe States
>Find all eventual safe states in a directed graph.

>Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]
- https://leetcode.com/problems/find-eventual-safe-states/
- Difficulty: 60
- Frequent: 50
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Find Eventual Safe States -- Shortest Solution:
```python
from typing import List

def eventualSafeNodes(graph: List[List[int]]) -> List[int]:
    def dfs(node):
        if colors[node] != 0:
            return colors[node] == 1
        colors[node] = 2
        for neighbor in graph[node]:
            if not dfs(neighbor):
                return False
        colors[node] = 1
        return True

    n = len(graph)
    colors = [0] * n
    return [i for i in range(n) if dfs(i)]
```
#### TC: O(N + E) **SC:** O(N)

The code uses a depth-first search (DFS) approach to determine the eventual safe nodes in a directed
graph. It marks nodes with colors (0 - unvisited, 1 - safe, 2 - unsafe) to track the state of each
node during the DFS traversal. The function 'dfs' recursively visits each node and its neighbors,
marking them as safe or unsafe based on the traversal. The eventual safe nodes are those that are
marked as safe after the traversal. The time complexity is O(N + E) where N is the number of nodes
and E is the number of edges in the graph. The space complexity is O(N) for the colors array storing
the state of each node.

---
---
---
 --- 
# Biconnected Components
>Find all biconnected components in an undirected graph.

>Input: n = 5, edges = [[0,1],[1,2],[2,0],[1,3],[3,4]]
Output: [[[0,1,2]],[[1,3]],[[3,4]]]
- https://leetcode.com/problems/biconnected-components/
- Difficulty: 75
- Frequent: 50
- Tags: ['Graph', 'DFS', "Tarjan's Algorithm"]

### Biconnected Components -- Shortest Solution:
```python
# Tarjan's Algorithm for finding Biconnected Components

def biconnected_components(graph):
    def dfs(u, p):
        nonlocal time
        low[u] = disc[u] = time
        time += 1
        for v in graph[u]:
            if disc[v] == -1:
                stack.append((u, v))
                dfs(v, u)
                low[u] = min(low[u], low[v])
                if low[v] >= disc[u]:
                    components.append([])
                    while stack[-1] != (u, v):
                        components[-1].append(stack.pop())
                    components[-1].append(stack.pop())
            elif v != p and disc[v] < disc[u]:
                low[u] = min(low[u], disc[v])
                stack.append((u, v))
    n = len(graph)
    disc = [-1] * n
    low = [-1] * n
    time = 0
    stack = []
    components = []
    for u in range(n):
        if disc[u] == -1:
            dfs(u, -1)
    return components

# Example Usage
graph = {
    0: [1, 2],
    1: [0, 2],
    2: [0, 1, 3],
    3: [2, 4, 5],
    4: [3, 5],
    5: [3, 4]
}

biconnected_components(graph)
```
#### TC: O(V + E) **SC:** O(V + E)

The code implements Tarjan's Algorithm to find the Biconnected Components in a graph. It uses depth-
first search (DFS) to traverse the graph and identify the biconnected components efficiently. The
algorithm maintains two arrays, disc[] and low[], to keep track of the discovery time and low time
of each vertex. By using a stack to store the edges in the current biconnected component, the
algorithm can identify and extract the biconnected components when a bridge is encountered. The time
complexity of the algorithm is O(V + E) where V is the number of vertices and E is the number of
edges in the graph. The space complexity is also O(V + E) to store the discovery and low times for
each vertex.

---
---
---