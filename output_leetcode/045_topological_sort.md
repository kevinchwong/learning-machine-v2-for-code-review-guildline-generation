# Topological Sort
## Frequent score for this algorithmic framework: 45 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Course Schedule II](#course-schedule-ii) | 70 | 50 |
| 2 | [Alien Dictionary](#alien-dictionary) | 60 | 60 |
| 3 | [Reconstruct Itinerary](#reconstruct-itinerary) | 60 | 60 |
| 4 | [Minimum Height Trees](#minimum-height-trees) | 50 | 50 |
| 5 | [Find the Town Judge](#find-the-town-judge) | 50 | 50 |
| 6 | [Find Eventual Safe States](#find-eventual-safe-states) | 50 | 60 |
| 7 | [Minimum Number of Vertices to Reach All Nodes](#minimum-number-of-vertices-to-reach-all-nodes) | 40 | 50 |
| 8 | [Sequence Reconstruction](#sequence-reconstruction) | 40 | 60 |
| 9 | [Course Schedule IV](#course-schedule-iv) | 40 | 60 |
| 10 | [Course Schedule III](#course-schedule-iii) | 40 | 70 |
| 11 | [Parallel Courses](#parallel-courses) | 30 | 50 |
| 12 | [All Ancestors of a Node in a Directed Acyclic Graph](#all-ancestors-of-a-node-in-a-directed-acyclic-graph) | 30 | 60 |
| 13 | [Longest Path With Different Adjacent Characters](#longest-path-with-different-adjacent-characters) | 30 | 70 |
| 14 | [Parallel Courses II](#parallel-courses-ii) | 30 | 70 |
| 15 | [Checking Existence of Edge Length Limited Paths](#checking-existence-of-edge-length-limited-paths) | 30 | 70 |
| 16 | [Longest Cycle in a Graph](#longest-cycle-in-a-graph) | 30 | 70 |
---
Topological Sort is a linear ordering of vertices in a directed acyclic graph (DAG) such that for every directed edge u -> v, vertex u comes before v in the ordering. It is used in scenarios like task scheduling, course prerequisite ordering, etc.
```python
from collections import defaultdict, deque

def topological_sort(vertices, edges):
    graph = defaultdict(list)
    in_degree = {i: 0 for i in range(vertices)}
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1
    queue = deque([u for u in in_degree if in_degree[u] == 0])
    topo_order = []
    while queue:
        u = queue.popleft()
        topo_order.append(u)
        for v in graph[u]:
            in_degree[v] -= 1
            if in_degree[v] == 0:
                queue.append(v)
    if len(topo_order) == vertices:
        return topo_order
    else:
        return []  # Graph has a cycle

# Example usage
vertices = 6
edges = [(5, 2), (5, 0), (4, 0), (4, 1), (2, 3), (3, 1)]
print(topological_sort(vertices, edges))
```
### Insight:
1. Topological Sort is only applicable to Directed Acyclic Graphs (DAGs).
2. The algorithm uses Kahn's algorithm which relies on in-degrees of vertices.
3. If the graph has a cycle, a topological sort is not possible.
4. The use of a queue helps in efficiently managing vertices with zero in-degree.
5. The algorithm ensures that each vertex and edge is processed exactly once, leading to its linear time complexity.
---
 --- 
# Course Schedule II
>Find the order in which you should take courses to finish all of them.

>Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3]
- https://leetcode.com/problems/course-schedule-ii/
- Difficulty: 50
- Frequent: 70
- Tags: ['Graph', 'Topological Sort']

### Course Schedule II -- Shortest Solution:
```python
from collections import defaultdict, deque

def findOrder(numCourses, prerequisites):
    graph = defaultdict(list)
    in_degree = [0] * numCourses
    for dest, src in prerequisites:
        graph[src].append(dest)
        in_degree[dest] += 1
    queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    return order if len(order) == numCourses else []
```
#### TC: O(V + E) **SC:** O(V + E)

1. **Graph Representation**: The graph is represented using an adjacency list where each course
points to the list of courses that depend on it. 2. **In-Degree Array**: This array keeps track of
the number of prerequisites each course has. 3. **Queue Initialization**: A queue is initialized
with all courses that have no prerequisites (in-degree of 0). 4. **Topological Sort**: The algorithm
processes each course, reducing the in-degree of its neighbors. If a neighbor's in-degree drops to
0, it is added to the queue. 5. **Order Validation**: Finally, the algorithm checks if the
topological sort includes all courses. If not, it returns an empty list indicating that it's
impossible to complete all courses.

---
---
---
 --- 
# Alien Dictionary
>Determine the order of characters in an alien language.

>Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"
- https://leetcode.com/problems/alien-dictionary/
- Difficulty: 60
- Frequent: 60
- Tags: ['Graph', 'Topological Sort']

### Alien Dictionary -- Shortest Solution:
```python
from collections import defaultdict, deque

def alienOrder(words):
    adj = defaultdict(set)
    in_degree = {c: 0 for word in words for c in word}
    for i in range(len(words) - 1):
        first, second = words[i], words[i + 1]
        min_len = min(len(first), len(second))
        if first[:min_len] == second[:min_len] and len(first) > len(second):
            return ""
        for j in range(min_len):
            if first[j] != second[j]:
                if second[j] not in adj[first[j]]:
                    adj[first[j]].add(second[j])
                    in_degree[second[j]] += 1
                break
    queue = deque([c for c in in_degree if in_degree[c] == 0])
    result = []
    while queue:
        c = queue.popleft()
        result.append(c)
        for neighbor in adj[c]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    if len(result) < len(in_degree):
        return ""
    return "".join(result)
```
#### TC: O(C) where C is the total length of all the words combined. **SC:** O(1) additional space, excluding the space needed for the output.

1. **Graph Construction**: The code constructs a directed graph where each node is a character, and
an edge from node A to node B means character A comes before character B in the alien language. 2.
**In-degree Calculation**: It calculates the in-degree (number of incoming edges) for each node
(character). 3. **Topological Sort**: It uses Kahn's algorithm for topological sorting. Characters
with zero in-degree are added to the queue and processed. For each processed character, its
neighbors' in-degrees are decremented. If a neighbor's in-degree becomes zero, it is added to the
queue. 4. **Cycle Detection**: If the result length is less than the number of unique characters, it
indicates a cycle, and thus, the order is invalid. 5. **Edge Cases**: The code handles edge cases
like prefix issues where a longer word appears before its prefix, which is invalid in the alien
language.

---
### Alien Dictionary -- Best TC Solution:
```python
from collections import defaultdict, deque
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        graph = defaultdict(set)
        in_degree = {c: 0 for word in words for c in word}
        for pair in zip(words, words[1:]):
            for c, d in zip(*pair):
                if c != d:
                    if d not in graph[c]:
                        graph[c].add(d)
                        in_degree[d] += 1
                    break
            else:
                if len(pair[0]) > len(pair[1]):
                    return ""
        queue = deque([c for c in in_degree if in_degree[c] == 0])
        result = ''
        while queue:
            c = queue.popleft()
            result += c
            for d in graph[c]:
                in_degree[d] -= 1
                if in_degree[d] == 0:
                    queue.append(d)
        return result if len(result) == len(in_degree) else ""
```
#### TC: O(C), where C is the total number of characters in all the words combined **SC:** O(1)

The code implements a topological sort approach to solve the Alien Dictionary problem. It builds a
graph based on the order of characters in the input words and calculates the in-degree of each
character. It then performs a topological sort using a queue to determine the order of characters.
The code efficiently handles cyclic dependencies and returns the correct order of characters if it
exists. The time complexity is O(C) where C is the total number of characters in all the words
combined, and the space complexity is O(1) as it uses a constant amount of extra space.

---
### Alien Dictionary -- Best SC Solution:
```python
from collections import defaultdict, deque
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        graph = defaultdict(set)
        in_degree = {c: 0 for word in words for c in word}
        for pair in zip(words, words[1:]):
            for a, b in zip(*pair):
                if a != b:
                    if b not in graph[a]:
                        graph[a].add(b)
                        in_degree[b] += 1
                    break
        queue = deque([c for c in in_degree if in_degree[c] == 0])
        result = ''
        while queue:
            node = queue.popleft()
            result += node
            for neighbor in graph[node]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)
        return result if len(result) == len(in_degree) else ''
```
#### TC: O(C) **SC:** O(C)

The code implements a topological sort approach to solve the Alien Dictionary problem. It builds a
graph based on the order of characters in the input words and calculates the in-degree of each
character. Then, it performs a topological sort using a queue to determine the order of characters.
The time complexity of the code is O(C), where C is the total number of characters in all words. The
space complexity is also O(C) to store the graph and in-degree information.

---
---
---
 --- 
# Reconstruct Itinerary
>Reconstruct the itinerary in order.

>Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
- https://leetcode.com/problems/reconstruct-itinerary/
- Difficulty: 60
- Frequent: 60
- Tags: ['Graph', 'Topological Sort']

### Reconstruct Itinerary -- Shortest Solution:
```python
from collections import defaultdict, deque

def findItinerary(tickets):
    graph = defaultdict(deque)
    for src, dst in sorted(tickets):
        graph[src].append(dst)
    route, stack = [], ['JFK']
    while stack:
        while graph[stack[-1]]:
            stack.append(graph[stack[-1]].popleft())
        route.append(stack.pop())
    return route[::-1]
```
#### TC: O(E + V) **SC:** O(E + V)

1. **Graph Construction**: The graph is built using a defaultdict of deques, where each key is an
airport and the value is a deque of destination airports sorted in lexical order. 2. **Hierholzer's
Algorithm**: The algorithm used is a variation of Hierholzer's algorithm to find an Eulerian path.
It ensures that we visit each edge exactly once. 3. **Stack-Based Traversal**: We use a stack to
perform the traversal. Starting from 'JFK', we keep visiting the next destination until we reach an
airport with no further destinations. We then backtrack and add the airports to the route. 4.
**Route Construction**: The route is constructed in reverse order, so we reverse it at the end to
get the correct itinerary.

---
### Reconstruct Itinerary -- Best TC Solution:
```python
from collections import defaultdict

class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph = defaultdict(list)
        for start, end in sorted(tickets, reverse=True):
            graph[start].append(end)
        route = []
        def dfs(node):
            while graph[node]:
                dfs(graph[node].pop())
            route.append(node)
        dfs('JFK')
        return route[::-1]
```
#### TC: O(E log E) where E is the number of edges **SC:** O(E)

The code constructs a graph from the given list of tickets and performs a depth-first search (DFS)
starting from 'JFK' airport. The tickets are sorted in reverse order to ensure the lexical order of
the destinations. The DFS function recursively visits the destinations until there are no more
outgoing flights. The route is built in reverse order and returned as the final itinerary.

---
---
---
 --- 
# Minimum Height Trees
>Find the root labels of Minimum Height Trees.

>Input: n = 4, edges = [[1,0],[1,2],[1,3]]
Output: [1]
- https://leetcode.com/problems/minimum-height-trees/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'Topological Sort']

### Minimum Height Trees -- Shortest Solution:
```python
from collections import deque, defaultdict

def findMinHeightTrees(n, edges):
    if n <= 2:
        return list(range(n))
    graph = defaultdict(set)
    for u, v in edges:
        graph[u].add(v)
        graph[v].add(u)
    leaves = deque([i for i in range(n) if len(graph[i]) == 1])
    remaining_nodes = n
    while remaining_nodes > 2:
        leaves_count = len(leaves)
        remaining_nodes -= leaves_count
        for _ in range(leaves_count):
            leaf = leaves.popleft()
            neighbor = graph[leaf].pop()
            graph[neighbor].remove(leaf)
            if len(graph[neighbor]) == 1:
                leaves.append(neighbor)
    return list(leaves)
```
#### TC: O(n) **SC:** O(n)

The solution uses a topological sort-like approach to iteratively remove leaves (nodes with only one
connection) from the graph until 1 or 2 nodes remain, which are the roots of the Minimum Height
Trees. This ensures that the remaining nodes are as close to the center as possible, minimizing the
tree height.

---
### Minimum Height Trees -- Best TC Solution:
```python
from collections import defaultdict, deque
def findMinHeightTrees(n, edges):
    if n == 1:
        return [0]
    graph = defaultdict(list)
    degrees = [0] * n
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
        degrees[u] += 1
        degrees[v] += 1
    leaves = deque([i for i in range(n) if degrees[i] == 1])
    while n > 2:
        n -= len(leaves)
        new_leaves = deque()
        for leaf in leaves:
            neighbor = graph[leaf].pop()
            degrees[neighbor] -= 1
            if degrees[neighbor] == 1:
                new_leaves.append(neighbor)
        leaves = new_leaves
    return list(leaves)

# Example
n = 4
edges = [[1, 0], [1, 2], [1, 3]]
print(findMinHeightTrees(n, edges))
```
#### TC: O(N) **SC:** O(N)

The code uses a Topological Sort approach to find the minimum height trees in a graph. It first
builds a graph and calculates the degrees of each node. Then, it iteratively removes leaves (nodes
with only one neighbor) until only the root or roots remain. The algorithm efficiently identifies
the center nodes that minimize the height of the tree. The time complexity is O(N) where N is the
number of nodes, and the space complexity is also O(N) to store the graph and degrees.

---
---
---
 --- 
# Find the Town Judge
>Find the town judge based on trust relationships.

>Input: n = 2, trust = [[1,2]]
Output: 2
- https://leetcode.com/problems/find-the-town-judge/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'Topological Sort']

### Find the Town Judge -- Shortest Solution:
```python
class Solution:
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        if N == 1:
            return 1
        trust_counts = [0] * (N + 1)
        for a, b in trust:
            trust_counts[a] -= 1
            trust_counts[b] += 1
        for i in range(1, N + 1):
            if trust_counts[i] == N - 1:
                return i
        return -1
```
#### TC: O(T + N) **SC:** O(N)

1. The code uses a list `trust_counts` to keep track of the trust balance for each person. 2. If a
person trusts someone, their trust balance decreases, and the trusted person's balance increases. 3.
The town judge should have a trust balance of `N-1` (trusted by everyone else but trusts no one). 4.
The code iterates through the trust list to update the trust balances and then checks for the judge.

---
### Find the Town Judge -- Best TC Solution:
```python
from collections import defaultdict
class Solution:
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        if len(trust) < N - 1:
            return -1
        trust_count = defaultdict(int)
        trusted_by_count = defaultdict(int)
        for a, b in trust:
            trust_count[a] -= 1
            trusted_by_count[b] += 1
        for i in range(1, N + 1):
            if trust_count[i] == 0 and trusted_by_count[i] == N - 1:
                return i
        return -1
```
#### TC: O(N) **SC:** O(N)

The code uses two defaultdicts to keep track of the trust count and the count of people trusted by
others. It iterates through the trust list to update these counts. Then, it checks for the town
judge by finding the person who is trusted by everyone (N-1 times) and does not trust anyone. If
such a person exists, they are the town judge. The time complexity of the code is O(N) where N is
the number of people in the town, and the space complexity is also O(N) to store the trust counts
and trusted by counts.

---
---
---
 --- 
# Find Eventual Safe States
>Find all the eventual safe states in a directed graph.

>Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]
- https://leetcode.com/problems/find-eventual-safe-states/
- Difficulty: 60
- Frequent: 50
- Tags: ['Graph', 'Topological Sort']

### Find Eventual Safe States -- Shortest Solution:
```python
def eventualSafeNodes(graph):
    from collections import deque
    n = len(graph)
    outdegree = [0] * n
    reverse_graph = [[] for _ in range(n)]
    for i in range(n):
        for j in graph[i]:
            reverse_graph[j].append(i)
            outdegree[i] += 1
    queue = deque([i for i in range(n) if outdegree[i] == 0])
    result = []
    while queue:
        node = queue.popleft()
        result.append(node)
        for neighbor in reverse_graph[node]:
            outdegree[neighbor] -= 1
            if outdegree[neighbor] == 0:
                queue.append(neighbor)
    return sorted(result)
```
#### TC: O(V + E) **SC:** O(V + E)

The solution uses a topological sort approach on the reverse graph. Nodes with zero outdegree are
initially added to the queue. By processing these nodes and reducing the outdegree of their
neighbors, we can identify all nodes that eventually lead to terminal nodes. The result is sorted
before returning to ensure the nodes are listed in ascending order.

---
### Find Eventual Safe States -- Best TC Solution:
```python
from typing import List

def eventualSafeNodes(graph: List[List[int]]) -> List[int]:
    def is_safe(node):
        if colors[node] != 0:
            return colors[node] == 1
        colors[node] = 2
        for neighbor in graph[node]:
            if not is_safe(neighbor):
                return False
        colors[node] = 1
        return True

    n = len(graph)
    colors = [0] * n
    return [i for i in range(n) if is_safe(i)]
```
#### TC: O(N + E) **SC:** O(N)

The code defines a function `eventualSafeNodes` that uses a depth-first search (DFS) approach to
determine the nodes that are eventually safe in a directed graph. It marks nodes with colors (0:
unvisited, 1: safe, 2: unsafe) and recursively checks if a node is safe by visiting its neighbors.
The function returns a list of nodes that are eventually safe. The time complexity is O(N + E) where
N is the number of nodes and E is the number of edges, and the space complexity is O(N) for the
colors array.

---
---
---
 --- 
# Minimum Number of Vertices to Reach All Nodes
>Find the minimum number of vertices needed to reach all nodes.

>Input: n = 6, edges = [[0,1],[0,2],[2,5],[3,4],[4,2]]
Output: [0,3]
- https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes/
- Difficulty: 50
- Frequent: 40
- Tags: ['Graph', 'Topological Sort']

### Minimum Number of Vertices to Reach All Nodes -- Shortest Solution:
```python
class Solution:
    def findSmallestSetOfVertices(self, n, edges):
        return list(set(range(n)) - {v for u, v in edges})
```
#### TC: O(E + V) **SC:** O(V)

The solution leverages the concept that nodes with no incoming edges must be included in the result.
By creating a set of all nodes and subtracting the set of nodes that have incoming edges, we
efficiently determine the minimum set of vertices needed to reach all nodes.

---
### Minimum Number of Vertices to Reach All Nodes -- Best TC Solution:
```python
from typing import List

def findSmallestSetOfVertices(n: int, edges: List[List[int]]) -> List[int]:
    indegree = [0] * n
    for _, end in edges:
        indegree[end] += 1
    return [i for i in range(n) if indegree[i] == 0]
```
#### TC: O(n) **SC:** O(n)

The code uses an array 'indegree' to store the indegree of each node. It then iterates through the
edges to calculate the indegree of each node. Finally, it returns a list of nodes with an indegree
of 0, which are the vertices that can reach all other nodes. The time complexity is O(n) as it
iterates through all edges once, and the space complexity is O(n) for the 'indegree' array.

---
---
---
 --- 
# Sequence Reconstruction
>Check if the original sequence can be uniquely reconstructed from the sequences.

>Input: org = [1,2,3], seqs = [[1,2],[1,3]]
Output: false
- https://leetcode.com/problems/sequence-reconstruction/
- Difficulty: 60
- Frequent: 40
- Tags: ['Graph', 'Topological Sort']

### Sequence Reconstruction -- Shortest Solution:
```python
from collections import defaultdict, deque

def sequenceReconstruction(org, seqs):
    graph = defaultdict(set)
    in_degree = defaultdict(int)
    nodes = set()
    for seq in seqs:
        for i in range(len(seq)):
            nodes.add(seq[i])
            if i > 0:
                if seq[i] not in graph[seq[i-1]]:
                    graph[seq[i-1]].add(seq[i])
                    in_degree[seq[i]] += 1
    if nodes != set(org):
        return False
    queue = deque([node for node in org if in_degree[node] == 0])
    index = 0
    while queue:
        if len(queue) > 1:
            return False
        current = queue.popleft()
        if org[index] != current:
            return False
        index += 1
        for neighbor in graph[current]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    return index == len(org)
```
#### TC: O(V + E) **SC:** O(V + E)

The solution uses a topological sort approach to determine if the original sequence can be uniquely
reconstructed from the given sequences. The graph is built using adjacency lists, and in-degrees are
tracked for each node. A queue is used to process nodes with zero in-degrees, ensuring that at each
step, there is only one node to process (to maintain uniqueness). The algorithm checks if the
processed sequence matches the original sequence.

---
### Sequence Reconstruction -- Best TC Solution:
```python
from collections import defaultdict
def sequenceReconstruction(org, seqs):
    graph = defaultdict(set)
    indegree = {num: 0 for seq in seqs for num in seq}
    for seq in seqs:
        for i in range(len(seq) - 1):
            if seq[i + 1] not in graph[seq[i]]:
                graph[seq[i]].add(seq[i + 1])
                indegree[seq[i + 1]] += 1
    if len(indegree) != len(org):
        return False
    queue = [num for num in indegree if indegree[num] == 0]
    res = []
    while queue:
        if len(queue) > 1:
            return False
        num = queue.pop(0)
        res.append(num)
        for neighbor in graph[num]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)
    return res == org
```
#### TC: O(N + E) **SC:** O(N + E)

The code implements a topological sort approach to reconstruct the sequence. It builds a graph based
on the given sequences and calculates the indegree of each node. Then, it performs a topological
sort using a queue to determine the sequence. The time complexity is O(N + E) where N is the number
of elements in the sequence and E is the number of edges in the graph. The space complexity is also
O(N + E) due to the graph and indegree data structures.

---
---
---
 --- 
# Course Schedule IV
>Determine if a course is a prerequisite for another course.

>Input: n = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
Output: [false,true]
- https://leetcode.com/problems/course-schedule-iv/
- Difficulty: 60
- Frequent: 40
- Tags: ['Graph', 'Topological Sort']

### Course Schedule IV -- Shortest Solution:
```python
from collections import defaultdict, deque

def checkIfPrerequisite(numCourses, prerequisites, queries):
    graph = defaultdict(list)
    in_degree = [0] * numCourses
    for u, v in prerequisites:
        graph[u].append(v)
        in_degree[v] += 1
    topo_order = []
    queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    while queue:
        node = queue.popleft()
        topo_order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    reachable = [set() for _ in range(numCourses)]
    for u in topo_order:
        for v in graph[u]:
            reachable[v].add(u)
            reachable[v].update(reachable[u])
    return [u in reachable[v] for u, v in queries]
```
#### TC: O(N + E + Q) **SC:** O(N^2)

1. **Graph Construction**: We first build the graph and compute in-degrees for each node. 2.
**Topological Sorting**: Using Kahn's algorithm, we perform a topological sort of the graph. 3.
**Reachability Set**: For each node, we maintain a set of nodes that can reach it. 4. **Query
Processing**: For each query, we simply check if one node is in the reachability set of the other.

---
### Course Schedule IV -- Best TC Solution:
```python
from collections import defaultdict
class Solution:
    def checkIfPrerequisite(self, numCourses: int, prerequisites: List[List[int]], queries: List[List[int]]) -> List[bool]:
        graph = defaultdict(list)
        for u, v in prerequisites:
            graph[u].append(v)
        reachable = [[False] * numCourses for _ in range(numCourses)]
        for i in range(numCourses):
            stack = [i]
            visited = set()
            while stack:
                node = stack.pop()
                visited.add(node)
                reachable[i][node] = True
                for neighbor in graph[node]:
                    if neighbor not in visited:
                        stack.append(neighbor)
        res = []
        for q in queries:
            res.append(reachable[q[0]][q[1]])
        return res
```
#### TC: O(N^2) **SC:** O(N^2)

The code uses a Topological Sort approach to determine if one course is a prerequisite of another.
It creates a graph of prerequisites and then checks for reachability between all pairs of courses
using a depth-first search. The reachable matrix stores whether a course is reachable from another
course. The time complexity is O(N^2) due to the nested loops, and the space complexity is also
O(N^2) for the reachable matrix.

---
---
---
 --- 
# Course Schedule III
>Find the maximum number of courses that can be taken.

>Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
Output: 3
- https://leetcode.com/problems/course-schedule-iii/
- Difficulty: 70
- Frequent: 40
- Tags: ['Graph', 'Topological Sort']

### Course Schedule III -- Shortest Solution:
```python
import heapq

def scheduleCourse(courses):
    courses.sort(key=lambda x: x[1])
    total_time = 0
    max_heap = []
    for duration, last_day in courses:
        if total_time + duration <= last_day:
            heapq.heappush(max_heap, -duration)
            total_time += duration
        elif max_heap and -max_heap[0] > duration:
            total_time += duration + heapq.heappop(max_heap)
            heapq.heappush(max_heap, -duration)
    return len(max_heap)
```
#### TC: O(n log n) **SC:** O(n)

1. **Sorting**: The courses are first sorted by their end days. This ensures that we consider each
course in the order of their deadlines. 2. **Max-Heap**: A max-heap is used to keep track of the
durations of the courses we've added to our schedule. This allows us to efficiently remove the
longest course if we need to make room for a shorter one. 3. **Greedy Approach**: The algorithm
iterates through each course and decides whether to add it to the schedule based on whether it can
fit within the current total time. If it can't, the algorithm checks if replacing the longest course
in the current schedule with the new course would be beneficial. 4. **Efficiency**: The use of a
heap ensures that both insertion and removal operations are efficient, leading to an overall time
complexity of O(n log n).

---
---
---
 --- 
# Parallel Courses
>Find the minimum number of semesters required to complete all courses.

>Input: n = 3, relations = [[1,3],[2,3]]
Output: 2
- https://leetcode.com/problems/parallel-courses/
- Difficulty: 50
- Frequent: 30
- Tags: ['Graph', 'Topological Sort']

### Parallel Courses -- Shortest Solution:
```python
from collections import deque, defaultdict

def minimumSemesters(n, relations):
    graph = defaultdict(list)
    in_degree = [0] * (n + 1)
    for u, v in relations:
        graph[u].append(v)
        in_degree[v] += 1
    queue = deque([i for i in range(1, n + 1) if in_degree[i] == 0])
    semester = 0
    studied_count = 0
    while queue:
        semester += 1
        for _ in range(len(queue)):
            node = queue.popleft()
            studied_count += 1
            for neighbor in graph[node]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)
    return semester if studied_count == n else -1
```
#### TC: O(V + E) **SC:** O(V + E)

1. The problem is modeled as a Directed Acyclic Graph (DAG) where nodes represent courses and edges
represent prerequisites. 2. We use Kahn's algorithm for Topological Sorting to determine the minimum
number of semesters required. 3. The `in_degree` array keeps track of the number of prerequisites
for each course. 4. Courses with no prerequisites are added to the queue initially. 5. We process
each course, reducing the in-degree of its neighbors and adding neighbors with zero in-degree to the
queue. 6. The number of semesters is incremented each time we process a level of the graph. 7. If
all courses are studied, we return the number of semesters; otherwise, we return -1 indicating it's
not possible to complete all courses.

---
### Parallel Courses -- Best TC Solution:
```python
from collections import defaultdict, deque
class Solution:
    def minimumSemesters(self, N: int, relations: List[List[int]]) -> int:
        graph = defaultdict(list)
        in_degree = [0] * (N + 1)
        for x, y in relations:
            graph[x].append(y)
            in_degree[y] += 1
        queue = deque([i for i in range(1, N + 1) if in_degree[i] == 0])
        semester = 0
        while queue:
            next_queue = deque()
            while queue:
                node = queue.popleft()
                N -= 1
                for neighbor in graph[node]:
                    in_degree[neighbor] -= 1
                    if in_degree[neighbor] == 0:
                        next_queue.append(neighbor)
            queue = next_queue
            semester += 1
        return semester if N == 0 else -1
```
#### TC: O(N + E) **SC:** O(N + E)

The code uses a Topological Sort approach to solve the problem efficiently. It builds a graph
representing the course dependencies and calculates the in-degree of each course. It then iterates
through the courses using a queue, decrementing the in-degree of each course as it progresses. The
code keeps track of the semester count and returns the minimum number of semesters required to
complete all courses. The time complexity is O(N + E) where N is the number of courses and E is the
number of dependencies, and the space complexity is O(N + E) to store the graph and in-degree
information.

---
---
---
 --- 
# All Ancestors of a Node in a Directed Acyclic Graph
>Find all ancestors of each node in a directed acyclic graph.

>Input: n = 8, edges = [[0,3],[0,4],[1,3],[2,4],[2,7],[3,5],[3,6],[3,7],[4,6]]
Output: [[],[],[],[0,1],[0,2],[0,1,3],[0,1,2,3,4],[0,1,2,3]]
- https://leetcode.com/problems/all-ancestors-of-a-node-in-a-directed-acyclic-graph/
- Difficulty: 60
- Frequent: 30
- Tags: ['Graph', 'Topological Sort']

### All Ancestors of a Node in a Directed Acyclic Graph -- Shortest Solution:
```python
from collections import defaultdict, deque

def getAncestors(n, edges):
    graph = defaultdict(list)
    indegree = [0] * n
    ancestors = [set() for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)
        indegree[v] += 1
    queue = deque([i for i in range(n) if indegree[i] == 0])
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            ancestors[neighbor].update(ancestors[node])
            ancestors[neighbor].add(node)
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)
    return [sorted(list(ancestor)) for ancestor in ancestors]
```
#### TC: O(V + E) **SC:** O(V + E)

1. The code uses a topological sort approach to process nodes in a Directed Acyclic Graph (DAG). 2.
It maintains an adjacency list `graph` and an `indegree` list to keep track of incoming edges for
each node. 3. A queue is initialized with all nodes having zero indegree (i.e., no incoming edges).
4. For each node processed, its ancestors are propagated to its neighbors, and the node itself is
added to its neighbors' ancestor sets. 5. The process continues until all nodes are processed,
ensuring that each node's ancestors are correctly determined. 6. Finally, the ancestor sets are
converted to sorted lists for the required output format.

---
---
---
 --- 
# Longest Path With Different Adjacent Characters
>Find the longest path in a tree with different adjacent characters.

>Input: parent = [-1,0,0,1,1,2], s = "abacbe"
Output: 3
- https://leetcode.com/problems/longest-path-with-different-adjacent-characters/
- Difficulty: 70
- Frequent: 30
- Tags: ['Graph', 'Topological Sort']

### Longest Path With Different Adjacent Characters -- Shortest Solution:
```python
from collections import defaultdict, deque

def longestPath(parent, s):
    n = len(parent)
    tree = defaultdict(list)
    for i in range(1, n):
        tree[parent[i]].append(i)
    dp = [1] * n
    q = deque([i for i in range(n) if len(tree[i]) == 0])
    res = 1
    while q:
        node = q.popleft()
        if node == 0: continue
        p = parent[node]
        if s[node] != s[p]:
            dp[p] = max(dp[p], dp[node] + 1)
        res = max(res, dp[p])
        tree[p].remove(node)
        if len(tree[p]) == 0:
            q.append(p)
    return res
```
#### TC: O(n) **SC:** O(n)

1. **Tree Construction**: The tree is constructed using a defaultdict where each node points to its
children. 2. **Dynamic Programming Array**: `dp` array is initialized to store the longest path
ending at each node. 3. **Queue Initialization**: A deque is initialized with all leaf nodes (nodes
with no children). 4. **Topological Sort**: Process nodes in topological order using the queue. For
each node, update its parent's `dp` value if the characters are different. 5. **Result
Calculation**: The result is updated with the maximum value found in the `dp` array. 6. **Edge
Case**: The root node is skipped in the processing loop to avoid unnecessary checks.

---
---
---
 --- 
# Parallel Courses II
>Find the minimum number of semesters required to complete all courses with a limit on courses per semester.

>Input: n = 4, dependencies = [[2,1],[3,1],[1,4]]
Output: 2
- https://leetcode.com/problems/parallel-courses-ii/
- Difficulty: 70
- Frequent: 30
- Tags: ['Graph', 'Topological Sort']

### Parallel Courses II -- Shortest Solution:
```python
from functools import lru_cache

class Solution:
    def minNumberOfSemesters(self, n: int, dependencies: List[List[int]], k: int) -> int:
        req = [0] * n
        for pre, course in dependencies:
            req[course - 1] |= 1 << (pre - 1)

        @lru_cache(None)
        def dp(taken):
            if taken == (1 << n) - 1:
                return 0
            can_take = []
            for i in range(n):
                if (taken & (1 << i)) == 0 and (taken & req[i]) == req[i]:
                    can_take.append(i)
            res = float('inf')
            for comb in combinations(can_take, min(k, len(can_take))):
                new_taken = taken
                for c in comb:
                    new_taken |= 1 << c
                res = min(res, 1 + dp(new_taken))
            return res

        return dp(0)
```
#### TC: O(3^n) **SC:** O(2^n)

1. **Bitmask Representation**: Courses are represented using bitmasks to efficiently track which
courses have been taken. 2. **Dependencies Encoding**: The prerequisites for each course are encoded
in a list where each index represents a course and the value is a bitmask of its prerequisites. 3.
**Memoization**: The `dp` function uses memoization to store results of subproblems, reducing
redundant calculations. 4. **Combination Generation**: For each state, the algorithm generates all
possible combinations of courses that can be taken in the current semester, up to the limit `k`. 5.
**Recursive Calculation**: The function recursively calculates the minimum number of semesters
needed to complete all courses from any given state.

---
### Parallel Courses II -- Best TC Solution:
```python
from collections import deque
class Solution:
    def minNumberOfSemesters(self, n: int, dependencies: List[List[int]], k: int) -> int:
        graph = {i: [] for i in range(1, n + 1)}
        indegree = {i: 0 for i in range(1, n + 1)}
        for u, v in dependencies:
            graph[u].append(v)
            indegree[v] += 1
        queue = deque([i for i in range(1, n + 1) if indegree[i] == 0])
        level = 0
        while queue:
            level += 1
            next_queue = deque()
            for _ in range(min(k, len(queue))):
                node = queue.popleft()
                for neighbor in graph[node]:
                    indegree[neighbor] -= 1
                    if indegree[neighbor] == 0:
                        next_queue.append(neighbor)
            queue = next_queue if next_queue else [i for i in range(1, n + 1) if indegree[i] == 0]
        return level
```
#### TC: O(2^n) **SC:** O(n)

The code uses a topological sort approach to solve the problem efficiently. It creates a graph and
calculates the indegree of each node. It then iterates through the nodes in a level-wise manner,
considering the dependencies and the number of courses that can be taken in each semester. The code
optimally handles the dependencies and tracks the levels to determine the minimum number of
semesters required. The time complexity is O(2^n) due to the exponential nature of the problem, and
the space complexity is O(n) to store the graph and indegree information.

---
---
---
 --- 
# Checking Existence of Edge Length Limited Paths
>Check if there exists a path between two nodes with edge lengths within a limit.

>Input: n = 3, edgeList = [[0,1,2],[1,2,4],[2,0,8]], queries = [[0,2,5]]
Output: [true]
- https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths/
- Difficulty: 70
- Frequent: 30
- Tags: ['Graph', 'Topological Sort']

### Checking Existence of Edge Length Limited Paths -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]

    def union(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            if self.rank[root_u] > self.rank[root_v]:
                self.parent[root_v] = root_u
            elif self.rank[root_u] < self.rank[root_v]:
                self.parent[root_u] = root_v
            else:
                self.parent[root_v] = root_u
                self.rank[root_u] += 1

class Solution:
    def distanceLimitedPathsExist(self, n, edgeList, queries):
        uf = UnionFind(n)
        edgeList.sort(key=lambda x: x[2])
        queries = sorted([(p, q, limit, i) for i, (p, q, limit) in enumerate(queries)], key=lambda x: x[2])
        res = [False] * len(queries)
        j = 0
        for p, q, limit, i in queries:
            while j < len(edgeList) and edgeList[j][2] < limit:
                uf.union(edgeList[j][0], edgeList[j][1])
                j += 1
            if uf.find(p) == uf.find(q):
                res[i] = True
        return res
```
#### TC: O((E + Q) log E) **SC:** O(N + E + Q)

The solution uses a Union-Find data structure to efficiently manage and merge disjoint sets. The
edges are sorted by their weights, and the queries are sorted by their limits. For each query, edges
with weights less than the query limit are added to the Union-Find structure. This allows us to
check if two nodes are connected without exceeding the given limit. The approach ensures that each
edge and query is processed in a logarithmic time complexity due to sorting, making the solution
efficient.

---
### Checking Existence of Edge Length Limited Paths -- Best TC Solution:
```python
from typing import List


class Solution:
    def __init__(self):
        self.parent = {}
        self.rank = {}
        self.edges = []

    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x == root_y:
            return
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        else:
            self.parent[root_y] = root_x
            if self.rank[root_x] == self.rank[root_y]:
                self.rank[root_x] += 1

    def maxNumEdgesToRemove(self, n: int, edges: List[List[int]]) -> int:
        self.parent = {i: i for i in range(1, n + 1)}
        self.rank = {i: 0 for i in range(1, n + 1)}
        self.edges = edges
        self.edges.sort(key=lambda x: -x[0])
        res = 0
        for edge in self.edges:
            if edge[0] == 3:
                if self.find(edge[1]) != self.find(edge[2]):
                    self.union(edge[1], edge[2])
                else:
                    res += 1
        temp_parent = self.parent.copy()
        temp_rank = self.rank.copy()
        for edge in self.edges:
            if edge[0] == 1:
                if self.find(edge[1]) != self.find(edge[2]):
                    self.union(edge[1], edge[2])
                else:
                    res += 1
        self.parent = temp_parent
        self.rank = temp_rank
        for edge in self.edges:
            if edge[0] == 2:
                if self.find(edge[1]) != self.find(edge[2]):
                    self.union(edge[1], edge[2])
                else:
                    res += 1
        if len(set(self.find(i) for i in range(1, n + 1))) > 1:
            return -1
        return len(edges) - n + res
```
#### TC: O(ElogE + ElogN) **SC:** O(N)

The code implements a disjoint set union (DSU) algorithm to find the maximum number of edges that
can be removed while maintaining connectivity between nodes. It uses the DSU data structure to keep
track of connected components and union-find operations to merge components. The code sorts the
edges based on their type and processes them accordingly. The time complexity is O(ElogE + ElogN)
where E is the number of edges and N is the number of nodes, and the space complexity is O(N) where
N is the number of nodes.

---
### Checking Existence of Edge Length Limited Paths -- Best SC Solution:
```python
from typing import List

class Solution:
    def __init__(self):
        self.parent = {}
        self.rank = {}
        self.edges = []

    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x == root_y:
            return
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        else:
            self.parent[root_y] = root_x
            if self.rank[root_x] == self.rank[root_y]:
                self.rank[root_x] += 1

    def max_edge_limit_path_exists(self, queries: List[List[int]], limit: List[int]) -> List[bool]:
        n = len(queries)
        for i in range(n):
            queries[i].append(i)
        self.edges = sorted(queries, key=lambda x: x[2])
        self.parent = {i: i for i in range(n)}
        self.rank = {i: 0 for i in range(n)}
        limit = [(limit[i], i) for i in range(len(limit))]
        limit.sort()
        res = [False] * len(limit)
        j = 0
        for l, idx in limit:
            while j < n and self.edges[j][2] < l:
                self.union(self.edges[j][0], self.edges[j][1])
                j += 1
            res[idx] = self.find(0) == self.find(n - 1)
        return res
```
#### TC: O(NlogN + QlogQ + Nα(N)) **SC:** O(N + Q)

The code implements a Union-Find algorithm to check the existence of edge length-limited paths. It
sorts the queries and limits, then iterates through the edges to union the nodes based on their edge
lengths. The find function uses path compression to optimize the search. The time complexity is
dominated by the sorting of queries and limits, as well as the union operations. The space
complexity is determined by the storage of parent and rank dictionaries. Overall, the code
efficiently determines the existence of paths within the given length limits.

---
---
---
 --- 
# Longest Cycle in a Graph
>Find the length of the longest cycle in a graph.

>Input: edges = [3,3,4,2,3]
Output: 3
- https://leetcode.com/problems/longest-cycle-in-a-graph/
- Difficulty: 70
- Frequent: 30
- Tags: ['Graph', 'Topological Sort']

### Longest Cycle in a Graph -- Shortest Solution:
```python
from collections import deque

def longestCycle(edges):
    n = len(edges)
    in_degree = [0] * n
    for v in edges:
        if v != -1:
            in_degree[v] += 1
    queue = deque([i for i in range(n) if in_degree[i] == 0])
    while queue:
        node = queue.popleft()
        neighbor = edges[node]
        if neighbor != -1:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    visited = [False] * n
    def dfs(node):
        stack = []
        while node != -1 and not visited[node]:
            visited[node] = True
            stack.append(node)
            node = edges[node]
        if node != -1 and node in stack:
            return len(stack) - stack.index(node)
        return 0
    return max(dfs(i) for i in range(n) if not visited[i])
```
#### TC: O(N) **SC:** O(N)

1. **In-Degree Calculation**: We first calculate the in-degree of each node. Nodes with zero in-
degree are added to a queue. 2. **Topological Sort**: We perform a topological sort by removing
nodes with zero in-degree and reducing the in-degree of their neighbors. This helps in identifying
nodes that are part of cycles. 3. **DFS for Cycle Detection**: For each unvisited node, we perform a
DFS to detect cycles. If a cycle is detected, we calculate its length. 4. **Result**: The maximum
length of all detected cycles is returned.

---
### Longest Cycle in a Graph -- Best TC Solution:
```python
from collections import defaultdict
class Solution:
    def longest_cycle(self, graph: List[List[int]]) -> int:
        def dfs(node, visited, path):
            visited[node] = True
            path.append(node)
            res = 0
            for neighbor in graph[node]:
                if neighbor in path:
                    res = max(res, len(path) - path.index(neighbor))
                elif not visited[neighbor]:
                    res = max(res, dfs(neighbor, visited, path))
            path.pop()
            return res
        longest = 0
        for i in range(len(graph)):
            visited = [False] * len(graph)
            longest = max(longest, dfs(i, visited, []))
        return longest if longest > 2 else -1
```
#### TC: O(V*(V+E)) **SC:** O(V)

The code defines a class Solution with a method longest_cycle that takes a graph as input. It uses a
depth-first search (DFS) approach to find the longest cycle in the graph. The DFS function
recursively explores the graph nodes, keeping track of visited nodes and the current path. It
calculates the length of the cycle by finding the index of the neighbor in the path. The main loop
iterates through each node in the graph and finds the longest cycle starting from that node. The
time complexity is O(V*(V+E)), where V is the number of vertices and E is the number of edges. The
space complexity is O(V) for the visited array.

---
---
---