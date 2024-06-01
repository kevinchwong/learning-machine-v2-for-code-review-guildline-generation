# Divide and Conquer
## Frequent score for this algorithmic framework: 60 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Pow(x, n)](#pow-x-n) | 85 | 30 |
| 2 | [Majority Element](#majority-element) | 80 | 20 |
| 3 | [Longest Common Prefix](#longest-common-prefix) | 80 | 20 |
| 4 | [Merge k Sorted Lists](#merge-k-sorted-lists) | 80 | 50 |
| 5 | [Kth Largest Element in an Array](#kth-largest-element-in-an-array) | 75 | 35 |
| 6 | [Sort an Array](#sort-an-array) | 70 | 25 |
| 7 | [Construct Binary Tree from Preorder and Inorder Traversal](#construct-binary-tree-from-preorder-and-inorder-traversal) | 70 | 45 |
| 8 | [The Skyline Problem](#the-skyline-problem) | 70 | 60 |
| 9 | [Construct Binary Tree from Inorder and Postorder Traversal](#construct-binary-tree-from-inorder-and-postorder-traversal) | 65 | 45 |
| 10 | [Reverse Pairs](#reverse-pairs) | 60 | 50 |
| 11 | [Different Ways to Add Parentheses](#different-ways-to-add-parentheses) | 55 | 40 |
| 12 | [Closest Binary Search Tree Value](#closest-binary-search-tree-value) | 50 | 40 |
| 13 | [Closest Binary Search Tree Value II](#closest-binary-search-tree-value-ii) | 50 | 50 |
---
Divide and Conquer is an algorithmic paradigm that solves a problem by breaking it down into smaller subproblems, solving each subproblem recursively, and then combining their solutions to solve the original problem. This approach is often used in algorithms like merge sort, quicksort, and binary search.
```python
# Merge Sort Example

def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)


def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

# Binary Search Example

def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
### Insight:
Divide and Conquer is powerful for problems that can be broken down into independent subproblems. Merge Sort divides the array into halves, sorts each half, and merges them. Binary Search repeatedly divides the search interval in half. The key is to ensure that the subproblems are simpler and that combining their solutions is efficient.
---
 --- 
# Pow(x, n)
>Implement pow(x, n), which calculates x raised to the power n.

>Input: x = 2.00000, n = 10
Output: 1024.00000
- https://leetcode.com/problems/powx-n/
- Difficulty: 30
- Frequent: 85
- Tags: ['Math', 'Recursion', 'Divide and Conquer']

### Pow(x, n) -- Shortest Solution:
```python
def myPow(x, n):
    if n == 0:
        return 1
    if n < 0:
        x = 1 / x
        n = -n
    half = myPow(x, n // 2)
    if n % 2 == 0:
        return half * half
    else:
        return half * half * x
```
#### TC: O(log n) **SC:** O(log n)

The function uses a recursive approach to calculate the power. It first handles the base cases where
the exponent is 0 or negative. For positive exponents, it recursively calculates the power of half
the exponent and then squares the result. If the exponent is odd, it multiplies the result by the
base once more. This approach leverages the divide-and-conquer strategy to reduce the number of
multiplications needed, making it efficient.

---
---
---
 --- 
# Majority Element
>Find the majority element in an array.

>Input: nums = [3,2,3]
Output: 3
- https://leetcode.com/problems/majority-element/
- Difficulty: 20
- Frequent: 80
- Tags: ['Array', 'Divide and Conquer', 'Hash Table', 'Sorting', 'Counting']

### Majority Element -- Shortest Solution:
```python
from collections import Counter

def majorityElement(nums):
    return Counter(nums).most_common(1)[0][0]
```
#### TC: O(n) **SC:** O(n)

The code uses Python's `collections.Counter` to count the frequency of each element in the list
`nums`. The `most_common(1)` method returns a list of the n most common elements and their counts
from the most common to the least. Since we need the majority element, we take the first element of
this list and return its value.

---
---
---
 --- 
# Longest Common Prefix
>Find the longest common prefix string amongst an array of strings.

>Input: strs = ["flower","flow","flight"]
Output: "fl"
- https://leetcode.com/problems/longest-common-prefix/
- Difficulty: 20
- Frequent: 80
- Tags: ['String', 'Divide and Conquer']

### Longest Common Prefix -- Shortest Solution:
```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs: return ""
        min_len = min(len(s) for s in strs)
        low, high = 0, min_len
        while low <= high:
            mid = (low + high) // 2
            if all(s[:mid] == strs[0][:mid] for s in strs):
                low = mid + 1
            else:
                high = mid - 1
        return strs[0][:(low + high) // 2]
```
#### TC: O(S * log m) **SC:** O(1)

The code uses a binary search approach to find the longest common prefix. It first determines the
minimum string length to set the search boundaries. Then, it performs a binary search on the prefix
length, checking if all strings share the same prefix of a given length. The search space is halved
each time, making the approach efficient. The final result is derived from the midpoint of the last
valid range.

---
### Longest Common Prefix -- Best TC Solution:
```python
from typing import List

class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        min_str = min(strs, key=len)
        low, high = 0, len(min_str)
        while low < high:
            mid = (low + high + 1) // 2
            prefix = strs[0][:mid]
            if all(s.startswith(prefix) for s in strs):
                low = mid
            else:
                high = mid - 1
        return strs[0][:low]
```
#### TC: O(S * log n) **SC:** O(1)

The code uses a binary search approach to find the longest common prefix among the given list of
strings. It first identifies the shortest string in the list to set the upper bound for the binary
search. Then, it iteratively narrows down the search space by checking if the prefix of the first
string up to a certain length is a common prefix for all strings. The binary search reduces the time
complexity to O(S * log n), where S is the length of the common prefix and n is the number of
strings. The space complexity is O(1) as the algorithm only uses a constant amount of extra space.

---
---
---
 --- 
# Merge k Sorted Lists
>Merge k sorted linked lists and return it as one sorted list.

>Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
- https://leetcode.com/problems/merge-k-sorted-lists/
- Difficulty: 50
- Frequent: 80
- Tags: ['Linked List', 'Divide and Conquer', 'Heap (Priority Queue)']

### Merge k Sorted Lists -- Shortest Solution:
```python
from heapq import heappop, heappush
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeKLists(lists):
    heap = []
    for i, node in enumerate(lists):
        if node:
            heappush(heap, (node.val, i, node))
    head = point = ListNode(0)
    while heap:
        val, i, node = heappop(heap)
        point.next = ListNode(val)
        point = point.next
        node = node.next
        if node:
            heappush(heap, (node.val, i, node))
    return head.next
```
#### TC: O(N log k) **SC:** O(k)

1. **Heap Usage**: The heap is used to keep track of the smallest elements across the k lists
efficiently. 2. **Initialization**: Initially, the heap is populated with the first node of each
list (if it exists). 3. **Merging Process**: The smallest element is extracted from the heap, added
to the result list, and the next element from the same list is pushed to the heap. 4.
**Efficiency**: Using a heap ensures that both insertion and extraction operations are logarithmic
in complexity, making the solution efficient for large inputs.

---
### Merge k Sorted Lists -- Best TC Solution:
```python
from queue import PriorityQueue

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        head = point = ListNode(0)
        q = PriorityQueue()
        for l in lists:
            if l:
                q.put((l.val, l))
        while not q.empty():
            val, node = q.get()
            point.next = ListNode(val)
            point = point.next
            node = node.next
            if node:
                q.put((node.val, node))
        return head.next
```
#### TC: O(Nlogk) **SC:** O(k)

The code uses a PriorityQueue to efficiently merge k sorted lists. It iterates through all the nodes
in the lists and adds them to the PriorityQueue based on their values. The PriorityQueue
automatically sorts the nodes based on their values. Then, it retrieves the nodes from the
PriorityQueue in sorted order and creates a new merged list. This approach has a time complexity of
O(Nlogk) and a space complexity of O(k), where N is the total number of nodes in all lists and k is
the number of lists.

---
---
---
 --- 
# Kth Largest Element in an Array
>Find the kth largest element in an unsorted array.

>Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
- https://leetcode.com/problems/kth-largest-element-in-an-array/
- Difficulty: 35
- Frequent: 75
- Tags: ['Array', 'Divide and Conquer', 'Sorting', 'Heap (Priority Queue)']

### Kth Largest Element in an Array -- Shortest Solution:
```python
import heapq

def findKthLargest(nums, k):
    return heapq.nlargest(k, nums)[-1]
```
#### TC: O(n log k) **SC:** O(k)

The code uses Python's `heapq` library to find the k-th largest element in an array. The `nlargest`
function retrieves the k largest elements from the list and returns them in descending order. The
k-th largest element is the last element in this list. This approach leverages a min-heap to
efficiently find the k largest elements.

---
### Kth Largest Element in an Array -- Best TC Solution:
```python
from typing import List


# Quick Select Algorithm

def findKthLargest(nums: List[int], k: int) -> int:
    def partition(left, right, pivot_index):
        pivot = nums[pivot_index]
        nums[pivot_index], nums[right] = nums[right], nums[pivot_index]
        store_index = left
        for i in range(left, right):
            if nums[i] < pivot:
                nums[store_index], nums[i] = nums[i], nums[store_index]
                store_index += 1
        nums[right], nums[store_index] = nums[store_index], nums[right]
        return store_index

    def quick_select(left, right, k_smallest):
        if left == right:
            return nums[left]
        pivot_index = random.randint(left, right)
        pivot_index = partition(left, right, pivot_index)
        if k_smallest == pivot_index:
            return nums[k_smallest]
        elif k_smallest < pivot_index:
            return quick_select(left, pivot_index - 1, k_smallest)
        else:
            return quick_select(pivot_index + 1, right, k_smallest)

    return quick_select(0, len(nums) - 1, len(nums) - k)
```
#### TC: O(N) on average, O(N^2) worst case **SC:** O(1)

The code implements the Quick Select algorithm to find the kth largest element in an array. Quick
Select is a variation of the Quick Sort algorithm where we only need to partially sort the array to
find the kth largest element. The algorithm selects a pivot element and partitions the array into
two parts. It then recursively selects the partition to search based on the kth smallest element's
position. This approach has an average time complexity of O(N) and a worst-case time complexity of
O(N^2). The space complexity is O(1) as the algorithm uses constant extra space.

---
---
---
 --- 
# Sort an Array
>Sort an array of integers in ascending order.

>Input: nums = [5,2,3,1]
Output: [1,2,3,5]
- https://leetcode.com/problems/sort-an-array/
- Difficulty: 25
- Frequent: 70
- Tags: ['Array', 'Divide and Conquer', 'Sorting', 'Heap (Priority Queue)', 'Merge Sort', 'Quick Sort']

### Sort an Array -- Shortest Solution:
```python
from heapq import heapify, heappop

def sortArray(nums):
    heapify(nums)
    return [heappop(nums) for _ in range(len(nums))]
```
#### TC: O(n log n) **SC:** O(n)

The provided solution uses a heap data structure to sort the array. The `heapify` function
transforms the list into a heap in O(n) time. The `heappop` function is then called n times, each
time extracting the smallest element from the heap, which takes O(log n) time per extraction. Thus,
the overall time complexity is O(n log n). The space complexity is O(n) because the heap data
structure requires additional space proportional to the number of elements in the list.

---
---
---
 --- 
# Construct Binary Tree from Preorder and Inorder Traversal
>Construct a binary tree from preorder and inorder traversal.

>Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
- https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/
- Difficulty: 45
- Frequent: 70
- Tags: ['Array', 'Hash Table', 'Divide and Conquer', 'Tree', 'Binary Tree']

### Construct Binary Tree from Preorder and Inorder Traversal -- Shortest Solution:
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(preorder, inorder):
    if not inorder:
        return None
    root = TreeNode(preorder.pop(0))
    index = inorder.index(root.val)
    root.left = buildTree(preorder, inorder[:index])
    root.right = buildTree(preorder, inorder[index+1:])
    return root
```
#### TC: O(n^2) **SC:** O(n)

1. The function `buildTree` takes two lists, `preorder` and `inorder`, as input and returns the root
of the constructed binary tree. 2. The base case for the recursion is when the `inorder` list is
empty, in which case the function returns `None`. 3. The root of the tree is the first element of
the `preorder` list, which is removed using `pop(0)`. 4. The index of the root value in the
`inorder` list is found using `index()`, which helps in dividing the `inorder` list into left and
right subtrees. 5. The function recursively constructs the left and right subtrees using the divided
`inorder` lists and the remaining `preorder` list. 6. The constructed tree is returned as the final
result.

---
### Construct Binary Tree from Preorder and Inorder Traversal -- Best TC Solution:
```python
# Definition for a binary tree node
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        inorder_map = {val: idx for idx, val in enumerate(inorder)}
        preorder_idx = 0
        def helper(left, right):
            nonlocal preorder_idx
            if left > right:
                return None
            root_val = preorder[preorder_idx]
            root = TreeNode(root_val)
            inorder_idx = inorder_map[root_val]
            preorder_idx += 1
            root.left = helper(left, inorder_idx - 1)
            root.right = helper(inorder_idx + 1, right)
            return root
        return helper(0, len(inorder) - 1)
```
#### TC: O(n) **SC:** O(n)

The code builds a binary tree from preorder and inorder traversal lists using a recursive approach.
It utilizes a helper function to construct the tree by recursively dividing the lists based on the
root node. The inorder_map dictionary is used to store the indices of values in the inorder list for
quick lookup. The preorder_idx variable keeps track of the current index in the preorder list. The
helper function constructs the tree by recursively creating nodes and updating the preorder_idx. The
time complexity is O(n) as each node is visited once, and the space complexity is O(n) for the
inorder_map and recursive stack space.

---
### Construct Binary Tree from Preorder and Inorder Traversal -- Best SC Solution:
```python
# Definition for a binary tree node
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        inorder_map = {val: idx for idx, val in enumerate(inorder)}
        preorder_idx = 0
        def helper(left, right):
            nonlocal preorder_idx
            if left > right:
                return None
            root_val = preorder[preorder_idx]
            root = TreeNode(root_val)
            inorder_idx = inorder_map[root_val]
            preorder_idx += 1
            root.left = helper(left, inorder_idx - 1)
            root.right = helper(inorder_idx + 1, right)
            return root
        return helper(0, len(inorder) - 1)
```
#### TC: O(N) **SC:** O(N)

The code constructs a binary tree from preorder and inorder traversal lists using a recursive
approach. It utilizes a helper function to build the tree by recursively dividing the lists based on
the root node. The inorder_map dictionary is used to store the indices of values in the inorder list
for quick lookup. The preorder_idx variable keeps track of the current index in the preorder list.
The helper function constructs the tree by recursively creating nodes and updating the preorder_idx.
The time complexity is O(N) as each node is visited once, and the space complexity is O(N) for the
inorder_map and recursive stack space.

---
---
---
 --- 
# The Skyline Problem
>Find the skyline formed by a collection of buildings.

>Input: buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
Output: [[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]
- https://leetcode.com/problems/the-skyline-problem/
- Difficulty: 60
- Frequent: 70
- Tags: ['Array', 'Divide and Conquer', 'Binary Indexed Tree', 'Segment Tree', 'Line Sweep', 'Heap (Priority Queue)']

### The Skyline Problem -- Shortest Solution:
```python
from heapq import heappush, heappop

def getSkyline(buildings):
    events = [(L, -H, R) for L, R, H in buildings] + list({(R, 0, 0) for _, R, _ in buildings})
    events.sort()
    res, hp = [[0, 0]], [(0, float('inf'))]
    for x, negH, R in events:
        while x >= hp[0][1]:
            heappop(hp)
        if negH:
            heappush(hp, (negH, R))
        if res[-1][1] + hp[0][0]:
            res.append([x, -hp[0][0]])
    return res[1:]
```
#### TC: O(N log N) **SC:** O(N)

1. **Events Creation**: We create events for both the start and end of buildings. The start event
has a negative height to distinguish it from the end event. 2. **Sorting Events**: Events are
sorted. Start events come before end events when they have the same x-coordinate. 3. **Heap
Management**: A heap is used to keep track of the tallest building at the current position. We push
the building height when we encounter a start event and pop from the heap when the building ends. 4.
**Result Construction**: We add a new point to the result whenever the current maximum height
changes.

---
### The Skyline Problem -- Best TC Solution:
```python
from sortedcontainers import SortedDict
def getSkyline(buildings):
    events = [(l, -h, r) for l, r, h in buildings]
    events += list({(r, 0, 0) for _, r, _ in buildings})
    events.sort()
    res = [[0, 0]]
    live = SortedDict([(0, 1)])
    for pos, negH, r in events:
        if negH:
            live[-negH] = live.get(-negH, 0) + 1
        else:
            del live[r]
        maxH = list(live.keys())[-1]
        if res[-1][1] != maxH:
            res.append([pos, maxH])
    return res[1:]
```
#### TC: O(n log n) **SC:** O(n)

The code uses a sorted list of events to track the buildings' left and right edges along with their
heights. It maintains a SortedDict to keep track of the current live buildings based on their
heights. By iterating through the events and updating the live buildings, the code efficiently
determines the skyline points. The time complexity is O(n log n) due to sorting the events, and the
space complexity is O(n) to store the events and live buildings.

---
---
---
 --- 
# Construct Binary Tree from Inorder and Postorder Traversal
>Construct a binary tree from inorder and postorder traversal.

>Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
- https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/
- Difficulty: 45
- Frequent: 65
- Tags: ['Array', 'Hash Table', 'Divide and Conquer', 'Tree', 'Binary Tree']

### Construct Binary Tree from Inorder and Postorder Traversal -- Shortest Solution:
```python
def buildTree(inorder, postorder):
    if not inorder or not postorder:
        return None
    root = TreeNode(postorder.pop())
    index = inorder.index(root.val)
    root.right = buildTree(inorder[index+1:], postorder)
    root.left = buildTree(inorder[:index], postorder)
    return root
```
#### TC: O(n^2) **SC:** O(n)

1. The last element in postorder is the root of the tree. 2. Find the index of this root in inorder
to divide the tree into left and right subtrees. 3. Recursively build the right subtree first
(because of the nature of postorder traversal) and then the left subtree. 4. The base case for the
recursion is when the inorder or postorder list is empty, in which case the tree node is None.

---
### Construct Binary Tree from Inorder and Postorder Traversal -- Best TC Solution:
```python
# Definition for a binary tree node
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        def build(inorder, postorder):
            if not inorder or not postorder:
                return None
            root_val = postorder.pop()
            root = TreeNode(root_val)
            root_index = inorder.index(root_val)
            root.right = build(inorder[root_index + 1:], postorder)
            root.left = build(inorder[:root_index], postorder)
            return root
        return build(inorder, postorder)
```
#### TC: O(N) **SC:** O(N)

The code defines a class Solution with a method buildTree that takes two lists, inorder and
postorder, as input. It recursively builds the binary tree using a divide and conquer approach. The
build function constructs the tree by popping the root value from the postorder list, finding its
index in the inorder list, and recursively building the right and left subtrees. The time complexity
is O(N) as each node is visited once, and the space complexity is O(N) due to the recursive calls
and the space used by the lists.

---
---
---
 --- 
# Reverse Pairs
>Count the number of important reverse pairs in an array.

>Input: nums = [1,3,2,3,1]
Output: 2
- https://leetcode.com/problems/reverse-pairs/
- Difficulty: 50
- Frequent: 60
- Tags: ['Array', 'Binary Search', 'Divide and Conquer', 'Binary Indexed Tree', 'Segment Tree', 'Merge Sort']

### Reverse Pairs -- Shortest Solution:
```python
class Solution:
    def reversePairs(self, nums):
        def merge_sort(start, end):
            if end - start <= 1:
                return 0
            mid = (start + end) // 2
            count = merge_sort(start, mid) + merge_sort(mid, end)
            j = mid
            for i in range(start, mid):
                while j < end and nums[i] > 2 * nums[j]:
                    j += 1
                count += j - mid
            nums[start:end] = sorted(nums[start:end])
            return count
        return merge_sort(0, len(nums))
```
#### TC: O(n log n) **SC:** O(n)

The solution uses a modified merge sort algorithm to count reverse pairs. The merge_sort function
recursively splits the array into halves, counts the reverse pairs in each half, and then counts the
reverse pairs across the halves. The sorted merge step ensures that the array is sorted for the next
level of recursion. This approach leverages the divide-and-conquer strategy to achieve an efficient
time complexity of O(n log n).

---
---
---
 --- 
# Different Ways to Add Parentheses
>Compute all possible results from computing all the different possible ways to group numbers and operators.

>Input: expression = "2-1-1"
Output: [0,2]
- https://leetcode.com/problems/different-ways-to-add-parentheses/
- Difficulty: 40
- Frequent: 55
- Tags: ['Math', 'String', 'Dynamic Programming', 'Divide and Conquer', 'Memoization']

### Different Ways to Add Parentheses -- Shortest Solution:
```python
class Solution:
    def diffWaysToCompute(self, expression: str) -> List[int]:
        if expression.isdigit():
            return [int(expression)]
        res = []
        for i, char in enumerate(expression):
            if char in "-+*":
                left = self.diffWaysToCompute(expression[:i])
                right = self.diffWaysToCompute(expression[i+1:])
                for l in left:
                    for r in right:
                        if char == '+':
                            res.append(l + r)
                        elif char == '-':
                            res.append(l - r)
                        else:
                            res.append(l * r)
        return res
```
#### TC: O(2^n) **SC:** O(2^n)

1. The function `diffWaysToCompute` takes an arithmetic expression as input and returns all possible
results from computing all the different possible ways to group numbers and operators using
parentheses. 2. If the expression is a single number, it returns that number as the only result. 3.
It iterates through the expression, and whenever it encounters an operator (`+`, `-`, `*`), it
recursively computes the results for the left and right sub-expressions. 4. It combines the results
of the left and right sub-expressions using the current operator and appends the result to the list.
5. The function returns the list of all possible results.

---
### Different Ways to Add Parentheses -- Best TC Solution:
```python
# Approach: Divide and Conquer

def diffWaysToCompute(input: str) -> List[int]:
    def calculate(a, b, op):
        if op == '+':
            return a + b
        elif op == '-':
            return a - b
        else:
            return a * b
    
    def helper(start, end):
        if start == end:
            return [int(input[start])]
        
        res = []
        for i in range(start, end+1):
            if input[i] in ['+', '-', '*']:
                left = helper(start, i-1)
                right = helper(i+1, end)
                for l in left:
                    for r in right:
                        res.append(calculate(l, r, input[i]))
        
        if not res:
            res.append(int(input[start:end+1]))
        
        return res
    
    return helper(0, len(input)-1)
```
#### TC: O(4^n / n^(3/2)) **SC:** O(4^n / n^(3/2))

1. The code uses a divide and conquer approach to solve the problem. 2. The calculate function is
used to perform arithmetic operations. 3. The helper function recursively divides the input string
into subproblems. 4. It explores all possible combinations of left and right operands with
operators. 5. The time complexity is O(4^n / n^(3/2)) due to the exponential growth of the number of
subproblems. 6. The space complexity is also O(4^n / n^(3/2)) due to the recursive calls and the
number of possible solutions.

---
### Different Ways to Add Parentheses -- Best SC Solution:
```python
from collections import defaultdict
def diffWaysToCompute(input: str) -> List[int]:
    def helper(s: str, memo: Dict[str, List[int]]) -> List[int]:
        if s in memo:
            return memo[s]
        if s.isdigit():
            return [int(s)]
        res = []
        for i, char in enumerate(s):
            if char in ['+', '-', '*']:
                left = helper(s[:i], memo)
                right = helper(s[i + 1:], memo)
                for l in left:
                    for r in right:
                        if char == '+':
                            res.append(l + r)
                        elif char == '-':
                            res.append(l - r)
                        else:
                            res.append(l * r)
        memo[s] = res
        return res
    return helper(input, defaultdict(list))
```
#### TC: O(3^n) **SC:** O(3^n)

The code uses a recursive approach to solve the problem by dividing the input string into
subproblems. It utilizes memoization to store the results of subproblems to avoid redundant
calculations. The helper function recursively calculates the possible results by splitting the input
string at each operator. The time complexity is O(3^n) as there are 3 choices (+, -, *) at each
operator position, leading to an exponential number of recursive calls. The space complexity is also
O(3^n) due to the memoization storage of results for each subproblem.

---
---
---
 --- 
# Closest Binary Search Tree Value
>Find the value in a BST that is closest to the target.

>Input: root = [4,2,5,1,3], target = 3.714286
Output: 4
- https://leetcode.com/problems/closest-binary-search-tree-value/
- Difficulty: 40
- Frequent: 50
- Tags: ['Tree', 'Depth-First Search', 'Binary Search', 'Divide and Conquer']

### Closest Binary Search Tree Value -- Shortest Solution:
```python
def closestValue(root, target):
    closest = root.val
    while root:
        if abs(root.val - target) < abs(closest - target):
            closest = root.val
        root = root.left if target < root.val else root.right
    return closest
```
#### TC: O(H) where H is the height of the tree **SC:** O(1)

The code iteratively traverses the binary search tree starting from the root. At each step, it
updates the closest value if the current node's value is closer to the target than the previously
recorded closest value. Depending on whether the target is less than or greater than the current
node's value, it moves left or right in the tree, respectively. This ensures that the traversal is
efficient and only goes as deep as necessary to find the closest value.

---
### Closest Binary Search Tree Value -- Best TC Solution:
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def closestValue(self, root: TreeNode, target: float) -> int:
        closest = root.val
        while root:
            closest = min(root.val, closest, key=lambda x: abs(target - x))
            root = root.left if target < root.val else root.right
        return closest
```
#### TC: O(log N) **SC:** O(1)

The code defines a TreeNode class and a Solution class with a method closestValue that finds the
closest value to the target in a binary search tree. It iterates through the tree while updating the
closest value based on the current node's value. The key lambda function is used to calculate the
absolute difference between the target and the node's value. The code has a time complexity of O(log
N) due to the binary search tree traversal and a space complexity of O(1) as it uses constant space
for variables.

---
---
---
 --- 
# Closest Binary Search Tree Value II
>Find k values in a BST that are closest to the target.

>Input: root = [4,2,5,1,3], target = 3.714286, k = 2
Output: [4,3]
- https://leetcode.com/problems/closest-binary-search-tree-value-ii/
- Difficulty: 50
- Frequent: 50
- Tags: ['Stack', 'Tree', 'Depth-First Search', 'Binary Search', 'Two Pointers', 'Divide and Conquer', 'Heap (Priority Queue)']

### Closest Binary Search Tree Value II -- Shortest Solution:
```python
from heapq import heappush, heappop

class Solution:
    def closestKValues(self, root, target, k):
        heap = []
        def inorder(node):
            if not node:
                return
            inorder(node.left)
            heappush(heap, (abs(node.val - target), node.val))
            if len(heap) > k:
                heappop(heap)
            inorder(node.right)
        inorder(root)
        return [val for _, val in heap]
```
#### TC: O(n log k) **SC:** O(k)

The solution uses a min-heap to keep track of the k closest values to the target. The inorder
traversal ensures that we process nodes in a sorted order. By pushing the absolute difference and
the node value into the heap, we can efficiently maintain the k closest values. The heap operations
(push and pop) ensure that the heap size does not exceed k, making the solution both time and space
efficient.

---
### Closest Binary Search Tree Value II -- Best TC Solution:
```python
from collections import deque

class Solution:
    def closestKValues(self, root: TreeNode, target: float, k: int) -> List[int]:
        def inorder(node):
            return inorder(node.left) + [node.val] + inorder(node.right) if node else []
        nums = inorder(root)
        closest = deque(nums[:k])
        for i in range(k, len(nums)):
            if abs(nums[i] - target) < abs(closest[0] - target):
                closest.popleft()
                closest.append(nums[i])
        return list(closest)
```
#### TC: O(N) **SC:** O(N)

The code defines a class Solution with a method closestKValues that takes a binary search tree root,
a target value, and an integer k as input. It performs an inorder traversal of the tree to get a
sorted list of nodes. It then initializes a deque with the first k elements from the sorted list. It
iterates through the remaining elements, updating the deque with the closest k values to the target.
The time complexity is O(N) where N is the number of nodes in the tree, and the space complexity is
O(N) to store the sorted list of nodes.

---
---
---