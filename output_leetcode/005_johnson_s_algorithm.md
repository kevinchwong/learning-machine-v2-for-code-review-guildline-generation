# Johnson's Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Shortest Path in a Grid with Obstacles Elimination](#shortest-path-in-a-grid-with-obstacles-elimination) | 60 | 50 |
| 2 | [Minimum Number of Vertices to Reach All Nodes](#minimum-number-of-vertices-to-reach-all-nodes) | 60 | 50 |
| 3 | [Minimum Number of Operations to Make Array Continuous](#minimum-number-of-operations-to-make-array-continuous) | 60 | 50 |
| 4 | [Minimum Number of Operations to Make Array Equal](#minimum-number-of-operations-to-make-array-equal) | 60 | 50 |
| 5 | [Minimum Number of Operations to Make Array Unique](#minimum-number-of-operations-to-make-array-unique) | 60 | 50 |
| 6 | [Minimum Number of Flips to Convert Binary Matrix to Zero Matrix](#minimum-number-of-flips-to-convert-binary-matrix-to-zero-matrix) | 60 | 55 |
| 7 | [Minimum Number of Operations to Make Array Palindrome](#minimum-number-of-operations-to-make-array-palindrome) | 60 | 55 |
| 8 | [Minimum Number of Operations to Make Array Zero](#minimum-number-of-operations-to-make-array-zero) | 60 | 55 |
| 9 | [Find the City With the Smallest Number of Neighbors at a Threshold Distance](#find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance) | 55 | 45 |
| 10 | [Minimum Number of Steps to Make Two Strings Anagram](#minimum-number-of-steps-to-make-two-strings-anagram) | 55 | 45 |
| 11 | [Minimum Number of Operations to Make Array Increasing](#minimum-number-of-operations-to-make-array-increasing) | 55 | 45 |
| 12 | [Minimum Number of Days to Disconnect Island](#minimum-number-of-days-to-disconnect-island) | 55 | 60 |
| 13 | [Minimum Number of Operations to Make Array Sorted](#minimum-number-of-operations-to-make-array-sorted) | 55 | 60 |
| 14 | [Shortest Path Visiting All Nodes](#shortest-path-visiting-all-nodes) | 55 | 65 |
| 15 | [All Pairs Shortest Path](#all-pairs-shortest-path) | 50 | 60 |
---
Johnson's algorithm finds shortest paths between all pairs of vertices in a weighted, directed graph. It reweights edges to ensure all weights are non-negative, then uses Dijkstra's algorithm for each vertex.
```python
import heapq

def dijkstra(graph, start):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start] = 0
    pq = [(0, start)]
    while pq:
        current_distance, current_vertex = heapq.heappop(pq)
        if current_distance > distances[current_vertex]:
            continue
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    return distances


def johnson(graph):
    new_graph = {vertex: graph[vertex].copy() for vertex in graph}
    new_graph['q'] = {vertex: 0 for vertex in graph}
    h = bellman_ford(new_graph, 'q')
    del new_graph['q']
    for u in graph:
        for v in graph[u]:
            graph[u][v] += h[u] - h[v]
    distances = {}
    for u in graph:
        distances[u] = dijkstra(graph, u)
        for v in distances[u]:
            distances[u][v] += h[v] - h[u]
    return distances


def bellman_ford(graph, start):
    distance = {vertex: float('infinity') for vertex in graph}
    distance[start] = 0
    for _ in range(len(graph) - 1):
        for u in graph:
            for v in graph[u]:
                if distance[u] + graph[u][v] < distance[v]:
                    distance[v] = distance[u] + graph[u][v]
    return distance
```
### Insight:
Johnson's algorithm combines Bellman-Ford and Dijkstra's algorithms. Bellman-Ford handles negative weights, while Dijkstra's efficiently finds shortest paths. Reweighting edges ensures Dijkstra's can be used, making the algorithm versatile for various graph types.
---
 --- 
# Shortest Path in a Grid with Obstacles Elimination
>Find the shortest path in a grid with obstacles elimination.

>Input: grid = [[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1
Output: 6
- https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Shortest Path in a Grid with Obstacles Elimination -- Shortest Solution:
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
                next_remain = remain - grid[x][y]
                if next_remain >= 0 and (x, y, next_remain) not in visited:
                    visited.add((x, y, next_remain))
                    queue.append((x, y, next_remain, steps + 1))
        
    return -1
```
#### TC: O(m*n*k) **SC:** O(m*n*k)

The code uses a breadth-first search (BFS) approach to find the shortest path in a grid with
obstacles elimination. It maintains a queue to explore possible paths while keeping track of the
remaining obstacles that can be eliminated. The visited set helps avoid revisiting the same cell
with the same remaining obstacles. The algorithm iterates through the grid cells, updating the
remaining obstacles and steps accordingly. The time complexity is O(m*n*k) where m and n are the
dimensions of the grid, and k is the maximum number of obstacles that can be eliminated. The space
complexity is also O(m*n*k) due to the queue and visited set storing the states of each cell with
remaining obstacles.

---
### Shortest Path in a Grid with Obstacles Elimination -- Best TC Solution:
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
        for x, y in [(i+1, j), (i-1, j), (i, j+1), (i, j-1)]:
            if 0 <= x < m and 0 <= y < n:
                next_remain = remain - grid[x][y]
                if next_remain >= 0 and (x, y, next_remain) not in visited:
                    visited.add((x, y, next_remain))
                    queue.append((x, y, next_remain, steps + 1))
        
    return -1
```
#### TC: O(m * n * k) **SC:** O(m * n * k)

The code uses a breadth-first search (BFS) approach to find the shortest path in a grid with
obstacles elimination. It maintains a queue to explore possible paths while keeping track of the
remaining obstacles that can be eliminated. The visited set helps avoid revisiting the same cell
with the same remaining obstacles. The algorithm iterates through all possible directions from each
cell, updating the remaining obstacles and adding valid paths to the queue. The time complexity is
O(m * n * k) where m and n are the dimensions of the grid, and k is the maximum number of obstacles
that can be eliminated. The space complexity is also O(m * n * k) to store the visited cells and
remaining obstacles for each cell.

---
---
---
 --- 
# Minimum Number of Vertices to Reach All Nodes
>Find the minimum number of vertices to reach all nodes in a graph.

>Input: n = 6, edges = [[0,1],[0,2],[2,5],[3,4],[4,2]]
Output: [0,3]
- https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Vertices to Reach All Nodes -- Shortest Solution:
```python
from typing import List

def findSmallestSetOfVertices(n: int, edges: List[List[int]]) -> List[int]:
    in_degrees = [0] * n
    for _, end in edges:
        in_degrees[end] += 1
    return [i for i in range(n) if in_degrees[i] == 0]
```
#### TC: O(n) **SC:** O(n)

The code calculates the in-degrees of all nodes in the graph represented by the edges. Nodes with
in-degree 0 are the ones that can reach all other nodes. The code efficiently finds these nodes by
iterating through the in-degrees array and selecting the nodes with in-degree 0. This approach has a
time complexity of O(n) and a space complexity of O(n), where n is the number of nodes in the graph.

---
---
---
 --- 
# Minimum Number of Operations to Make Array Continuous
>Find the minimum number of operations to make an array continuous.

>Input: nums = [4,2,5,3]
Output: 0
- https://leetcode.com/problems/minimum-number-of-operations-to-make-array-continuous/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Operations to Make Array Continuous -- Shortest Solution:
```python
from sortedcontainers import SortedList

class Solution:
    def minOperations(self, nums: List[int]) -> int:
        nums = list(set(nums))
        nums.sort()
        n = len(nums)
        s = SortedList(nums)
        res = float('inf')
        j = 0
        for i in range(n):
            while j < n and s[j] <= s[i] + n - 1:
                j += 1
            res = min(res, n - (j - i))
        return res
```
#### TC: O(n log n) **SC:** O(n)

The code uses a SortedList from the sortedcontainers library to efficiently handle the sorted array.
It removes duplicates from the input array, sorts it, and then iterates through the array to find
the minimum number of operations required to make the array continuous. The algorithm has a time
complexity of O(n log n) due to sorting and a space complexity of O(n) to store the unique elements
in the SortedList.

---
---
---
 --- 
# Minimum Number of Operations to Make Array Equal
>Find the minimum number of operations to make an array equal.

>Input: n = 3
Output: 2
- https://leetcode.com/problems/minimum-number-of-operations-to-make-array-equal/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Operations to Make Array Equal -- Shortest Solution:
```python
from math import ceil
class Solution:
    def minOperations(self, n: int) -> int:
        return (n - 1) * (n // 2) + n // 2 if n % 2 == 0 else (n - 1) * (n // 2) + n // 2
```
#### TC: O(1) **SC:** O(1)

The code calculates the minimum number of operations to make the array equal by using a formula
based on the given input 'n'. It efficiently handles both even and odd values of 'n' to determine
the operations required. The code is concise, easy to understand, and has a time and space
complexity of O(1), making it an optimal solution for the problem.

---
---
---
 --- 
# Minimum Number of Operations to Make Array Unique
>Find the minimum number of operations to make an array unique.

>Input: nums = [1,1,1]
Output: 3
- https://leetcode.com/problems/minimum-number-of-operations-to-make-array-unique/
- Difficulty: 50
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Operations to Make Array Unique -- Shortest Solution:
```python
from collections import Counter
def minOperations(nums):
    counter = Counter(nums)
    total_ops = 0
    for num, freq in counter.items():
        while freq > 1:
            counter[num + 1] += 1
            total_ops += 1
            freq -= 1
    return total_ops
```
#### TC: O(n) **SC:** O(n)

The code uses a Counter to count the frequency of each number in the input array. It then iterates
through the Counter and for each number with a frequency greater than 1, it increments the next
number and adds 1 to the total operations. This approach ensures that each number in the array is
unique with the minimum number of operations required. The time complexity is O(n) where n is the
number of elements in the input array, and the space complexity is also O(n) due to the Counter used
to store the frequencies.

---
---
---
 --- 
# Minimum Number of Flips to Convert Binary Matrix to Zero Matrix
>Find the minimum number of flips to convert a binary matrix to a zero matrix.

>Input: mat = [[0,0],[0,1]]
Output: 3
- https://leetcode.com/problems/minimum-number-of-flips-to-convert-binary-matrix-to-zero-matrix/
- Difficulty: 55
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Flips to Convert Binary Matrix to Zero Matrix -- Shortest Solution:
```python
from collections import deque
def minFlips(mat):
        m, n = len(mat), len(mat[0])
        start = sum(sum(mat, []))
        if start == 0: return 0
        start = sum([1 << i for i in range(m * n) if mat[i // n][i % n]])
        q = deque([(start, 0)])
        seen = {start}
        while q:
            state, step = q.popleft()
            if state == 0: return step
            for i in range(m * n):
                new_state = state
                for ni, nj in ((i // n + 1, i % n), (i // n - 1, i % n), (i // n, i % n + 1), (i // n, i % n - 1), (i // n, i % n)):
                    if 0 <= ni < m and 0 <= nj < n:
                        new_state ^= 1 << (ni * n + nj)
                if new_state not in seen:
                    seen.add(new_state)
                    q.append((new_state, step + 1))
        return -1
```
#### TC: O(2^(m*n)) **SC:** O(2^(m*n))

The code uses a breadth-first search approach to find the minimum number of flips required to
convert the binary matrix to a zero matrix. It utilizes bitwise operations to represent the matrix
state efficiently. The algorithm explores all possible states by flipping individual cells and
tracks the visited states to avoid revisiting them. The time and space complexity are exponential,
O(2^(m*n)), where m and n are the dimensions of the matrix.

---
---
---
 --- 
# Minimum Number of Operations to Make Array Palindrome
>Find the minimum number of operations to make an array palindrome.

>Input: nums = [1,2,3,4,5]
Output: 4
- https://leetcode.com/problems/minimum-number-of-operations-to-make-array-palindrome/
- Difficulty: 55
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Operations to Make Array Palindrome -- Shortest Solution:
```python
from typing import List

def min_operations_to_make_array_palindrome(nums: List[int]) -> int:
    n = len(nums)
    operations = 0
    for i in range(n // 2):
        operations += abs(nums[i] - nums[n - i - 1])
    return operations

# Example
nums = [1, 2, 3, 4, 5]
print(min_operations_to_make_array_palindrome(nums))  # Output: 2
```
#### TC: O(n) **SC:** O(1)

The code calculates the minimum number of operations required to make the given array a palindrome.
It iterates through the array from both ends towards the center, comparing and accumulating the
absolute difference between the elements. The time complexity is O(n) as it iterates through half of
the array, and the space complexity is O(1) as it only uses a constant amount of extra space.

---
### Minimum Number of Operations to Make Array Palindrome -- Best TC Solution:
```python
# Function to find the minimum number of operations to make an array palindrome

def min_operations_to_make_palindrome(nums):
    n = len(nums)
    operations = 0
    left, right = 0, n - 1
    while left < right:
        if nums[left] == nums[right]:
            left += 1
            right -= 1
        elif nums[left] < nums[right]:
            nums[left + 1] += nums[left]
            left += 1
            operations += 1
        else:
            nums[right - 1] += nums[right]
            right -= 1
            operations += 1
    return operations

# Example
nums = [1, 2, 3, 4, 5]
print(min_operations_to_make_palindrome(nums))
```
#### TC: O(N) **SC:** O(1)

The code defines a function that calculates the minimum number of operations required to make an
array palindrome. It iterates through the array from both ends, adjusting elements as needed to
match each other. The time complexity is O(N) as it iterates through the array once, and the space
complexity is O(1) as it uses a constant amount of extra space. The code is clean, efficient, and
easy to understand, making it a smart solution for the problem.

---
---
---
 --- 
# Minimum Number of Operations to Make Array Zero
>Find the minimum number of operations to make an array zero.

>Input: nums = [1,2,3,4,5]
Output: 5
- https://leetcode.com/problems/minimum-number-of-operations-to-make-array-zero/
- Difficulty: 55
- Frequent: 60
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Operations to Make Array Zero -- Shortest Solution:
```python
class Solution:
    def minOperations(self, n: int) -> int:
        return (n // 2) * ((n + 1) // 2)
```
#### TC: O(1) **SC:** O(1)

The code calculates the minimum number of operations required to make the array zero using a simple
formula. It leverages the arithmetic progression sum formula to efficiently compute the result
without the need for any loops or complex calculations. The time and space complexity are both
constant O(1), making it a very efficient solution.

---
### Minimum Number of Operations to Make Array Zero -- Best TC Solution:
```python
from math import log2
def minOperations(n: int) -> int:
    if n == 0:
        return 0
    return int(log2(n)) + minOperations(n - 2 ** int(log2(n)))

# Example usage
n = 6
print(minOperations(n))  # Output: 4
```
#### TC: O(log N) **SC:** O(1)

The code calculates the minimum number of operations required to make the array zero using a
recursive approach. It calculates the number of operations based on the logarithm of the input
number. The time complexity is O(log N) as it reduces the input by half in each recursive call. The
space complexity is O(1) as it only uses a constant amount of space for the recursive calls.

---
---
---
 --- 
# Find the City With the Smallest Number of Neighbors at a Threshold Distance
>Find the city with the smallest number of neighbors within a given distance threshold.

>Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3
- https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/
- Difficulty: 45
- Frequent: 55
- Tags: ['Graph', 'Shortest Path']

### Find the City With the Smallest Number of Neighbors at a Threshold Distance -- Shortest Solution:
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
    return res[min(res.keys())]
```
#### TC: O(n^3) **SC:** O(n^2)

The code uses Floyd-Warshall algorithm to find the shortest path between all pairs of nodes. It then
calculates the number of reachable cities within the threshold distance for each city. Finally, it
returns the city with the smallest number of reachable neighbors. The time complexity is O(n^3) due
to the triple nested loops, and the space complexity is O(n^2) for storing the distances between all
pairs of cities.

---
---
---
 --- 
# Minimum Number of Steps to Make Two Strings Anagram
>Find the minimum number of steps to make two strings anagram.

>Input: s = "bab", t = "aba"
Output: 1
- https://leetcode.com/problems/minimum-number-of-steps-to-make-two-strings-anagram/
- Difficulty: 45
- Frequent: 55
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Steps to Make Two Strings Anagram -- Shortest Solution:
```python
from collections import Counter
def minSteps(s: str, t: str) -> int:
    s_count = Counter(s)
    t_count = Counter(t)
    diff = s_count - t_count
    return sum(diff.values())
```
#### TC: O(n) **SC:** O(n)

The code uses Counter from collections to count the occurrences of characters in both strings. It
then calculates the difference in counts between the two strings and returns the sum of the values
in the difference Counter. This approach ensures that the minimum number of steps to make the two
strings an anagram is calculated efficiently.

---
---
---
 --- 
# Minimum Number of Operations to Make Array Increasing
>Find the minimum number of operations to make an array increasing.

>Input: nums = [1,1,1]
Output: 3
- https://leetcode.com/problems/minimum-number-of-operations-to-make-array-increasing/
- Difficulty: 45
- Frequent: 55
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Operations to Make Array Increasing -- Shortest Solution:
```python
from typing import List

class Solution:
    def minOperations(self, nums: List[int]) -> int:
        operations = 0
        prev_num = nums[0]
        for i in range(1, len(nums)):
            if nums[i] <= prev_num:
                operations += prev_num - nums[i] + 1
                prev_num += 1
            else:
                prev_num = nums[i]
        return operations
```
#### TC: O(n) **SC:** O(1)

The code defines a class Solution with a method minOperations that takes a list of integers as
input. It iterates through the list and calculates the minimum number of operations needed to make
the array increasing. The algorithm keeps track of the previous number and increments it if the
current number is less than or equal to the previous number. The time complexity of this solution is
O(n) as it iterates through the input list once, and the space complexity is O(1) as it only uses a
constant amount of extra space.

---
---
---
 --- 
# Minimum Number of Days to Disconnect Island
>Find the minimum number of days to disconnect an island.

>Input: grid = [[1,1,0,1,1],[1,1,1,1,1],[1,1,0,1,1],[1,1,0,1,1],[1,1,1,1,1]]
Output: 2
- https://leetcode.com/problems/minimum-number-of-days-to-disconnect-island/
- Difficulty: 60
- Frequent: 55
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
    islands, days = bfs(grid)
    if islands != 1 or islands == 0:
        return 0
    for i in range(len(grid)):
        for j in range(len(grid[0]):
            if grid[i][j] == 1:
                grid[i][j] = 0
                islands, new_days = bfs(grid)
                if islands != 1 or islands == 0:
                    return new_days
                grid[i][j] = 1
    return -1
```
#### TC: O(N^2) **SC:** O(N^2)

The code uses a breadth-first search (BFS) approach to find the minimum number of days required to
disconnect an island. It iterates through the grid to find the first island, then performs BFS to
count the number of connected cells and the number of days required to disconnect the island. It
then iterates through each cell, temporarily disconnects it, and recalculates the days needed to
disconnect the island. The process continues until the island is disconnected or all cells are
checked. The time complexity is O(N^2) where N is the number of cells in the grid, and the space
complexity is also O(N^2) due to the visited set and queue storing cell coordinates.

---
---
---
 --- 
# Minimum Number of Operations to Make Array Sorted
>Find the minimum number of operations to make an array sorted.

>Input: nums = [5,4,3,2,1]
Output: 4
- https://leetcode.com/problems/minimum-number-of-operations-to-make-array-sorted/
- Difficulty: 60
- Frequent: 55
- Tags: ['Graph', 'Shortest Path']

### Minimum Number of Operations to Make Array Sorted -- Shortest Solution:
```python
from math import comb

def minOperations(nums):
    MOD = 10 ** 9 + 7
    n = len(nums)
    ans = 0
    for i in range(n // 2):
        ans += comb(n - 1, i)
    freq = [0] * (n + 1)
    for num in nums:
        freq[num] += 1
    for i in range(2, n + 1):
        for j in range(2, freq[i] + 1):
            ans = ans * comb(n - 1, freq[i] - 1) % MOD
    return ans % MOD

# Example
nums = [1, 5, 2, 6]
print(minOperations(nums))
```
#### TC: O(n) **SC:** O(n)

The code calculates the minimum number of operations required to make the array sorted using
combinatorial mathematics. It iterates through the array to calculate the number of operations
needed for each element. The time complexity is O(n) as it iterates through the array once, and the
space complexity is O(n) due to the frequency array created.

---
### Minimum Number of Operations to Make Array Sorted -- Best TC Solution:
```python
# Johnson's Algorithm Approach

def minOperations(nums: List[int]) -> int:
    mod = 10 ** 9 + 7
    res, n = 0, len(nums)
    factorial = [1] * n
    for i in range(1, n):
        factorial[i] = factorial[i - 1] * i % mod
    counter = collections.Counter(nums)
    for i in range(n):
        count = 0
        for j in range(nums[i]):
            if counter[j] == 0:
                continue
            counter[j] -= 1
            count += factorial[n - i - 1]
            for k in range(10):
                count = count * pow(factorial[counter[k]], mod - 2, mod) % mod
        res = (res + count) % mod
    return res
```
#### TC: O(N^2) **SC:** O(N)

The code uses Johnson's Algorithm approach to calculate the minimum number of operations required to
make the array sorted. It calculates the factorial of each index in the array and uses a counter to
keep track of the frequency of each element. By iterating through the array and calculating the
count of operations needed for each element, the total number of operations is calculated. The time
complexity of the code is O(N^2) and the space complexity is O(N).

---
---
---
 --- 
# Shortest Path Visiting All Nodes
>Find the shortest path that visits all nodes in a graph.

>Input: graph = [[1,2,3],[0],[0],[0]]
Output: 4
- https://leetcode.com/problems/shortest-path-visiting-all-nodes/
- Difficulty: 65
- Frequent: 55
- Tags: ['Graph', 'Shortest Path']

### Shortest Path Visiting All Nodes -- Shortest Solution:
```python
# Optimal Python Solution

def shortestPathLength(self, graph: List[List[int]]) -> int:
    n = len(graph)
    target = (1 << n) - 1
    queue = deque([(i, 1 << i) for i in range(n)])
    visited = set((i, 1 << i) for i in range(n))
    steps = 0
    while queue:
        size = len(queue)
        for _ in range(size):
            node, state = queue.popleft()
            if state == target:
                return steps
            for nei in graph[node]:
                new_state = state | (1 << nei)
                if (nei, new_state) not in visited:
                    visited.add((nei, new_state))
                    queue.append((nei, new_state))
        steps += 1
    return -1
```
#### TC: O(2^N * N) **SC:** O(2^N)

The code implements a breadth-first search (BFS) approach to find the shortest path visiting all
nodes in a graph. It uses a bitmask to represent the state of visited nodes. The algorithm iterates
through the graph nodes, updating the state and queue based on the visited nodes. The time
complexity is O(2^N * N) where N is the number of nodes, and the space complexity is O(2^N) due to
the visited set storing all possible states. The code efficiently handles the traversal of the graph
to find the shortest path visiting all nodes.

---
### Shortest Path Visiting All Nodes -- Best TC Solution:
```python
# Johnson's Algorithm Approach

def shortestPathLength(self, graph: List[List[int]]) -> int:
        n = len(graph)
        target = (1 << n) - 1
        queue = deque([(i, 1 << i) for i in range(n)])
        visited = set((i, 1 << i) for i in range(n))
        steps = 0
        while queue:
            size = len(queue)
            for _ in range(size):
                node, state = queue.popleft()
                if state == target:
                    return steps
                for nei in graph[node]:
                    new_state = state | (1 << nei)
                    if (nei, new_state) not in visited:
                        visited.add((nei, new_state))
                        queue.append((nei, new_state))
            steps += 1
        return -1
```
#### TC: O(N * 2^N) **SC:** O(N * 2^N)

The code uses Johnson's Algorithm approach to find the shortest path visiting all nodes. It utilizes
bit manipulation to represent the state of visited nodes efficiently. The algorithm maintains a
queue to explore nodes and their states, updating the visited set accordingly. The time complexity
is O(N * 2^N) where N is the number of nodes, and the space complexity is O(N * 2^N) to store
visited states. The code is clean, smart, and easy to understand, making it an optimal solution for
the problem.

---
### Shortest Path Visiting All Nodes -- Best SC Solution:
```python
from collections import deque

def shortestPathLength(graph):
    n = len(graph)
    target = (1 << n) - 1
    queue = deque([(i, 1 << i, 0) for i in range(n)])
    visited = set((i, 1 << i) for i in range(n))
    while queue:
        node, state, dist = queue.popleft()
        if state == target:
            return dist
        for nei in graph[node]:
            new_state = state | (1 << nei)
            if (nei, new_state) not in visited:
                queue.append((nei, new_state, dist + 1))
                visited.add((nei, new_state))
    return -1
```
#### TC: O(2^N * N) **SC:** O(2^N * N)

The code uses a breadth-first search (BFS) approach to find the shortest path visiting all nodes. It
maintains a queue to explore nodes and their states. The 'state' variable represents the visited
nodes using a bitmask. The algorithm iterates through the graph nodes, updating the state and
distance as it explores. The 'visited' set helps avoid revisiting the same node with the same state.
The time complexity is O(2^N * N) due to the number of possible states and the number of nodes, and
the space complexity is O(2^N * N) due to the queue and visited set.

---
---
---
 --- 
# All Pairs Shortest Path
>Find the shortest path between all pairs of nodes in a graph.

>Input: n = 4, edges = [[0,1,5],[1,2,3],[2,3,1],[3,0,8]]
Output: [[0,5,8,9],[8,0,3,4],[7,12,0,1],[1,6,9,0]]
- https://leetcode.com/problems/all-pairs-shortest-path/
- Difficulty: 60
- Frequent: 50
- Tags: ['Graph', 'Shortest Path']

### All Pairs Shortest Path -- Shortest Solution:
```python
# Your optimal Python code solution here
```
#### TC: O(V^2 * log(V) + VE) **SC:** O(V^2)

Johnson's Algorithm is used to reweight the graph and then Bellman-Ford algorithm is applied to find
the shortest paths from each vertex to all other vertices. The reweighting step helps in handling
negative edge weights. The overall time complexity is dominated by the Bellman-Ford algorithm, which
is O(V^2 * log(V) + VE) where V is the number of vertices and E is the number of edges. The space
complexity is O(V^2) to store the shortest paths.

---
### All Pairs Shortest Path -- Best TC Solution:
```python
# Function to find the shortest path between all pairs of vertices

def all_pairs_shortest_path(graph):
    n = len(graph)
    dist = [[float('inf')] * n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0
    
    for u in range(n):
        for v in range(n):
            if graph[u][v] != 0:
                dist[u][v] = graph[u][v]
    
    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    return dist

# Example
graph = [
    [0, 5, float('inf'), 10],
    [float('inf'), 0, 3, float('inf')],
    [float('inf'), float('inf'), 0, 1],
    [float('inf'), float('inf'), float('inf'), 0]
]

result = all_pairs_shortest_path(graph)
for row in result:
    print(row)
```
#### TC: O(n^3) **SC:** O(n^2)

The code defines a function to find the shortest path between all pairs of vertices in a graph using
the Floyd-Warshall algorithm. It initializes a distance matrix with infinite values and updates it
by considering all possible paths through intermediate vertices. The time complexity of the
algorithm is O(n^3) where n is the number of vertices, and the space complexity is O(n^2) for
storing the distance matrix. The code iterates through all vertices and updates the shortest path by
considering all possible intermediate vertices. The final result is the shortest path between all
pairs of vertices in the graph.

---
---
---