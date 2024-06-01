# Union Find
## Frequent score for this algorithmic framework: 50 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Longest Consecutive Sequence](#longest-consecutive-sequence) | 75 | 55 |
| 2 | [Accounts Merge](#accounts-merge) | 70 | 60 |
| 3 | [Redundant Connection](#redundant-connection) | 65 | 40 |
| 4 | [Friend Circles](#friend-circles) | 55 | 50 |
| 5 | [Minimum Cost to Connect Sticks](#minimum-cost-to-connect-sticks) | 55 | 50 |
| 6 | [Earliest Moment When Everyone Become Friends](#earliest-moment-when-everyone-become-friends) | 55 | 60 |
| 7 | [Sentence Similarity II](#sentence-similarity-ii) | 50 | 55 |
| 8 | [Smallest String With Swaps](#smallest-string-with-swaps) | 50 | 60 |
| 9 | [Similar String Groups](#similar-string-groups) | 50 | 60 |
| 10 | [Swim in Rising Water](#swim-in-rising-water) | 50 | 65 |
| 11 | [Connecting Cities With Minimum Cost](#connecting-cities-with-minimum-cost) | 50 | 65 |
| 12 | [Find the City With the Smallest Number of Neighbors at a Threshold Distance](#find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance) | 45 | 60 |
| 13 | [Optimize Water Distribution in a Village](#optimize-water-distribution-in-a-village) | 45 | 70 |
| 14 | [Bricks Falling When Hit](#bricks-falling-when-hit) | 45 | 70 |
---
Union Find, also known as Disjoint Set Union (DSU), is a data structure that keeps track of a partition of a set into disjoint (non-overlapping) subsets. It supports two main operations: union, which merges two subsets into a single subset, and find, which identifies the subset a particular element is in. This is useful for network connectivity, image processing, and more.
```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [1] * size

    def find(self, p):
        if self.parent[p] != p:
            self.parent[p] = self.find(self.parent[p])  # Path compression
        return self.parent[p]

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP != rootQ:
            if self.rank[rootP] > self.rank[rootQ]:
                self.parent[rootQ] = rootP
            elif self.rank[rootP] < self.rank[rootQ]:
                self.parent[rootP] = rootQ
            else:
                self.parent[rootQ] = rootP
                self.rank[rootP] += 1

# Example usage
uf = UnionFind(10)
uf.union(1, 2)
uf.union(3, 4)
print(uf.find(1))  # Output: 1
print(uf.find(2))  # Output: 1
print(uf.find(3))  # Output: 3
print(uf.find(4))  # Output: 3
```
### Insight:
1. Path compression in the find operation flattens the structure of the tree, ensuring that future queries are faster.
2. Union by rank ensures that the smaller tree is always added under the root of the larger tree, keeping the tree flat.
3. The combination of these two techniques ensures that the operations are nearly constant time.
4. This data structure is particularly useful in scenarios where you need to dynamically connect and query the connected components.
---
 --- 
# Longest Consecutive Sequence
>Find the length of the longest consecutive elements sequence.

>Input: nums = [100, 4, 200, 1, 3, 2]
Output: 4
- https://leetcode.com/problems/longest-consecutive-sequence/
- Difficulty: 55
- Frequent: 75
- Tags: ['Union Find', 'Array']

### Longest Consecutive Sequence -- Shortest Solution:
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        num_set = set(nums)
        longest_streak = 0
        for num in num_set:
            if num - 1 not in num_set:
                current_num = num
                current_streak = 1
                while current_num + 1 in num_set:
                    current_num += 1
                    current_streak += 1
                longest_streak = max(longest_streak, current_streak)
        return longest_streak
```
#### TC: O(n) **SC:** O(n)

The code uses a set to store the numbers for O(1) average-time complexity lookups. It iterates
through each number and checks if it's the start of a sequence (i.e., num - 1 is not in the set). If
it is, it counts the length of the sequence by incrementing the number and checking if the next
number is in the set. The longest sequence length is tracked and returned.

---
---
---
 --- 
# Accounts Merge
>Merge accounts with common email addresses.

>Input: accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
Output: [["John", "john00@mail.com", "john_newyork@mail.com", "johnsmith@mail.com"], ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
- https://leetcode.com/problems/accounts-merge/
- Difficulty: 60
- Frequent: 70
- Tags: ['Union Find', 'Depth-First Search']

### Accounts Merge -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, u):
        if u != self.parent[u]:
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
    def accountsMerge(self, accounts):
        email_to_id = {}
        uf = UnionFind(len(accounts))
        for i, account in enumerate(accounts):
            for email in account[1:]:
                if email in email_to_id:
                    uf.union(i, email_to_id[email])
                email_to_id[email] = i
        id_to_emails = collections.defaultdict(list)
        for email, id in email_to_id.items():
            root_id = uf.find(id)
            id_to_emails[root_id].append(email)
        return [[accounts[i][0]] + sorted(emails) for i, emails in id_to_emails.items()]
```
#### TC: O(N * 
E * log(E)) **SC:** O(N * E)

1. **Union-Find Data Structure**: This is used to efficiently perform union and find operations. It
helps in grouping emails that belong to the same account. 2. **Mapping Emails to Account IDs**: We
map each email to an account ID. If an email is already mapped, we union the current account ID with
the previously mapped account ID. 3. **Grouping Emails by Root Account ID**: After processing all
emails, we group them by their root account ID using the union-find structure. 4. **Constructing the
Result**: Finally, we construct the result by combining the account name with the sorted list of
emails for each group.

---
### Accounts Merge -- Best TC Solution:
```python
# Function to perform accounts merge

def accountsMerge(accounts):
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    
    def union(x, y):
        parent[find(x)] = find(y)
    
    parent = {}
    email_to_name = {}
    
    for account in accounts:
        name = account[0]
        for email in account[1:]:
            if email not in parent:
                parent[email] = email
            email_to_name[email] = name
            union(email, account[1])
    
    merged_accounts = {}
    for email in parent:
        root = find(email)
        if root not in merged_accounts:
            merged_accounts[root] = []
        merged_accounts[root].append(email)
    
    return [[email_to_name[root]] + sorted(emails) for root, emails in merged_accounts.items()]

# Example
accounts = [
    ["John", "johnsmith@mail.com", "john00@mail.com"],
    ["John", "johnnybravo@mail.com"],
    ["John", "johnsmith@mail.com", "john_newyork@mail.com"],
    ["Mary", "mary@mail.com"]
]

print(accountsMerge(accounts))
```
#### TC: O(N log N) **SC:** O(N)

The code uses the Union Find approach to merge accounts efficiently. It iterates through each
account and connects emails to the same parent. By using path compression and union by rank, it
optimizes the find and union operations. The code then organizes the merged accounts and returns the
final result. The time complexity is O(N log N) due to the sorting operation, and the space
complexity is O(N) to store the parent and email mappings.

---
### Accounts Merge -- Best SC Solution:
```python
# Function to perform accounts merge

def accountsMerge(accounts):
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    
    def union(x, y):
        parent[find(x)] = find(y)
    
    parent = {}
    email_to_name = {}
    
    for account in accounts:
        name = account[0]
        for email in account[1:]:
            if email not in parent:
                parent[email] = email
            email_to_name[email] = name
            union(email, account[1])
    
    merged_accounts = {}
    for email in parent:
        root = find(email)
        if root not in merged_accounts:
            merged_accounts[root] = []
        merged_accounts[root].append(email)
    
    return [[email_to_name[root]] + sorted(emails) for root, emails in merged_accounts.items()]
```
#### TC: O(n * α(n)) **SC:** O(n)

The code uses the Union Find approach to merge accounts efficiently. It utilizes the find and union
functions to find the root of each email and merge accounts accordingly. The code maintains a parent
dictionary to keep track of the root of each email and an email_to_name dictionary to store the
mapping of email to name. Finally, it constructs the merged_accounts dictionary to group emails by
their root and returns the merged accounts in the required format.

---
---
---
 --- 
# Redundant Connection
>Find the redundant connection in a graph.

>Input: edges = [[1,2], [1,3], [2,3]]
Output: [2,3]
- https://leetcode.com/problems/redundant-connection/
- Difficulty: 40
- Frequent: 65
- Tags: ['Union Find', 'Graph']

### Redundant Connection -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, size):
        self.root = list(range(size))
        self.rank = [1] * size

    def find(self, x):
        if x == self.root[x]:
            return x
        self.root[x] = self.find(self.root[x])
        return self.root[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.root[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.root[rootX] = rootY
            else:
                self.root[rootY] = rootX
                self.rank[rootX] += 1

class Solution:
    def findRedundantConnection(self, edges):
        uf = UnionFind(len(edges) + 1)
        for u, v in edges:
            if uf.find(u) == uf.find(v):
                return [u, v]
            uf.union(u, v)
```
#### TC: O(N * α(N)) **SC:** O(N)

The code uses the Union-Find data structure to detect a cycle in an undirected graph. The
`UnionFind` class manages the connected components. The `find` method uses path compression to
flatten the structure, making future queries faster. The `union` method uses union by rank to keep
the tree shallow. The `findRedundantConnection` method iterates through each edge and uses the
Union-Find structure to check if adding the edge would form a cycle. If it does, that edge is the
redundant connection.

---
### Redundant Connection -- Best TC Solution:
```python
# Union Find Approach

class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n + 1)]

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x == root_y:
            return [x, y]
        self.parent[root_x] = root_y


def find_redundant_connection(edges):
    n = len(edges)
    uf = UnionFind(n)
    for edge in edges:
        x, y = edge
        if uf.union(x, y):
            return edge
```
#### TC: O(N * alpha(N)) **SC:** O(N)

The code implements the Union Find algorithm to find the redundant connection in a graph. The
UnionFind class is used to keep track of the parent nodes and perform union operations efficiently.
The find method recursively finds the root parent of a node, while the union method merges two sets
by updating the parent pointers. The find_redundant_connection function initializes the UnionFind
data structure and iterates through the edges to find the redundant connection. The time complexity
is O(N * alpha(N)), where N is the number of edges and alpha(N) is the inverse Ackermann function,
which is very slow-growing. The space complexity is O(N) to store the parent array.

---
---
---
 --- 
# Friend Circles
>Find the number of friend circles in a given matrix.

>Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
- https://leetcode.com/problems/friend-circles/
- Difficulty: 50
- Frequent: 55
- Tags: ['Union Find', 'Depth-First Search']

### Friend Circles -- Shortest Solution:
```python
class Solution:
    def findCircleNum(self, M: List[List[int]]) -> int:
        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
        def union(x, y):
            rootX = find(x)
            rootY = find(y)
            if rootX != rootY:
                parent[rootX] = rootY
        n = len(M)
        parent = list(range(n))
        for i in range(n):
            for j in range(i + 1, n):
                if M[i][j] == 1:
                    union(i, j)
        return len(set(find(x) for x in range(n)))
```
#### TC: O(n^2) **SC:** O(n)

The solution uses the Union-Find algorithm to efficiently group friends into circles. The `find`
function locates the root of a node, applying path compression for efficiency. The `union` function
merges two sets if they have different roots. The main loop iterates through the upper triangle of
the matrix to avoid redundant checks, performing unions for each pair of friends. Finally, the
number of unique roots gives the number of friend circles.

---
### Friend Circles -- Best TC Solution:
```python
from typing import List

class Solution:
    def findCircleNum(self, M: List[List[int]) -> int:
        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(x, y):
            root_x = find(x)
            root_y = find(y)
            if root_x != root_y:
                parent[root_x] = root_y

        n = len(M)
        parent = [i for i in range(n)]
        for i in range(n):
            for j in range(i + 1, n):
                if M[i][j] == 1:
                    union(i, j)

        circles = set()
        for i in range(n):
            circles.add(find(i))

        return len(circles)
```
#### TC: O(n^2) where n is the number of students **SC:** O(n) where n is the number of students

The code uses the Union Find approach to find friend circles in a given matrix. It iterates through
the matrix to union friends and find the root parent. Finally, it counts the number of unique root
parents to determine the number of friend circles. The time complexity is O(n^2) due to the nested
loops, and the space complexity is O(n) for the parent array.

---
---
---
 --- 
# Minimum Cost to Connect Sticks
>Find the minimum cost to connect all sticks into one.

>Input: sticks = [2,4,3]
Output: 14
- https://leetcode.com/problems/minimum-cost-to-connect-sticks/
- Difficulty: 50
- Frequent: 55
- Tags: ['Union Find', 'Heap']

### Minimum Cost to Connect Sticks -- Shortest Solution:
```python
import heapq

def connectSticks(sticks):
    heapq.heapify(sticks)
    total_cost = 0
    while len(sticks) > 1:
        cost = heapq.heappop(sticks) + heapq.heappop(sticks)
        total_cost += cost
        heapq.heappush(sticks, cost)
    return total_cost
```
#### TC: O(n log n) **SC:** O(n)

1. The problem is about connecting sticks with the minimum cost, where the cost is the sum of the
sticks being connected. 2. A min-heap (priority queue) is used to always connect the two smallest
sticks first, ensuring the minimum cost. 3. The `heapq` library in Python provides an efficient way
to maintain the heap property. 4. The total cost is accumulated by repeatedly popping the two
smallest elements, summing them, and pushing the result back into the heap until only one stick
remains.

---
---
---
 --- 
# Earliest Moment When Everyone Become Friends
>Find the earliest moment when everyone becomes friends.

>Input: n = 6, logs = [[20190101,0,1],[20190104,3,4],[20190107,2,3],[20190211,1,5],[20190224,2,4],[20190301,0,3],[20190312,1,2],[20190322,4,5]]
Output: 20190301
- https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/
- Difficulty: 60
- Frequent: 55
- Tags: ['Union Find', 'Graph']

### Earliest Moment When Everyone Become Friends -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [1] * size
        self.components = size

    def find(self, p):
        if self.parent[p] != p:
            self.parent[p] = self.find(self.parent[p])
        return self.parent[p]

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP != rootQ:
            if self.rank[rootP] > self.rank[rootQ]:
                self.parent[rootQ] = rootP
            elif self.rank[rootP] < self.rank[rootQ]:
                self.parent[rootP] = rootQ
            else:
                self.parent[rootQ] = rootP
                self.rank[rootP] += 1
            self.components -= 1

class Solution:
    def earliestAcq(self, logs, n):
        logs.sort()
        uf = UnionFind(n)
        for t, u, v in logs:
            uf.union(u, v)
            if uf.components == 1:
                return t
        return -1
```
#### TC: O(N log N) **SC:** O(N)

The solution uses the Union-Find data structure to efficiently manage and merge groups of friends.
The logs are sorted by time, and for each log entry, the union operation is performed to merge the
groups of friends. The process continues until all individuals are in the same group, at which point
the current time is returned. If it's not possible to merge all individuals into one group, -1 is
returned.

---
---
---
 --- 
# Sentence Similarity II
>Determine if two sentences are similar.

>Input: words1 = ["great","acting","skills"], words2 = ["fine","drama","talent"], pairs = [["great","good"],["fine","good"],["acting","drama"],["skills","talent"]]
Output: true
- https://leetcode.com/problems/sentence-similarity-ii/
- Difficulty: 55
- Frequent: 50
- Tags: ['Union Find', 'String']

### Sentence Similarity II -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            self.parent[rootY] = rootX

class Solution:
    def areSentencesSimilarTwo(self, words1, words2, pairs):
        if len(words1) != len(words2):
            return False
        index = {}
        count = 0
        for w1, w2 in pairs:
            if w1 not in index:
                index[w1] = count
                count += 1
            if w2 not in index:
                index[w2] = count
                count += 1
        uf = UnionFind(count)
        for w1, w2 in pairs:
            uf.union(index[w1], index[w2])
        for w1, w2 in zip(words1, words2):
            if w1 == w2:
                continue
            if w1 not in index or w2 not in index or uf.find(index[w1]) != uf.find(index[w2]):
                return False
        return True
,
```
#### TC: O(P + W) **SC:** O(P + W)

The solution uses the Union-Find data structure to group words that are similar. Each word is
assigned a unique index, and the Union-Find structure is used to connect indices of similar words.
The algorithm then checks if each pair of words in the two sentences are either the same or belong
to the same group. The time and space complexity are both linear with respect to the number of pairs
and words.

---
### Sentence Similarity II -- Best TC Solution:
```python
# Union Find Approach

class Solution:
    def areSentencesSimilarTwo(self, words1: List[str], words2: List[str], pairs: List[List[str]]) -> bool:
        if len(words1) != len(words2):
            return False
        parent = {}
        def find(x):
            if x not in parent:
                parent[x] = x
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
        def union(x, y):
            parent[find(x)] = find(y)
        for w1, w2 in pairs:
            union(w1, w2)
        return all(w1 == w2 or (w1 in parent and w2 in parent and find(w1) == find(w2)) for w1, w2 in zip(words1, words2))
```
#### TC: O(N + P) where N is the number of words and P is the number of pairs **SC:** O(P)

The code uses the Union Find approach to determine if two sentences are similar. It creates a parent
dictionary to store the relationships between words in pairs. The find function recursively finds
the root parent of a word, and the union function merges two sets by updating their parent pointers.
Finally, it checks if each pair of words in the sentences are similar based on their parent roots.
The time complexity is O(N + P) where N is the number of words and P is the number of pairs, and the
space complexity is O(P) for the parent dictionary.

---
---
---
 --- 
# Smallest String With Swaps
>Return the lexicographically smallest string after swaps.

>Input: s = "dcab", pairs = [[0,3],[1,2]]
Output: "bacd"
- https://leetcode.com/problems/smallest-string-with-swaps/
- Difficulty: 60
- Frequent: 50
- Tags: ['Union Find', 'String']

### Smallest String With Swaps -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, size):
        self.root = list(range(size))
        self.rank = [1] * size

    def find(self, x):
        if self.root[x] != x:
            self.root[x] = self.find(self.root[x])
        return self.root[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.root[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.root[rootX] = rootY
            else:
                self.root[rootY] = rootX
                self.rank[rootX] += 1

from collections import defaultdict

def smallestStringWithSwaps(s, pairs):
    uf = UnionFind(len(s))
    for x, y in pairs:
        uf.union(x, y)
    components = defaultdict(list)
    for i in range(len(s)):
        components[uf.find(i)].append(s[i])
    for comp in components.values():
        comp.sort(reverse=True)
    res = []
    for i in range(len(s)):
        res.append(components[uf.find(i)].pop())
    return ''.join(res)
```
#### TC: O((N + E) log N) **SC:** O(N)

1. **Union-Find Data Structure**: This is used to group indices that can be swapped. The `find`
function helps in finding the root of an element, and the `union` function helps in connecting two
elements. 2. **Component Grouping**: After forming the connected components using Union-Find, we
group characters belonging to the same component. 3. **Sorting**: Each component's characters are
sorted in reverse order to facilitate easy construction of the smallest lexicographical string. 4.
**Reconstruction**: Finally, we reconstruct the string by picking the smallest available character
from each component.

---
### Smallest String With Swaps -- Best TC Solution:
```python
# Function to find the smallest string with swaps

def smallestStringWithSwaps(s: str, pairs: List[List[int]]) -> str:
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        root_x = find(x)
        root_y = find(y)
        if rank[root_x] > rank[root_y]:
            parent[root_y] = root_x
        else:
            parent[root_x] = root_y
            if rank[root_x] == rank[root_y]:
                rank[root_y] += 1

    parent = [i for i in range(len(s))]
    rank = [0] * len(s)
    groups = collections.defaultdict(list)

    for x, y in pairs:
        union(x, y)

    for i in range(len(s)):
        groups[find(i)].append(s[i])

    for group in groups.values():
        group.sort(reverse=True)

    res = []
    for i in range(len(s)):
        res.append(groups[find(i)].pop())

    return ''.join(res)
```
#### TC: O(N log N) **SC:** O(N)

The code uses the Union Find approach to group characters that can be swapped together. It then
sorts the characters within each group in reverse order. Finally, it constructs the smallest string
by popping characters from the sorted groups. The time complexity is O(N log N) due to sorting, and
the space complexity is O(N) to store the parent, rank, and groups data structures.

---
---
---
 --- 
# Similar String Groups
>Count the number of similar string groups.

>Input: strs = ["tars","rats","arts","star"]
Output: 2
- https://leetcode.com/problems/similar-string-groups/
- Difficulty: 60
- Frequent: 50
- Tags: ['Union Find', 'String']

### Similar String Groups -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.parent[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.parent[rootX] = rootY
            else:
                self.parent[rootY] = rootX
                self.rank[rootX] += 1

class Solution:
    def numSimilarGroups(self, strs):
        def is_similar(a, b):
            diff = 0
            for x, y in zip(a, b):
                if x != y:
                    diff += 1
                if diff > 2:
                    return False
            return True

        n = len(strs)
        uf = UnionFind(n)
        for i in range(n):
            for j in range(i + 1, n):
                if is_similar(strs[i], strs[j]):
                    uf.union(i, j)

        return len(set(uf.find(i) for i in range(n)))
```
#### TC: O(n^2 * m) **SC:** O(n)

The solution uses the Union-Find data structure to group similar strings. Two strings are considered
similar if they differ by at most two characters. The `is_similar` function checks this condition.
The Union-Find structure helps in efficiently merging and finding groups. The final number of unique
groups is determined by the number of unique roots in the Union-Find structure.

---
### Similar String Groups -- Best TC Solution:
```python
# Union Find Approach

class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n)]
        self.rank = [0] * n
        self.count = n
    
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
            self.count -= 1
    
    def get_count(self):
        return self.count


def num_similar_groups(A):
    def is_similar(a, b):
        diff_count = 0
        for i in range(len(a)):
            if a[i] != b[i]:
                diff_count += 1
                if diff_count > 2:
                    return False
        return True
    
    n = len(A)
    uf = UnionFind(n)
    
    for i in range(n):
        for j in range(i + 1, n):
            if is_similar(A[i], A[j]):
                uf.union(i, j)
    
    return uf.get_count()
```
#### TC: O(N^2 * M) **SC:** O(N)

The code uses the Union Find approach to group similar strings. It defines a UnionFind class to
perform union and find operations efficiently. The num_similar_groups function checks for similarity
between strings and unions them if they are similar. The time complexity is O(N^2 * M) where N is
the number of strings and M is the length of each string. The space complexity is O(N) to store the
parent and rank arrays in the UnionFind class.

---
---
---
 --- 
# Swim in Rising Water
>Find the minimum time to swim from top left to bottom right in a grid.

>Input: grid = [[0,2],[1,3]]
Output: 3
- https://leetcode.com/problems/swim-in-rising-water/
- Difficulty: 65
- Frequent: 50
- Tags: ['Union Find', 'Graph']

### Swim in Rising Water -- Shortest Solution:
```python
import heapq

def swimInWater(grid):
    n = len(grid)
    pq = [(grid[0][0], 0, 0)]
    visited = set((0, 0))
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    while pq:
        t, x, y = heapq.heappop(pq)
        if x == n - 1 and y == n - 1:
            return t
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and (nx, ny) not in visited:
                visited.add((nx, ny))
                heapq.heappush(pq, (max(t, grid[nx][ny]), nx, ny))
```
#### TC: O(n^2 log n) **SC:** O(n^2)

The code uses a priority queue (min-heap) to always expand the least costly path first. It starts
from the top-left corner and explores all possible directions (right, down, left, up). The priority
queue ensures that the path with the minimum maximum height is chosen. The algorithm continues until
it reaches the bottom-right corner of the grid. The use of a set for visited nodes ensures that each
node is processed only once.

---
### Swim in Rising Water -- Best TC Solution:
```python
import heapq

def swimInWater(grid):
    n = len(grid)
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    visited = set()
    pq = [(grid[0][0], 0, 0)]
    time = 0
    while pq:
        t, x, y = heapq.heappop(pq)
        time = max(time, t)
        if x == n - 1 and y == n - 1:
            return time
        visited.add((x, y))
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and (nx, ny) not in visited:
                heapq.heappush(pq, (grid[nx][ny], nx, ny))
    return -1
```
#### TC: O(N^2 log N) **SC:** O(N^2)

The code uses a priority queue to keep track of the cells to visit based on the water level. It
iterates through the cells in increasing order of water levels, updating the maximum time taken to
reach each cell. The algorithm terminates when it reaches the destination cell. The time complexity
is O(N^2 log N) due to the priority queue operations, and the space complexity is O(N^2) to store
the visited cells.

---
---
---
 --- 
# Connecting Cities With Minimum Cost
>Find the minimum cost to connect all cities.

>Input: n = 3, connections = [[1,2,5],[1,3,6],[2,3,1]]
Output: 6
- https://leetcode.com/problems/connecting-cities-with-minimum-cost/
- Difficulty: 65
- Frequent: 50
- Tags: ['Union Find', 'Graph']

### Connecting Cities With Minimum Cost -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, u):
        if u != self.parent[u]:
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
    def minimumCost(self, n, connections):
        uf = UnionFind(n)
        connections.sort(key=lambda x: x[2])
        total_cost = 0
        edges_used = 0
        for u, v, cost in connections:
            if uf.find(u - 1) != uf.find(v - 1):
                uf.union(u - 1, v - 1)
                total_cost += cost
                edges_used += 1
                if edges_used == n - 1:
                    return total_cost
        return -1
```
#### TC: O(E log E + E α(N)) **SC:** O(N)

1. **Union-Find Data Structure**: This is used to efficiently manage and merge disjoint sets. It
helps in detecting cycles and connecting components. 2. **Sorting**: The connections are sorted
based on their cost to ensure that we always consider the smallest available edge, which is crucial
for Kruskal's algorithm. 3. **Kruskal's Algorithm**: This algorithm is used to find the Minimum
Spanning Tree (MST) by adding edges in increasing order of cost and ensuring no cycles are formed.
4. **Edge Counting**: We keep track of the number of edges used. If it reaches `n-1`, we have
successfully connected all cities with the minimum cost. 5. **Early Termination**: The algorithm
terminates early if all cities are connected before considering all edges, optimizing performance.

---
### Connecting Cities With Minimum Cost -- Best TC Solution:
```python
from typing import List

def find(parent, i):
    if parent[i] == i:
        return i
    return find(parent, parent[i])


def union(parent, rank, x, y):
    x_root = find(parent, x)
    y_root = find(parent, y)

    if rank[x_root] < rank[y_root]:
        parent[x_root] = y_root
    elif rank[x_root] > rank[y_root]:
        parent[y_root] = x_root
    else:
        parent[y_root] = x_root
        rank[x_root] += 1


def minimumCost(n: int, connections: List[List[int]]) -> int:
    parent = [i for i in range(n + 1)]
    rank = [0] * (n + 1)
    connections.sort(key=lambda x: x[2])
    cost = 0
    for connection in connections:
        city1, city2, c = connection
        if find(parent, city1) != find(parent, city2):
            union(parent, rank, city1, city2)
            cost += c
    root = find(parent, 1)
    for i in range(2, n + 1):
        if find(parent, i) != root:
            return -1
    return cost
```
#### TC: O(n log n) **SC:** O(n)

The code uses the Union Find algorithm to find the minimum cost to connect all cities. It first
defines helper functions for finding the parent and performing union operation. Then, it sorts the
connections based on cost. It iterates through the connections, unions the cities if they are not
already connected, and calculates the total cost. Finally, it ensures that all cities are connected
to the same root node. The time complexity is O(n log n) due to sorting, and the space complexity is
O(n) for storing parent and rank arrays.

---
---
---
 --- 
# Find the City With the Smallest Number of Neighbors at a Threshold Distance
>Find the city with the smallest number of neighbors within a threshold distance.

>Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3
- https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/
- Difficulty: 60
- Frequent: 45
- Tags: ['Union Find', 'Graph']

### Find the City With the Smallest Number of Neighbors at a Threshold Distance -- Shortest Solution:
```python
import heapq

class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:
        graph = {i: [] for i in range(n)}
        for u, v, w in edges:
            graph[u].append((v, w))
            graph[v].append((u, w))
        
        def dijkstra(start):
            min_heap = [(0, start)]
            distances = {i: float('inf') for i in range(n)}
            distances[start] = 0
            while min_heap:
                current_distance, u = heapq.heappop(min_heap)
                if current_distance > distances[u]:
                    continue
                for v, weight in graph[u]:
                    distance = current_distance + weight
                    if distance < distances[v]:
                        distances[v] = distance
                        heapq.heappush(min_heap, (distance, v))
            return distances
        
        min_count = n
        result_city = 0
        for i in range(n):
            distances = dijkstra(i)
            count = sum(1 for d in distances.values() if d <= distanceThreshold)
            if count <= min_count:
                min_count = count
                result_city = i
        return result_city
```
#### TC: O(n^2 log n + n * m) **SC:** O(n^2)

The code uses Dijkstra's algorithm to find the shortest paths from each city to all other cities. It
then counts the number of reachable cities within the distance threshold for each city and keeps
track of the city with the smallest count. The use of a priority queue (min-heap) ensures that the
algorithm runs efficiently.

---
### Find the City With the Smallest Number of Neighbors at a Threshold Distance -- Best TC Solution:
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

The code uses Floyd-Warshall algorithm to find the shortest path between all pairs of nodes. It
initializes a distance matrix with infinite values, updates it with the given edge weights, and then
iterates to find the shortest path. The result is a dictionary with the number of reachable cities
within the threshold for each city. Finally, it returns the city with the smallest number of
reachable cities.

---
---
---
 --- 
# Optimize Water Distribution in a Village
>Optimize the cost of water distribution in a village.

>Input: n = 3, wells = [1,2,2], pipes = [[1,2,1],[2,3,1]]
Output: 3
- https://leetcode.com/problems/optimize-water-distribution-in-a-village/
- Difficulty: 70
- Frequent: 45
- Tags: ['Union Find', 'Graph']

### Optimize Water Distribution in a Village -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, size):
        self.root = list(range(size))
        self.rank = [1] * size

    def find(self, x):
        if x == self.root[x]:
            return x
        self.root[x] = self.find(self.root[x])
        return self.root[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.root[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.root[rootX] = rootY
            else:
                self.root[rootY] = rootX
                self.rank[rootX] += 1

class Solution:
    def minCostToSupplyWater(self, n, wells, pipes):
        edges = [(cost, 0, i + 1) for i, cost in enumerate(wells)] + [(cost, house1, house2) for house1, house2, cost in pipes]
        edges.sort()
        uf = UnionFind(n + 1)
        total_cost = 0
        for cost, house1, house2 in edges:
            if uf.find(house1) != uf.find(house2):
                uf.union(house1, house2)
                total_cost += cost
        return total_cost
```
#### TC: O(E log E) **SC:** O(E + V)

The solution uses Kruskal's algorithm with a Union-Find data structure to find the Minimum Spanning
Tree (MST). The wells are treated as edges from a virtual node 0 to each house. The edges are sorted
by cost, and the Union-Find structure helps in efficiently finding and unioning sets to avoid
cycles. The total cost is accumulated as edges are added to the MST.

---
### Optimize Water Distribution in a Village -- Best TC Solution:
```python
from typing import List

def minCostToSupplyWater(n: int, wells: List[int], pipes: List[List[int]]) -> int:
    parent = [i for i in range(n + 1)]
    cost = [(c, 0, i) for i, c in enumerate(wells, 1)]
    for u, v, w in pipes:
        cost.append((w, u, v))
    cost.sort()
    res = 0
    for c, u, v in cost:
        pu, pv = find(u), find(v)
        if pu != pv:
            res += c
            parent[pu] = pv
    return res


def find(x: int) -> int:
    if x != parent[x]:
        parent[x] = find(parent[x])
    return parent[x]
```
#### TC: O(nlogn) **SC:** O(n)

The code uses the Union Find approach to solve the problem of optimizing water distribution in a
village. It initializes the parent array and cost array, sorts the costs, and iterates through the
costs to find the minimum cost. The find function is used to find the parent of a node. The time
complexity is O(nlogn) due to sorting, and the space complexity is O(n) for the parent array.

---
---
---
 --- 
# Bricks Falling When Hit
>Count the number of bricks that will fall after each hit.

>Input: grid = [[1,0,0,0],[1,1,1,0]]
Output: [2]
- https://leetcode.com/problems/bricks-falling-when-hit/
- Difficulty: 70
- Frequent: 45
- Tags: ['Union Find', 'Graph']

### Bricks Falling When Hit -- Shortest Solution:
```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [1] * size
        self.size = [1] * size

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.parent[rootY] = rootX
                self.size[rootX] += self.size[rootY]
            elif self.rank[rootX] < self.rank[rootY]:
                self.parent[rootX] = rootY
                self.size[rootY] += self.size[rootX]
            else:
                self.parent[rootY] = rootX
                self.rank[rootX] += 1
                self.size[rootX] += self.size[rootY]

    def getSize(self, x):
        return self.size[self.find(x)]

class Solution:
    def hitBricks(self, grid, hits):
        rows, cols = len(grid), len(grid[0])
        uf = UnionFind(rows * cols + 1)
        def index(r, c):
            return r * cols + c
        for r, c in hits:
            grid[r][c] -= 1
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    if r == 0:
                        uf.union(index(r, c), rows * cols)
                    if r > 0 and grid[r-1][c] == 1:
                        uf.union(index(r, c), index(r-1, c))
                    if c > 0 and grid[r][c-1] == 1:
                        uf.union(index(r, c), index(r, c-1))
        result = []
        for r, c in reversed(hits):
            grid[r][c] += 1
            if grid[r][c] == 1:
                prevSize = uf.getSize(rows * cols)
                if r == 0:
                    uf.union(index(r, c), rows * cols)
                for nr, nc in [(r-1, c), (r+1, c), (r, c-1), (r, c+1)]:
                    if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                        uf.union(index(r, c), index(nr, nc))
                newSize = uf.getSize(rows * cols)
                result.append(max(0, newSize - prevSize - 1))
            else:
                result.append(0)
        return result[::-1]
```
#### TC: O((R*C) + H) **SC:** O(R*C)

1. **Union-Find Data Structure**: This is used to efficiently manage and merge disjoint sets. It
helps in determining the connected components in the grid. 2. **Grid Indexing**: Each cell in the
grid is mapped to a unique index to facilitate union-find operations. 3. **Initial Hit Processing**:
The grid is first updated by virtually removing the bricks as per the hits. This helps in processing
the grid as if the hits never happened. 4. **Union Operations**: After processing the hits, the grid
is traversed to perform union operations for the remaining bricks, ensuring that the top row bricks
are connected to a virtual top node. 5. **Reverse Hit Processing**: The hits are processed in
reverse order to determine the number of bricks that fall after each hit is reverted. The difference
in the size of the connected component before and after reverting a hit gives the number of bricks
that fall.

---
### Bricks Falling When Hit -- Best TC Solution:
```python
from typing import List

class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.count = n
        
    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
        
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x != root_y:
            if self.size[root_x] < self.size[root_y]:
                root_x, root_y = root_y, root_x
            self.parent[root_y] = root_x
            self.size[root_x] += self.size[root_y]
            self.count -= 1
        
    def get_count(self):
        return self.count

class Solution:
    def hitBricks(self, grid: List[List[int]], hits: List[List[int]]) -> List[int]:
        rows, cols = len(grid), len(grid[0])
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        
        def index(x, y):
            return x * cols + y
        
        def is_valid(x, y):
            return 0 <= x < rows and 0 <= y < cols
        
        def union_around(x, y):
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if is_valid(nx, ny) and grid[nx][ny] == 1:
                    uf.union(index(x, y), index(nx, ny))
        
        for x, y in hits:
            grid[x][y] -= 1
        
        uf = UnionFind(rows * cols + 1)
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 1:
                    if i == 0:
                        uf.union(cols * rows, index(i, j))
                    if i > 0 and grid[i - 1][j] == 1:
                        uf.union(index(i, j), index(i - 1, j))
                    if j > 0 and grid[i][j - 1] == 1:
                        uf.union(index(i, j), index(i, j - 1))
        
        res = []
        for x, y in reversed(hits):
            prev_roof = uf.get_count()
            if grid[x][y] == 0:
                continue
            grid[x][y] = 1
            if x == 0:
                uf.union(cols * rows, index(x, y))
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if is_valid(nx, ny) and grid[nx][ny] == 1:
                    uf.union(index(x, y), index(nx, ny))
            current_roof = uf.get_count()
            res.append(max(0, current_roof - prev_roof - 1))
        
        return res[::-1]
```
#### TC: O(N * M + H) **SC:** O(N * M)

The code uses the Union Find data structure to efficiently track the connected components of bricks.
It first simulates the removal of hits by updating the grid values. Then, it initializes the
UnionFind object and connects the bricks in the grid. Finally, it iterates through the reversed
hits, reinstates the bricks, and recalculates the connected components to determine the number of
bricks that fall. The time complexity is O(N * M + H) where N and M are the dimensions of the grid,
and H is the number of hits. The space complexity is O(N * M) to store the UnionFind data structure.

---
### Bricks Falling When Hit -- Best SC Solution:
```python
from typing import List

class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.count = n

    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x != root_y:
            self.parent[root_x] = root_y
            self.size[root_y] += self.size[root_x]
            self.count -= 1

    def get_count(self):
        return self.count


def hitBricks(grid: List[List[int]], hits: List[List[int]]) -> List[int]:
    rows, cols = len(grid), len(grid[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    def index(x, y):
        return x * cols + y

    def is_valid(x, y):
        return 0 <= x < rows and 0 <= y < cols

    def union_around(x, y):
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if is_valid(nx, ny) and grid[nx][ny] == 1:
                uf.union(index(x, y), index(nx, ny))

    def is_connected_to_roof(x, y):
        return x == 0

    for x, y in hits:
        grid[x][y] -= 1

    uf = UnionFind(rows * cols + 1)
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 1:
                if i == 0:
                    uf.union(index(i, j), rows * cols)
                union_around(i, j)

    result = []
    for x, y in reversed(hits):
        prev_roof_bricks = uf.get_count()
        if grid[x][y] == 0:
            continue
        index_curr = index(x, y)
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if is_valid(nx, ny) and grid[nx][ny] == 1:
                uf.union(index_curr, index(nx, ny))
        if x == 0:
            uf.union(index_curr, rows * cols)
        result.append(max(0, uf.get_count() - prev_roof_bricks - 1))
        grid[x][y] = 1

    return result
```
#### TC: O(N*Q + M*N) **SC:** O(N*M)

The code uses the Union Find data structure to efficiently track the connected components of bricks.
It iterates through the grid to connect bricks and simulate hits. The 'hitBricks' function handles
the process of hitting bricks and updating the connected components. The 'UnionFind' class helps in
union operations and counting the remaining connected components. The code optimizes by reversing
the hits list and simulating hits in reverse order to efficiently calculate the number of bricks
that fall after each hit. Overall, the code effectively handles the brick falling scenario with a
reasonable time and space complexity.

---
---
---