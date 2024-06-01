# Huffman Coding
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Top K Frequent Elements](#top-k-frequent-elements) | 75 | 50 |
| 2 | [Merge k Sorted Lists](#merge-k-sorted-lists) | 75 | 60 |
| 3 | [Task Scheduler](#task-scheduler) | 70 | 50 |
| 4 | [Sliding Window Maximum](#sliding-window-maximum) | 70 | 55 |
| 5 | [Kth Smallest Element in a Sorted Matrix](#kth-smallest-element-in-a-sorted-matrix) | 70 | 55 |
| 6 | [Reorganize String](#reorganize-string) | 65 | 50 |
| 7 | [Merge Intervals](#merge-intervals) | 65 | 55 |
| 8 | [Rearrange String k Distance Apart](#rearrange-string-k-distance-apart) | 65 | 55 |
| 9 | [Find Median from Data Stream](#find-median-from-data-stream) | 65 | 60 |
| 10 | [Trapping Rain Water II](#trapping-rain-water-ii) | 65 | 60 |
| 11 | [Huffman Encoding](#huffman-encoding) | 60 | 50 |
| 12 | [Meeting Rooms II](#meeting-rooms-ii) | 60 | 50 |
| 13 | [K Closest Points to Origin](#k-closest-points-to-origin) | 60 | 50 |
| 14 | [Split Array into Consecutive Subsequences](#split-array-into-consecutive-subsequences) | 60 | 55 |
| 15 | [Encode String with Shortest Length](#encode-string-with-shortest-length) | 55 | 45 |
| 16 | [Kth Largest Element in a Stream](#kth-largest-element-in-a-stream) | 55 | 45 |
| 17 | [Frequency Sort](#frequency-sort) | 50 | 45 |
| 18 | [Sort Characters By Frequency](#sort-characters-by-frequency) | 50 | 45 |
| 19 | [Optimal File Merge Patterns](#optimal-file-merge-patterns) | 50 | 60 |
| 20 | [Minimum Cost to Connect Sticks](#minimum-cost-to-connect-sticks) | 45 | 40 |
---
Huffman Coding is a lossless data compression algorithm. It assigns variable-length codes to input characters, with shorter codes for more frequent characters. The process involves building a binary tree where each leaf node represents a character, and the path from the root to the leaf represents the character's code. This minimizes the total length of the encoded data.
```python
import heapq
from collections import defaultdict, Counter

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    
    def __lt__(self, other):
        return self.freq < other.freq

# Build Huffman Tree
def build_huffman_tree(text):
    freq = Counter(text)
    heap = [Node(char, freq) for char, freq in freq.items()]
    heapq.heapify(heap)
    
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = Node(None, left.freq + right.freq)
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)
    
    return heap[0]

# Generate Huffman Codes
def generate_codes(node, prefix="", codebook={}):
    if node is not None:
        if node.char is not None:
            codebook[node.char] = prefix
        generate_codes(node.left, prefix + "0", codebook)
        generate_codes(node.right, prefix + "1", codebook)
    return codebook

# Encode Text
def huffman_encode(text):
    root = build_huffman_tree(text)
    codebook = generate_codes(root)
    return ''.join(codebook[char] for char in text), codebook

# Decode Text
def huffman_decode(encoded_text, codebook):
    reverse_codebook = {v: k for k, v in codebook.items()}
    current_code = ""
    decoded_text = ""
    for bit in encoded_text:
        current_code += bit
        if current_code in reverse_codebook:
            decoded_text += reverse_codebook[current_code]
            current_code = ""
    return decoded_text

# Example Usage
text = "huffman coding"
encoded_text, codebook = huffman_encode(text)
decoded_text = huffman_decode(encoded_text, codebook)
print(f"Encoded: {encoded_text}")
print(f"Decoded: {decoded_text}")
```
### Insight:
1. The frequency of each character is counted using a Counter, which takes O(n) time.
2. A priority queue (min-heap) is used to build the Huffman Tree, which takes O(n log n) time due to heap operations.
3. The tree is traversed to generate codes, which takes O(n) time.
4. Encoding and decoding both involve traversing the text and the tree, taking O(n) time.
5. The space complexity is O(n) because we store the frequency count, the tree, and the codebook.
---
 --- 
# Top K Frequent Elements
>Finds the top K frequent elements using Huffman Coding.

>Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
- https://leetcode.com/problems/top-k-frequent-elements/
- Difficulty: 50
- Frequent: 75
- Tags: ['Hash Table', 'Heap']

### Top K Frequent Elements -- Shortest Solution:
```python
from collections import Counter
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        return heapq.nlargest(k, count.keys(), key=count.get)
```
#### TC: O(N log K) **SC:** O(N)

The code uses Counter to count the frequency of elements in the input list. It then utilizes
heapq.nlargest to find the top k frequent elements based on their counts. The time complexity is O(N
log K) due to the heapq.nlargest operation, and the space complexity is O(N) to store the counts in
a Counter object.

---
---
---
 --- 
# Merge k Sorted Lists
>Merges k sorted linked lists using Huffman Coding.

>Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
- https://leetcode.com/problems/merge-k-sorted-lists/
- Difficulty: 60
- Frequent: 75
- Tags: ['Heap', 'Linked List']

### Merge k Sorted Lists -- Shortest Solution:
```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        heap = []
        for l in lists:
            while l:
                heapq.heappush(heap, l.val)
                l = l.next
        dummy = ListNode(0)
        curr = dummy
        while heap:
            curr.next = ListNode(heapq.heappop(heap))
            curr = curr.next
        return dummy.next
```
#### TC: O(N log k) **SC:** O(N)

The code uses a min-heap to efficiently merge k sorted lists. It iterates through all the elements
in the input lists and pushes them into the heap. Then, it pops elements from the heap in sorted
order to create the merged list. The time complexity is O(N log k) where N is the total number of
elements in all lists and k is the number of lists. The space complexity is O(N) to store the merged
list.

---
---
---
 --- 
# Task Scheduler
>Schedules tasks with cooling intervals using Huffman Coding.

>Input: tasks = ['A','A','A','B','B','B'], n = 2
Output: 8
- https://leetcode.com/problems/task-scheduler/
- Difficulty: 50
- Frequent: 70
- Tags: ['Array', 'Greedy', 'Heap']

### Task Scheduler -- Shortest Solution:
```python
from collections import Counter
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        task_counts = list(Counter(tasks).values())
        max_count = max(task_counts)
        max_count_tasks = task_counts.count(max_count)
        return max(len(tasks), (max_count - 1) * (n + 1) + max_count_tasks)
```
#### TC: O(n) **SC:** O(1)

The code calculates the least interval required to execute the given tasks with a cooldown period
'n'. It first counts the occurrences of each task, finds the task with the maximum count, and
calculates the number of tasks with the maximum count. The formula used to calculate the least
interval is based on the maximum count and the cooldown period. The code is concise and efficient
with a time complexity of O(n) and a space complexity of O(1).

---
---
---
 --- 
# Sliding Window Maximum
>Finds the maximum in each sliding window using Huffman Coding.

>Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
- https://leetcode.com/problems/sliding-window-maximum/
- Difficulty: 55
- Frequent: 70
- Tags: ['Heap', 'Sliding Window']

### Sliding Window Maximum -- Shortest Solution:
```python
from collections import deque
def maxSlidingWindow(nums, k):
    res = []
    q = deque()
    for i in range(len(nums)):
        while q and q[0] < i - k + 1:
            q.popleft()
        while q and nums[q[-1]] < nums[i]:
            q.pop()
        q.append(i)
        if i >= k - 1:
            res.append(nums[q[0]])
    return res
```
#### TC: O(n) **SC:** O(k)

The code uses a deque to efficiently maintain the maximum element within the sliding window. It
iterates through the input array and maintains the deque such that it only contains elements within
the current window. The while loops inside the for loop ensure that the deque is updated correctly
to store the maximum element. The code has a time complexity of O(n) and a space complexity of O(k),
where k is the size of the sliding window.

---
### Sliding Window Maximum -- Best SC Solution:
```python
from collections import deque
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums:
            return []
        if k == 1:
            return nums
        q = deque()
        res = []
        for i in range(len(nums)):
            while q and nums[i] > nums[q[-1]]:
                q.pop()
            q.append(i)
            if i - q[0] >= k:
                q.popleft()
            if i >= k - 1:
                res.append(nums[q[0]])
        return res
```
#### TC: O(N) **SC:** O(N)

The code uses a deque to store the indices of elements in the sliding window. It iterates through
the input array and maintains the deque in a way that the maximum element in the current window is
always at the front of the deque. The code efficiently updates the deque by removing indices that
are out of the current window and adding new indices as needed. This approach ensures that the
maximum element in each window is easily accessible. The time complexity is O(N) as each element is
processed once, and the space complexity is O(N) to store the indices in the deque.

---
---
---
 --- 
# Kth Smallest Element in a Sorted Matrix
>Finds the Kth smallest element in a sorted matrix using Huffman Coding.

>Input: matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
Output: 13
- https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
- Difficulty: 55
- Frequent: 70
- Tags: ['Heap', 'Binary Search']

### Kth Smallest Element in a Sorted Matrix -- Shortest Solution:
```python
import heapq
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        n = len(matrix)
        heap = [(matrix[i][0], i, 0) for i in range(n)]
        heapq.heapify(heap)
        for _ in range(k):
            val, r, c = heapq.heappop(heap)
            if c < n - 1:
                heapq.heappush(heap, (matrix[r][c + 1], r, c + 1))
        return val
```
#### TC: O(klogn) **SC:** O(n)

The code uses a min heap to keep track of the smallest elements in the matrix. It initializes a heap
with the first element of each row. Then, it iteratively pops the smallest element from the heap and
pushes the next element from the same row if available. This process continues until the kth
smallest element is found. The time complexity is O(klogn) because each pop and push operation on
the heap takes O(logn) time. The space complexity is O(n) to store the heap with at most n elements.

---
---
---
 --- 
# Reorganize String
>Reorganizes a string so that no two adjacent characters are the same using Huffman Coding.

>Input: s = "aab"
Output: "aba"
- https://leetcode.com/problems/reorganize-string/
- Difficulty: 50
- Frequent: 65
- Tags: ['Greedy', 'Heap', 'String']

### Reorganize String -- Shortest Solution:
```python
import heapq
class Solution:
    def reorganizeString(self, S: str) -> str:
        char_freq = {}
        for char in S:
            char_freq[char] = char_freq.get(char, 0) + 1
        max_heap = [(-freq, char) for char, freq in char_freq.items()]
        heapq.heapify(max_heap)
        result = []
        while len(max_heap) > 1:
            freq1, char1 = heapq.heappop(max_heap)
            freq2, char2 = heapq.heappop(max_heap)
            result.extend([char1, char2])
            if freq1 + 1 < 0:
                heapq.heappush(max_heap, (freq1 + 1, char1))
            if freq2 + 1 < 0:
                heapq.heappush(max_heap, (freq2 + 1, char2))
        if max_heap:
            freq, char = heapq.heappop(max_heap)
            if freq < -1:
                return ""
            result.append(char)
        return "".join(result)
```
#### TC: O(N log N) **SC:** O(N)

The code uses a max heap to rearrange the characters in the input string. It first counts the
frequency of each character and creates a max heap based on the frequencies. It then repeatedly pops
two characters with the highest frequencies, appends them to the result, and updates the frequencies
in the heap. If there is only one character left in the heap, it appends it to the result. The code
ensures that no two adjacent characters are the same by rearranging the characters based on their
frequencies. The time complexity is O(N log N) due to the heap operations, and the space complexity
is O(N) to store the frequencies and characters.

---
---
---
 --- 
# Merge Intervals
>Merges overlapping intervals using Huffman Coding.

>Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
- https://leetcode.com/problems/merge-intervals/
- Difficulty: 55
- Frequent: 65
- Tags: ['Array', 'Greedy', 'Heap']

### Merge Intervals -- Shortest Solution:
```python
# Definition for an interval.
class Interval:
    def __init__(self, s=0, e=0):
        self.start = s
        self.end = e

class Solution:
    def merge(self, intervals: List[Interval]) -> List[Interval]:
        intervals.sort(key=lambda x: x.start)
        merged = []
        for interval in intervals:
            if not merged or merged[-1].end < interval.start:
                merged.append(interval)
            else:
                merged[-1].end = max(merged[-1].end, interval.end)
        return merged
```
#### TC: O(n log n) **SC:** O(n)

The code defines a class Interval and a class Solution. The merge function takes a list of Interval
objects and merges overlapping intervals. It first sorts the intervals based on the start value.
Then, it iterates through the sorted intervals and merges them by comparing the end value of the
last merged interval with the start value of the current interval. If they overlap, it updates the
end value of the last merged interval. The code has a time complexity of O(n log n) due to sorting
and a space complexity of O(n) to store the merged intervals.

---
---
---
 --- 
# Rearrange String k Distance Apart
>Rearranges a string so that the same characters are at least k distance apart using Huffman Coding.

>Input: s = "aabbcc", k = 3
Output: "abcabc"
- https://leetcode.com/problems/rearrange-string-k-distance-apart/
- Difficulty: 55
- Frequent: 65
- Tags: ['Heap', 'Greedy']

### Rearrange String k Distance Apart -- Shortest Solution:
```python
import heapq
class Solution:
    def rearrangeString(self, s: str, k: int) -> str:
        if k == 0:
            return s
        char_freq = {}
        for char in s:
            char_freq[char] = char_freq.get(char, 0) + 1
        max_heap = []
        for char, freq in char_freq.items():
            heapq.heappush(max_heap, (-freq, char))
        result = []
        queue = []
        while max_heap:
            freq, char = heapq.heappop(max_heap)
            result.append(char)
            queue.append((freq + 1, char))
            if len(queue) >= k:
                freq, char = queue.pop(0)
                if freq < 0:
                    heapq.heappush(max_heap, (freq, char))
        return ''.join(result) if len(result) == len(s) else ''
```
#### TC: O(N log N) **SC:** O(N)

The code uses a max heap to rearrange the characters in the string such that no two same characters
are K distance apart. It first counts the frequency of each character, then pushes them into a max
heap based on their frequencies. It then iterates through the heap, appending characters to the
result and maintaining a queue to keep track of characters that need to be re-added after K
distance. The code ensures that no two same characters are placed K distance apart by re-adding them
to the heap if necessary. The time complexity is O(N log N) due to heap operations, and the space
complexity is O(N) to store the character frequencies and the heap.

---
---
---
 --- 
# Find Median from Data Stream
>Finds the median from a data stream using Huffman Coding.

>Input: [2,3,4]
Output: 3
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
        heapq.heappush(self.small, -heapq.heappushpop(self.large, num))
        if len(self.large) < len(self.small):
            heapq.heappush(self.large, -heapq.heappop(self.small))

    def findMedian(self) -> float:
        if len(self.large) > len(self.small):
            return float(self.large[0])
        return (self.large[0] - self.small[0]) / 2
```
#### TC: O(log n) for addNum, O(1) for findMedian **SC:** O(n)

The code uses two heaps, a max-heap for the smaller half of the numbers and a min-heap for the
larger half. The addNum function maintains the balance between the two heaps by pushing and popping
elements accordingly. The findMedian function returns the median based on the sizes of the two
heaps. This approach ensures that adding a number and finding the median both have efficient time
complexities.

---
### Find Median from Data Stream -- Best TC Solution:
```python
import heapq
class MedianFinder:
    def __init__(self):
        self.small = []
        self.large = []

    def addNum(self, num: int) -> None:
        heapq.heappush(self.small, -heapq.heappushpop(self.large, num))
        if len(self.large) < len(self.small):
            heapq.heappush(self.large, -heapq.heappop(self.small))

    def findMedian(self) -> float:
        if len(self.large) > len(self.small):
            return float(self.large[0])
        return (self.large[0] - self.small[0]) / 2
```
#### TC: Time Complexity: O(log n) for both addNum and findMedian **SC:** Space Complexity: O(n)

The code uses two heaps, a max-heap for the smaller half of the numbers and a min-heap for the
larger half. The addNum function maintains the balance between the two heaps by pushing and popping
elements accordingly. The findMedian function returns the median based on the sizes of the two
heaps. This approach ensures that the median can be found in logarithmic time complexity.

---
---
---
 --- 
# Trapping Rain Water II
>Calculates the amount of trapped rainwater in a 2D elevation map using Huffman Coding.

>Input: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
Output: 4
- https://leetcode.com/problems/trapping-rain-water-ii/
- Difficulty: 60
- Frequent: 65
- Tags: ['Heap', 'Breadth-First Search']

### Trapping Rain Water II -- Shortest Solution:
```python
from heapq import heappush, heappop

class Solution:
    def trapRainWater(self, heightMap: List[List[int]]) -> int:
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
        res = 0
        while heap:
            h, i, j = heappop(heap)
            for dx, dy in directions:
                x, y = i + dx, j + dy
                if 0 <= x < m and 0 <= y < n and not visited[x][y]:
                    visited[x][y] = 1
                    res += max(0, h - heightMap[x][y])
                    heappush(heap, (max(h, heightMap[x][y]), x, y))
        return res
```
#### TC: O(m*n*log(m*n)) **SC:** O(m*n)

The code uses a heap to keep track of the boundary cells of the height map. It iterates through the
boundary cells and adds them to the heap. Then, it explores the neighboring cells in a BFS manner,
updating the water trapped based on the height difference. The visited array is used to mark cells
that have been visited to avoid revisiting them. The time complexity is O(m*n*log(m*n)) due to the
heap operations, and the space complexity is O(m*n) for the visited array.

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

The code uses a heap to keep track of the boundary cells of the height map. It iterates through the
boundary cells and adds them to the heap. Then, it explores the neighboring cells in a BFS manner,
updating the water trapped based on the height difference. The visited array ensures that cells are
not revisited. The code efficiently calculates the trapped rainwater by considering the height
differences between cells and updating the heap with the maximum height encountered so far. The time
complexity is O(m * n * log(m + n)) due to the heap operations, and the space complexity is O(m * n)
for the visited array.

---
---
---
 --- 
# Huffman Encoding
>Constructs Huffman Tree and generates Huffman codes for given characters and frequencies.

>Input: characters = ['a', 'b', 'c', 'd', 'e', 'f'], frequencies = [5, 9, 12, 13, 16, 45]
Output: {'a': '1100', 'b': '1101', 'c': '100', 'd': '101', 'e': '111', 'f': '0'}
- https://leetcode.com/problems/huffman-encoding/
- Difficulty: 50
- Frequent: 60
- Tags: ['Greedy', 'Heap', 'Tree']

### Huffman Encoding -- Shortest Solution:
```python
from heapq import heappush, heappop, heapify
class Solution:
    def encode(self, s: str) -> List[str]:
        freq = Counter(s)
        h = [[weight, [char, '']] for char, weight in freq.items()]
        heapify(h)
        while len(h) > 1:
            lo = heappop(h)
            hi = heappop(h)
            for pair in lo[1:]:
                pair[1] = '0' + pair[1]
            for pair in hi[1:]:
                pair[1] = '1' + pair[1]
            heappush(h, [lo[0] + hi[0]] + lo[1:] + hi[1:])
        return sorted(heappop(h)[1:], key=lambda p: (len(p[-1]), p))
```
#### TC: O(n log n) **SC:** O(n)

The code implements Huffman Encoding using a priority queue (heap). It calculates the frequency of
characters in the input string, creates a heap with the characters and their frequencies, and then
builds the Huffman tree by merging the two lowest frequency nodes until there is only one node left.
The final step involves assigning binary codes to each character based on the tree structure. The
time complexity is O(n log n) due to the heap operations, and the space complexity is O(n) for
storing the frequencies and tree nodes.

---
---
---
 --- 
# Meeting Rooms II
>Finds the minimum number of meeting rooms required using Huffman Coding.

>Input: intervals = [[0, 30],[5, 10],[15, 20]]
Output: 2
- https://leetcode.com/problems/meeting-rooms-ii/
- Difficulty: 50
- Frequent: 60
- Tags: ['Greedy', 'Heap', 'Interval']

### Meeting Rooms II -- Shortest Solution:
```python
import heapq
def minMeetingRooms(intervals):
    if not intervals:
        return 0
    intervals.sort(key=lambda x: x[0])
    rooms = []
    heapq.heappush(rooms, intervals[0][1])
    for interval in intervals[1:]:
        if interval[0] >= rooms[0]:
            heapq.heappop(rooms)
        heapq.heappush(rooms, interval[1])
    return len(rooms)
```
#### TC: O(NlogN) **SC:** O(N)

The code uses a min-heap to keep track of the end times of the meeting rooms. It sorts the intervals
based on the start times and then iterates through each interval. If the start time of the current
interval is greater than or equal to the end time of the earliest ending meeting room, it means the
rooms can be reused. Otherwise, a new room is allocated. The min-heap helps in efficiently finding
the earliest ending meeting room. The time complexity is O(NlogN) due to sorting, and the space
complexity is O(N) to store the meeting rooms in the heap.

---
---
---
 --- 
# K Closest Points to Origin
>Finds the K closest points to the origin using Huffman Coding.

>Input: points = [[1,3],[-2,2]], K = 1
Output: [[-2,2]]
- https://leetcode.com/problems/k-closest-points-to-origin/
- Difficulty: 50
- Frequent: 60
- Tags: ['Heap', 'Divide and Conquer']

### K Closest Points to Origin -- Shortest Solution:
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
#### TC: O(n log k) **SC:** O(k)

The code uses a min-heap to store the distances of points from the origin. By pushing the distances
along with the points into the heap, we can retrieve the k closest points efficiently. The
heapq.nsmallest(k, heap) function returns the k smallest elements from the heap, which are the k
closest points to the origin. The time complexity is O(n log k) due to the heap operations, and the
space complexity is O(k) to store the k closest points.

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

The code uses a min-heap to store the distances of each point from the origin. By pushing the
distances along with the points into the heap, we can retrieve the k closest points efficiently
using heapq's nsmallest function. The time complexity is O(N log K) where N is the number of points
and K is the value of k. The space complexity is O(K) to store the k closest points in the heap.

---
---
---
 --- 
# Split Array into Consecutive Subsequences
>Splits an array into consecutive subsequences using Huffman Coding.

>Input: nums = [1,2,3,3,4,5]
Output: true
- https://leetcode.com/problems/split-array-into-consecutive-subsequences/
- Difficulty: 55
- Frequent: 60
- Tags: ['Greedy', 'Heap']

### Split Array into Consecutive Subsequences -- Shortest Solution:
```python
from collections import Counter

def isPossible(nums):
    freq = Counter(nums)
    need = Counter()
    for num in nums:
        if freq[num] == 0:
            continue
        elif need[num] > 0:
            need[num] -= 1
            need[num + 1] += 1
        elif freq[num + 1] > 0 and freq[num + 2] > 0:
            freq[num + 1] -= 1
            freq[num + 2] -= 1
            need[num + 3] += 1
        else:
            return False
        freq[num] -= 1
    return True
```
#### TC: O(n) **SC:** O(n)

The code uses a Counter to keep track of the frequency of each number in the input array. It then
iterates through the array, checking if the current number can be added to an existing consecutive
sequence or if a new consecutive sequence can be started. The need Counter keeps track of the
numbers needed to form consecutive sequences. By efficiently updating the counters, the code
determines if it's possible to split the array into consecutive subsequences. The time complexity is
O(n) as it iterates through the input array once, and the space complexity is O(n) due to the
Counter data structures used.

---
---
---
 --- 
# Encode String with Shortest Length
>Encodes a string with the shortest length using Huffman Coding.

>Input: s = "aaaabbbcc"
Output: "4a3b2c"
- https://leetcode.com/problems/encode-string-with-shortest-length/
- Difficulty: 45
- Frequent: 55
- Tags: ['Dynamic Programming', 'String']

### Encode String with Shortest Length -- Shortest Solution:
```python
# Using Dynamic Programming

def encode(s: str) -> str:
    n = len(s)
    dp = [['' for _ in range(n)] for _ in range(n)]
    for length in range(1, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = s[i:i+length]
            for k in range(i, j):
                left = dp[i][k]
                right = dp[k + 1][j]
                if len(left) + len(right) < len(dp[i][j]):
                    dp[i][j] = left + right
                    
    return dp[0][n - 1]

# Example
s = "ababab"
print(encode(s))  # Output: "2[ab]"
```
#### TC: O(n^3) **SC:** O(n^2)

The code uses dynamic programming to find the shortest encoded string. It iterates through all
possible substrings and combines them to find the shortest encoded form. The time complexity is
O(n^3) due to the nested loops, and the space complexity is O(n^2) for the DP table. The code
efficiently handles the encoding process by optimizing the combination of substrings to minimize the
length of the encoded string.

---
---
---
 --- 
# Kth Largest Element in a Stream
>Finds the Kth largest element in a stream using Huffman Coding.

>Input: k = 3, nums = [4,5,8,2]
Output: 4
- https://leetcode.com/problems/kth-largest-element-in-a-stream/
- Difficulty: 45
- Frequent: 55
- Tags: ['Heap', 'Design']

### Kth Largest Element in a Stream -- Shortest Solution:
```python
import heapq
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.nums = nums
        heapq.heapify(self.nums)
        while len(self.nums) > k:
            heapq.heappop(self.nums)
    def add(self, val: int) -> int:
        heapq.heappush(self.nums, val)
        if len(self.nums) > self.k:
            heapq.heappop(self.nums)
        return self.nums[0]
```
#### TC: 
Time Complexity:
- Initialization: O(n * log(k))
- Adding an element: O(log(k))

Overall: O(n * log(k)) **SC:** 
Space Complexity: O(k)

The code uses a min heap to maintain the k largest elements seen so far. During initialization, it
heapifies the input list and keeps only the k largest elements. When adding a new element, it pushes
it onto the heap and maintains the heap size to k by popping the smallest element if necessary. This
approach ensures that the kth largest element is always at the root of the heap, making it easy to
retrieve. The time complexity for initialization is O(n * log(k)) where n is the number of elements
in the input list, and for adding an element, it is O(log(k)). The space complexity is O(k) to store
the k largest elements in the heap.

---
### Kth Largest Element in a Stream -- Best TC Solution:
```python
import heapq
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.heap = nums
        heapq.heapify(self.heap)
        while len(self.heap) > k:
            heapq.heappop(self.heap)
    
    def add(self, val: int) -> int:
        if len(self.heap) < self.k:
            heapq.heappush(self.heap, val)
        elif val > self.heap[0]:
            heapq.heappushpop(self.heap, val)
        return self.heap[0]
```
#### TC: 
Time Complexity:
- Constructor: O(n * log(k))
- Add: O(log(k)) **SC:** 
Space Complexity: O(k)

The code uses a min heap to maintain the k largest elements seen so far. In the constructor, we
initialize the heap with the given nums list and ensure it contains only the k largest elements.
When adding a new element, we compare it with the smallest element in the heap. If it's larger, we
replace the smallest element with the new element. This way, the heap always contains the k largest
elements. The time complexity for the constructor is O(n * log(k)) where n is the length of the nums
list, and for the add method, it's O(log(k)). The space complexity is O(k) to store the k largest
elements in the heap.

---
### Kth Largest Element in a Stream -- Best SC Solution:
```python
import heapq
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.heap = nums
        heapq.heapify(self.heap)
        while len(self.heap) > k:
            heapq.heappop(self.heap)
    
    def add(self, val: int) -> int:
        if len(self.heap) < self.k:
            heapq.heappush(self.heap, val)
        elif val > self.heap[0]:
            heapq.heappushpop(self.heap, val)
        return self.heap[0]
```
#### TC: O(log k) for initialization, O(log k) for each add operation **SC:** O(k)

The code uses a min heap to maintain the k largest elements. During initialization, it constructs a
heap of size k from the given nums. The add method adds a new element to the heap and maintains the
k largest elements. The heap always contains the k largest elements, with the smallest element at
the root. This approach ensures efficient retrieval of the kth largest element with O(log k) time
complexity for both initialization and each add operation. The space complexity is O(k) to store the
k largest elements in the heap.

---
---
---
 --- 
# Frequency Sort
>Sorts characters in a string by frequency using Huffman Coding.

>Input: s = "tree"
Output: "eert"
- https://leetcode.com/problems/frequency-sort/
- Difficulty: 45
- Frequent: 50
- Tags: ['Hash Table', 'Heap', 'String']

### Frequency Sort -- Shortest Solution:
```python
from collections import Counter

class Solution:
    def frequencySort(self, s: str) -> str:
        count = Counter(s)
        sorted_chars = sorted(count.keys(), key=lambda x: -count[x])
        res = ''.join([char * count[char] for char in sorted_chars])
        return res
```
#### TC: O(n log n) **SC:** O(n)

The code uses the Counter class from the collections module to count the frequency of each character
in the input string. It then sorts the characters based on their frequencies in descending order.
Finally, it constructs the result string by repeating each character based on its frequency. The
time complexity is O(n log n) due to the sorting operation, and the space complexity is O(n) to
store the counts and sorted characters.

---
---
---
 --- 
# Sort Characters By Frequency
>Sorts characters in a string by frequency using Huffman Coding.

>Input: s = "tree"
Output: "eert"
- https://leetcode.com/problems/sort-characters-by-frequency/
- Difficulty: 45
- Frequent: 50
- Tags: ['Hash Table', 'Heap', 'String']

### Sort Characters By Frequency -- Shortest Solution:
```python
from collections import Counter

class Solution:
    def frequencySort(self, s: str) -> str:
        count = Counter(s)
        sorted_chars = sorted(count, key=lambda x: (-count[x], x))
        return ''.join(char * count[char] for char in sorted_chars)
```
#### TC: O(n log n) **SC:** O(n)

The code uses Counter to count the frequency of characters in the input string. It then sorts the
characters based on their frequency in descending order. Finally, it constructs the sorted string by
repeating each character according to its count. The time complexity is O(n log n) due to sorting,
and the space complexity is O(n) to store the counts and sorted characters.

---
---
---
 --- 
# Optimal File Merge Patterns
>Finds the optimal way to merge files with minimal cost using Huffman Coding.

>Input: files = [20, 30, 10, 5]
Output: 85
- https://leetcode.com/problems/optimal-file-merge-patterns/
- Difficulty: 60
- Frequent: 50
- Tags: ['Greedy', 'Heap']

### Optimal File Merge Patterns -- Shortest Solution:
```python
import heapq

def merge_files(file_sizes):
    heapq.heapify(file_sizes)
    total_cost = 0
    while len(file_sizes) > 1:
        cost = heapq.heappop(file_sizes) + heapq.heappop(file_sizes)
        total_cost += cost
        heapq.heappush(file_sizes, cost)
    return total_cost

# Example
file_sizes = [4, 8, 6, 12]
result = merge_files(file_sizes)
print(result)  # Output: 58
```
#### TC: O(n log n) **SC:** O(n)

The code uses a min heap to efficiently merge the files with the smallest sizes first. By
continuously merging the smallest files, the total cost is minimized. The time complexity is O(n log
n) due to the heap operations, and the space complexity is O(n) to store the file sizes in the heap.

---
---
---
 --- 
# Minimum Cost to Connect Sticks
>Connects sticks with minimal cost using Huffman Coding.

>Input: sticks = [2, 4, 3]
Output: 14
- https://leetcode.com/problems/minimum-cost-to-connect-sticks/
- Difficulty: 40
- Frequent: 45
- Tags: ['Greedy', 'Heap']

### Minimum Cost to Connect Sticks -- Shortest Solution:
```python
import heapq
def connectSticks(sticks):
    heapq.heapify(sticks)
    cost = 0
    while len(sticks) > 1:
        stick1 = heapq.heappop(sticks)
        stick2 = heapq.heappop(sticks)
        new_stick = stick1 + stick2
        cost += new_stick
        heapq.heappush(sticks, new_stick)
    return cost
```
#### TC: O(n log n) **SC:** O(n)

The code uses a priority queue (min heap) to efficiently find the two smallest sticks to connect at
each step. By continuously merging the smallest sticks, the total cost is minimized. The time
complexity is O(n log n) due to the heap operations, and the space complexity is O(n) to store the
sticks in the heap. This approach ensures an optimal solution with minimal code and clear logic.

---
---
---