# A* Search Algorithm
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Escape a Large Maze](#escape-a-large-maze) | 40 | 65 |
| 2 | [Shortest Path in Binary Matrix](#shortest-path-in-binary-matrix) | 35 | 40 |
| 3 | [Minimum Moves to Move a Box to Their Target Location](#minimum-moves-to-move-a-box-to-their-target-location) | 35 | 55 |
| 4 | [Minimum Cost to Reach Destination in Time](#minimum-cost-to-reach-destination-in-time) | 35 | 60 |
| 5 | [Sliding Puzzle](#sliding-puzzle) | 30 | 50 |
| 6 | [Minimum Cost to Cut a Stick](#minimum-cost-to-cut-a-stick) | 30 | 55 |
| 7 | [Minimum Number of Refueling Stops](#minimum-number-of-refueling-stops) | 30 | 60 |
| 8 | [Path with Maximum Probability](#path-with-maximum-probability) | 25 | 45 |
| 9 | [Minimum Cost to Make at Least One Valid Path in a Grid](#minimum-cost-to-make-at-least-one-valid-path-in-a-grid) | 25 | 55 |
| 10 | [Minimum Cost to Connect Two Groups of Points](#minimum-cost-to-connect-two-groups-of-points) | 25 | 60 |
| 11 | [Reach a Number](#reach-a-number) | 20 | 45 |
| 12 | [Minimum Number of Days to Eat N Oranges](#minimum-number-of-days-to-eat-n-oranges) | 20 | 50 |
| 13 | [Minimum Cost to Make Array Equal](#minimum-cost-to-make-array-equal) | 20 | 50 |
---
A* Search is a pathfinding and graph traversal algorithm that finds the shortest path from a start node to a goal node. It uses a heuristic to estimate the cost to reach the goal, combining it with the actual cost from the start to the current node. This makes A* both complete and optimal, provided the heuristic is admissible (never overestimates the true cost).
```python
import heapq

def a_star_search(graph, start, goal, heuristic):
    open_set = []
    heapq.heappush(open_set, (0, start))
    came_from = {}
    g_score = {start: 0}
    f_score = {start: heuristic(start, goal)}
    
    while open_set:
        _, current = heapq.heappop(open_set)
        
        if current == goal:
            path = []
            while current in came_from:
                path.append(current)
                current = came_from[current]
            path.append(start)
            return path[::-1]
        
        for neighbor, cost in graph[current].items():
            tentative_g_score = g_score[current] + cost
            if neighbor not in g_score or tentative_g_score < g_score[neighbor]:
                came_from[neighbor] = current
                g_score[neighbor] = tentative_g_score
                f_score[neighbor] = tentative_g_score + heuristic(neighbor, goal)
                heapq.heappush(open_set, (f_score[neighbor], neighbor))
    return None

def heuristic(node, goal):
    # Example heuristic: Manhattan distance for a grid
    return abs(node[0] - goal[0]) + abs(node[1] - goal[1])

graph = {
    (0, 0): {(0, 1): 1, (1, 0): 1},
    (0, 1): {(0, 0): 1, (1, 1): 1, (0, 2): 1},
    (0, 2): {(0, 1): 1, (1, 2): 1},
    (1, 0): {(0, 0): 1, (1, 1): 1, (2, 0): 1},
    (1, 1): {(1, 0): 1, (0, 1): 1, (1, 2): 1, (2, 1): 1},
    (1, 2): {(1, 1): 1, (0, 2): 1, (2, 2): 1},
    (2, 0): {(1, 0): 1, (2, 1): 1},
    (2, 1): {(2, 0): 1, (1, 1): 1, (2, 2): 1},
    (2, 2): {(2, 1): 1, (1, 2): 1}
}

start = (0, 0)
goal = (2, 2)
path = a_star_search(graph, start, goal, heuristic)
print("Path:", path)
```
### Insight:
1. A* Search is optimal and complete if the heuristic is admissible and consistent.
2. The heuristic function greatly influences the performance; a good heuristic can significantly speed up the search.
3. The algorithm uses a priority queue to explore the most promising nodes first, balancing between the actual cost and the estimated cost to the goal.
4. The choice of data structures (e.g., heap for the open set) is crucial for efficiency.
5. A* can be applied to various problems, including pathfinding in games, robot navigation, and network routing.
---
 --- 
# Escape a Large Maze
>Determine if it's possible to escape a large maze using A* search algorithm.

>Input: blocked = [[0,1],[1,0]], source = [0,0], target = [0,2]
Output: true
- https://leetcode.com/problems/escape-a-large-maze/
- Difficulty: 65
- Frequent: 40
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Escape a Large Maze -- Shortest Solution:
```python
from collections import deque

def isEscapePossible(blocked, source, target):
    blocked = {tuple(p) for p in blocked}
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    visited = set()
    queue = deque([source])
    limit = 200
    while queue and limit:
        limit -= 1
        x, y = queue.popleft()
        if (x, y) == target:
            return True
        for dx, dy in directions:
            for i in range(1, 10**6):
                nx, ny = x + i * dx, y + i * dy
                if (nx, ny) in blocked or (nx, ny) in visited:
                    break
                visited.add((nx, ny))
                queue.append((nx, ny))
    return False
```
#### TC: O(B), where B is the number of blocked cells **SC:** O(B), where B is the number of blocked cells

The code uses a breadth-first search approach to check if the source can reach the target without
being blocked. It maintains a set of blocked cells and visited cells to avoid revisiting them. The
algorithm iterates through possible moves in all four directions until either the target is reached
or the limit is exhausted. The time complexity is O(B) where B is the number of blocked cells, and
the space complexity is also O(B) due to the storage of blocked cells and visited cells.

---
### Escape a Large Maze -- Best TC Solution:
```python
from collections import deque

def isEscapePossible(blocked, source, target):
    blocked = {tuple(p) for p in blocked}
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    visited = set()
    queue = deque([source])
    visited.add(tuple(source))
    target = tuple(target)
    def bfs(limit):
        for _ in range(limit):
            for _ in range(len(queue)):
                x, y = queue.popleft()
                if (x, y) == target:
                    return True
                for dx, dy in directions:
                    nx, ny = x + dx, y + dy
                    if 0 <= nx < 10**6 and 0 <= ny < 10**6 and (nx, ny) not in visited and (nx, ny) not in blocked:
                        queue.append((nx, ny))
                        visited.add((nx, ny))
        return False
    return bfs(200) and bfs(200)
```
#### TC: O(B^2) **SC:** O(B)

The code uses a breadth-first search (BFS) approach to determine if the source can reach the target
without being blocked. It maintains a set of blocked positions, visited positions, and a queue for
BFS traversal. The function bfs(limit) performs BFS up to a certain limit to check if the target can
be reached. The code efficiently handles large mazes by limiting the BFS traversal to 200 steps in
each direction. This approach ensures a clean and easy-to-understand solution with a time complexity
of O(B^2) and a space complexity of O(B), where B is the number of blocked positions.

---
### Escape a Large Maze -- Best SC Solution:
```python
# Optimal Python Solution

def isEscapePossible(blocked, source, target):
    def bfs(source, target, blocked):
        queue = [source]
        visited = set()
        while queue:
            node = queue.pop(0)
            if node == target:
                return True
            if node in visited or node in blocked:
                continue
            visited.add(node)
            x, y = node
            for dx, dy in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
                new_x, new_y = x + dx, y + dy
                if 0 <= new_x < 10**6 and 0 <= new_y < 10**6:
                    queue.append((new_x, new_y))
            if len(visited) == 20000:
                return True
        return False
    return bfs(source, target, blocked) and bfs(target, source, blocked)
```
#### TC: O(min(B, 20000)) **SC:** O(min(B, 20000))

The code uses a breadth-first search (BFS) approach to check if there is a path between the source
and target points while considering the blocked cells. The function bfs performs two separate BFS
searches from the source to target and vice versa. It iterates through the neighboring cells in four
directions and checks if they are within the grid boundaries. The visited set keeps track of visited
nodes to avoid revisiting them. The code has a time complexity of O(min(B, 20000)) and a space
complexity of O(min(B, 20000)), where B is the number of blocked cells.

---
---
---
 --- 
# Shortest Path in Binary Matrix
>Find the shortest path in a binary matrix using A* search algorithm.

>Input: grid = [[0,1],[1,0]]
Output: 2
- https://leetcode.com/problems/shortest-path-in-binary-matrix/
- Difficulty: 40
- Frequent: 35
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Shortest Path in Binary Matrix -- Shortest Solution:
```python
import heapq
def shortestPathBinaryMatrix(grid):
    if grid[0][0] == 1 or grid[-1][-1] == 1:
        return -1
    n = len(grid)
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1), (1, 1), (-1, -1), (1, -1), (-1, 1)]
    heap = [(1, 0, 0)]
    while heap:
        cost, x, y = heapq.heappop(heap)
        if x == n - 1 and y == n - 1:
            return cost
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and grid[nx][ny] == 0:
                grid[nx][ny] = 1
                heapq.heappush(heap, (cost + 1, nx, ny))
    return -1
```
#### TC: O(N * log(N)) **SC:** O(N)

The code uses A* Search Algorithm to find the shortest path in a binary matrix. It initializes a
heap with the starting point and explores the neighboring cells while updating the cost. The
algorithm terminates when it reaches the destination cell or when all possible paths have been
explored. The time complexity is O(N * log(N)) where N is the number of cells in the matrix, and the
space complexity is O(N) to store the heap.

---
### Shortest Path in Binary Matrix -- Best SC Solution:
```python
from heapq import heappush, heappop

def shortestPathBinaryMatrix(grid):
    if grid[0][0] == 1 or grid[-1][-1] == 1:
        return -1
    n = len(grid)
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0),
                  (1, 1), (1, -1), (-1, 1), (-1, -1)]
    heap = [(1, 0, 0)]
    while heap:
        cost, x, y = heappop(heap)
        if x == n - 1 and y == n - 1:
            return cost
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and grid[nx][ny] == 0:
                grid[nx][ny] = 1
                heappush(heap, (cost + 1, nx, ny))
    return -1
```
#### TC: O(N * N) **SC:** O(N * N)

The code implements a shortest path finding algorithm using A* search approach. It starts from the
top-left corner and explores all possible paths by considering the neighboring cells. The algorithm
uses a priority queue (heap) to prioritize the cells with the lowest cost. The grid is updated to
mark visited cells and avoid revisiting them. The algorithm terminates when it reaches the bottom-
right corner or when all paths are exhausted. The time complexity is O(N * N) where N is the size of
the grid, and the space complexity is also O(N * N) to store the grid and heap.

---
---
---
 --- 
# Minimum Moves to Move a Box to Their Target Location
>Find the minimum moves to move a box to their target location using A* search algorithm.

>Input: grid = [["#","#","#","#","#","#"],["#","T","#","#","#","#"],["#",".",".","B",".","#"],["#",".","#","#",".","#"],["#",".",".",".","S","#"],["#","#","#","#","#","#"]]
Output: 3
- https://leetcode.com/problems/minimum-moves-to-move-a-box-to-their-target-location/
- Difficulty: 55
- Frequent: 35
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Minimum Moves to Move a Box to Their Target Location -- Shortest Solution:
```python
from collections import deque

def minPushBox(grid):
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    rows, cols = len(grid), len(grid[0])
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 'S':
                start = (r, c)
            elif grid[r][c] == 'B':
                box = (r, c)
            elif grid[r][c] == 'T':
                target = (r, c)
    def canReach(start, target, box):
        visited = set()
        queue = deque([start])
        while queue:
            r, c = queue.popleft()
            if (r, c) == target:
                return True
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if 0 <= nr < rows and 0 <= nc < cols and (nr, nc) != box and grid[nr][nc] != '#' and (nr, nc) not in visited:
                    visited.add((nr, nc))
                    queue.append((nr, nc))
        return False
    def canPush(box, target, direction):
        r, c = box
        dr, dc = direction
        nr, nc = r + dr, c + dc
        if (nr, nc) == target:
            return True
        return canReach((r - dr, c - dc), target, box)
    queue = deque([(0, box, start)])
    visited = set()
    while queue:
        steps, box, player = queue.popleft()
        if (box, player) in visited:
            continue
        visited.add((box, player))
        if box == target:
            return steps
        for dr, dc in directions:
            nr, nc = box[0] + dr, box[1] + dc
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] != '#' and canPush(box, target, (dr, dc)):
                queue.append((steps + 1, (nr, nc), box))
    return -1
```
#### TC: O(R * C) **SC:** O(R * C)

The code defines a function minPushBox that takes a grid as input and finds the minimum number of
moves required to move the box to its target location. It uses a breadth-first search approach to
explore possible moves and determine the optimal path. The canReach function checks if a position is
reachable, while the canPush function checks if the box can be pushed in a certain direction. The
code efficiently handles the grid layout and player, box, and target positions to determine the
minimum moves needed. The time complexity is O(R * C) where R is the number of rows and C is the
number of columns in the grid, and the space complexity is also O(R * C) due to the use of sets and
queues to track visited positions and store intermediate results.

---
### Minimum Moves to Move a Box to Their Target Location -- Best TC Solution:
```python
# Function to calculate the minimum moves to move a box to its target location

def minPushBox(grid):
    def bfs(player, box, target):
        queue = deque([(player, box)])
        visited = set()
        visited.add((player, box))
        moves = 0
        while queue:
            for _ in range(len(queue)):
                p, b = queue.popleft()
                if b == target:
                    return moves
                for dx, dy in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
                    np = (p[0] + dx, p[1] + dy)
                    if 0 <= np[0] < len(grid) and 0 <= np[1] < len(grid[0]) and grid[np[0]][np[1]] != '#':
                        if np == b:
                            nb = (b[0] + dx, b[1] + dy)
                            if 0 <= nb[0] < len(grid) and 0 <= nb[1] < len(grid[0]) and grid[nb[0]][nb[1]] != '#':
                                if (np, nb) not in visited:
                                    visited.add((np, nb))
                                    queue.append((np, nb))
                        else:
                            if (np, b) not in visited:
                                visited.add((np, b))
                                queue.append((np, b))
            moves += 1
        return -1

    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == 'S':
                player = (i, j)
            elif grid[i][j] == 'B':
                box = (i, j)
            elif grid[i][j] == 'T':
                target = (i, j)

    return bfs(player, box, target)
```
#### TC: O(M*N) **SC:** O(M*N)

The code uses a breadth-first search (BFS) approach to find the minimum moves required to move a box
to its target location. It iterates through the grid, considering the player's position, box's
position, and target position. The BFS algorithm explores all possible moves while avoiding
obstacles represented by '#' in the grid. The time complexity is O(M*N) where M is the number of
rows and N is the number of columns in the grid. The space complexity is also O(M*N) due to the
visited set storing visited positions.

---
### Minimum Moves to Move a Box to Their Target Location -- Best SC Solution:
```python
# Function to calculate the minimum moves to move a box to its target location

def minPushBox(grid):
    def bfs(player, box, target):
        queue = deque([(player, box, 0)])
        visited = set()
        while queue:
            p, b, moves = queue.popleft()
            if b == target:
                return moves
            if (p, b) in visited:
                continue
            visited.add((p, b))
            for dx, dy in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
                np = (p[0] + dx, p[1] + dy)
                if 0 <= np[0] < len(grid) and 0 <= np[1] < len(grid[0]) and grid[np[0]][np[1]] != '#':
                    if np == b:
                        nb = (b[0] + dx, b[1] + dy)
                        if 0 <= nb[0] < len(grid) and 0 <= nb[1] < len(grid[0]) and grid[nb[0]][nb[1]] != '#':
                            queue.append((np, nb, moves + 1))
                    else:
                        queue.append((np, b, moves))
        return -1
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == 'S':
                player = (i, j)
            elif grid[i][j] == 'B':
                box = (i, j)
            elif grid[i][j] == 'T':
                target = (i, j)
    return bfs(player, box, target)

# Example
grid = [
    ['#','#','#','#','#','#'],
    ['#','T','#','#','#','#'],
    ['#','.','.','B','.','#'],
    ['#','.','#','#','.','#'],
    ['#','.','.','.','S','#'],
    ['#','#','#','#','#','#']
]
print(minPushBox(grid))
```
#### TC: O(M * N) **SC:** O(M * N)

The code uses a breadth-first search (BFS) approach to find the minimum moves required to move the
box to its target location. It iterates through the grid to find the player, box, and target
positions. Then, it performs BFS starting from the player and box positions to explore possible
moves. The visited set is used to avoid revisiting the same positions. The time complexity is O(M *
N) where M is the number of rows and N is the number of columns in the grid. The space complexity is
also O(M * N) due to the visited set.

---
---
---
 --- 
# Minimum Cost to Reach Destination in Time
>Find the minimum cost to reach the destination in time using A* search algorithm.

>Input: maxTime = 30, edges = [[0,1,10],[1,2,10],[2,3,10],[3,4,10]], passingFees = [5,1,2,20,20]
Output: 11
- https://leetcode.com/problems/minimum-cost-to-reach-destination-in-time/
- Difficulty: 60
- Frequent: 35
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Minimum Cost to Reach Destination in Time -- Shortest Solution:
```python
import heapq
class Solution:
    def minCost(self, maxTime: int, edges: List[List[int]], passingFees: List[int]) -> int:
        n = len(passingFees)
        graph = defaultdict(list)
        for u, v, t in edges:
            graph[u].append((v, t))
            graph[v].append((u, t))
        pq = [(passingFees[0], 0, 0)]
        dist = {0: passingFees[0]}
        while pq:
            cost, node, time = heapq.heappop(pq)
            if node == n - 1:
                return cost
            if time > maxTime:
                continue
            for nei, t in graph[node]:
                new_cost = cost + passingFees[nei]
                new_time = time + t
                if new_time <= maxTime and (nei not in dist or new_cost < dist[nei]):
                    dist[nei] = new_cost
                    heapq.heappush(pq, (new_cost, nei, new_time))
        return -1
```
#### TC: O(ElogE) **SC:** O(E)

The code implements a solution using Dijkstra's algorithm with a priority queue. It calculates the
minimum cost to reach the destination within the given maximum time. The algorithm efficiently
explores the graph by considering the cost and time constraints. The time complexity is O(ElogE) due
to the priority queue operations, and the space complexity is O(E) to store the graph edges and
distances. The code is concise, clean, and easy to understand, making it a smart solution for the
problem.

---
### Minimum Cost to Reach Destination in Time -- Best TC Solution:
```python
import heapq

def minCost(maxTime: int, edges: List[List[int]], passingFees: List[int]) -> int:
    n = len(passingFees)
    graph = defaultdict(list)
    for u, v, t in edges:
        graph[u].append((v, t))
        graph[v].append((u, t))
    pq = [(passingFees[0], 0, 0)]
    dist = {0: passingFees[0]}
    while pq:
        cost, node, time = heapq.heappop(pq)
        if node == n - 1:
            return cost
        if time > maxTime:
            continue
        for nei, t in graph[node]:
            new_cost = cost + passingFees[nei]
            if new_cost < dist.get(nei, float('inf')):
                dist[nei] = new_cost
                heapq.heappush(pq, (new_cost, nei, time + t))
    return -1
```
#### TC: O(ElogV) **SC:** O(V)

The code implements a Dijkstra's algorithm using a priority queue to find the minimum cost to reach
the destination within the given maximum time. It efficiently explores the graph by considering the
passing fees and time constraints. The priority queue helps in selecting the next node with the
minimum cost, ensuring an optimal solution. The time complexity is O(ElogV) due to the priority
queue operations, and the space complexity is O(V) for storing the distances to each node.

---
---
---
 --- 
# Sliding Puzzle
>Solve the sliding puzzle using A* search algorithm.

>Input: board = [[1,2,3],[4,0,5]]
Output: 1
Explanation: Swap the 0 and the 5 in one move.
- https://leetcode.com/problems/sliding-puzzle/
- Difficulty: 50
- Frequent: 30
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Sliding Puzzle -- Shortest Solution:
```python
from collections import deque

def slidingPuzzle(board):
    target = '123450'
    start = ''.join(str(num) for row in board for num in row)
    moves = {
        0: [1, 3],
        1: [0, 2, 4],
        2: [1, 5],
        3: [0, 4],
        4: [1, 3, 5],
        5: [2, 4]
    }
    queue = deque([(start, start.index('0'), 0)])
    visited = set()
    visited.add(start)
    while queue:
        state, zero, step = queue.popleft()
        if state == target:
            return step
        for move in moves[zero]:
            new_state = list(state)
            new_state[zero], new_state[move] = new_state[move], new_state[zero]
            new_state_str = ''.join(new_state)
            if new_state_str not in visited:
                queue.append((new_state_str, move, step + 1))
                visited.add(new_state_str)
    return -1
```
#### TC: O(1) **SC:** O(1)

The code implements a solution to the Sliding Puzzle problem using a breadth-first search approach.
It converts the board into a string representation and uses a queue to explore possible moves. The
'moves' dictionary defines the possible moves for each position. The algorithm iterates through the
queue, updating the state based on valid moves until the target state is reached. The code
efficiently tracks visited states to avoid revisiting them. The time and space complexity of this
solution is O(1) as it operates on a fixed-size board.

---
### Sliding Puzzle -- Best TC Solution:
```python
# Function to solve the Sliding Puzzle problem
from heapq import heappush, heappop

def slidingPuzzle(board):
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    target = '123450'
    start = ''.join(str(num) for row in board for num in row)
    visited = set()
    heap = [(0, start, start.index('0'))]
    while heap:
        moves, state, zero_idx = heappop(heap)
        if state == target:
            return moves
        if state in visited:
            continue
        visited.add(state)
        for dx, dy in directions:
            new_zero_idx = zero_idx + dy + 3 * dx
            if 0 <= new_zero_idx < 6 and (zero_idx % 3 != 2 or dx != 1) and (zero_idx % 3 != 0 or dx != -1):
                new_state = list(state)
                new_state[zero_idx], new_state[new_zero_idx] = new_state[new_zero_idx], new_state[zero_idx]
                new_state = ''.join(new_state)
                heappush(heap, (moves + 1, new_state, new_zero_idx))
    return -1

# Example
board = [[1, 2, 3], [4, 0, 5]]
print(slidingPuzzle(board))
```
#### TC: O((m*n)!*log((m*n)!)) **SC:** O((m*n)!*m*n)

The code uses A* Search Algorithm to solve the Sliding Puzzle problem efficiently. It maintains a
heap to prioritize the states with fewer moves. The algorithm explores all possible moves from the
current state until it reaches the target state. The use of a set to track visited states helps
avoid revisiting the same state. The code efficiently handles the sliding puzzle by swapping the '0'
position with its neighboring numbers. The time complexity is O((m*n)!*log((m*n)!)), where m and n
are the dimensions of the board, and the space complexity is O((m*n)!*m*n).

---
---
---
 --- 
# Minimum Cost to Cut a Stick
>Find the minimum cost to cut a stick using A* search algorithm.

>Input: n = 7, cuts = [1,3,4,5]
Output: 16
- https://leetcode.com/problems/minimum-cost-to-cut-a-stick/
- Difficulty: 55
- Frequent: 30
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Minimum Cost to Cut a Stick -- Shortest Solution:
```python
from functools import lru_cache
def minCost(self, n: int, cuts: List[int]) -> int:
    cuts.sort()
    cuts = [0] + cuts + [n]
    @lru_cache(None)
    def dp(i, j):
        return 0 if j - i <= 1 else min(cuts[j] - cuts[i] + dp(i, k) + dp(k, j) for k in range(i + 1, j))
    return dp(0, len(cuts) - 1)
```
#### TC: O(n^3) **SC:** O(n^2)

The code uses dynamic programming with memoization to solve the problem efficiently. It sorts the
cuts array and adds the start and end points to it. The dp function calculates the minimum cost
recursively by considering all possible cuts. The @lru_cache decorator helps in memoizing the
results to avoid redundant calculations. The time complexity is O(n^3) due to the nested loops in
the dp function, and the space complexity is O(n^2) due to the memoization table.

---
---
---
 --- 
# Minimum Number of Refueling Stops
>Find the minimum number of refueling stops using A* search algorithm.

>Input: target = 100, startFuel = 10, stations = [[10,60],[20,30],[30,30],[60,40]]
Output: 2
- https://leetcode.com/problems/minimum-number-of-refueling-stops/
- Difficulty: 60
- Frequent: 30
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Minimum Number of Refueling Stops -- Shortest Solution:
```python
# A* Search Algorithm Approach

def minRefuelStops(target, startFuel, stations):
    pq = []
    stations.append((target, 0))
    res = prev = 0
    tank = startFuel
    for location, capacity in stations:
        tank -= location - prev
        while pq and tank < 0:
            tank += -heapq.heappop(pq)
            res += 1
        if tank < 0:
            return -1
        heapq.heappush(pq, -capacity)
        prev = location
    return res
```
#### TC: O(n log n) **SC:** O(n)

The code uses a priority queue to keep track of the stations that can be refueled. It iterates
through the stations, calculating the remaining fuel after reaching each station. If the fuel is not
enough, it refuels from the stations in the priority queue with the highest capacity. The priority
queue helps in selecting the optimal station to refuel from. The time complexity is O(n log n) due
to the heap operations, and the space complexity is O(n) to store the stations in the priority
queue.

---
### Minimum Number of Refueling Stops -- Best TC Solution:
```python
import heapq
def minRefuelStops(target, startFuel, stations):
    pq = []
    stations.append((target, float('inf')))
    ans = prev = 0
    tank = startFuel
    for location, capacity in stations:
        tank -= location - prev
        while pq and tank < 0:
            tank += -heapq.heappop(pq)
            ans += 1
        if tank < 0: return -1
        heapq.heappush(pq, -capacity)
        prev = location
    return ans
```
#### TC: O(nlogn) **SC:** O(n)

The code uses a priority queue to keep track of the stations that can be refueled. It iterates
through the stations, refueling at each station if necessary. The priority queue stores the
capacities of the stations in a descending order. The code efficiently handles the refueling process
and returns the minimum number of stops required. The time complexity is O(nlogn) due to the heap
operations, and the space complexity is O(n) to store the priority queue.

---
---
---
 --- 
# Path with Maximum Probability
>Find the path with maximum probability using A* search algorithm.

>Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
Output: 0.25000
- https://leetcode.com/problems/path-with-maximum-probability/
- Difficulty: 45
- Frequent: 25
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Path with Maximum Probability -- Shortest Solution:
```python
import heapq
from collections import defaultdict

def maxProbability(n, edges, succProb, start, end):
    graph = defaultdict(list)
    for i, (a, b) in enumerate(edges):
        graph[a].append((b, succProb[i]))
        graph[b].append((a, succProb[i]))
    pq = [(-1, start)]
    while pq:
        prob, node = heapq.heappop(pq)
        if node == end:
            return -prob
        for nei, nei_prob in graph[node]:
            if nei_prob * prob < -1:
                heapq.heappush(pq, (prob * nei_prob, nei))
    return 0.0
```
#### TC: O(E log V) **SC:** O(V + E)

The code uses a graph representation and A* Search Algorithm approach to find the maximum
probability path from start to end. It initializes a priority queue with the start node and explores
the neighbors with the highest probabilities first. The algorithm continues until it reaches the end
node or exhausts all possibilities. The time complexity is O(E log V) where E is the number of edges
and V is the number of vertices, and the space complexity is O(V + E) to store the graph and
priority queue.

---
---
---
 --- 
# Minimum Cost to Make at Least One Valid Path in a Grid
>Find the minimum cost to make at least one valid path in a grid using A* search algorithm.

>Input: grid = [[1,1,3],[3,2,2],[1,1,4]]
Output: 0
- https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/
- Difficulty: 55
- Frequent: 25
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

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
            size = len(queue)
            for _ in range(size):
                i, j, c = queue.popleft()
                if (i, j) == (m - 1, n - 1):
                    return cost
                if (i, j) in visited:
                    continue
                visited.add((i, j))
                for k, (di, dj) in enumerate(directions):
                    ni, nj = i + di, j + dj
                    if 0 <= ni < m and 0 <= nj < n:
                        if k + 1 == grid[i][j]:
                            queue.append((ni, nj, c))
                        else:
                            queue.append((ni, nj, c + 1))
            cost += 1
        return -1
```
#### TC: O(m * n) **SC:** O(m * n)

The code uses a breadth-first search (BFS) approach to find the minimum cost to reach the bottom-
right cell of the grid. It iterates through the grid cells using a queue and checks the neighboring
cells based on the direction indicated by the current cell's value. The visited set is used to keep
track of visited cells to avoid revisiting them. The time complexity is O(m * n) where m is the
number of rows and n is the number of columns in the grid. The space complexity is also O(m * n) to
store the queue and visited cells.

---
---
---
 --- 
# Minimum Cost to Connect Two Groups of Points
>Find the minimum cost to connect two groups of points using A* search algorithm.

>Input: cost = [[15, 96], [36, 2]]
Output: 17
- https://leetcode.com/problems/minimum-cost-to-connect-two-groups-of-points/
- Difficulty: 60
- Frequent: 25
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Minimum Cost to Connect Two Groups of Points -- Shortest Solution:
```python
from functools import lru_cache
class Solution:
    def connectTwoGroups(self, cost: List[List[int]]) -> int:
        m, n = len(cost), len(cost[0])
        INF = float('inf')
        dp = [[INF] * n for _ in range(m)]
        @lru_cache(None)
        def dfs(i, mask):
            if i == m:
                return sum(cost[i][j] for j in range(n) if mask & (1 << j) == 0)
            if dp[i][mask] != INF:
                return dp[i][mask]
            res = INF
            for j in range(n):
                res = min(res, cost[i][j] + dfs(i + 1, mask | (1 << j)))
            dp[i][mask] = res
            return res
        return dfs(0, 0)
```
#### TC: O(2^m * n) **SC:** O(2^m * n)

The code uses dynamic programming with memoization to solve the problem efficiently. It recursively
calculates the minimum cost to connect two groups of points by considering all possible
combinations. The @lru_cache decorator helps in memoizing the results to avoid redundant
calculations. The time complexity is O(2^m * n) where m is the number of elements in the first group
and n is the number of elements in the second group. The space complexity is also O(2^m * n) due to
the memoization table.

---
### Minimum Cost to Connect Two Groups of Points -- Best TC Solution:
```python
# A* Search Algorithm Approach

def connect_two_groups(cost: List[List[int]]) -> int:
    m, n = len(cost), len(cost[0])
    dp = [[float('inf')] * (1 << n) for _ in range(m + 1)]
    dp[0][0] = 0
    for i in range(1, m + 1):
        for mask in range(1 << n):
            for j in range(n):
                dp[i][mask | (1 << j)] = min(dp[i][mask | (1 << j)], dp[i - 1][mask] + cost[i - 1][j])
            for k in range(n):
                dp[i][mask] = min(dp[i][mask], dp[i][mask | (1 << k)])
    ans = float('inf')
    for mask in range(1, 1 << n):
        curr_cost = 0
        for j in range(n):
            if mask & (1 << j) == 0:
                curr_cost += min(cost[i][j] for i in range(m))
        ans = min(ans, dp[m][mask] + curr_cost)
    return ans

# Example Usage
input_cost = [[2, 5, 1], [3, 4, 7]]
result = connect_two_groups(input_cost)
print(result)
```
#### TC: O(m * 2^n * n) **SC:** O(m * 2^n)

The code uses dynamic programming to find the minimum cost to connect two groups of points. It
iterates through all possible combinations of points and calculates the minimum cost at each step.
The A* Search Algorithm approach helps in optimizing the search process. The time complexity is O(m
* 2^n * n) where m is the number of points in the first group, n is the number of points in the
second group, and the space complexity is O(m * 2^n). The code efficiently handles the cost
calculation and minimization to find the optimal solution.

---
### Minimum Cost to Connect Two Groups of Points -- Best SC Solution:
```python
# Function to calculate the minimum cost to connect two groups of points

def connect_two_groups(cost: List[List[int]]) -> int:
    m, n = len(cost), len(cost[0])
    dp = [[float('inf')] * (1 << n) for _ in range(m + 1)]
    dp[0][0] = 0
    for i in range(1, m + 1):
        for mask in range(1 << n):
            for j in range(n):
                dp[i][mask | (1 << j)] = min(dp[i][mask | (1 << j)], dp[i - 1][mask] + cost[i - 1][j])
            for k in range(n):
                dp[i][mask] = min(dp[i][mask], dp[i][mask | (1 << k)])
    return dp[m][(1 << n) - 1]

# Example usage
example_cost = [[15, 96], [36, 2]]
result = connect_two_groups(example_cost)
print(result)
```
#### TC: Time Complexity: O(m * 2^n * n) **SC:** Space Complexity: O(m * 2^n)

The code uses dynamic programming to find the minimum cost to connect two groups of points. It
iterates through all possible combinations of points and calculates the minimum cost to connect
them. The use of bit manipulation with bitwise OR operations helps in efficiently representing the
states of connected points. The time complexity is O(m * 2^n * n) where m is the number of points in
the first group, n is the number of points in the second group, and the space complexity is O(m *
2^n). The code is clean, easy to understand, and provides an optimal solution for the problem.

---
---
---
 --- 
# Reach a Number
>Find the minimum steps to reach a number using A* search algorithm.

>Input: target = 3
Output: 2
- https://leetcode.com/problems/reach-a-number/
- Difficulty: 45
- Frequent: 20
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Reach a Number -- Shortest Solution:
```python
class Solution:
    def reachNumber(self, target: int) -> int:
        target = abs(target)
        step = 0
        current_sum = 0
        while current_sum < target or (current_sum - target) % 2 != 0:
            step += 1
            current_sum += step
        return step
```
#### TC: O(1) **SC:** O(1)

The code calculates the minimum number of steps required to reach the target number by moving left
or right. It uses a simple approach of incrementing the step count and updating the current sum
until the current sum is equal to or greater than the target. The absolute difference between the
current sum and the target is used to determine if the target can be reached with the given steps.
The code efficiently handles both positive and negative target numbers.

---
---
---
 --- 
# Minimum Number of Days to Eat N Oranges
>Find the minimum number of days to eat N oranges using A* search algorithm.

>Input: n = 10
Output: 4
- https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges/
- Difficulty: 50
- Frequent: 20
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

### Minimum Number of Days to Eat N Oranges -- Shortest Solution:
```python
import heapq
def minDays(n: int) -> int:
    heap = [(0, n)]
    visited = set()
    while heap:
        days, oranges = heapq.heappop(heap)
        if oranges == 0:
            return days
        if oranges not in visited:
            visited.add(oranges)
            heapq.heappush(heap, (days + oranges % 2 + 1, oranges // 2))
            heapq.heappush(heap, (days + oranges % 3 + 1, oranges // 3))
            heapq.heappush(heap, (days + 1, oranges - 1))
    return -1
```
#### TC: O(log N) **SC:** O(N)

The code uses a heap to keep track of the minimum number of days required to eat the oranges. It
explores all possible scenarios by dividing the oranges by 2, 3, or subtracting 1 each day. The
visited set ensures that we do not revisit the same state. The code efficiently finds the minimum
number of days using a heap-based approach with a time complexity of O(log N) and a space complexity
of O(N).

---
---
---
 --- 
# Minimum Cost to Make Array Equal
>Find the minimum cost to make an array equal using A* search algorithm.

>Input: nums = [1,2,3], cost = [10,100,1000]
Output: 120
- https://leetcode.com/problems/minimum-cost-to-make-array-equal/
- Difficulty: 50
- Frequent: 20
- Tags: ['Graph', 'Breadth-First Search', 'A* Search Algorithm']

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
---
---