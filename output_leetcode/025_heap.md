# Heap
## Frequent score for this algorithmic framework: 25 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Top K Frequent Elements](#top-k-frequent-elements) | 70 | 40 |
| 2 | [Merge Intervals](#merge-intervals) | 70 | 45 |
| 3 | [Find Median from Data Stream](#find-median-from-data-stream) | 65 | 60 |
| 4 | [Task Scheduler](#task-scheduler) | 60 | 50 |
| 5 | [K Closest Points to Origin](#k-closest-points-to-origin) | 60 | 50 |
| 6 | [Find K Pairs with Smallest Sums](#find-k-pairs-with-smallest-sums) | 60 | 55 |
| 7 | [Sort Characters By Frequency](#sort-characters-by-frequency) | 55 | 45 |
| 8 | [Maximum Frequency Stack](#maximum-frequency-stack) | 55 | 60 |
| 9 | [Smallest Range Covering Elements from K Lists](#smallest-range-covering-elements-from-k-lists) | 55 | 65 |
| 10 | [Trapping Rain Water II](#trapping-rain-water-ii) | 55 | 70 |
| 11 | [Rearrange Barcodes](#rearrange-barcodes) | 50 | 50 |
| 12 | [Find the Most Competitive Subsequence](#find-the-most-competitive-subsequence) | 50 | 55 |
| 13 | [IPO](#ipo) | 50 | 65 |
| 14 | [Minimize Deviation in Array](#minimize-deviation-in-array) | 50 | 70 |
---
A heap is a specialized binary tree-based data structure that satisfies the heap property. In a max heap, for any given node I, the value of I is greater than or equal to the values of its children, and the highest value is at the root. In a min heap, the value of I is less than or equal to the values of its children, and the lowest value is at the root. Heaps are commonly used to implement priority queues.
```python
import heapq

# Min-Heap
min_heap = []
heapq.heappush(min_heap, 3)
heapq.heappush(min_heap, 1)
heapq.heappush(min_heap, 2)
print(heapq.heappop(min_heap))  # Output: 1

# Max-Heap
max_heap = []
heapq.heappush(max_heap, -3)
heapq.heappush(max_heap, -1)
heapq.heappush(max_heap, -2)
print(-heapq.heappop(max_heap))  # Output: 3
```
### Insight:
1. Python's `heapq` module only provides a min-heap, but a max-heap can be simulated by inserting the negative of the values.
2. Heaps are useful for algorithms like Dijkstra's or for implementing priority queues.
3. The root element of a heap can be accessed in constant time, making it efficient for finding the minimum or maximum element.
---
 --- 
# Top K Frequent Elements
>Return the k most frequent elements.

>Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
- https://leetcode.com/problems/top-k-frequent-elements/
- Difficulty: 40
- Frequent: 70
- Tags: ['Heap', 'Hash Table']

### Top K Frequent Elements -- Shortest Solution:
```python
from collections import Counter
import heapq

def topKFrequent(nums, k):
    return [item for item, _ in heapq.nlargest(k, Counter(nums).items(), key=lambda x: x[1])]
```
#### TC: O(N log k) **SC:** O(N)

1. The `Counter` from the `collections` module is used to count the frequency of each element in the
list `nums`. 2. `heapq.nlargest` is used to find the `k` elements with the highest frequency. It
uses a heap internally, which ensures that the operation is efficient. 3. The result is a list of
the `k` most frequent elements, extracted from the tuples returned by `heapq.nlargest`.

---
### Top K Frequent Elements -- Best TC Solution:
```python
from collections import Counter
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        return heapq.nlargest(k, count.keys(), key=count.get)
```
#### TC: O(N log K) **SC:** O(N)

The code uses the Counter class from the collections module to count the frequency of each element
in the input list. It then utilizes the heapq module to find the 'k' most frequent elements
efficiently. The nlargest function from heapq is used to get the 'k' largest elements based on the
frequency count. This approach helps in achieving a time complexity of O(N log K) and a space
complexity of O(N), where N is the number of elements in the input list.

---
---
---
 --- 
# Merge Intervals
>Merge all overlapping intervals.

>Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
- https://leetcode.com/problems/merge-intervals/
- Difficulty: 45
- Frequent: 70
- Tags: ['Heap', 'Array', 'Sorting']

### Merge Intervals -- Shortest Solution:
```python
def merge(intervals):
    intervals.sort(key=lambda x: x[0])
    merged = []
    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])
    return merged
```
#### TC: O(n log n) **SC:** O(n)

1. **Sorting**: The intervals are first sorted based on their start times. This helps in easily
merging overlapping intervals. 2. **Merging**: We iterate through the sorted intervals and merge
them if they overlap. If the current interval does not overlap with the last merged interval, it is
simply appended to the result list. 3. **Efficiency**: Sorting takes O(n log n) time, and the
merging process takes O(n) time, making the overall time complexity O(n log n). The space complexity
is O(n) due to the space required to store the merged intervals.

---
---
---
 --- 
# Find Median from Data Stream
>Find the median from a data stream.

>Input: ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
Output: [null,null,null,1.5,null,2.0]
- https://leetcode.com/problems/find-median-from-data-stream/
- Difficulty: 60
- Frequent: 65
- Tags: ['Heap', 'Design']

### Find Median from Data Stream -- Shortest Solution:
```python
import heapq
class MedianFinder:
    def __init__(self):
        self.small = []
        self.large = []
    def addNum(self, num: int) -> None:
        heapq.heappush(self.small, -num)
        if self.small and self.large and -self.small[0] > self.large[0]:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        if len(self.small) > len(self.large) + 1:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        if len(self.large) > len(self.small):
            heapq.heappush(self.small, -heapq.heappop(self.large))
    def findMedian(self) -> float:
        if len(self.small) > len(self.large):
            return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2.0
```
#### TC: O(log n) for addNum, O(1) for findMedian **SC:** O(n)

The code uses two heaps: a max-heap for the lower half of numbers and a min-heap for the upper half.
This allows efficient retrieval of the median. When a new number is added, it is first pushed onto
the max-heap (small). If the max-heap's largest number is greater than the min-heap's smallest
number, the heaps are rebalanced. The findMedian function then checks the sizes of the heaps to
determine whether to return the middle element or the average of the two middle elements.

---
---
---
 --- 
# Task Scheduler
>Given a char array representing tasks, return the least number of intervals the CPU will take to finish all the given tasks.

>Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
- https://leetcode.com/problems/task-scheduler/
- Difficulty: 50
- Frequent: 60
- Tags: ['Heap', 'Greedy', 'Queue']

### Task Scheduler -- Shortest Solution:
```python
from collections import Counter
import heapq

def leastInterval(tasks, n):
    task_counts = list(Counter(tasks).values())
    max_heap = [-count for count in task_counts]
    heapq.heapify(max_heap)
    time = 0
    while max_heap:
        temp = []
        for _ in range(n + 1):
            if max_heap:
                temp.append(heapq.heappop(max_heap))
        for item in temp:
            if item + 1 < 0:
                heapq.heappush(max_heap, item + 1)
        time += n + 1 if max_heap else len(temp)
    return time
```
#### TC: O(N log N) **SC:** O(N)

1. The code uses a max heap to keep track of the most frequent tasks. 2. The heap is used to always
execute the most frequent task available, ensuring the CPU cooldown period is respected. 3. The time
is incremented by `n + 1` for each cycle of task execution, or by the number of tasks executed if
the heap is empty. 4. The use of a heap ensures that the task with the highest remaining count is
always executed first, optimizing the task scheduling.

---
### Task Scheduler -- Best TC Solution:
```python
from collections import Counter
import heapq

class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        counter = Counter(tasks)
        max_freq = max(counter.values())
        max_freq_tasks = sum(1 for task, freq in counter.items() if freq == max_freq)
        return max(len(tasks), (max_freq - 1) * (n + 1) + max_freq_tasks)
```
#### TC: O(n) **SC:** O(1)

The code uses Counter to count the frequency of each task. It then calculates the maximum frequency
and the number of tasks with that frequency. The formula used to calculate the minimum intervals
required is based on the maximum frequency and the cooldown period. The code is concise and easy to
understand, with a time complexity of O(n) and space complexity of O(1).

---
---
---
 --- 
# K Closest Points to Origin
>Find the k closest points to the origin.

>Input: points = [[1,3],[-2,2]], K = 1
Output: [[-2,2]]
- https://leetcode.com/problems/k-closest-points-to-origin/
- Difficulty: 50
- Frequent: 60
- Tags: ['Heap', 'Divide and Conquer', 'Geometry']

### K Closest Points to Origin -- Shortest Solution:
```python
import heapq

def kClosest(points, k):
    return heapq.nsmallest(k, points, key=lambda x: x[0]**2 + x[1]**2)
```
#### TC: O(n log k) **SC:** O(k)

The function uses a min-heap to find the k closest points to the origin. The `heapq.nsmallest`
function efficiently retrieves the k smallest elements based on the Euclidean distance from the
origin, calculated as `x[0]**2 + x[1]**2`. This approach ensures that the solution is both optimal
and easy to understand.

---
### K Closest Points to Origin -- Best TC Solution:
```python
import heapq
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]:
        heap = []
        for x, y in points:
            dist = x*x + y*y
            heapq.heappush(heap, (dist, [x, y]))
        return [point for dist, point in heapq.nsmallest(k, heap)]
```
#### TC: O(N log K) **SC:** O(K)

The code defines a class Solution with a method kClosest that takes a list of points and an integer
k as input. It uses a heap to store the distances of each point from the origin. The heapq.heappush
function is used to push the distances along with the points into the heap. Then, heapq.nsmallest is
used to retrieve the k closest points based on their distances. The time complexity is O(N log K)
where N is the number of points and K is the input parameter k. The space complexity is O(K) to
store the k closest points in the heap.

---
### K Closest Points to Origin -- Best SC Solution:
```python
import heapq
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]:
        heap = []
        for (x, y) in points:
            dist = x*x + y*y
            heapq.heappush(heap, (dist, x, y))
        res = []
        for _ in range(k):
            _, x, y = heapq.heappop(heap)
            res.append([x, y])
        return res
```
#### TC: O(N log N) **SC:** O(N)

The code uses a heap to store the distances of each point from the origin. By pushing the distances
along with the coordinates into the heap, we can maintain the order based on distance. The code then
pops the k closest points from the heap and returns them. The time complexity is O(N log N) due to
the heap operations, and the space complexity is O(N) to store the heap.

---
---
---
 --- 
# Find K Pairs with Smallest Sums
>Find k pairs with the smallest sums.

>Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]]
- https://leetcode.com/problems/find-k-pairs-with-smallest-sums/
- Difficulty: 55
- Frequent: 60
- Tags: ['Heap', 'Two Pointers']

### Find K Pairs with Smallest Sums -- Shortest Solution:
```python
import heapq

def kSmallestPairs(nums1, nums2, k):
    if not nums1 or not nums2 or k == 0:
        return []
    min_heap = []
    for i in range(min(k, len(nums1))):
        heapq.heappush(min_heap, (nums1[i] + nums2[0], i, 0))
    result = []
    while k > 0 and min_heap:
        sum, i, j = heapq.heappop(min_heap)
        result.append((nums1[i], nums2[j]))
        if j + 1 < len(nums2):
            heapq.heappush(min_heap, (nums1[i] + nums2[j + 1], i, j + 1))
        k -= 1
    return result
```
#### TC: O(k log k) **SC:** O(k)

The code uses a min-heap to efficiently find the k pairs with the smallest sums. Initially, it
pushes the first k pairs (nums1[i] + nums2[0]) into the heap. Then, it repeatedly pops the smallest
pair from the heap and pushes the next pair in the sequence (nums1[i] + nums2[j+1]) until k pairs
are found. This ensures that we always get the smallest possible sums in an efficient manner.

---
### Find K Pairs with Smallest Sums -- Best TC Solution:
```python
import heapq
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]:
        if not nums1 or not nums2:
            return []
        heap = []
        for n1 in nums1:
            for n2 in nums2:
                if len(heap) < k:
                    heapq.heappush(heap, (n1 + n2, [n1, n2]))
                else:
                    if n1 + n2 < heap[0][0]:
                        heapq.heappop(heap)
                        heapq.heappush(heap, (n1 + n2, [n1, n2]))
                    else:
                        break
        return [pair[1] for pair in heap]
```
#### TC: O(k * log(min(N, M, k))) **SC:** O(min(N*M, k))

The code uses a heap to efficiently find the k pairs with the smallest sums. It iterates through all
possible pairs from nums1 and nums2, maintaining a heap of size k to store the pairs with the
smallest sums seen so far. The heap ensures that the pairs with the smallest sums are always at the
top. By comparing the sum of the current pair with the smallest sum in the heap, the code updates
the heap accordingly. This approach optimizes the process of finding the k smallest pairs without
sorting the entire list of pairs.

---
---
---
 --- 
# Sort Characters By Frequency
>Sort characters in a string by frequency.

>Input: s = "tree"
Output: "eert"
- https://leetcode.com/problems/sort-characters-by-frequency/
- Difficulty: 45
- Frequent: 55
- Tags: ['Heap', 'Hash Table', 'Sorting']

### Sort Characters By Frequency -- Shortest Solution:
```python
from collections import Counter
import heapq

s = "tree"
count = Counter(s)
max_heap = [(-freq, char) for char, freq in count.items()]
heapq.heapify(max_heap)
result = []
while max_heap:
    freq, char = heapq.heappop(max_heap)
    result.append(char * -freq)
print(''.join(result))
```
#### TC: O(n log n) **SC:** O(n)

1. **Counter**: We use `Counter` from the `collections` module to count the frequency of each
character in the string. 2. **Max-Heap**: We create a max-heap using the negative of the frequencies
to simulate a max-heap using Python's `heapq`, which is a min-heap by default. 3. **Heapify**:
Convert the list into a heap in O(n) time. 4. **Pop and Append**: Continuously pop the heap to get
the character with the highest frequency and append it to the result list. 5. **Join**: Finally,
join the list into a string to get the result.

---
### Sort Characters By Frequency -- Best TC Solution:
```python
from collections import Counter
import heapq

class Solution:
    def frequencySort(self, s: str) -> str:
        freq_map = Counter(s)
        max_heap = [(-freq, char) for char, freq in freq_map.items()]
        heapq.heapify(max_heap)
        result = []
        while max_heap:
            freq, char = heapq.heappop(max_heap)
            result.append(char * (-freq))
        return ''.join(result)
```
#### TC: O(N log N) **SC:** O(N)

The code uses a Counter to count the frequency of each character in the input string. It then
creates a max heap with tuples of (-frequency, character) to sort characters by their frequency. By
popping elements from the heap, it constructs the result string with characters sorted by frequency.
The time complexity is O(N log N) due to heap operations, and the space complexity is O(N) to store
the frequency map and heap.

---
---
---
 --- 
# Maximum Frequency Stack
>Implement a stack that supports push, pop, and retrieving the most frequent element.

>Input: ["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"], [[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
Output: [null,null,null,null,null,null,null,5,7,5,4]
- https://leetcode.com/problems/maximum-frequency-stack/
- Difficulty: 60
- Frequent: 55
- Tags: ['Heap', 'Stack', 'Design']

### Maximum Frequency Stack -- Shortest Solution:
```python
from collections import defaultdict
import heapq

class FreqStack:
    def __init__(self):
        self.freq = defaultdict(int)
        self.heap = []
        self.index = 0

    def push(self, x: int) -> None:
        self.freq[x] += 1
        heapq.heappush(self.heap, (-self.freq[x], -self.index, x))
        self.index += 1

    def pop(self) -> int:
        _, _, x = heapq.heappop(self.heap)
        self.freq[x] -= 1
        if self.freq[x] == 0:
            del self.freq[x]
        return x
```
#### TC: O(log n) for both push and pop operations **SC:** O(n)

The solution uses a heap to keep track of the elements based on their frequency and the order they
were added. The `freq` dictionary keeps count of the frequency of each element. The `heap` stores
tuples of negative frequency, negative index, and the element itself to simulate a max-heap using
Python's min-heap. The `index` ensures that the order of insertion is maintained. When popping, the
element with the highest frequency and most recent insertion is removed from the heap.

---
### Maximum Frequency Stack -- Best TC Solution:
```python
import collections

class FreqStack:
    def __init__(self):
        self.freq = collections.Counter()
        self.group = collections.defaultdict(list)
        self.max_freq = 0

    def push(self, x: int) -> None:
        f = self.freq[x] + 1
        self.freq[x] = f
        if f > self.max_freq:
            self.max_freq = f
        self.group[f].append(x)

    def pop(self) -> int:
        x = self.group[self.max_freq].pop()
        self.freq[x] -= 1
        if not self.group[self.max_freq]:
            self.max_freq -= 1
        return x

# Your FreqStack object will be instantiated and called as such:
# obj = FreqStack()
# obj.push(x)
# param_2 = obj.pop()
```
#### TC: O(1) for both push and pop operations **SC:** O(n)

The code uses a Counter to keep track of the frequency of each element and a defaultdict to group
elements by their frequency. The max_freq variable is updated whenever a new element is pushed. The
push operation increments the frequency of the element and adds it to the corresponding frequency
group. The pop operation retrieves the element with the highest frequency, decrements its frequency,
and updates the max_freq if needed. This approach allows for constant time complexity for both push
and pop operations.

---
---
---
 --- 
# Smallest Range Covering Elements from K Lists
>Find the smallest range that includes at least one number from each of the k lists.

>Input: nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]
Output: [20,24]
- https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/
- Difficulty: 65
- Frequent: 55
- Tags: ['Heap', 'Sliding Window']

### Smallest Range Covering Elements from K Lists -- Shortest Solution:
```python
import heapq

def smallestRange(nums):
    min_heap = []
    max_val = float('-inf')
    for i, row in enumerate(nums):
        heapq.heappush(min_heap, (row[0], i, 0))
        max_val = max(max_val, row[0])
    range_start, range_end = float('-inf'), float('inf')
    while min_heap:
        min_val, r, c = heapq.heappop(min_heap)
        if max_val - min_val < range_end - range_start:
            range_start, range_end = min_val, max_val
        if c + 1 == len(nums[r]):
            break
        next_val = nums[r][c + 1]
        heapq.heappush(min_heap, (next_val, r, c + 1))
        max_val = max(max_val, next_val)
    return [range_start, range_end]
```
#### TC: O(N log K) **SC:** O(K)

1. **Heap Initialization**: We initialize a min-heap with the first element of each list and track
the maximum value among these elements. 2. **Heap Operations**: We continuously pop the smallest
element from the heap and push the next element from the same list into the heap. This ensures that
the heap always contains the smallest current elements from each list. 3. **Range Update**: After
each pop operation, we update the smallest range if the current range is smaller than the previously
recorded range. 4. **Termination**: The process terminates when we reach the end of any list,
ensuring that all lists are covered in the range.

---
### Smallest Range Covering Elements from K Lists -- Best TC Solution:
```python
import heapq

def smallestRange(nums):
    pq = [(row[0], i, 0) for i, row in enumerate(nums)]
    heapq.heapify(pq)
    right = max(row[0] for row in nums)
    res = (float('-inf'), float('inf'))
    while pq:
        left, i, j = heapq.heappop(pq)
        if right - left < res[1] - res[0]:
            res = (left, right)
        if j + 1 == len(nums[i]):
            return res
        v = nums[i][j + 1]
        right = max(right, v)
        heapq.heappush(pq, (v, i, j + 1))
    return res
```
#### TC: O(n log k) **SC:** O(k)

The code uses a heap approach to efficiently find the smallest range covering elements from k lists.
It initializes a priority queue with the first element from each list and iterates while updating
the range. The time complexity is O(n log k) where n is the total number of elements and k is the
number of lists. The space complexity is O(k) to store the priority queue.

---
---
---
 --- 
# Trapping Rain Water II
>Calculate the total amount of rainwater trapped in a 2D elevation map.

>Input: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
Output: 4
- https://leetcode.com/problems/trapping-rain-water-ii/
- Difficulty: 70
- Frequent: 55
- Tags: ['Heap', 'Breadth-First Search']

### Trapping Rain Water II -- Shortest Solution:
```python
import heapq

def trapRainWater(heightMap):
    if not heightMap or not heightMap[0]:
        return 0
    m, n = len(heightMap), len(heightMap[0])
    visited = [[False] * n for _ in range(m)]
    heap = []
    for i in range(m):
        for j in range(n):
            if i == 0 or i == m - 1 or j == 0 or j == n - 1:
                heapq.heappush(heap, (heightMap[i][j], i, j))
                visited[i][j] = True
    water = 0
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    while heap:
        height, x, y = heapq.heappop(heap)
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < m and 0 <= ny < n and not visited[nx][ny]:
                water += max(0, height - heightMap[nx][ny])
                heapq.heappush(heap, (max(height, heightMap[nx][ny]), nx, ny))
                visited[nx][ny] = True
    return water
```
#### TC: O(m * n * log(m * n)) **SC:** O(m * n)

The code uses a min-heap to keep track of the boundary cells and their heights. It starts by pushing
all the boundary cells into the heap and marking them as visited. Then, it processes each cell in
the heap, exploring its neighbors. If a neighbor hasn't been visited, it calculates the trapped
water (if any) and pushes the neighbor into the heap with the updated height. This ensures that the
water is trapped between the boundaries efficiently.

---
### Trapping Rain Water II -- Best TC Solution:
```python
# Trapping Rain Water II
from heapq import heappush, heappop

def trapRainWater(heightMap):
    if not heightMap or not heightMap[0]:
        return 0
    m, n = len(heightMap), len(heightMap[0])
    visited = [[0] * n for _ in range(m)]
    heap = []
    for i in range(m):
        for j in range(n):
            if i == 0 or i == m - 1 or j == 0 or j == n - 1:
                heappush(heap, (heightMap[i][j], i, j))
                visited[i][j] = 1
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    water = 0
    while heap:
        h, i, j = heappop(heap)
        for dx, dy in directions:
            x, y = i + dx, j + dy
            if 0 <= x < m and 0 <= y < n and not visited[x][y]:
                visited[x][y] = 1
                water += max(0, h - heightMap[x][y])
                heappush(heap, (max(h, heightMap[x][y]), x, y))
    return water
```
#### TC: O(m * n * log(m + n)) **SC:** O(m * n)

The code implements a solution for the Trapping Rain Water II problem using a heap approach. It
initializes a heap with the boundary cells and their heights, then iterates through the heap while
updating the water trapped based on the surrounding cells. The visited array is used to keep track
of visited cells to avoid revisiting them. The code efficiently calculates the trapped water by
considering the heights of neighboring cells. The time complexity is O(m * n * log(m + n)) due to
the heap operations, and the space complexity is O(m * n) for the visited array.

---
---
---
 --- 
# Rearrange Barcodes
>Rearrange barcodes so that no two adjacent barcodes are the same.

>Input: barcodes = [1,1,1,2,2,3]
Output: [1,3,1,2,1,2]
- https://leetcode.com/problems/rearrange-barcodes/
- Difficulty: 50
- Frequent: 50
- Tags: ['Heap', 'Greedy', 'Sorting']

### Rearrange Barcodes -- Shortest Solution:
```python
from collections import Counter
import heapq

def rearrangeBarcodes(barcodes):
    count = Counter(barcodes)
    max_heap = [(-freq, num) for num, freq in count.items()]
    heapq.heapify(max_heap)
    prev_freq, prev_num = 0, 0
    result = []
    while max_heap:
        freq, num = heapq.heappop(max_heap)
        result.append(num)
        if prev_freq < 0:
            heapq.heappush(max_heap, (prev_freq, prev_num))
        prev_freq, prev_num = freq + 1, num
    return result
```
#### TC: O(N log N) **SC:** O(N)

1. **Counter**: We use `Counter` to count the frequency of each barcode. 2. **Max-Heap**: We use a
max-heap to always get the barcode with the highest frequency. Python's `heapq` is a min-heap by
default, so we store negative frequencies to simulate a max-heap. 3. **Rearranging**: We pop the
most frequent barcode and append it to the result list. We then push the previous barcode back into
the heap if it still has a remaining count. 4. **Result**: This ensures that no two adjacent
barcodes are the same, as we always place the most frequent remaining barcode next.

---
### Rearrange Barcodes -- Best TC Solution:
```python
from collections import Counter
from heapq import heappush, heappop

def rearrange_barcodes(barcodes):
    counter = Counter(barcodes)
    heap = [(-freq, num) for num, freq in counter.items()]
    heapq.heapify(heap)
    result = []
    while len(result) < len(barcodes):
        if len(heap) >= 2:
            freq1, num1 = heapq.heappop(heap)
            freq2, num2 = heapq.heappop(heap)
            result.extend([num1, num2])
            if freq1 + 1 < 0:
                heapq.heappush(heap, (freq1 + 1, num1))
            if freq2 + 1 < 0:
                heapq.heappush(heap, (freq2 + 1, num2))
        else:
            freq, num = heapq.heappop(heap)
            result.append(num)
            if freq + 1 < 0:
                heapq.heappush(heap, (freq + 1, num))
    return result
```
#### TC: O(n log n) **SC:** O(n)

The code uses a heap approach to rearrange the barcodes in a way that no two adjacent barcodes are
the same. It first counts the frequency of each barcode using a Counter. Then, it creates a max heap
with tuples of (-frequency, barcode) to prioritize the most frequent barcodes. It iteratively pops
two barcodes from the heap, adds them to the result list, and updates their frequencies. If there is
only one barcode left in the heap, it adds it to the result list. The process continues until all
barcodes are rearranged. The time complexity is O(n log n) due to heap operations, and the space
complexity is O(n) to store the heap and result list.

---
---
---
 --- 
# Find the Most Competitive Subsequence
>Find the most competitive subsequence of a given length.

>Input: nums = [3,5,2,6], k = 2
Output: [2,6]
- https://leetcode.com/problems/find-the-most-competitive-subsequence/
- Difficulty: 55
- Frequent: 50
- Tags: ['Heap', 'Greedy', 'Stack']

### Find the Most Competitive Subsequence -- Shortest Solution:
```python
def mostCompetitive(nums, k):
    stack = []
    to_remove = len(nums) - k
    for num in nums:
        while stack and to_remove and stack[-1] > num:
            stack.pop()
            to_remove -= 1
        stack.append(num)
    return stack[:k]
```
#### TC: O(n) **SC:** O(k)

The solution uses a stack to maintain the most competitive subsequence. The `to_remove` variable
keeps track of how many elements we can still remove to ensure the subsequence length is `k`. As we
iterate through the `nums` array, we pop elements from the stack if they are greater than the
current element and we still have elements left to remove. This ensures that the stack always
contains the smallest possible elements in the correct order. Finally, we return the first `k`
elements of the stack.

---
### Find the Most Competitive Subsequence -- Best TC Solution:
```python
from collections import deque
def mostCompetitive(nums, k):
    stack = []
    n = len(nums)
    for i, num in enumerate(nums):
        while stack and stack[-1] > num and len(stack) - 1 + n - i >= k:
            stack.pop()
        if len(stack) < k:
            stack.append(num)
    return stack
```
#### TC: O(N) **SC:** O(K)

The code uses a stack to maintain the most competitive subsequence. It iterates through the input
array and keeps track of the elements in the stack based on certain conditions. The stack is used to
ensure that the subsequence remains competitive by removing elements that are greater than the
current element and maintaining the required length of the subsequence. The time complexity is O(N)
where N is the length of the input array, and the space complexity is O(K) where K is the size of
the subsequence.

---
---
---
 --- 
# IPO
>Find the maximum capital that can be achieved by completing at most k distinct projects.

>Input: k = 2, W = 0, Profits = [1,2,3], Capital = [0,1,1]
Output: 4
- https://leetcode.com/problems/ipo/
- Difficulty: 65
- Frequent: 50
- Tags: ['Heap', 'Greedy']

### IPO -- Shortest Solution:
```python
import heapq

def findMaximizedCapital(k, W, Profits, Capital):
    projects = sorted(zip(Capital, Profits))
    max_heap = []
    i = 0
    for _ in range(k):
        while i < len(projects) and projects[i][0] <= W:
            heapq.heappush(max_heap, -projects[i][1])
            i += 1
        if max_heap:
            W -= heapq.heappop(max_heap)
        else:
            break
    return W
```
#### TC: O(n log n + k log n) **SC:** O(n)

1. The code first sorts the projects based on their capital requirements. 2. It uses a max-heap to
keep track of the most profitable projects that can be started with the current available capital.
3. For each of the k rounds, it adds all feasible projects to the heap and then selects the most
profitable one to execute. 4. The process continues until either k projects have been executed or no
more projects can be started with the available capital.

---
### IPO -- Best TC Solution:
```python
import heapq
def findMaximizedCapital(k, W, Profits, Capital):
    projects = sorted(zip(Profits, Capital), key=lambda x: x[1])
    pq = []
    i = 0
    for _ in range(k):
        while i < len(projects) and projects[i][1] <= W:
            heapq.heappush(pq, -projects[i][0])
            i += 1
        if pq:
            W -= heapq.heappop(pq)
        else:
            break
    return W
```
#### TC: O(NlogN + KlogN) **SC:** O(N)

The code uses a heap approach to efficiently select projects based on their capital and profits. It
sorts the projects by capital and iterates through them, pushing profitable projects onto a priority
queue. It then selects the most profitable project that can be completed with the current capital.
The time complexity is O(NlogN + KlogN) where N is the number of projects and K is the number of
selected projects. The space complexity is O(N) due to the priority queue storing the profits of
projects.

---
---
---
 --- 
# Minimize Deviation in Array
>Minimize the deviation in an array.

>Input: nums = [1,2,3,4]
Output: 1
- https://leetcode.com/problems/minimize-deviation-in-array/
- Difficulty: 70
- Frequent: 50
- Tags: ['Heap', 'Greedy']

### Minimize Deviation in Array -- Shortest Solution:
```python
import heapq

def minimumDeviation(nums):
    nums = [-x*2 if x % 2 else -x for x in nums]
    heapq.heapify(nums)
    min_val = -max(nums)
    min_deviation = float('inf')
    while True:
        max_val = -heapq.heappop(nums)
        min_deviation = min(min_deviation, max_val - min_val)
        if max_val % 2:
            break
        max_val //= 2
        min_val = min(min_val, max_val)
        heapq.heappush(nums, -max_val)
    return min_deviation
```
#### TC: O(n log n) **SC:** O(n)

1. The code uses a max-heap to keep track of the maximum values efficiently. 2. All numbers are
initially converted to even by multiplying odd numbers by 2. 3. The heap is used to repeatedly
extract the maximum value and minimize it by dividing by 2 until it becomes odd. 4. The minimum
deviation is updated in each iteration. 5. The process stops when the maximum value is odd, ensuring
the smallest possible deviation.

---
### Minimize Deviation in Array -- Best TC Solution:
```python
import heapq
candidates = []
for num in nums:
    if num % 2 == 1:
        candidates.append(num * 2)
    else:
        candidates.append(num)
heapq.heapify(candidates)
res = float('inf')
while len(candidates) == len(nums):
    x = heapq.heappop(candidates)
    res = min(res, x - candidates[0])
    if x % 2 == 0:
        heapq.heappush(candidates, x // 2)
return res
```
#### TC: O(NlogN) **SC:** O(N)

The code first creates a list 'candidates' where odd numbers are doubled and added, while even
numbers are added as is. The list is then converted into a min heap using heapq.heapify(). The code
then iterates through the heap, updating the result 'res' by calculating the minimum difference
between the current element and the smallest element in the heap. If the current element is even, it
is divided by 2 and added back to the heap. The loop continues until the heap size matches the input
list size. Finally, the minimum deviation 'res' is returned.

---
---
---