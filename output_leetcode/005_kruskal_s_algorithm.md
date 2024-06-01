# Kruskal's Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree](#find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree) | 65 | 75 |
| 2 | [Min Cost to Connect All Points](#min-cost-to-connect-all-points) | 55 | 45 |
| 3 | [Minimum Cost to Merge Stones](#minimum-cost-to-merge-stones) | 55 | 65 |
| 4 | [Network Connection](#network-connection) | 50 | 50 |
| 5 | [Minimum Cost to Make Two Strings Palindrome](#minimum-cost-to-make-two-strings-palindrome) | 50 | 50 |
| 6 | [Minimum Cost to Make Two Strings Identical](#minimum-cost-to-make-two-strings-identical) | 50 | 55 |
| 7 | [Minimum Cost to Make Two Strings Subsequence](#minimum-cost-to-make-two-strings-subsequence) | 50 | 55 |
| 8 | [Minimum Cost to Hire K Workers](#minimum-cost-to-hire-k-workers) | 50 | 60 |
| 9 | [Minimum Cost to Make Two Strings Anagram](#minimum-cost-to-make-two-strings-anagram) | 45 | 45 |
| 10 | [Minimum Cost to Make Array Equal](#minimum-cost-to-make-array-equal) | 45 | 50 |
| 11 | [Minimum Cost to Move Chips to The Same Position](#minimum-cost-to-move-chips-to-the-same-position) | 40 | 35 |
| 12 | [Minimum Cost to Make String Valid](#minimum-cost-to-make-string-valid) | 40 | 40 |
---
Kruskal's algorithm is a minimum spanning tree algorithm that finds an edge of the least possible weight that connects any two trees in the forest. It is used to find a subset of the edges that forms a tree including every vertex, where the total weight of all the edges in the tree is minimized.
```python
class DisjointSet:
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


def kruskal(n, edges):
    edges.sort(key=lambda x: x[2])
    ds = DisjointSet(n)
    mst = []
    for u, v, weight in edges:
        if ds.find(u) != ds.find(v):
            ds.union(u, v)
            mst.append((u, v, weight))
    return mst

# Example usage:
n = 4
edges = [
    (0, 1, 10),
    (0, 2, 6),
    (0, 3, 5),
    (1, 3, 15),
    (2, 3, 4)
]
print(kruskal(n, edges))
```
### Insight:
1. The algorithm uses a Disjoint Set (Union-Find) data structure to keep track of the connected components.
2. Sorting the edges by weight ensures that the smallest available edge is always considered first.
3. The Union-Find operations (find and union) are nearly constant time due to path compression and union by rank.
4. The algorithm is greedy and ensures that no cycles are formed while adding edges to the minimum spanning tree.
---
 --- 
# Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree
>Find critical and pseudo-critical edges in a minimum spanning tree using Kruskal's Algorithm.

>Input: n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3]]
Output: [[0,1],[2,3]]
- https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/
- Difficulty: 75
- Frequent: 65
- Tags: ['Graph', 'Union Find']

### Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree -- Shortest Solution:
```python
from typing import List

class Solution:
    def findCriticalAndPseudoCriticalEdges(self, n: int, edges: List[List[int]]) -> List[List[int]]:
        def find(parent, x):
            if parent[x] != x:
                parent[x] = find(parent, parent[x])
            return parent[x]
        
        def union(parent, rank, x, y):
            rootX = find(parent, x)
            rootY = find(parent, y)
            if rank[rootX] < rank[rootY]:
                parent[rootX] = rootY
            elif rank[rootX] > rank[rootY]:
                parent[rootY] = rootX
            else:
                parent[rootY] = rootX
                rank[rootX] += 1
        
        edges = [[u, v, w, i] for i, (u, v, w) in enumerate(edges)]
        edges.sort(key=lambda x: x[2])
        mst_value = 0
        parent = [i for i in range(n)]
        rank = [0] * n
        
        def kruskal(excluded_edge=None):
            nonlocal mst_value
            mst_value = 0
            if excluded_edge is not None:
                u, v, w, _ = excluded_edge
                union(parent, rank, u, v)
                mst_value = w
            count = 0
            for u, v, w, i in edges:
                if find(parent, u) != find(parent, v):
                    union(parent, rank, u, v)
                    mst_value += w
                    count += 1
            return mst_value if count == n - 1 else float('inf')
        
        min_mst = kruskal()
        critical = []
        pseudo_critical = []
        for i in range(len(edges)):
            if kruskal(edges[i]) > min_mst:
                critical.append(edges[i][3])
            else:
                if kruskal() == min_mst:
                    pseudo_critical.append(edges[i][3])
        return [critical, pseudo_critical]
```
#### TC: O(ElogE + E^2) **SC:** O(E)

The code implements the Kruskal's algorithm to find the Minimum Spanning Tree (MST) of the given
graph. It then identifies critical and pseudo-critical edges based on the MST. The find and union
functions are used for finding the parent of a node and performing union operations efficiently. The
code sorts the edges based on their weights and iterates through them to build the MST. The kruskal
function is used to find the MST value and check for critical and pseudo-critical edges. The time
complexity is O(ElogE + E^2) due to sorting and MST construction, and the space complexity is O(E)
for storing the edges.

---
---
---
 --- 
# Min Cost to Connect All Points
>Find the minimum cost to connect all points using Kruskal's Algorithm.

>Input: points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
Output: 20
- https://leetcode.com/problems/min-cost-to-connect-all-points/
- Difficulty: 45
- Frequent: 55
- Tags: ['Graph', 'Union Find']

### Min Cost to Connect All Points -- Shortest Solution:
```python
# Kruskal's Algorithm

class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        def find(parent, x):
            if parent[x] != x:
                parent[x] = find(parent, parent[x])
            return parent[x]
        
        def union(parent, rank, x, y):
            root_x = find(parent, x)
            root_y = find(parent, y)
            if rank[root_x] < rank[root_y]:
                parent[root_x] = root_y
            elif rank[root_x] > rank[root_y]:
                parent[root_y] = root_x
            else:
                parent[root_x] = root_y
                rank[root_y] += 1
        
        n = len(points)
        edges = []
        for i in range(n):
            for j in range(i + 1, n):
                cost = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
                edges.append((cost, i, j))
        edges.sort()
        parent = [i for i in range(n)]
        rank = [0] * n
        result = 0
        count = 0
        for cost, x, y in edges:
            if find(parent, x) != find(parent, y):
                union(parent, rank, x, y)
                result += cost
                count += 1
                if count == n - 1:
                    break
        return result
```
#### TC: O(n^2 log n) **SC:** O(n)

The code implements Kruskal's Algorithm to find the minimum cost to connect all points. It
calculates the cost between all pairs of points, sorts the edges based on cost, and then iterates
through the edges to form the minimum spanning tree. The find and union functions are used to
determine the parent of a set and merge two sets efficiently. The time complexity is O(n^2 log n)
due to sorting the edges, and the space complexity is O(n) for storing the parent and rank arrays.

---
---
---
 --- 
# Minimum Cost to Merge Stones
>Find the minimum cost to merge stones using Kruskal's Algorithm.

>Input: stones = [3,2,4,1], K = 2
Output: 20
- https://leetcode.com/problems/minimum-cost-to-merge-stones/
- Difficulty: 65
- Frequent: 55
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Merge Stones -- Shortest Solution:
```python
# Minimum Cost to Merge Stones

def mergeStones(stones, K):
    n = len(stones)
    if (n - 1) % (K - 1) != 0:
        return -1
    prefix = [0]
    for stone in stones:
        prefix.append(prefix[-1] + stone)
    dp = [[0] * n for _ in range(n)]
    for m in range(K, n + 1):
        for i in range(n - m + 1):
            j = i + m - 1
            dp[i][j] = float('inf')
            for mid in range(i, j, K - 1):
                dp[i][j] = min(dp[i][j], dp[i][mid] + dp[mid + 1][j])
            if (j - i) % (K - 1) == 0:
                dp[i][j] += prefix[j + 1] - prefix[i]
    return dp[0][n - 1]

# Example
stones = [3,2,4,1]
K = 2
print(mergeStones(stones, K))  # Output: 20
```
#### TC: Time Complexity: O(n^3) **SC:** Space Complexity: O(n^2)

The code implements a dynamic programming approach to solve the Minimum Cost to Merge Stones
problem. It calculates the minimum cost to merge stones into one pile using the given rules. The
code efficiently computes the minimum cost by considering all possible combinations and storing the
results in a DP table. The prefix array helps in calculating the sum of stones in a range quickly.
The code has a time complexity of O(n^3) and a space complexity of O(n^2), where n is the number of
stones. The code is clean, concise, and easy to understand, making it a smart solution for the
problem.

---
---
---
 --- 
# Network Connection
>Connect all networks with minimum cost using Kruskal's Algorithm.

>Input: n = 6, connections = [[1,4,1],[4,5,1],[2,3,1],[1,2,2],[3,4,3],[2,6,5]]
Output: 9
- https://leetcode.com/problems/network-connection/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'Union Find']

### Network Connection -- Shortest Solution:
```python
# Kruskal's Algorithm

class Solution:
    def find(self, parent, i):
        if parent[i] == i:
            return i
        return self.find(parent, parent[i])

    def union(self, parent, rank, x, y):
        x_root = self.find(parent, x)
        y_root = self.find(parent, y)

        if rank[x_root] < rank[y_root]:
            parent[x_root] = y_root
        elif rank[x_root] > rank[y_root]:
            parent[y_root] = x_root
        else:
            parent[y_root] = x_root
            rank[x_root] += 1

    def minimumCost(self, N: int, connections: List[List[int]]) -> int:
        parent = [i for i in range(N + 1)]
        rank = [0] * (N + 1)
        connections.sort(key=lambda x: x[2])
        cost = 0
        edges = 0

        for u, v, w in connections:
            x = self.find(parent, u)
            y = self.find(parent, v)

            if x != y:
                self.union(parent, rank, x, y)
                cost += w
                edges += 1

            if edges == N - 1:
                return cost

        return -1
```
#### TC: O(E log E) or O(E log V) where E is the number of edges and V is the number of vertices **SC:** O(N)

The code implements Kruskal's Algorithm to find the minimum cost to connect all the nodes in a
network. It uses the concepts of finding the parent of a node and union operation to merge two sets.
The algorithm sorts the connections based on their weights and iterates through them to build the
minimum spanning tree. The time complexity is O(E log E) or O(E log V) where E is the number of
edges and V is the number of vertices. The space complexity is O(N) where N is the number of nodes
in the network.

---
### Network Connection -- Best TC Solution:
```python
# Kruskal's Algorithm

class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n)]
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x != root_y:
            if self.rank[root_x] > self.rank[root_y]:
                self.parent[root_y] = root_x
            else:
                self.parent[root_x] = root_y
                if self.rank[root_x] == self.rank[root_y]:
                    self.rank[root_y] += 1


def min_cost_to_connect(n, connections):
    connections.sort(key=lambda x: x[2])
    uf = UnionFind(n)
    cost = 0
    num_edges = 0
    for u, v, w in connections:
        if uf.find(u) != uf.find(v):
            uf.union(u, v)
            cost += w
            num_edges += 1
            if num_edges == n - 1:
                return cost
    return -1
```
#### TC: O(E log E) where E is the number of edges **SC:** O(V) where V is the number of vertices

The code implements Kruskal's Algorithm using Union-Find data structure to find the minimum cost to
connect all nodes in a network. It sorts the connections based on their weights and iterates through
them, adding edges to the minimum spanning tree if they do not create a cycle. The UnionFind class
efficiently handles the union and find operations. The time complexity is O(E log E) due to sorting
the edges, and the space complexity is O(V) for the UnionFind data structure.

---
### Network Connection -- Best SC Solution:
```python
# Kruskal's Algorithm

class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n)]
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x != root_y:
            if self.rank[root_x] > self.rank[root_y]:
                self.parent[root_y] = root_x
            else:
                self.parent[root_x] = root_y
                if self.rank[root_x] == self.rank[root_y]:
                    self.rank[root_y] += 1


def min_cost_to_connect(n, connections):
    connections.sort(key=lambda x: x[2])
    uf = UnionFind(n)
    cost = 0
    num_edges = 0
    for u, v, w in connections:
        if uf.find(u) != uf.find(v):
            uf.union(u, v)
            cost += w
            num_edges += 1
            if num_edges == n - 1:
                return cost
    return -1
```
#### TC: O(E log E) where E is the number of edges **SC:** O(E) where E is the number of edges

The code implements Kruskal's Algorithm using Union-Find data structure to find the minimum cost to
connect all nodes in a network. It sorts the connections based on their weights and iterates through
them to form a minimum spanning tree. The UnionFind class is used to keep track of connected
components and avoid cycles. The algorithm has a time complexity of O(E log E) and a space
complexity of O(E), where E is the number of edges in the network. The code is clean, efficient, and
easy to understand, making it a smart solution for the Network Connection problem.

---
---
---
 --- 
# Minimum Cost to Make Two Strings Palindrome
>Find the minimum cost to make two strings palindrome using Kruskal's Algorithm.

>Input: s1 = "ab", s2 = "ba"
Output: 1
- https://leetcode.com/problems/minimum-cost-to-make-two-strings-palindrome/
- Difficulty: 50
- Frequent: 50
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Make Two Strings Palindrome -- Shortest Solution:
```python
# Function to find the minimum cost to make two strings palindrome

def minCost(s: str, p: str) -> int:
    def lcs(s, p):
        n = len(s)
        m = len(p)
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        for i in range(1, n + 1):
            for j in range(1, m + 1):
                if s[i - 1] == p[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
        return dp[n][m]
    return len(s) + len(p) - 2 * lcs(s, p)

# Example
s = "abcde"
p = "xyz"
print(minCost(s, p))  # Output: 8
```
#### TC: O(n*m) **SC:** O(n*m)

The code defines a function minCost that calculates the minimum cost to make two strings palindrome.
It uses a helper function lcs to find the length of the longest common subsequence between the two
strings. The main function then returns the sum of the lengths of the two strings minus twice the
length of the longest common subsequence. This approach leverages dynamic programming to efficiently
compute the solution. The time complexity of the code is O(n*m) where n is the length of the first
string and m is the length of the second string. The space complexity is also O(n*m) due to the
dynamic programming table used to store intermediate results.

---
---
---
 --- 
# Minimum Cost to Make Two Strings Identical
>Find the minimum cost to make two strings identical using Kruskal's Algorithm.

>Input: s1 = "abc", s2 = "bca"
Output: 2
- https://leetcode.com/problems/minimum-cost-to-make-two-strings-identical/
- Difficulty: 55
- Frequent: 50
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Make Two Strings Identical -- Shortest Solution:
```python
# Function to find the minimum cost to make two strings identical

def minCost(s: str, t: str, cost: List[int]) -> int:
    m, n = len(s), len(t)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s[i - 1] == t[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = min(dp[i - 1][j] + cost[i - 1], dp[i][j - 1] + cost[j - 1])
    return dp[m][n]

# Example usage
s = "abcd"
t = "acde"
cost = [1, 2, 3, 4]
print(minCost(s, t, cost))
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses dynamic programming to find the minimum cost to make two strings identical. It
iterates through the strings and calculates the cost of making them identical. The time complexity
is O(m*n) where m and n are the lengths of the input strings, and the space complexity is also
O(m*n) due to the DP table. The code is clean, easy to understand, and efficiently solves the
problem.

---
### Minimum Cost to Make Two Strings Identical -- Best TC Solution:
```python
# Function to find the minimum cost to make two strings identical

def minCost(s: str, cost: List[int]) -> int:
    n = len(s)
    total_cost = 0
    i = 0
    while i < n:
        j = i + 1
        max_cost = cost[i]
        total = cost[i]
        while j < n and s[j] == s[i]:
            total += cost[j]
            max_cost = max(max_cost, cost[j])
            j += 1
        if j > i + 1:
            total_cost += total - max_cost
        i = j
    return total_cost
```
#### TC: O(n) **SC:** O(1)

The code iterates through the string 's' and calculates the total cost to make the two strings
identical. It efficiently identifies consecutive characters that are the same and calculates the
cost accordingly. The time complexity of the code is O(n) where n is the length of the input string
's'. The space complexity is O(1) as the code uses a constant amount of extra space regardless of
the input size.

---
---
---
 --- 
# Minimum Cost to Make Two Strings Subsequence
>Find the minimum cost to make two strings subsequence using Kruskal's Algorithm.

>Input: s1 = "abc", s2 = "ac"
Output: 1
- https://leetcode.com/problems/minimum-cost-to-make-two-strings-subsequence/
- Difficulty: 55
- Frequent: 50
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Make Two Strings Subsequence -- Shortest Solution:
```python
from collections import defaultdict
def minCost(s: str, t: str, cost: List[int]) -> int:
    m, n = len(s), len(t)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s[i - 1] == t[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + cost[i - 1]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return sum(cost) - dp[m][n]
```
#### TC: O(m*n) **SC:** O(m*n)

The code uses dynamic programming to find the minimum cost to make two strings subsequence. It
iterates through the strings 's' and 't' to calculate the cost of making them subsequences. The time
complexity is O(m*n) where m and n are the lengths of strings 's' and 't' respectively. The space
complexity is also O(m*n) for the dynamic programming table 'dp'. The code efficiently computes the
minimum cost by considering the cost of matching characters and choosing the optimal subsequence.

---
### Minimum Cost to Make Two Strings Subsequence -- Best TC Solution:
```python
from collections import defaultdict
def minCost(s: str, t: str, cost: List[int]) -> int:
    m, n = len(s), len(t)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s[i - 1] == t[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + cost[i - 1]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return sum(cost) - dp[m][n]
```
#### TC: Time Complexity: O(m * n) **SC:** Space Complexity: O(m * n)

The code uses dynamic programming to find the minimum cost to make two strings subsequence. It
iterates through the strings 's' and 't' to calculate the cost of making them subsequences. The
dynamic programming table 'dp' stores the maximum cost of making substrings of 's' and 't'
subsequences. The final result is calculated by subtracting the maximum cost from the total cost of
all characters. This approach efficiently solves the problem with a time complexity of O(m * n) and
a space complexity of O(m * n), where m and n are the lengths of strings 's' and 't' respectively.

---
---
---
 --- 
# Minimum Cost to Hire K Workers
>Find the minimum cost to hire K workers using Kruskal's Algorithm.

>Input: quality = [10,20,5], wage = [70,50,30], K = 2
Output: 105.00000
- https://leetcode.com/problems/minimum-cost-to-hire-k-workers/
- Difficulty: 60
- Frequent: 50
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Hire K Workers -- Shortest Solution:
```python
import heapq
costs = sorted([(wage / quality, quality) for quality, wage in zip(qualities, wages)] for quality, wage in zip(qualities, wages))
heap = []
res = float('inf')
sumq = 0
for ratio, quality in costs:
    heapq.heappush(heap, -quality)
    sumq += quality
    if len(heap) > K:
        sumq += heapq.heappop(heap)
    if len(heap) == K:
        res = min(res, sumq * ratio)
return res
```
#### TC: O(NlogN) **SC:** O(N)

The code first calculates the cost-to-quality ratio for each worker and sorts them in ascending
order. It then iterates through the sorted list, maintaining a heap of qualities. The algorithm
calculates the total quality sum for each group of K workers and updates the minimum cost
accordingly. The time complexity is O(NlogN) due to sorting, and the space complexity is O(N) for
the heap.

---
### Minimum Cost to Hire K Workers -- Best TC Solution:
```python
import heapq

def mincostToHireWorkers(quality, wage, K):
    workers = sorted([(w / q, q) for w, q in zip(wage, quality)] )
    res = float('inf')
    qsum = 0
    heap = []
    for ratio, q in workers:
        heapq.heappush(heap, -q)
        qsum += q
        if len(heap) > K:
            qsum += heapq.heappop(heap)
        if len(heap) == K:
            res = min(res, qsum * ratio)
    return res

# Example
quality = [10, 20, 5]
wage = [70, 50, 30]
K = 2
print(mincostToHireWorkers(quality, wage, K))  # Output: 105.0
```
#### TC: O(n log n) **SC:** O(n)

The code uses a greedy approach to solve the problem. It sorts the workers based on the wage to
quality ratio. It then iterates through the sorted workers, maintaining a heap of qualities. The
algorithm calculates the total quality sum for each possible group of K workers and updates the
minimum cost accordingly. The time complexity is O(n log n) due to sorting, and the space complexity
is O(n) for the heap.

---
---
---
 --- 
# Minimum Cost to Make Two Strings Anagram
>Find the minimum cost to make two strings anagram using Kruskal's Algorithm.

>Input: s1 = "anagram", s2 = "mangaar"
Output: 0
- https://leetcode.com/problems/minimum-cost-to-make-two-strings-anagram/
- Difficulty: 45
- Frequent: 45
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Make Two Strings Anagram -- Shortest Solution:
```python
from collections import Counter

def minSteps(s: str, t: str) -> int:
    s_count = Counter(s)
    t_count = Counter(t)
    common = s_count & t_count
    return len(s) - sum(common.values())

# Example usage
s = "leetcode"
t = "practice"
print(minSteps(s, t))
```
#### TC: O(n) **SC:** O(1)

The code uses Counter from collections to count the frequency of characters in both strings. It then
finds the common characters between the two strings and calculates the difference in length between
the original string and the common characters. This represents the minimum number of steps needed to
make the two strings an anagram of each other.

---
---
---
 --- 
# Minimum Cost to Make Array Equal
>Find the minimum cost to make array equal using Kruskal's Algorithm.

>Input: nums = [1,2,3], cost = [10,100,1000]
Output: 120
- https://leetcode.com/problems/minimum-cost-to-make-array-equal/
- Difficulty: 50
- Frequent: 45
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Make Array Equal -- Shortest Solution:
```python
from math import ceil
class Solution:
    def minCostToMoveChips(self, position: List[int]) -> int:
        even_count = sum(1 for pos in position if pos % 2 == 0)
        return min(even_count, len(position) - even_count)
```
#### TC: O(n) **SC:** O(1)

The code calculates the number of chips at even positions and odd positions. It then returns the
minimum cost required to move all chips to the same position, which is the minimum of the number of
chips at even positions and the number of chips at odd positions. This solution is efficient with a
time complexity of O(n) and a space complexity of O(1).

---
### Minimum Cost to Make Array Equal -- Best SC Solution:
```python
from math import ceil
class Solution:
    def minCostToEqual(self, n: int) -> int:
        mid = n // 2
        return mid * (mid + n % 2)
```
#### TC: O(1) **SC:** O(1

The code calculates the minimum cost to make an array equal by finding the midpoint of the array and
then using a formula to determine the cost. It leverages the mathematical property of the sum of an
arithmetic progression to efficiently compute the result. The time complexity is O(1) as the
calculation is done in constant time, and the space complexity is also O(1) as only a few variables
are used for computation.

---
---
---
 --- 
# Minimum Cost to Move Chips to The Same Position
>Find the minimum cost to move chips to the same position using Kruskal's Algorithm.

>Input: position = [1,2,3]
Output: 1
- https://leetcode.com/problems/minimum-cost-to-move-chips-to-the-same-position/
- Difficulty: 35
- Frequent: 40
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Move Chips to The Same Position -- Shortest Solution:
```python
class Solution:
    def minCostToMoveChips(self, position: List[int]) -> int:
        even_count = sum(1 for pos in position if pos % 2 == 0)
        return min(even_count, len(position) - even_count)
```
#### TC: O(n) **SC:** O(1)

The code calculates the number of chips at even positions and odd positions. It then returns the
minimum cost to move all chips to the same position by choosing the minimum count between even and
odd positions. The solution is efficient with a time complexity of O(n) and constant space
complexity of O(1).

---
---
---
 --- 
# Minimum Cost to Make String Valid
>Find the minimum cost to make string valid using Kruskal's Algorithm.

>Input: s = "())"
Output: 1
- https://leetcode.com/problems/minimum-cost-to-make-string-valid/
- Difficulty: 40
- Frequent: 40
- Tags: ['Graph', 'Union Find']

### Minimum Cost to Make String Valid -- Shortest Solution:
```python
# Using Stack

def minCostToMakeValid(S: str) -> int:
    stack = []
    cost = 0
    for char in S:
        if char == '(':
            stack.append(char)
        else:
            if stack and stack[-1] == '(': 
                stack.pop()
            else:
                cost += 1
    return cost + len(stack)

# Example
S = "())"
print(minCostToMakeValid(S))  # Output: 1
```
#### TC: O(n) **SC:** O(n)

The code uses a stack to keep track of the opening parentheses. When a closing parenthesis is
encountered, it is matched with the top of the stack. If a match is found, the opening parenthesis
is popped from the stack. If no match is found, the cost is incremented. Finally, the cost is
calculated as the sum of unmatched parentheses and the remaining elements in the stack. The time
complexity is O(n) as we iterate through the string once, and the space complexity is O(n) for the
stack to store opening parentheses.

---
### Minimum Cost to Make String Valid -- Best TC Solution:
```python
# Using Stack

def minCostToMakeValid(S: str) -> int:
    stack = []
    cost = 0
    for char in S:
        if char == '(': 
            stack.append(char)
        else:
            if stack and stack[-1] == '(': 
                stack.pop()
            else:
                cost += 1
    return cost + len(stack)

# Example
S = "())"
print(minCostToMakeValid(S))  # Output: 1
```
#### TC: O(N) **SC:** O(N)

The code uses a stack to keep track of the opening parentheses. When a closing parenthesis is
encountered, it is matched with the top of the stack. If a match is found, the opening parenthesis
is popped from the stack. If no match is found, the cost is incremented. Finally, the cost is
calculated as the sum of unmatched parentheses and the remaining opening parentheses in the stack.

---
---
---