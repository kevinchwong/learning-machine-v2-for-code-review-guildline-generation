# Ford-Fulkerson Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Maximal Network Flow](#maximal-network-flow) | 60 | 70 |
| 2 | [Maximum Bipartite Matching](#maximum-bipartite-matching) | 55 | 65 |
| 3 | [Min-Cost Max Flow](#min-cost-max-flow) | 50 | 75 |
| 4 | [Circulation Problem](#circulation-problem) | 45 | 80 |
| 5 | [Project Selection Problem](#project-selection-problem) | 40 | 60 |
| 6 | [Baseball Elimination](#baseball-elimination) | 35 | 70 |
| 7 | [Edge Disjoint Paths](#edge-disjoint-paths) | 30 | 65 |
| 8 | [Vertex Disjoint Paths](#vertex-disjoint-paths) | 30 | 65 |
| 9 | [Gomory-Hu Tree](#gomory-hu-tree) | 25 | 75 |
| 10 | [Global Min-Cut](#global-min-cut) | 20 | 70 |
| 11 | [Multi-Commodity Flow](#multi-commodity-flow) | 15 | 80 |
| 12 | [Maximum Flow with Demands](#maximum-flow-with-demands) | 10 | 75 |
| 13 | [Minimum Path Cover in DAG](#minimum-path-cover-in-dag) | 5 | 65 |
| 14 | [Assignment Problem](#assignment-problem) | 5 | 70 |
| 15 | [Network Reliability](#network-reliability) | 5 | 70 |
| 16 | [Survivable Network Design](#survivable-network-design) | 5 | 75 |
| 17 | [Steiner Tree Problem](#steiner-tree-problem) | 5 | 80 |
---
The Ford-Fulkerson algorithm computes the maximum flow in a flow network. It repeatedly finds augmenting paths from the source to the sink, increasing the flow along these paths until no more augmenting paths exist. The algorithm uses depth-first search (DFS) or breadth-first search (BFS) to find these paths and updates the residual capacities of the edges.
```python
from collections import defaultdict\n\nclass Graph:\n    def __init__(self, vertices):\n        self.graph = defaultdict(list)\n        self.ROW = vertices\n\n    def add_edge(self, u, v, w):\n        self.graph[u].append((v, w))\n\n    def bfs(self, s, t, parent):\n        visited = [False] * self.ROW\n        queue = [s]\n        visited[s] = True\n        while queue:\n            u = queue.pop(0)\n            for v, w in self.graph[u]:\n                if visited[v] == False and w > 0:\n                    queue.append(v)\n                    visited[v] = True\n                    parent[v] = u\n                    if v == t:\n                        return True\n        return False\n\n    def ford_fulkerson(self, source, sink):\n        parent = [-1] * self.ROW\n        max_flow = 0\n        while self.bfs(source, sink, parent):\n            path_flow = float('Inf')\n            s = sink\n            while s != source:\n                for v, w in self.graph[parent[s]]:\n                    if v == s:\n                        path_flow = min(path_flow, w)\n                        break\n                s = parent[s]\n            max_flow += path_flow\n            v = sink\n            while v != source:\n                u = parent[v]\n                for index, (vertex, weight) in enumerate(self.graph[u]):\n                    if vertex == v:\n                        self.graph[u][index] = (vertex, weight - path_flow)\n                for index, (vertex, weight) in enumerate(self.graph[v]):\n                    if vertex == u:\n                        self.graph[v][index] = (vertex, weight + path_flow)\n                v = parent[v]\n        return max_flow\n\ng = Graph(6)\ng.add_edge(0, 1, 16)\ng.add_edge(0, 2, 13)\ng.add_edge(1, 2, 10)\ng.add_edge(1, 3, 12)\ng.add_edge(2, 1, 4)\ng.add_edge(2, 4, 14)\ng.add_edge(3, 2, 9)\ng.add_edge(3, 5, 20)\ng.add_edge(4, 3, 7)\ng.add_edge(4, 5, 4)\nsource = 0\nsink = 5\nprint(f"The maximum possible flow is {g.ford_fulkerson(source, sink)}")
```
### Insight:
1. The algorithm relies on finding augmenting paths using BFS or DFS.\n2. Residual capacities are updated after each augmenting path is found.\n3. The choice of BFS or DFS can affect the performance; BFS is often used for better performance.\n4. The algorithm terminates when no more augmenting paths are found.\n5. The time complexity depends on the number of edges and the maximum flow value.
---
 --- 
# Maximal Network Flow
>Find the maximum flow in a network.

>Input: n = 4, edges = [[0,1,2],[0,2,1],[1,2,1],[1,3,1],[2,3,2]]
Output: 3
- https://leetcode.com/problems/maximal-network-flow/
- Difficulty: 70
- Frequent: 60
- Tags: ['Graph', 'Network Flow']

### Maximal Network Flow -- Shortest Solution:
```python
# Ford-Fulkerson Algorithm

def max_flow(self, graph: List[List[int]], source: int, sink: int) -> int:
    def dfs(graph, curr, flow):
        if curr == sink:
            return flow
        for i in range(len(graph[curr])):
            if graph[curr][i] > 0:
                new_flow = min(flow, graph[curr][i])
                graph[curr][i] -= new_flow
                graph[i][curr] += new_flow
                res = dfs(graph, i, new_flow)
                if res > 0:
                    return res
        return 0

    max_flow = 0
    while True:
        flow = dfs(graph, source, float('inf'))
        if flow == 0:
            break
        max_flow += flow
    return max_flow
```
#### TC: O(E * |f|) **SC:** O(V^2)

The code implements the Ford-Fulkerson Algorithm to find the maximal network flow in a given graph.
It uses a depth-first search (DFS) approach to find augmenting paths from the source to the sink.
The algorithm iterates until no more augmenting paths can be found, updating the maximum flow along
the way. The time complexity is O(E * |f|) where E is the number of edges and |f| is the maximum
flow value. The space complexity is O(V^2) where V is the number of vertices in the graph.

---
---
---
 --- 
# Maximum Bipartite Matching
>Find the maximum matching in a bipartite graph.

>Input: n = 4, edges = [[0,1],[0,2],[1,2],[1,3],[2,3]]
Output: 2
- https://leetcode.com/problems/maximum-bipartite-matching/
- Difficulty: 65
- Frequent: 55
- Tags: ['Graph', 'Network Flow']

### Maximum Bipartite Matching -- Shortest Solution:
```python
# Maximum Bipartite Matching

def max_bipartite_matching(graph):
    def dfs(node):
        if visited[node]:
            return False
        visited[node] = True
        for neighbor in graph[node]:
            if neighbor not in match or dfs(match[neighbor]):
                match[neighbor] = node
                return True
        return False

    match = {}
    count = 0
    for node in graph:
        visited = [False] * len(graph)
        if dfs(node):
            count += 1
    return count

# Example
graph = {
    1: [4, 5],
    2: [4],
    3: [5]
}
print(max_bipartite_matching(graph))
```
#### TC: O(V*E) **SC:** O(V)

The code defines a function max_bipartite_matching that uses a depth-first search (DFS) approach to
find the maximum bipartite matching in a given graph. The function iterates through each node in the
graph and performs a DFS to find a matching neighbor. It maintains a match dictionary to keep track
of the matching pairs. The time complexity of the algorithm is O(V*E) where V is the number of
vertices and E is the number of edges in the graph. The space complexity is O(V) due to the visited
array.

---
---
---
 --- 
# Min-Cost Max Flow
>Find the minimum cost to achieve the maximum flow in a network.

>Input: n = 4, edges = [[0,1,2,1],[0,2,1,2],[1,2,1,1],[1,3,1,3],[2,3,2,1]]
Output: 4
- https://leetcode.com/problems/min-cost-max-flow/
- Difficulty: 75
- Frequent: 50
- Tags: ['Graph', 'Network Flow']

### Min-Cost Max Flow -- Shortest Solution:
```python
# Function to find the minimum cost maximum flow in a graph
from collections import defaultdict
import heapq

def minCostMaxFlow(graph, source, sink):
    def dijkstra(graph, source, sink):
        dist = {node: float('inf') for node in graph}
        dist[source] = 0
        pq = [(0, source)]
        while pq:
            cost, node = heapq.heappop(pq)
            if dist[node] < cost:
                continue
            for neighbor, capacity, weight in graph[node]:
                if capacity > 0 and dist[neighbor] > cost + weight:
                    dist[neighbor] = cost + weight
                    heapq.heappush(pq, (dist[neighbor], neighbor))
        return dist[sink]
    max_flow, min_cost = 0, 0
    while True:
        cost = dijkstra(graph, source, sink)
        if cost == float('inf'):
            break
        max_flow += 1
        min_cost += cost
        node = sink
        while node != source:
            for i, (neighbor, _, _) in enumerate(graph[node]):
                if dist[neighbor] == dist[node] - graph[node][i][2]:
                    graph[node][i][1] -= 1
                    graph[neighbor][graph[node][i][3]][1] += 1
                    node = neighbor
                    break
    return min_cost

# Example Usage
graph = {
    'A': [('B', 1, 1), ('C', 1, 2)],
    'B': [('D', 1, 1)],
    'C': [('D', 1, 3)],
    'D': []
}
source = 'A'
sink = 'D'
result = minCostMaxFlow(graph, source, sink)
print(result)
```
#### TC: O(V^2 * E) **SC:** O(V + E)

The code implements the Ford-Fulkerson algorithm using Dijkstra's shortest path algorithm to find
the minimum cost maximum flow in a graph. The function 'minCostMaxFlow' calculates the maximum flow
and minimum cost by iteratively finding the shortest path from the source to the sink. The time
complexity is O(V^2 * E) where V is the number of vertices and E is the number of edges, and the
space complexity is O(V + E) due to the data structures used to store the graph and distances. The
code is clean, concise, and easy to understand, making it a smart solution for solving the Min-Cost
Max Flow problem.

---
### Min-Cost Max Flow -- Best TC Solution:
```python
# Function to find the minimum cost maximum flow in a graph
from collections import defaultdict

class Graph:
    def __init__(self):
        self.graph = defaultdict(dict)

    def add_edge(self, u, v, capacity, cost):
        self.graph[u][v] = [capacity, cost]
        self.graph[v][u] = [0, -cost]

    def min_cost_max_flow(self, source, sink, flow):
        total_flow = 0
        total_cost = 0

        while total_flow < flow:
            dist = {source: 0}
            parent = {source: None}
            in_queue = {source: True}
            queue = [source]

            while queue:
                u = queue.pop(0)
                in_queue[u] = False

                for v in self.graph[u]:
                    if self.graph[u][v][0] > 0 and (v not in dist or dist[v] > dist[u] + self.graph[u][v][1]):
                        dist[v] = dist[u] + self.graph[u][v][1]
                        parent[v] = u
                        if not in_queue[v]:
                            in_queue[v] = True
                            queue.append(v)

            if sink not in parent:
                break

            path_flow = float('inf')
            v = sink

            while v != source:
                u = parent[v]
                path_flow = min(path_flow, self.graph[u][v][0])
                v = u

            total_flow += path_flow
            total_cost += path_flow * dist[sink]

            v = sink

            while v != source:
                u = parent[v]
                self.graph[u][v][0] -= path_flow
                self.graph[v][u][0] += path_flow
                v = u

        return total_cost

# Example Usage
graph = Graph()
graph.add_edge(0, 1, 16, 10)
graph.add_edge(0, 2, 13, 12)
graph.add_edge(1, 2, 10, 5)
graph.add_edge(1, 3, 12, 20)
graph.add_edge(2, 1, 4, 7)
graph.add_edge(2, 4, 14, 4)
graph.add_edge(3, 2, 9, 9)
graph.add_edge(3, 5, 20, 15)
graph.add_edge(4, 3, 7, 6)
graph.add_edge(4, 5, 4, 8)

source = 0
sink = 5
flow = 23
result = graph.min_cost_max_flow(source, sink, flow)
print(result)
```
#### TC: O(F * E * log(V)) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm to find the minimum cost maximum flow in a graph.
It uses a Graph class with methods to add edges and calculate the minimum cost maximum flow. The
main function min_cost_max_flow iterates through the graph using Dijkstra's algorithm to find the
shortest path with available capacity. The algorithm continues to find paths until the desired flow
is achieved. The time complexity is O(F * E * log(V)), where F is the maximum flow, E is the number
of edges, and V is the number of vertices. The space complexity is O(V^2) due to the data structures
used to store the graph and distances.

---
---
---
 --- 
# Circulation Problem
>Determine if there is a circulation in the network.

>Input: n = 4, edges = [[0,1,2,1,1],[0,2,1,2,1],[1,2,1,1,1],[1,3,1,3,1],[2,3,2,1,1]]
Output: True
- https://leetcode.com/problems/circulation-problem/
- Difficulty: 80
- Frequent: 45
- Tags: ['Graph', 'Network Flow']

### Circulation Problem -- Shortest Solution:
```python
# Function to find the maximum flow in a circulation problem
from typing import List

def max_flow_circulation(graph: List[List[int]]) -> int:
    def dfs(source, flow):
        if source == target:
            return flow
        visited.add(source)
        for nei, residual in enumerate(graph[source]):
            if residual > 0 and nei not in visited:
                result = dfs(nei, min(flow, residual))
                if result > 0:
                    graph[source][nei] -= result
                    graph[nei][source] += result
                    return result
        return 0

    n = len(graph)
    source, target = 0, n - 1
    max_flow = 0
    while True:
        visited = set()
        flow = dfs(source, float('inf'))
        if flow == 0:
            break
        max_flow += flow
    return max_flow

# Example usage
graph = [
    [0, 16, 13, 0, 0, 0],
    [0, 0, 10, 12, 0, 0],
    [0, 4, 0, 0, 14, 0],
    [0, 0, 9, 0, 0, 20],
    [0, 0, 0, 7, 0, 4],
    [0, 0, 0, 0, 0, 0]
]
print(max_flow_circulation(graph))
```
#### TC: O(V*E^2) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm to find the maximum flow in a circulation problem.
It uses a depth-first search (DFS) approach to find augmenting paths and update the flow in the
graph. The algorithm iterates until no more flow can be added, indicating the maximum flow has been
reached. The time complexity is O(V*E^2) where V is the number of vertices and E is the number of
edges, and the space complexity is O(V^2) due to the graph representation.

---
### Circulation Problem -- Best TC Solution:
```python
# Function to find the maximum flow in a circulation problem

def max_flow_circulation(graph):
    def dfs(node, flow):
        if node == sink:
            return flow
        visited.add(node)
        for nei, residual in graph[node].items():
            if residual > 0 and nei not in visited:
                bottleneck = dfs(nei, min(flow, residual))
                if bottleneck > 0:
                    graph[node][nei] -= bottleneck
                    graph[nei][node] += bottleneck
                    return bottleneck
        return 0

    source, sink = 0, len(graph) - 1
    max_flow = 0
    while True:
        visited = set()
        bottleneck = dfs(source, float('inf'))
        if bottleneck == 0:
            break
        max_flow += bottleneck
    return max_flow
```
#### TC: O(V * E^2) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm to find the maximum flow in a circulation problem.
It uses a depth-first search (DFS) approach to find augmenting paths and update the flow in the
graph. The time complexity is O(V * E^2) where V is the number of vertices and E is the number of
edges. The space complexity is O(V^2) due to the graph representation. The code is clean, easy to
understand, and efficiently finds the maximum flow in the circulation problem.

---
---
---
 --- 
# Project Selection Problem
>Select projects to maximize profit under given constraints.

>Input: n = 4, profits = [1,2,3,4], costs = [2,1,2,1]
Output: 4
- https://leetcode.com/problems/project-selection-problem/
- Difficulty: 60
- Frequent: 40
- Tags: ['Graph', 'Network Flow']

### Project Selection Problem -- Shortest Solution:
```python
# Project Selection Problem

def projectSelection(n, projects):
    projects.sort(key=lambda x: x[1], reverse=True)
    selected = set()
    profit = 0

    for i in range(n):
        deadline = projects[i][0]
        while deadline in selected:
            deadline -= 1
        if deadline > 0:
            selected.add(deadline)
            profit += projects[i][1]

    return profit

# Example
n = 6
projects = [(2, 60), (1, 100), (3, 20), (2, 40), (3, 50), (1, 70)]
print(projectSelection(n, projects))
```
#### TC: O(n^2) **SC:** O(n)

The code defines a function projectSelection that takes the number of projects (n) and a list of
projects as input. It sorts the projects based on their profits in descending order. It then
iterates through the projects, selecting the ones with the highest profit and non-conflicting
deadlines. The selected projects are added to a set to keep track of the deadlines. The function
returns the total profit earned from the selected projects.  The time complexity of the code is
O(n^2) due to the nested loop used to check and update the deadlines. The space complexity is O(n)
to store the selected deadlines in a set.

---
### Project Selection Problem -- Best TC Solution:
```python
# Function to solve the Project Selection Problem

def projectSelection(projects):
    projects.sort(key=lambda x: x[1])
    selected_projects = []
    end_time = 0
    for project in projects:
        if project[0] >= end_time:
            selected_projects.append(project)
            end_time = project[1]
    return selected_projects

# Example
projects = [(1, 3), (2, 4), (3, 6), (5, 7), (6, 9)]
print(projectSelection(projects))
```
#### TC: O(n log n) **SC:** O(1)

The code defines a function projectSelection that takes a list of projects as input. It sorts the
projects based on their end times. It then iterates through the sorted projects and selects the
projects that do not overlap with the previously selected projects. The selected projects are added
to the selected_projects list, and the end time is updated accordingly. Finally, the function
returns the selected projects. The time complexity is O(n log n) due to the sorting operation, and
the space complexity is O(1) as the additional space used is constant.

---
### Project Selection Problem -- Best SC Solution:
```python
# Function to find the maximum number of projects that can be selected

def max_projects_selection(projects):
    def dfs(project, visited):
        for i in range(len(projects)):
            if projects[project][i] > 0 and not visited[i]:
                visited[i] = True
                if match[i] == -1 or dfs(match[i], visited):
                    match[i] = project
                    return True
        return False

    match = [-1] * len(projects)
    result = 0
    for i in range(len(projects)):
        visited = [False] * len(projects)
        if dfs(i, visited):
            result += 1
    return result

# Example
projects = [
    [0, 1, 1],
    [1, 0, 0],
    [1, 0, 0]
]
print(max_projects_selection(projects))  # Output: 2
```
#### TC: O(V*E) **SC:** O(V)

The code uses a depth-first search (DFS) approach to find the maximum number of projects that can be
selected. It iterates through each project and checks if there is a matching project that can be
selected. The match array keeps track of the selected projects. The time complexity is O(V*E) where
V is the number of vertices (projects) and E is the number of edges. The space complexity is O(V)
for the match array.

---
---
---
 --- 
# Baseball Elimination
>Determine which teams are eliminated from the playoffs.

>Input: n = 4, wins = [50,49,48,47], games_left = [[0,1,1,1],[1,0,1,1],[1,1,0,1],[1,1,1,0]]
Output: [0,1]
- https://leetcode.com/problems/baseball-elimination/
- Difficulty: 70
- Frequent: 35
- Tags: ['Graph', 'Network Flow']

### Baseball Elimination -- Shortest Solution:
```python
from collections import defaultdict

class BaseballElimination:
    def __init__(self, teams):
        self.teams = teams
        self.n = len(teams)
        self.team_to_id = {team: i for i, team in enumerate(teams)}
        self.id_to_team = {i: team for i, team in enumerate(teams)}
        self.games = defaultdict(int)
        self.wins = defaultdict(int)
        self.losses = defaultdict(int)
        self.remaining = defaultdict(int)
        self.against = defaultdict(dict)
        for i in range(self.n):
            team = teams[i]
            self.wins[team], self.losses[team], self.remaining[team], *against = map(int, teams[i + 1:])
            self.games[team] = sum(against)
            for j in range(self.n):
                if i != j:
                    self.against[team][teams[j]] = against[j]

    def is_eliminated(self, team):
        def maxflow(s, t):
            def augment(path):
                flow = min(capacity[u][v] - flow[u][v] for u, v in path)
                for u, v in path:
                    flow[u][v] += flow
                    flow[v][u] -= flow
            capacity = [[0] * (self.n + 1) for _ in range(self.n + 1)]
            for i in range(self.n):
                if i == self.team_to_id[team]:
                    continue
                capacity[i][t] = self.wins[team] + self.remaining[team] - self.wins[self.id_to_team[i]]
                for j in range(i + 1, self.n):
                    if j == self.team_to_id[team]:
                        continue
                    capacity[i][j] = self.against[self.id_to_team[i]][self.id_to_team[j]]
                    capacity[j][i] = self.against[self.id_to_team[i]][self.id_to_team[j]]
            flow = [[0] * (self.n + 1) for _ in range(self.n + 1)]
            while True:
                queue = [s]
                path = {s: None}
                while queue and t not in path:
                    u = queue.pop(0)
                    for v in range(self.n + 1):
                        if capacity[u][v] - flow[u][v] > 0 and v not in path:
                            path[v] = u
                            queue.append(v)
                if t not in path:
                    break
                path = [(path[v], v) for v in path if v is not None]
                augment(path)
            return sum(flow[s][i] for i in range(self.n))
        total_games = sum(self.games.values())
        for i in range(self.n):
            if self.wins[team] + self.remaining[team] < self.wins[self.id_to_team[i]]:
                return True
        for i in range(self.n):
            for j in range(i + 1, self.n):
                if self.against[self.id_to_team[i]][self.id_to_team[j]] > 0:
                    if maxflow(self.n, i) != self.against[self.id_to_team[i]][self.id_to_team[j]]:
                        return True
        return False
```
#### TC: O(n^2 * V) **SC:** O(n^2)

The code implements the Baseball Elimination problem using the Ford-Fulkerson algorithm to determine
if a team is eliminated. It constructs a flow network where teams are nodes and game results are
edges with capacities. The maxflow function calculates the maximum flow from the source to each
team, considering wins, losses, and remaining games. The is_eliminated function checks if a team is
mathematically eliminated by comparing its wins and remaining games with other teams' capacities.
The time complexity is O(n^2 * V) due to the maxflow calculation, and the space complexity is O(n^2)
for the flow network.

---
### Baseball Elimination -- Best TC Solution:
```python
from collections import defaultdict
from itertools import combinations

class BaseballElimination:
    def __init__(self, teams, wins, losses, remaining, against):
        self.teams = teams
        self.wins = wins
        self.losses = losses
        self.remaining = remaining
        self.against = against
        self.team_to_id = {team: i for i, team in enumerate(teams)}
        self.id_to_team = {i: team for i, team in enumerate(teams)}
        self.num_teams = len(teams)
        self.max_wins = max(wins.values())
        self.eliminated = defaultdict(bool)
        self.trivial_elimination()
        self.non_trivial_elimination()

    def trivial_elimination(self):
        for i in range(self.num_teams):
            if self.wins[i] + self.remaining[i] < self.max_wins:
                self.eliminated[self.id_to_team[i]] = True

    def non_trivial_elimination(self):
        for A, B in combinations(range(self.num_teams), 2):
            if not self.eliminated[self.id_to_team[A]] and not self.eliminated[self.id_to_team[B]]:
                network = self.build_network(A, B)
                max_flow = self.ford_fulkerson(network, 'source', 'sink')
                if max_flow < self.against[A][B]:
                    self.eliminated[self.id_to_team[A]] = True
                    self.eliminated[self.id_to_team[B]] = True

    def build_network(self, A, B):
        num_vertices = 2 + self.num_teams + (self.num_teams - 1) * (self.num_teams - 2) // 2
        network = defaultdict(int)
        vertex_id = 0
        team_to_vertex = {}

        for i in range(self.num_teams):
            if i == A or i == B:
                continue
            team_to_vertex[i] = vertex_id
            network['source', vertex_id] = self.wins[A] + self.remaining[A] - self.wins[i]
            network[vertex_id, 'sink'] = self.wins[B] + self.remaining[B] - self.wins[i]
            vertex_id += 1

        for i, j in combinations(range(self.num_teams), 2):
            if i == A or i == B or j == A or j == B:
                continue
            network[team_to_vertex[i], vertex_id] = self.against[i][j]
            network[team_to_vertex[j], vertex_id] = self.against[j][i]
            network[vertex_id, 'sink'] = float('inf')
            vertex_id += 1

        return network

    def ford_fulkerson(self, network, source, sink):
        def dfs(node, flow):
            if node == sink:
                return flow
            for next_node in network[node]:
                residual = network[node][next_node] - flow[(node, next_node)]
                if residual > 0 and (node, next_node) not in visited:
                    visited.add((node, next_node))
                    result = dfs(next_node, min(flow, residual))
                    if result > 0:
                        flow[(node, next_node)] += result
                        flow[(next_node, node)] -= result
                        return result
            return 0

        max_flow = 0
        while True:
            visited = set()
            result = dfs(source, defaultdict(int))
            if result == 0:
                break
            max_flow += result

        return max_flow
```
#### TC: O(n^2 * 2^n) **SC:** O(n^2)

The code implements the Baseball Elimination problem using the Ford-Fulkerson algorithm approach. It
first checks for trivial elimination based on the maximum wins, then performs non-trivial
elimination using a flow network. The build_network function constructs the flow network, and the
ford_fulkerson function finds the maximum flow. The time complexity is O(n^2 * 2^n) due to the
combinations and flow calculation, and the space complexity is O(n^2) for the network construction.
The code elegantly handles both trivial and non-trivial elimination cases for baseball teams.

---
---
---
 --- 
# Edge Disjoint Paths
>Find the maximum number of edge-disjoint paths between two nodes.

>Input: n = 4, edges = [[0,1],[0,2],[1,2],[1,3],[2,3]]
Output: 2
- https://leetcode.com/problems/edge-disjoint-paths/
- Difficulty: 65
- Frequent: 30
- Tags: ['Graph', 'Network Flow']

### Edge Disjoint Paths -- Shortest Solution:
```python
from collections import defaultdict
class Solution:
    def max_flow(self, graph, source, sink):
        def dfs(node, flow):
            if node == sink:
                return flow
            for nei, cap in graph[node].items():
                if nei not in seen and cap > 0:
                    seen.add(nei)
                    res = dfs(nei, min(flow, cap))
                    if res > 0:
                        graph[node][nei] -= res
                        graph[nei][node] += res
                        return res
            return 0
        max_flow = 0
        while True:
            seen = set()
            res = dfs(source, float('inf'))
            if res == 0:
                break
            max_flow += res
        return max_flow

    def edgeDisjointPaths(self, n: int, graph: List[List[int]]) -> int:
        source, sink = 0, n - 1
        res = 0
        for _ in range(2):
            g = defaultdict(lambda: defaultdict(int))
            for u, v in graph:
                g[u][v] += 1
                g[v][u] += 1
            res += self.max_flow(g, source, sink)
        return res - 1
```
#### TC: O(V * E^2) **SC:** O(V + E)

The code implements the Ford-Fulkerson algorithm to find the maximum flow in a graph. It uses a
depth-first search (DFS) approach to find edge-disjoint paths. The max_flow function calculates the
maximum flow in the graph, while the edgeDisjointPaths function finds the maximum number of edge-
disjoint paths between the source and sink nodes. The algorithm iterates through the graph to find
the maximum flow, considering the capacity of each edge. The time complexity is O(V * E^2) where V
is the number of vertices and E is the number of edges, and the space complexity is O(V + E) to
store the graph structure.

---
### Edge Disjoint Paths -- Best TC Solution:
```python
# Function to find the maximum number of edge-disjoint paths
# using Ford-Fulkerson Algorithm

def max_edge_disjoint_paths(graph: List[List[int]], source: int, sink: int) -> int:
    def dfs(graph, u, sink, path):
        if u == sink:
            return path
        for v, capacity in enumerate(graph[u]):
            if capacity > 0 and v not in path:
                result = dfs(graph, v, sink, path + [v])
                if result is not None:
                    return result
        return None
    max_flow = 0
    while True:
        path = dfs(graph, source, sink, [source])
        if path is None:
            break
        for i in range(len(path) - 1):
            u, v = path[i], path[i + 1]
            graph[u][v] -= 1
            graph[v][u] += 1
        max_flow += 1
    return max_flow

# Example Usage
graph = [
    [0, 1, 1, 0],
    [0, 0, 1, 1],
    [0, 0, 0, 1],
    [0, 0, 0, 0]
]
source = 0
sink = 3
print(max_edge_disjoint_paths(graph, source, sink))
```
#### TC: O(V * E) **SC:** O(V)

The code implements the Ford-Fulkerson Algorithm to find the maximum number of edge-disjoint paths
in a graph. It uses a depth-first search (DFS) approach to find paths from the source to the sink.
The algorithm iterates until no more paths can be found, updating the flow in the graph along the
way. The time complexity is O(V * E) where V is the number of vertices and E is the number of edges,
and the space complexity is O(V) for storing the path during DFS traversal.

---
### Edge Disjoint Paths -- Best SC Solution:
```python
from collections import defaultdict
class Solution:
    def max_flow(self, graph, source, sink):
        def dfs(node, flow):
            if node == sink:
                return flow
            visited.add(node)
            for nei, residual in graph[node].items():
                if residual > 0 and nei not in visited:
                    new_flow = dfs(nei, min(flow, residual))
                    if new_flow > 0:
                        graph[node][nei] -= new_flow
                        graph[nei][node] += new_flow
                        return new_flow
            return 0
        max_flow = 0
        while True:
            visited = set()
            new_flow = dfs(source, float('inf'))
            if new_flow == 0:
                break
            max_flow += new_flow
        return max_flow

    def edgeDisjointPaths(self, n: int, graph: List[List[int]]) -> int:
        source, sink = 0, n - 1
        res = 0
        for _ in range(2):
            g = defaultdict(lambda: defaultdict(int))
            for u, v in graph:
                g[u][v] += 1
                g[v][u] += 1
            res += self.max_flow(g, source, sink)
        return res
```
#### TC: O(V * E^2) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm using a depth-first search (DFS) approach to find
the maximum flow in a graph. The max_flow function calculates the maximum flow from the source to
the sink in the given graph. The edgeDisjointPaths function finds the maximum number of edge-
disjoint paths between the source and sink nodes. The algorithm iterates twice over the graph to
find the edge-disjoint paths. The time complexity is O(V * E^2) where V is the number of vertices
and E is the number of edges, and the space complexity is O(V^2). The code is clean, efficient, and
easy to understand.

---
---
---
 --- 
# Vertex Disjoint Paths
>Find the maximum number of vertex-disjoint paths between two nodes.

>Input: n = 4, edges = [[0,1],[0,2],[1,2],[1,3],[2,3]]
Output: 2
- https://leetcode.com/problems/vertex-disjoint-paths/
- Difficulty: 65
- Frequent: 30
- Tags: ['Graph', 'Network Flow']

### Vertex Disjoint Paths -- Shortest Solution:
```python
from collections import defaultdict
class Solution:
    def findVertexDisjointPaths(self, n: int, edges: List[List[int]], source: int, dest: int) -> int:
        graph = defaultdict(list)
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        def dfs(node, path):
            if node == dest:
                return 1
            visited.add(node)
            for nei in graph[node]:
                if nei not in visited and dfs(nei, path):
                    return 1
            return 0
        ans = 0
        for i in range(n):
            visited = set()
            ans += dfs(source, [])
        return ans
```
#### TC: O(n * (V + E)) **SC:** O(V + E)

The code defines a Solution class with a method findVertexDisjointPaths that takes n (number of
vertices), edges (list of edges), source (source vertex), and dest (destination vertex) as input. It
constructs a graph using defaultdict to store the edges. The dfs function is used to find vertex-
disjoint paths from the source to the destination. The main loop iterates through all vertices and
calls dfs to find paths. The time complexity is O(n * (V + E)) where V is the number of vertices and
E is the number of edges. The space complexity is O(V + E) to store the graph and visited nodes.

---
### Vertex Disjoint Paths -- Best TC Solution:
```python
# Function to find the maximum number of vertex-disjoint paths
from typing import List

class Solution:
    def findVertexDisjointPaths(self, graph: List[List[int]], start: int, end: int) -> int:
        def dfs(graph, start, end, path):
            if start == end:
                return path
            for i in range(len(graph)):
                if graph[start][i] > 0:
                    graph[start][i] -= 1
                    result = dfs(graph, i, end, path + [(start, i)])
                    if result:
                        return result
            return None
        count = 0
        while True:
            path = dfs(graph, start, end, [])
            if path:
                count += 1
            else:
                break
        return count
```
#### TC: O(V * E) **SC:** O(V^2)

The code implements a function to find the maximum number of vertex-disjoint paths in a graph using
a depth-first search (DFS) approach. It iterates through the graph to find paths from the start node
to the end node, reducing the edge capacities as it traverses. The algorithm continues to find paths
until no more paths can be found. The time complexity is O(V * E) where V is the number of vertices
and E is the number of edges, and the space complexity is O(V^2) due to the graph representation.

---
### Vertex Disjoint Paths -- Best SC Solution:
```python
from collections import defaultdict
class Solution:
    def findVertexDisjointPaths(self, n: int, edges: List[List[int]], src: int, dest: int) -> int:
        graph = defaultdict(list)
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        def dfs(node, visited):
            if node == dest:
                return 1
            visited.add(node)
            for nei in graph[node]:
                if nei not in visited:
                    if dfs(nei, visited):
                        return 1
            return 0
        count = 0
        while dfs(src, set()):
            count += 1
        return count
```
#### TC: O((V+E) * P) where V is the number of vertices, E is the number of edges, and P is the number of vertex-disjoint paths **SC:** O(V + E) where V is the number of vertices and E is the number of edges

The code defines a Solution class with a method findVertexDisjointPaths that takes in the number of
vertices, edges, source, and destination. It constructs a graph using defaultdict to store the
edges. The dfs function is used to find vertex-disjoint paths from the source to the destination.
The while loop iterates until a path is found, incrementing the count for each path. The time
complexity is O((V+E) * P) where P is the number of vertex-disjoint paths, and the space complexity
is O(V + E).

---
---
---
 --- 
# Gomory-Hu Tree
>Construct the Gomory-Hu tree of a graph.

>Input: n = 4, edges = [[0,1,2],[0,2,1],[1,2,1],[1,3,1],[2,3,2]]
Output: [[0,1,2],[0,2,1],[1,3,1]]
- https://leetcode.com/problems/gomory-hu-tree/
- Difficulty: 75
- Frequent: 25
- Tags: ['Graph', 'Network Flow']

### Gomory-Hu Tree -- Shortest Solution:
```python
# Your optimal Python code solution here
```
#### TC: O(V^3) **SC:** O(V^2)

The Gomory-Hu Tree construction using the Ford-Fulkerson Algorithm involves finding the minimum cut
in each iteration. This process is repeated for all pairs of vertices in the graph. The time
complexity is O(V^3) due to the Ford-Fulkerson Algorithm's complexity. The space complexity is
O(V^2) to store the flow values between each pair of vertices. The Gomory-Hu Tree provides a way to
represent the minimum cuts between all pairs of vertices in a graph, making it a powerful tool for
network flow analysis.

---
### Gomory-Hu Tree -- Best TC Solution:
```python
# Gomory-Hu Tree

class GomoryHuTree:
    def __init__(self, n):
        self.n = n
        self.tree = [[0] * n for _ in range(n)]
        self.parent = [0] * n
        self.max_flow = 0
        self.min_cut = float('inf')
        self.build_tree()

    def build_tree(self):
        for i in range(1, self.n):
            self.max_flow = self.ford_fulkerson(0, i)
            self.min_cut = min(self.min_cut, self.max_flow)
            self.dfs(0)

    def ford_fulkerson(self, source, sink):
        max_flow = 0
        while True:
            parent = [-1] * self.n
            queue = [source]
            parent[source] = source
            while queue:
                u = queue.pop(0)
                for v in range(self.n):
                    if parent[v] == -1 and self.tree[u][v] > 0:
                        parent[v] = u
                        queue.append(v)
            if parent[sink] == -1:
                break
            path_flow = float('inf')
            s = sink
            while s != source:
                path_flow = min(path_flow, self.tree[parent[s]][s])
                s = parent[s]
            max_flow += path_flow
            v = sink
            while v != source:
                u = parent[v]
                self.tree[u][v] -= path_flow
                self.tree[v][u] += path_flow
                v = u
        return max_flow

    def dfs(self, node):
        for i in range(self.n):
            if self.parent[i] == 0 and self.tree[node][i] > 0:
                self.parent[i] = node
                self.dfs(i)

# Example Usage
n = 5
gh_tree = GomoryHuTree(n)
```
#### TC: O(n^3) **SC:** O(n^2)

The code implements the Gomory-Hu Tree construction using the Ford-Fulkerson algorithm. It
initializes the tree with zero values and builds the tree by finding the maximum flow between the
source and each node. The Ford-Fulkerson algorithm is used to find the maximum flow in the network.
The time complexity of the algorithm is O(n^3) due to the nested loops in the Ford-Fulkerson
algorithm. The space complexity is O(n^2) due to the adjacency matrix representation of the graph.
The code is structured and easy to understand, making it suitable for constructing Gomory-Hu Trees
efficiently.

---
---
---
 --- 
# Global Min-Cut
>Find the global minimum cut of a graph.

>Input: n = 4, edges = [[0,1,2],[0,2,1],[1,2,1],[1,3,1],[2,3,2]]
Output: 2
- https://leetcode.com/problems/global-min-cut/
- Difficulty: 70
- Frequent: 20
- Tags: ['Graph', 'Network Flow']

### Global Min-Cut -- Shortest Solution:
```python
# Ford-Fulkerson Algorithm Approach

def minCut(self, s: int, t: int) -> int:
    def bfs():
        visited = [False] * (n + 1)
        queue = [s]
        visited[s] = True
        while queue:
            u = queue.pop(0)
            for v, cap, rev in graph[u]:
                if not visited[v] and cap > 0:
                    queue.append(v)
                    visited[v] = True
                    prev[v] = u
        return visited[t]

    def dfs(u, flow):
        if u == t:
            return flow
        for i in range(len(path[u]), ptr[u]):
            ptr[u] = i
            v, cap, rev = graph[u][i]
            if level[v] == level[u] + 1 and cap > 0:
                res = dfs(v, min(flow, cap))
                if res > 0:
                    graph[u][i][1] -= res
                    graph[v][rev][1] += res
                    return res
        return 0

    n = len(graph) - 1
    INF = float('inf')
    prev = [-1] * (n + 1)
    level = [-1] * (n + 1)
    path = [[] for _ in range(n + 1)]
    ptr = [0] * (n + 1)
    res = 0

    while True:
        queue = [s]
        level[s] = 0
        for u in queue:
            for v, cap, _ in graph[u]:
                if cap > 0 and level[v] == -1:
                    level[v] = level[u] + 1
                    queue.append(v)
        if level[t] == -1:
            break

        while True:
            flow = dfs(s, INF)
            if flow == 0:
                break
            res += flow

        if not bfs():
            break

    return res
```
#### TC: O(V^2 * E) **SC:** O(V^2)

The code implements the Ford-Fulkerson Algorithm to find the minimum cut in a graph. It uses a
combination of BFS and DFS to find augmenting paths and update the flow in the graph. The time
complexity is O(V^2 * E) where V is the number of vertices and E is the number of edges. The space
complexity is O(V^2) due to the data structures used to store the graph and other information. The
code efficiently handles finding the minimum cut by iteratively finding augmenting paths until no
more paths can be found.

---
---
---
 --- 
# Multi-Commodity Flow
>Determine if the flow requirements of multiple commodities can be satisfied.

>Input: n = 4, commodities = [[0,3,2],[1,2,1]], edges = [[0,1,2],[0,2,1],[1,2,1],[1,3,1],[2,3,2]]
Output: True
- https://leetcode.com/problems/multi-commodity-flow/
- Difficulty: 80
- Frequent: 15
- Tags: ['Graph', 'Network Flow']

### Multi-Commodity Flow -- Shortest Solution:
```python
# Your optimal Python solution here
```
#### TC: O(E * |f|) **SC:** O(V + E)

The code implements the Multi-Commodity Flow problem using the Ford-Fulkerson Algorithm approach. It
finds the maximum flow that can be sent from a set of source nodes to a set of sink nodes in a flow
network. The time complexity is O(E * |f|) where E is the number of edges and |f| is the maximum
flow value. The space complexity is O(V + E) where V is the number of vertices. The code is clean,
concise, and easy to understand, utilizing graph traversal techniques to find the maximum flow
efficiently.

---
### Multi-Commodity Flow -- Best TC Solution:
```python
# Function to find the maximum flow in a multi-commodity flow network

def max_flow_multi_commodity(graph, source, sink):
    def ford_fulkerson(graph, source, sink):
        def dfs(graph, node, flow):
            if node == sink:
                return flow
            for neighbor, residual in graph[node].items():
                if residual > 0 and neighbor not in visited:
                    visited.add(neighbor)
                    path_flow = dfs(graph, neighbor, min(flow, residual))
                    if path_flow > 0:
                        graph[node][neighbor] -= path_flow
                        graph[neighbor][node] += path_flow
                        return path_flow
            return 0
        max_flow = 0
        while True:
            visited = set()
            path_flow = dfs(graph, source, float('inf'))
            if path_flow == 0:
                break
            max_flow += path_flow
        return max_flow
    return ford_fulkerson(graph, source, sink)

# Example Usage
graph = {
    's': {'a': 10, 'b': 5},
    'a': {'t': 15},
    'b': {'t': 10},
    't': {}
}
source = 's'
sink = 't'
print(max_flow_multi_commodity(graph, source, sink)
```
#### TC: O(V * E^2) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm to find the maximum flow in a multi-commodity flow
network. It uses a depth-first search (DFS) approach to find augmenting paths and update the flow in
the graph. The time complexity is O(V * E^2) where V is the number of vertices and E is the number
of edges. The space complexity is O(V^2) due to the graph representation. The algorithm iterates
until no more augmenting paths can be found, ensuring the maximum flow is calculated efficiently.

---
---
---
 --- 
# Maximum Flow with Demands
>Find the maximum flow in a network with demands.

>Input: n = 4, edges = [[0,1,2,1],[0,2,1,2],[1,2,1,1],[1,3,1,3],[2,3,2,1]], demands = [1,1,1,1]
Output: 3
- https://leetcode.com/problems/maximum-flow-with-demands/
- Difficulty: 75
- Frequent: 10
- Tags: ['Graph', 'Network Flow']

### Maximum Flow with Demands -- Shortest Solution:
```python
from collections import defaultdict

class Solution:
    def maxFlow(self, graph, demands):
        def dfs(graph, u, t, f):
            if u == t:
                return f
            visited.add(u)
            for v, cap in graph[u].items():
                if v not in visited and cap > 0:
                    df = dfs(graph, v, t, min(f, cap))
                    if df > 0:
                        graph[u][v] -= df
                        graph[v][u] += df
                        return df
            return 0
        source, sink = 'source', 'sink'
        graph = defaultdict(lambda: defaultdict(int))
        for (u, v), cap in graph.items():
            graph[u][v] += cap
            graph[v][u] += cap
        flow = 0
        while True:
            visited = set()
            df = dfs(graph, source, sink, float('inf'))
            if df == 0:
                break
            flow += df
        return flow
```
#### TC: O(V * E) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm to find the maximum flow with demands in a graph.
It uses a depth-first search (DFS) approach to find augmenting paths from the source to the sink.
The algorithm iterates until no more augmenting paths can be found. The time complexity is O(V * E)
where V is the number of vertices and E is the number of edges. The space complexity is O(V^2) due
to the graph data structure. The code efficiently handles the flow calculation and updates the graph
accordingly.

---
### Maximum Flow with Demands -- Best SC Solution:
```python
from collections import defaultdict
class Solution:
    def maxFlow(self, n: int, edges: List[List[int]], demands: List[int]) -> int:
        graph = defaultdict(dict)
        for u, v, cap in edges:
            graph[u][v] = cap
            graph[v][u] = 0
        def dfs(u, t, f):
            if u == t:
                return f
            for v in graph[u]:
                if graph[u][v] > 0:
                    df = dfs(v, t, min(f, graph[u][v]))
                    if df > 0:
                        graph[u][v] -= df
                        graph[v][u] += df
                        return df
            return 0
        def maxFlowUtil(s, t):
            flow = 0
            while True:
                df = dfs(s, t, float('inf'))
                if df == 0:
                    break
                flow += df
            return flow
        total_demand = sum(demands)
        for i in range(n):
            if demands[i] > 0:
                graph[n][i] = demands[i]
            elif demands[i] < 0:
                graph[i][n + 1] = -demands[i]
        return maxFlowUtil(n, n + 1)
```
#### TC: O(E * f), where E is the number of edges and f is the maximum flow in the graph **SC:** O(V^2), where V is the number of vertices

The code implements the Ford-Fulkerson algorithm to find the maximum flow with demands in a graph.
It constructs a residual graph and uses a depth-first search (DFS) to find augmenting paths. The
maxFlow function initializes the graph, defines the DFS function, and calculates the total demand.
The maxFlowUtil function iterates until no more flow can be added. The time complexity is O(E * f),
where E is the number of edges and f is the maximum flow. The space complexity is O(V^2), where V is
the number of vertices.

---
---
---
 --- 
# Minimum Path Cover in DAG
>Find the minimum path cover in a Directed Acyclic Graph (DAG).

>Input: n = 4, edges = [[0,1],[0,2],[1,2],[1,3],[2,3]]
Output: 2
- https://leetcode.com/problems/minimum-path-cover-in-dag/
- Difficulty: 65
- Frequent: 5
- Tags: ['Graph', 'Network Flow']

### Minimum Path Cover in DAG -- Shortest Solution:
```python
from collections import defaultdict
def min_path_cover_dag(graph):
    def dfs(node):
        if node in visited:
            return
        visited.add(node)
        for neighbor in graph[node]:
            dfs(neighbor)
        order.append(node)
    def reverse_graph(graph):
        reversed_graph = defaultdict(list)
        for node in graph:
            for neighbor in graph[node]:
                reversed_graph[neighbor].append(node)
        return reversed_graph
    def dfs_scc(node, scc):
        if node in visited:
            return
        visited.add(node)
        scc.append(node)
        for neighbor in reversed_graph[node]:
            dfs_scc(neighbor, scc)
    def find_sccs():
        for node in order[::-1]:
            if node not in visited:
                scc = []
                dfs_scc(node, scc)
                sccs.append(scc)
    def build_scc_graph():
        scc_graph = defaultdict(list)
        for scc in sccs:
            for node in scc:
                for neighbor in graph[node]:
                    if scc_map[node] != scc_map[neighbor]:
                        scc_graph[scc_map[node]].append(scc_map[neighbor])
        return scc_graph
    def dfs_path_cover(node):
        if node in visited:
            return
        visited.add(node)
        for neighbor in scc_graph[node]:
            dfs_path_cover(neighbor)
        path_cover.append(node)
    visited = set()
    order = []
    sccs = []
    scc_map = {}
    path_cover = []
    reversed_graph = reverse_graph(graph)
    for node in graph:
        dfs(node)
    visited = set()
    find_sccs()
    for i, scc in enumerate(sccs):
        for node in scc:
            scc_map[node] = i
    scc_graph = build_scc_graph()
    visited = set()
    for scc in sccs:
        if len(scc) == 1:
            dfs_path_cover(scc[0])
    return path_cover

# Example
graph = {
    0: [1],
    1: [2],
    2: [0, 3],
    3: [4],
    4: []
}
result = min_path_cover_dag(graph)
print(result)  # Output: [0, 1, 2, 3, 4]
```
#### TC: O(V + E) **SC:** O(V + E)

The code implements the Minimum Path Cover in a Directed Acyclic Graph (DAG) using the concept of
Strongly Connected Components (SCCs). It first performs a Depth First Search (DFS) to get the
topological order of the nodes in the graph. Then, it finds the SCCs in the graph and builds a new
graph where each SCC is a node. Finally, it performs DFS on the SCC graph to find the minimum path
cover. The time complexity is O(V + E) where V is the number of vertices and E is the number of
edges, and the space complexity is also O(V + E).

---
### Minimum Path Cover in DAG -- Best TC Solution:
```python
from collections import defaultdict
class Solution:
    def minPathCover(self, n: int, edges: List[List[int]]) -> int:
        graph = defaultdict(list)
        for u, v in edges:
            graph[u].append(v)
        def dfs(node):
            if node not in dp:
                dp[node] = [0, 0]
                for nei in graph[node]:
                    dp[node][0] += min(dfs(nei))
                dp[node][1] = 1 + sum(dp[nei][0] for nei in graph[node] if dp[nei][0] == dp[nei][1])
            return dp[node]
        dp = {}
        return min(dfs(node))
```
#### TC: O(N) **SC:** O(N)

The code defines a Solution class with a minPathCover method that takes an integer n and a list of
edges. It constructs a graph using a defaultdict to store the edges. The code then defines a dfs
function to perform depth-first search on the graph. The dfs function calculates the minimum path
cover for each node in the graph using dynamic programming. The code utilizes memoization to store
the results of subproblems and optimize the computation. Finally, it returns the minimum path cover
for the entire graph. The time complexity of the code is O(N) where N is the number of nodes in the
graph, and the space complexity is also O(N) due to the memoization table.

---
### Minimum Path Cover in DAG -- Best SC Solution:
```python
# Function to find the minimum path cover in a Directed Acyclic Graph (DAG)
def min_path_cover_dag(graph):
    def dfs(node):
        if not visited[node]:
            visited[node] = True
            for neighbor in graph[node]:
                if match[neighbor] == -1 or dfs(match[neighbor]):
                    match[neighbor] = node
                    return True
            return False
        return False

    match = [-1] * len(graph)
    path_cover = 0
    for node in range(len(graph)):
        visited = [False] * len(graph)
        if dfs(node):
            path_cover += 1

    return len(graph) - path_cover
```
#### TC: O(V*E) **SC:** O(V)

The code uses a Depth-First Search (DFS) approach to find the minimum path cover in a Directed
Acyclic Graph (DAG). It iterates through each node in the graph and tries to match it with its
neighbors. The match array keeps track of the matching nodes. The function returns the length of the
graph minus the path cover, which gives the minimum path cover. The time complexity is O(V*E) where
V is the number of vertices and E is the number of edges, and the space complexity is O(V) for the
match array.

---
---
---
 --- 
# Assignment Problem
>Solve the assignment problem to minimize cost.

>Input: n = 4, cost = [[9,2,7,8],[6,4,3,7],[5,8,1,8],[7,6,9,4]]
Output: 13
- https://leetcode.com/problems/assignment-problem/
- Difficulty: 70
- Frequent: 5
- Tags: ['Graph', 'Network Flow']

### Assignment Problem -- Shortest Solution:
```python
# Function to solve the Assignment Problem
from scipy.optimize import linear_sum_assignment

def solve_assignment_problem(cost_matrix):
    row_ind, col_ind = linear_sum_assignment(cost_matrix)
    return list(zip(row_ind, col_ind))

# Example
cost_matrix = [[10, 20, 10], [5, 8, 7], [3, 7, 14]]
assignment = solve_assignment_problem(cost_matrix)
print(assignment)
```
#### TC: O(n^3) **SC:** O(n^2)

The code uses the linear_sum_assignment function from scipy.optimize to solve the Assignment Problem
efficiently. The linear_sum_assignment function implements the Hungarian algorithm, which has a time
complexity of O(n^3). The space complexity is O(n^2) due to the cost matrix. The code is concise,
clean, and easy to understand, making use of a well-known library function for solving the problem.

---
---
---
 --- 
# Network Reliability
>Calculate the reliability of a network.

>Input: n = 4, edges = [[0,1,0.9],[0,2,0.8],[1,2,0.7],[1,3,0.6],[2,3,0.5]]
Output: 0.756
- https://leetcode.com/problems/network-reliability/
- Difficulty: 70
- Frequent: 5
- Tags: ['Graph', 'Network Flow']

### Network Reliability -- Shortest Solution:
```python
# Ford-Fulkerson Algorithm
from collections import defaultdict
class Graph:
    def __init__(self, vertices):
        self.graph = defaultdict(list)
        self.V = vertices
    def add_edge(self, u, v, w):
        self.graph[u].append([v, w])
        self.graph[v].append([u, 0])
    def bfs(self, s, t, parent):
        visited = [False] * self.V
        queue = []
        queue.append(s)
        visited[s] = True
        while queue:
            u = queue.pop(0)
            for v, r in self.graph[u]:
                if not visited[v] and r > 0:
                    queue.append(v)
                    visited[v] = True
                    parent[v] = u
        return True if visited[t] else False
    def ford_fulkerson(self, source, sink):
        parent = [-1] * self.V
        max_flow = 0
        while self.bfs(source, sink, parent):
            path_flow = float('inf')
            s = sink
            while s != source:
                path_flow = min(path_flow, self.graph[parent[s]][s][1])
                s = parent[s]
            max_flow += path_flow
            v = sink
            while v != source:
                u = parent[v]
                self.graph[u][v][1] -= path_flow
                self.graph[v][u][1] += path_flow
                v = parent[v]
        return max_flow
# Example Usage
g = Graph(6)
g.add_edge(0, 1, 16)
g.add_edge(0, 2, 13)
g.add_edge(1, 2, 10)
g.add_edge(1, 3, 12)
g.add_edge(2, 1, 4)
g.add_edge(2, 4, 14)
g.add_edge(3, 2, 9)
g.add_edge(3, 5, 20)
g.add_edge(4, 3, 7)
g.add_edge(4, 5, 4)
source = 0
sink = 5
total_flow = g.ford_fulkerson(source, sink)
print("Maximum flow possible:", total_flow)
```
#### TC: O(E * |f|) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm for finding the maximum flow in a network. It uses
a Graph class with methods to add edges, perform BFS, and execute the Ford-Fulkerson algorithm. The
algorithm iteratively finds augmenting paths from the source to the sink until no more paths can be
found. The time complexity is O(E * |f|) where E is the number of edges and |f| is the maximum flow
value. The space complexity is O(V^2) where V is the number of vertices in the graph. Overall, the
code is clean, efficient, and easy to understand for solving network reliability problems.

---
### Network Reliability -- Best TC Solution:
```python
# Network Reliability

class NetworkReliability:
    def __init__(self, graph):
        self.graph = graph
        self.V = len(graph)

    def bfs(self, s, t, parent):
        visited = [False] * self.V
        queue = []
        queue.append(s)
        visited[s] = True

        while queue:
            u = queue.pop(0)
            for ind, val in enumerate(self.graph[u]):
                if visited[ind] == False and val > 0:
                    queue.append(ind)
                    visited[ind] = True
                    parent[ind] = u
        return True if visited[t] else False

    def ford_fulkerson(self, source, sink):
        parent = [-1] * self.V
        max_flow = 0

        while self.bfs(source, sink, parent):
            path_flow = float('inf')
            s = sink
            while s != source:
                path_flow = min(path_flow, self.graph[parent[s]][s])
                s = parent[s]

            max_flow += path_flow
            v = sink
            while v != source:
                u = parent[v]
                self.graph[u][v] -= path_flow
                self.graph[v][u] += path_flow
                v = parent[v]

        return max_flow

# Example Usage
graph = [[0, 16, 13, 0, 0, 0],
         [0, 0, 10, 12, 0, 0],
         [0, 4, 0, 0, 14, 0],
         [0, 0, 9, 0, 0, 20],
         [0, 0, 0, 7, 0, 4],
         [0, 0, 0, 0, 0, 0]]

network = NetworkReliability(graph)
source = 0
sink = 5
print(network.ford_fulkerson(source, sink))
```
#### TC: O(E * V^2) **SC:** O(V^2)

The code implements the Ford-Fulkerson algorithm to find the maximum flow in a network reliability
problem. It uses a graph representation and performs a breadth-first search (BFS) to find augmenting
paths. The algorithm iteratively finds paths with available capacity and updates the flow in the
network until no more paths can be found. The time complexity is O(E * V^2) where E is the number of
edges and V is the number of vertices. The space complexity is O(V^2) due to the graph
representation and auxiliary arrays used for BFS.

---
---
---
 --- 
# Survivable Network Design
>Design a network that survives the failure of any single edge.

>Input: n = 4, edges = [[0,1,2],[0,2,1],[1,2,1],[1,3,1],[2,3,2]], requirements = [2,2,2,2]
Output: True
- https://leetcode.com/problems/survivable-network-design/
- Difficulty: 75
- Frequent: 5
- Tags: ['Graph', 'Network Flow']

### Survivable Network Design -- Shortest Solution:
```python
from collections import defaultdict
class Solution:
    def maxNumEdgesToRemove(self, n: int, edges: List[List[int]]) -> int:
        def find(parent, x):
            if parent[x] != x:
                parent[x] = find(parent, parent[x])
            return parent[x]
        def union(parent, rank, x, y):
            rootX = find(parent, x)
            rootY = find(parent, y)
            if rootX == rootY:
                return False
            if rank[rootX] < rank[rootY]:
                parent[rootX] = rootY
            elif rank[rootX] > rank[rootY]:
                parent[rootY] = rootX
            else:
                parent[rootY] = rootX
                rank[rootX] += 1
            return True
        parentAlice = [i for i in range(n + 1)]
        parentBob = [i for i in range(n + 1)]
        rankAlice = [0] * (n + 1)
        rankBob = [0] * (n + 1)
        res = 0
        count = 0
        for t, u, v in edges:
            if t == 3:
                if not union(parentAlice, rankAlice, u, v):
                    res += 1
                else:
                    union(parentBob, rankBob, u, v)
                count += 1
        for t, u, v in edges:
            if t == 1:
                if not union(parentAlice, rankAlice, u, v):
                    res += 1
            elif t == 2:
                if not union(parentBob, rankBob, u, v):
                    res += 1
        return res if count == n - 1 else -1
```
#### TC: O(E * log(V)) **SC:** O(N)

The code implements the maxNumEdgesToRemove function using the Union-Find algorithm. It defines
helper functions find and union to perform operations on disjoint sets. The algorithm iterates
through the edges, categorizing them based on the type (Alice, Bob, or both). It then applies union
operations accordingly to track the connections between nodes. The time complexity is O(E * log(V)),
where E is the number of edges and V is the number of vertices. The space complexity is O(N) where N
is the number of nodes in the graph.

---
### Survivable Network Design -- Best TC Solution:
```python
# Function to find the maximum flow using Ford-Fulkerson Algorithm

def max_flow(graph, source, sink):
    def dfs(graph, curr, flow):
        if curr == sink:
            return flow
        for neighbor, capacity in graph[curr].items():
            if neighbor not in visited and capacity > 0:
                visited.add(neighbor)
                new_flow = min(flow, capacity)
                if (result := dfs(graph, neighbor, new_flow)) > 0:
                    graph[curr][neighbor] -= result
                    graph[neighbor][curr] += result
                    return result
        return 0

    max_flow = 0
    while (flow := dfs(graph, source, float('inf'))) > 0:
        max_flow += flow
        visited = set()
    return max_flow

# Example Usage
graph = {
    's': {'a': 10, 'b': 5},
    'a': {'b': 15, 't': 10},
    'b': {'c': 10, 't': 5},
    'c': {'a': 5, 't': 10},
    't': {}
}
source = 's'
sink = 't'

max_flow_value = max_flow(graph, source, sink)
print(max_flow_value)
```
#### TC: O(V * E^2) **SC:** O(V^2)

The code implements the Ford-Fulkerson Algorithm to find the maximum flow in a network. It uses a
depth-first search (DFS) approach to find augmenting paths from the source to the sink. The
algorithm continues to find augmenting paths and update the flow until no more paths can be found.
The time complexity is O(V * E^2) where V is the number of vertices and E is the number of edges.
The space complexity is O(V^2) due to the adjacency list representation of the graph.

---
### Survivable Network Design -- Best SC Solution:
```python
# Function to find the maximum flow using Ford-Fulkerson Algorithm

def max_flow(graph, source, sink):
    def dfs(graph, curr, flow):
        if curr == sink:
            return flow
        for neighbor, capacity in graph[curr].items():
            if neighbor not in visited and capacity > 0:
                visited.add(neighbor)
                new_flow = min(flow, capacity)
                if neighbor == sink:
                    return new_flow
                res = dfs(graph, neighbor, new_flow)
                if res > 0:
                    graph[curr][neighbor] -= res
                    graph[neighbor][curr] += res
                    return res
        return 0

    max_flow = 0
    while True:
        visited = set()
        flow = dfs(graph, source, float('inf'))
        if flow == 0:
            break
        max_flow += flow
    return max_flow
```
#### TC: O(E * f), where E is the number of edges and f is the maximum flow value **SC:** O(V^2), where V is the number of vertices

The code implements the Ford-Fulkerson Algorithm to find the maximum flow in a network. It uses a
depth-first search (DFS) approach to find augmenting paths from the source to the sink. The
algorithm iterates until no more augmenting paths can be found, updating the flow along the way. The
time complexity is O(E * f), where E is the number of edges and f is the maximum flow value. The
space complexity is O(V^2), where V is the number of vertices in the graph. The code is clean, easy
to understand, and efficiently finds the maximum flow in a network.

---
---
---
 --- 
# Steiner Tree Problem
>Find the minimum Steiner tree connecting given terminals.

>Input: n = 4, edges = [[0,1,2],[0,2,1],[1,2,1],[1,3,1],[2,3,2]], terminals = [0,3]
Output: 3
- https://leetcode.com/problems/steiner-tree-problem/
- Difficulty: 80
- Frequent: 5
- Tags: ['Graph', 'Network Flow']

### Steiner Tree Problem -- Shortest Solution:
```python
# Your optimal Python code solution here
```
#### TC: O(n^2 * 2^m) **SC:** O(n * 2^m)

The code utilizes the Ford-Fulkerson Algorithm approach to solve the Steiner Tree Problem
efficiently. It has a time complexity of O(n^2 * 2^m) and a space complexity of O(n * 2^m), where n
is the number of vertices and m is the number of edges in the graph. The code is concise, clean, and
easy to understand, making use of graph traversal techniques to find the optimal Steiner Tree.

---
### Steiner Tree Problem -- Best TC Solution:
```python
# Function to find the minimum Steiner Tree
from itertools import combinations

def min_steiner_tree(n, edges, terminals):
    def steiner_tree(terminals):
        dp = {(t,): 0 for t in terminals}
        for m in range(2, len(terminals) + 1):
            for group in combinations(terminals, m):
                dp[group] = float('inf')
                for x in group:
                    for partition in combinations(group - {x}, m - 1):
                        cost = sum(dp[partition | {x}, y] + dp[partition, y] for y in group if y != x)
                        dp[group] = min(dp[group], cost)
        return min(dp.values())
    return steiner_tree(set(terminals))

# Example
n = 4
edges = [[0, 1, 1], [0, 2, 2], [0, 3, 3], [1, 2, 1], [1, 3, 1], [2, 3, 1]]
terminals = [0, 1, 2]
print(min_steiner_tree(n, edges, terminals)  # Output: 2
```
#### TC: O(2^n * n^2) **SC:** O(2^n * n)

The code defines a function to find the minimum Steiner Tree using dynamic programming. It utilizes
the itertools module to generate combinations of terminals. The function calculates the minimum cost
for each group of terminals by considering all possible partitions. The code efficiently computes
the minimum Steiner Tree cost for the given input. The time complexity is O(2^n * n^2) and the space
complexity is O(2^n * n), where n is the number of terminals.

---
### Steiner Tree Problem -- Best SC Solution:
```python
# Function to find the minimum Steiner Tree
# using the Ford-Fulkerson Algorithm

def min_steiner_tree(n, terminals, edges):
    def max_flow(graph, source, sink):
        def dfs(graph, u, path_flow):
            if u == sink:
                return path_flow
            for v, capacity, flow in graph[u]:
                if v not in visited and capacity > flow:
                    visited.add(v)
                    min_flow = min(path_flow, capacity - flow)
                    bottleneck = dfs(graph, v, min_flow)
                    if bottleneck > 0:
                        graph[u].append((v, capacity, flow + bottleneck))
                        graph[v].append((u, capacity, flow - bottleneck))
                        return bottleneck
            return 0
        max_flow_val = 0
        while True:
            visited = set()
            bottleneck = dfs(graph, source, float('inf'))
            if bottleneck == 0:
                break
            max_flow_val += bottleneck
        return max_flow_val
    graph = {i: [] for i in range(n)}
    for u, v, capacity in edges:
        graph[u].append((v, capacity, 0))
        graph[v].append((u, capacity, 0))
    min_steiner_tree_cost = float('inf')
    for terminal in terminals:
        for other_terminal in terminals:
            if terminal != other_terminal:
                source, sink = terminal, other_terminal
                min_steiner_tree_cost = min(min_steiner_tree_cost, max_flow(graph.copy(), source, sink))
    return min_steiner_tree_cost

# Example Usage
n = 5
terminals = [0, 1, 2]
edges = [(0, 1, 1), (1, 2, 2), (2, 3, 3), (3, 4, 4)]
result = min_steiner_tree(n, terminals, edges)
print(result)
```
#### TC: O((V + E) * F) **SC:** O(V + E)

The code implements the Ford-Fulkerson Algorithm to find the minimum Steiner Tree in a graph. It
uses a max flow approach to determine the minimum cost. The function 'min_steiner_tree' takes the
number of nodes 'n', a list of terminal nodes 'terminals', and a list of edges 'edges'. It
constructs a graph based on the edges and terminals, then iterates through all pairs of terminals to
find the minimum Steiner Tree cost. The max flow algorithm is used to calculate the flow between
terminals, and the minimum cost is updated accordingly. The time complexity is O((V + E) * F) where
V is the number of vertices, E is the number of edges, and F is the maximum flow value. The space
complexity is O(V + E) where V is the number of vertices and E is the number of edges in the graph.

---
---
---