# Segment Tree
## Frequent score for this algorithmic framework: 35 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Range Sum Query - Mutable](#range-sum-query-mutable) | 70 | 50 |
| 2 | [Range Sum Query 2D - Mutable](#range-sum-query-2d-mutable) | 65 | 60 |
| 3 | [Range Sum Query V](#range-sum-query-v) | 65 | 60 |
| 4 | [Range Sum Query VIII](#range-sum-query-viii) | 65 | 60 |
| 5 | [Range Sum Query XI](#range-sum-query-xi) | 65 | 60 |
| 6 | [Range Minimum Query](#range-minimum-query) | 60 | 45 |
| 7 | [Range Sum Query II](#range-sum-query-ii) | 60 | 55 |
| 8 | [Range Sum Query III](#range-sum-query-iii) | 55 | 50 |
| 9 | [Range Sum Query VI](#range-sum-query-vi) | 55 | 50 |
| 10 | [Range Sum Query IX](#range-sum-query-ix) | 55 | 50 |
| 11 | [Range Sum Query XII](#range-sum-query-xii) | 55 | 50 |
| 12 | [Range Sum Query - Immutable](#range-sum-query-immutable) | 50 | 30 |
| 13 | [Range Sum Query IV](#range-sum-query-iv) | 45 | 35 |
| 14 | [Range Sum Query VII](#range-sum-query-vii) | 45 | 35 |
| 15 | [Range Sum Query X](#range-sum-query-x) | 45 | 35 |
---
A Segment Tree is a binary tree used for storing intervals or segments. It allows querying which of the stored segments contain a given point efficiently. It's particularly useful for answering range queries and updating elements in logarithmic time.
```python
class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.tree = [0] * (2 * self.n)
        self.build(data)

    def build(self, data):
        for i in range(self.n):
            self.tree[self.n + i] = data[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, pos, value):
        pos += self.n
        self.tree[pos] = value
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def query(self, left, right):
        result = 0
        left += self.n
        right += self.n
        while left < right:
            if left % 2:
                result += self.tree[left]
                left += 1
            if right % 2:
                right -= 1
                result += self.tree[right]
            left //= 2
            right //= 2
        return result

# Example usage:
data = [1, 3, 5, 7, 9, 11]
st = SegmentTree(data)
print(st.query(1, 3))  # Output: 8
st.update(1, 10)
print(st.query(1, 3))  # Output: 15
```
### Insight:
1. Segment Trees are ideal for scenarios where you need to perform multiple range queries and updates.
2. The tree is built in a bottom-up manner, and each node represents a segment of the array.
3. The update operation modifies the value at a specific position and propagates the change up the tree.
4. The query operation traverses the tree to sum up the values in the specified range.
5. Segment Trees are versatile and can be adapted for different types of range queries, such as minimum, maximum, and greatest common divisor.
---
 --- 
# Range Sum Query - Mutable
>Given an integer array nums, handle multiple queries efficiently.

>nums = [1, 3, 5]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 9
numArray.update(1, 2)
numArray.sumRange(0, 2) -> 8
- https://leetcode.com/problems/range-sum-query-mutable/
- Difficulty: 50
- Frequent: 70
- Tags: ['Segment Tree']

### Range Sum Query - Mutable -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[i * 2] + self.tree[i * 2 + 1]

    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        l, r = left + self.n, right + self.n
        sum = 0
        while l <= r:
            if l % 2 == 1:
                sum += self.tree[l]
                l += 1
            if r % 2 == 0:
                sum += self.tree[r]
                r -= 1
            l //= 2
            r //= 2
        return sum
```
#### TC: O(log n) **SC:** O(n)

1. **Initialization**: The segment tree is built using an array `tree` of size `2 * n`, where `n` is
the length of the input array `nums`. The second half of the `tree` array is initialized with the
values from `nums`, and the first half is used to store the sums of segments.  2. **Update
Operation**: To update an element, we update the corresponding position in the `tree` array and then
update its ancestors to reflect the change. This is done by moving up the tree and updating the
parent nodes.  3. **Sum Range Query**: To get the sum of a range, we traverse the tree from the
leaves to the root, summing the relevant segments. We adjust the left and right pointers to include
the necessary segments and move up the tree.  4. **Efficiency**: The segment tree allows both update
and sum range operations to be performed in O(log n) time, making it efficient for large arrays with
frequent updates and queries.

---
### Range Sum Query - Mutable -- Best TC Solution:
```python
# Segment Tree Approach

class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)
        
    def build_tree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]
        
    def update(self, index, val):
        index += self.n
        self.tree[index] = val
        while index > 0:
            left = right = index
            if index % 2 == 0:
                right = index + 1
            else:
                left = index - 1
            self.tree[index // 2] = self.tree[left] + self.tree[right]
            index //= 2
        
    def sum_range(self, left, right):
        left += self.n
        right += self.n
        total = 0
        while left <= right:
            if left % 2 == 1:
                total += self.tree[left]
                left += 1
            if right % 2 == 0:
                total += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return total


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(index, val)
# param_2 = obj.sumRange(left, right)
```
#### TC: Time Complexity:
- Build Tree: O(n)
- Update: O(log n)
- Sum Range: O(log n) **SC:** Space Complexity: O(n)

- The Segment Tree approach efficiently handles both updates and range sum queries. - Building the
tree initially takes O(n) time and O(n) space. - Updating an element and calculating the sum within
a range both take O(log n) time. - The tree structure allows for efficient updates and queries by
dividing the range into smaller segments. - The update operation propagates the changes up the tree
to maintain the correct sum values. - The sum_range operation traverses the tree to calculate the
total sum within the specified range.

---
---
---
 --- 
# Range Sum Query 2D - Mutable
>Given a 2D matrix, handle multiple queries efficiently.

>matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
numMatrix = NumMatrix(matrix)
numMatrix.sumRegion(2, 1, 4, 3) -> 8
numMatrix.update(3, 2, 2)
numMatrix.sumRegion(2, 1, 4, 3) -> 10
- https://leetcode.com/problems/range-sum-query-2d-mutable/
- Difficulty: 60
- Frequent: 65
- Tags: ['Segment Tree']

### Range Sum Query 2D - Mutable -- Shortest Solution:
```python
class NumMatrix:
    def __init__(self, matrix):
        if not matrix or not matrix[0]:
            return
        self.rows, self.cols = len(matrix), len(matrix[0])
        self.tree = [[0] * (self.cols + 1) for _ in range(self.rows + 1)]
        self.nums = [[0] * self.cols for _ in range(self.rows)]
        for r in range(self.rows):
            for c in range(self.cols):
                self.update(r, c, matrix[r][c])

    def update(self, row, col, val):
        if not self.rows or not self.cols:
            return
        delta = val - self.nums[row][col]
        self.nums[row][col] = val
        r, c = row + 1, col + 1
        while r <= self.rows:
            while c <= self.cols:
                self.tree[r][c] += delta
                c += c & -c
            r += r & -r
            c = col + 1

    def sumRegion(self, row1, col1, row2, col2):
        if not self.rows or not self.cols:
            return 0
        return (self._sum(row2 + 1, col2 + 1) - self._sum(row1, col2 + 1) -
                self._sum(row2 + 1, col1) + self._sum(row1, col1))

    def _sum(self, row, col):
        s = 0
        while row > 0:
            while col > 0:
                s += self.tree[row][col]
                col -= col & -col
            row -= row & -row
            col = col
        return s
```
#### TC: O(log(m) * log(n)) for both update and sumRegion operations **SC:** O(m * n)

The solution uses a Binary Indexed Tree (BIT) or Fenwick Tree to efficiently handle the update and
sum queries in a 2D matrix. The BIT is a data structure that provides efficient methods for prefix
sum queries and updates. The main idea is to use a 2D BIT to keep track of the cumulative sums and
update them as needed. The update operation adjusts the BIT to reflect changes in the matrix, while
the sumRegion operation calculates the sum of elements in a submatrix by combining results from the
BIT. This approach ensures that both update and sumRegion operations are efficient, with a time
complexity of O(log(m) * log(n)).

---
### Range Sum Query 2D - Mutable -- Best TC Solution:
```python
# Implementing the 2D Binary Indexed Tree (Fenwick Tree) for Range Sum Query 2D - Mutable

class NumMatrix:
    def __init__(self, matrix: List[List[int]]):
        if not matrix or not matrix[0]:
            return
        self.m, self.n = len(matrix), len(matrix[0])
        self.bit = [[0] * (self.n + 1) for _ in range(self.m + 1)]
        self.matrix = [[0] * self.n for _ in range(self.m)]
        for i in range(self.m):
            for j in range(self.n):
                self.update(i, j, matrix[i][j])

    def update(self, row: int, col: int, val: int) -> None:
        if self.m == 0 or self.n == 0:
            return
        delta = val - self.matrix[row][col]
        self.matrix[row][col] = val
        i = row + 1
        while i <= self.m:
            j = col + 1
            while j <= self.n:
                self.bit[i][j] += delta
                j += (j & -j)
            i += (i & -i)

    def query(self, row: int, col: int) -> int:
        if self.m == 0 or self.n == 0:
            return 0
        total = 0
        i = row
        while i > 0:
            j = col
            while j > 0:
                total += self.bit[i][j]
                j -= (j & -j)
            i -= (i & -i)
        return total

# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# obj.update(row,col,val)
# param_2 = obj.query(row,col)
```
#### TC: Time Complexity:
- Constructor: O(m*n * log(m) * log(n))
- Update: O(log(m) * log(n))
- Query: O(log(m) * log(n)) **SC:** Space Complexity: O(m*n)

The code implements the 2D Binary Indexed Tree (Fenwick Tree) to efficiently handle range sum
queries in a 2D matrix. The constructor initializes the tree and updates the matrix values. The
update function adjusts the tree values based on the delta change. The query function calculates the
sum of values within a given range efficiently. The time complexity for constructor, update, and
query operations is optimized to O(log(m) * log(n)), where m and n are the dimensions of the matrix.
The space complexity is O(m*n) to store the Binary Indexed Tree.

---
---
---
 --- 
# Range Sum Query V
>Given a 2D matrix, handle multiple sum range queries efficiently.

>matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
numMatrix = NumMatrix(matrix)
numMatrix.sumRegion(2, 1, 4, 3) -> 8
numMatrix.update(3, 2, 2)
numMatrix.sumRegion(2, 1, 4, 3) -> 10
- https://leetcode.com/problems/range-sum-query-v/
- Difficulty: 60
- Frequent: 65
- Tags: ['Segment Tree']

### Range Sum Query V -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        left += self.n
        right += self.n + 1
        sum = 0
        while left < right:
            if left % 2 == 1:
                sum += self.tree[left]
                left += 1
            if right % 2 == 1:
                right -= 1
                sum += self.tree[right]
            left //= 2
            right //= 2
        return sum
```
#### TC: O(log n) **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor, where the leaves are initialized with the input array values, and internal
nodes store the sum of their child nodes. The update function modifies a value and updates the tree
accordingly. The sumRange function calculates the sum of a given range by traversing the tree and
summing the relevant segments.

---
### Range Sum Query V -- Best TC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.tree[self.n:] = nums
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[i * 2] + self.tree[i * 2 + 1]

    def update(self, i: int, val: int) -> None:
        i += self.n
        self.tree[i] = val
        while i > 0:
            left = right = i
            if i % 2 == 0:
                right = i + 1
            else:
                left = i - 1
            self.tree[i // 2] = self.tree[left] + self.tree[right]
            i //= 2

    def sumRange(self, left: int, right: int) -> int:
        left += self.n
        right += self.n
        total = 0
        while left <= right:
            if left % 2 == 1:
                total += self.tree[left]
                left += 1
            if right % 2 == 0:
                total += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(left,right)
```
#### TC: Time Complexity:
- Initialization: O(n)
- Update: O(log n)
- Range Sum Query: O(log n)

where n is the number of elements in the input array. **SC:** Space Complexity: O(n)

The Segment Tree approach efficiently handles the range sum query problem by storing the cumulative
sum of ranges in a tree structure. The initialization builds the segment tree with O(n) time
complexity. The update operation modifies the tree in O(log n) time complexity, and the range sum
query operation retrieves the sum in O(log n) time complexity. The space complexity is O(n) to store
the segment tree. Overall, this approach provides a clean and efficient solution for the Range Sum
Query problem.

---
---
---
 --- 
# Range Sum Query VIII
>Given a 2D matrix, handle multiple sum range queries efficiently.

>matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
numMatrix = NumMatrix(matrix)
numMatrix.sumRegion(2, 1, 4, 3) -> 8
numMatrix.update(3, 2, 2)
numMatrix.sumRegion(2, 1, 4, 3) -> 10
- https://leetcode.com/problems/range-sum-query-viii/
- Difficulty: 60
- Frequent: 65
- Tags: ['Segment Tree']

### Range Sum Query VIII -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)
    
    def build_tree(self, nums):
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]
    
    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]
    
    def sumRange(self, left, right):
        left += self.n
        right += self.n
        sum = 0
        while left <= right:
            if left % 2 == 1:
                sum += self.tree[left]
                left += 1
            if right % 2 == 0:
                sum += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return sum
```
#### TC: O(log n) for both update and sumRange operations **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor, where the second half of the tree array is initialized with the input
numbers, and the first half is filled with the sums of the child nodes. The update function modifies
the value at a specific index and updates the tree accordingly. The sumRange function calculates the
sum of elements in a given range by traversing the tree and summing the relevant segments.

---
### Range Sum Query VIII -- Best TC Solution:
```python
# Segment Tree Approach

class SegmentTree:
    def __init__(self, nums):
        n = len(nums)
        self.tree = [0] * (4 * n)
        self.nums = nums
        self.build(0, 0, n - 1)

    def build(self, node, start, end):
        if start == end:
            self.tree[node] = self.nums[start]
        else:
            mid = (start + end) // 2
            left_child = 2 * node + 1
            right_child = 2 * node + 2
            self.build(left_child, start, mid)
            self.build(right_child, mid + 1, end)
            self.tree[node] = self.tree[left_child] + self.tree[right_child]

    def update(self, node, start, end, index, val):
        if start == end:
            self.nums[index] = val
            self.tree[node] = val
        else:
            mid = (start + end) // 2
            left_child = 2 * node + 1
            right_child = 2 * node + 2
            if start <= index <= mid:
                self.update(left_child, start, mid, index, val)
            else:
                self.update(right_child, mid + 1, end, index, val)
            self.tree[node] = self.tree[left_child] + self.tree[right_child]

    def sumRange(self, node, start, end, left, right):
        if start >= left and end <= right:
            return self.tree[node]
        if start > right or end < left:
            return 0
        mid = (start + end) // 2
        left_child = 2 * node + 1
        right_child = 2 * node + 2
        return self.sumRange(left_child, start, mid, left, right) + self.sumRange(right_child, mid + 1, end, left, right)


# Range Sum Query VIII

class RangeSumQuery:
    def __init__(self, nums):
        self.segment_tree = SegmentTree(nums)

    def update(self, index, val):
        self.segment_tree.update(0, 0, len(self.segment_tree.nums) - 1, index, val)

    def sumRange(self, left, right):
        return self.segment_tree.sumRange(0, 0, len(self.segment_tree.nums) - 1, left, right)
```
#### TC: Time Complexity:
- Build Segment Tree: O(n)
- Update Segment Tree: O(log n)
- Query Segment Tree: O(log n)

Overall Time Complexity: O(n) for building the segment tree and O(log n) for update and query operations. **SC:** Space Complexity: O(n) for the segment tree data structure.

The code implements the Range Sum Query VIII problem using the Segment Tree approach. The
SegmentTree class is used to build, update, and query the segment tree efficiently. The
RangeSumQuery class utilizes the SegmentTree class to perform update and sum range operations on the
given array.  Insights: - Building the segment tree has a time complexity of O(n) as each element is
visited once. - Updating and querying the segment tree have a time complexity of O(log n) due to the
binary tree structure. - The space complexity is O(n) for storing the segment tree data structure. -
The code provides a clean and efficient solution for handling range sum queries on an array.

---
---
---
 --- 
# Range Sum Query XI
>Given a 2D matrix, handle multiple sum range queries efficiently.

>matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
numMatrix = NumMatrix(matrix)
numMatrix.sumRegion(2, 1, 4, 3) -> 8
numMatrix.update(3, 2, 2)
numMatrix.sumRegion(2, 1, 4, 3) -> 10
- https://leetcode.com/problems/range-sum-query-xi/
- Difficulty: 60
- Frequent: 65
- Tags: ['Segment Tree']

### Range Sum Query XI -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        left += self.n
        right += self.n
        sum = 0
        while left <= right:
            if left % 2 == 1:
                sum += self.tree[left]
                left += 1
            if right % 2 == 0:
                sum += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return sum
```
#### TC: O(log n) **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor, where the second half of the tree array holds the input numbers, and the
first half holds the sums of the segments. The update function modifies the value at a specific
index and updates the tree accordingly. The sumRange function calculates the sum of elements in a
given range by traversing the tree and summing the relevant segments.

---
### Range Sum Query XI -- Best TC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)

    def build_tree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index: int, val: int) -> None:
        index += self.n
        self.tree[index] = val
        while index > 0:
            left = right = index
            if index % 2 == 0:
                right = index + 1
            else:
                left = index - 1
            self.tree[index // 2] = self.tree[left] + self.tree[right]
            index //= 2

    def sumRange(self, left: int, right: int) -> int:
        left += self.n
        right += self.n
        total = 0
        while left <= right:
            if left % 2 == 1:
                total += self.tree[left]
                left += 1
            if right % 2 == 0:
                total += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(index, val)
# param_2 = obj.sumRange(left, right)
```
#### TC: Time Complexity:
- Initialization: O(n)
- Update: O(log n)
- Range Sum Query: O(log n)

Overall Time Complexity: O(n) for initialization and O(log n) for update and range sum query. **SC:** Space Complexity: O(n) for the segment tree

The code implements the Range Sum Query XI problem using the Segment Tree approach. The NumArray
class initializes the segment tree with the given array of numbers. The build_tree method constructs
the segment tree. The update method updates a value at a specific index in the array and updates the
segment tree accordingly. The sumRange method calculates the sum of elements within a given range
using the segment tree. The code achieves a time complexity of O(n) for initialization and O(log n)
for update and range sum query, with a space complexity of O(n) for the segment tree. The segment
tree efficiently handles updates and range sum queries in logarithmic time, making it a smart and
efficient solution for the problem.

---
### Range Sum Query XI -- Best SC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)

    def build_tree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index: int, val: int) -> None:
        index += self.n
        self.tree[index] = val
        while index > 0:
            left = right = index
            if index % 2 == 0:
                right = index + 1
            else:
                left = index - 1
            self.tree[index // 2] = self.tree[left] + self.tree[right]
            index //= 2

    def sumRange(self, left: int, right: int) -> int:
        left += self.n
        right += self.n
        total = 0
        while left <= right:
            if left % 2 == 1:
                total += self.tree[left]
                left += 1
            if right % 2 == 0:
                total += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(index, val)
# param_2 = obj.sumRange(left, right)
```
#### TC: Time Complexity:
- Build Tree: O(n)
- Update: O(log n)
- Sum Range: O(log n) **SC:** Space Complexity: O(n)

The code implements the Range Sum Query XI problem using the Segment Tree approach. The NumArray
class is designed to efficiently handle updates and range sum queries on a given array of numbers.
The build_tree method constructs the segment tree, update method updates a value at a specific
index, and sumRange method calculates the sum of values within a given range. The use of a segment
tree allows for logarithmic time complexity for both update and sumRange operations, making it an
optimal solution for this problem.

---
---
---
 --- 
# Range Minimum Query
>Find the minimum value in a given range of an array.

>arr = [1, 3, 2, 7, 9, 11]
segmentTree = SegmentTree(arr)
segmentTree.rangeMinQuery(1, 5) -> 2
segmentTree.update(3, 0)
segmentTree.rangeMinQuery(1, 5) -> 0
- https://leetcode.com/problems/range-minimum-query/
- Difficulty: 45
- Frequent: 60
- Tags: ['Segment Tree']

### Range Minimum Query -- Shortest Solution:
```python
class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.tree = [0] * (2 * self.n)
        self.build(data)

    def build(self, data):
        for i in range(self.n):
            self.tree[self.n + i] = data[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = min(self.tree[i * 2], self.tree[i * 2 + 1])

    def update(self, index, value):
        pos = index + self.n
        self.tree[pos] = value
        while pos > 1:
            pos //= 2
            self.tree[pos] = min(self.tree[2 * pos], self.tree[2 * pos + 1])

    def range_min_query(self, left, right):
        left += self.n
        right += self.n
        min_val = float('inf')
        while left < right:
            if left % 2:
                min_val = min(min_val, self.tree[left])
                left += 1
            if right % 2:
                right -= 1
                min_val = min(min_val, self.tree[right])
            left //= 2
            right //= 2
        return min_val

data = [1, 3, 2, 7, 9, 11]
st = SegmentTree(data)
print(st.range_min_query(1, 5))
st.update(3, 0)
print(st.range_min_query(1, 5))
```
#### TC: O(log n) **SC:** O(n)

1. The Segment Tree is built using an array where the first half is reserved for leaves and the
second half for internal nodes. 2. The `build` function initializes the tree by placing the input
data in the leaves and then computing the internal nodes as the minimum of their respective
children. 3. The `update` function modifies a leaf and then updates the internal nodes to maintain
the Segment Tree property. 4. The `range_min_query` function computes the minimum value in a given
range by traversing the tree from the leaves to the root, adjusting the range as it goes.

---
### Range Minimum Query -- Best TC Solution:
```python
# Segment Tree Approach

class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.tree[self.n:] = nums
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = min(self.tree[i * 2], self.tree[i * 2 + 1])

    def update(self, i, val):
        i += self.n
        self.tree[i] = val
        while i > 1:
            self.tree[i // 2] = min(self.tree[i], self.tree[i ^ 1])
            i //= 2

    def query(self, left, right):
        left += self.n
        right += self.n
        res = float('inf')
        while left < right:
            if left % 2 == 1:
                res = min(res, self.tree[left])
                left += 1
            if right % 2 == 1:
                right -= 1
                res = min(res, self.tree[right])
            left //= 2
            right //= 2
        return res


# Example Usage
nums = [2, 4, 3, 1, 6, 7, 8, 9]
seg_tree = SegmentTree(nums)
print(seg_tree.query(1, 5))  # Output: 1
```
#### TC: 
Time Complexity:
- Construction: O(n)
- Update: O(log n)
- Query: O(log n)
 **SC:** 
Space Complexity: O(n)

The Segment Tree approach efficiently handles the Range Minimum Query problem by constructing a tree
data structure. The construction time complexity is O(n), where n is the number of elements. Updates
and queries have a time complexity of O(log n) due to the tree structure. The space complexity is
O(n) to store the tree. The code is clean, easy to understand, and provides a smart solution to the
problem.

---
---
---
 --- 
# Range Sum Query II
>Given a 2D matrix, handle multiple sum range queries efficiently.

>matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
numMatrix = NumMatrix(matrix)
numMatrix.sumRegion(2, 1, 4, 3) -> 8
numMatrix.update(3, 2, 2)
numMatrix.sumRegion(2, 1, 4, 3) -> 10
- https://leetcode.com/problems/range-sum-query-ii/
- Difficulty: 55
- Frequent: 60
- Tags: ['Segment Tree']

### Range Sum Query II -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        left += self.n
        right += self.n + 1
        sum = 0
        while left < right:
            if left % 2 == 1:
                sum += self.tree[left]
                left += 1
            if right % 2 == 1:
                right -= 1
                sum += self.tree[right]
            left //= 2
            right //= 2
        return sum
```
#### TC: O(log n) **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor, where the second half of the tree array stores the input numbers, and the
first half stores the sums of the segments. The update function modifies the value at a specific
index and updates the tree accordingly. The sumRange function calculates the sum of a given range by
traversing the tree and summing the relevant segments.

---
### Range Sum Query II -- Best TC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.tree[self.n:] = nums
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[i * 2] + self.tree[i * 2 + 1]

    def update(self, i: int, val: int) -> None:
        i += self.n
        self.tree[i] = val
        while i > 0:
            left = right = i
            if i % 2 == 0:
                right = i + 1
            else:
                left = i - 1
            self.tree[i // 2] = self.tree[left] + self.tree[right]
            i //= 2

    def sumRange(self, i: int, j: int) -> int:
        i += self.n
        j += self.n
        total = 0
        while i <= j:
            if i % 2 == 1:
                total += self.tree[i]
                i += 1
            if j % 2 == 0:
                total += self.tree[j]
                j -= 1
            i //= 2
            j //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```
#### TC: Time Complexity:
- Initialization: O(n)
- Update: O(log n)
- Range Sum Query: O(log n) **SC:** Space Complexity: O(n)

The code implements the Range Sum Query II problem using the Segment Tree approach. The NumArray
class is initialized with the input array 'nums' and constructs a segment tree to store the
cumulative sum of elements. The update method modifies the value at a specific index and updates the
segment tree accordingly. The sumRange method calculates the sum of elements within a given range
efficiently using the segment tree structure. The time complexity for initialization, update, and
range sum query is O(n) and O(log n) respectively. The space complexity is O(n) to store the segment
tree.

---
---
---
 --- 
# Range Sum Query III
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [1, 3, 5]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 9
numArray.update(1, 2)
numArray.sumRange(0, 2) -> 8
- https://leetcode.com/problems/range-sum-query-iii/
- Difficulty: 50
- Frequent: 55
- Tags: ['Segment Tree']

### Range Sum Query III -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[i * 2] + self.tree[i * 2 + 1]

    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        left += self.n
        right += self.n + 1
        sum = 0
        while left < right:
            if left % 2 == 1:
                sum += self.tree[left]
                left += 1
            if right % 2 == 1:
                right -= 1
                sum += self.tree[right]
            left //= 2
            right //= 2
        return sum
```
#### TC: O(log n) **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor by initializing the leaves with the input array and then computing the
internal nodes. The update function modifies a value and updates the tree accordingly. The sumRange
function calculates the sum of a range by traversing the tree and summing the relevant segments.

---
### Range Sum Query III -- Best TC Solution:
```python
# Segment Tree Approach

class SegmentTreeNode:
    def __init__(self, start, end):
        self.start = start
        self.end = end
        self.total = 0
        self.left = None
        self.right = None


class NumArray:
    def __init__(self, nums: List[int]):
        self.root = self.build_tree(nums, 0, len(nums) - 1)

    def build_tree(self, nums, start, end):
        if start > end:
            return None
        root = SegmentTreeNode(start, end)
        if start == end:
            root.total = nums[start]
        else:
            mid = start + (end - start) // 2
            root.left = self.build_tree(nums, start, mid)
            root.right = self.build_tree(nums, mid + 1, end)
            root.total = root.left.total + root.right.total
        return root

    def sum_range(self, i: int, j: int) -> int:
        return self.query(self.root, i, j)

    def query(self, root, start, end):
        if root.start == start and root.end == end:
            return root.total
        mid = root.start + (root.end - root.start) // 2
        if end <= mid:
            return self.query(root.left, start, end)
        elif start >= mid + 1:
            return self.query(root.right, start, end)
        else:
            return self.query(root.left, start, mid) + self.query(root.right, mid + 1, end)
```
#### TC: Time Complexity: 
- Construction: O(n)
- Query: O(log n) **SC:** Space Complexity: O(n)

The code implements the Range Sum Query III problem using the Segment Tree approach. The
SegmentTreeNode class represents each node in the segment tree, and the NumArray class builds the
segment tree from the input array. The build_tree function constructs the segment tree recursively,
and the sum_range function calculates the sum of a given range using the query function. The query
function traverses the segment tree to find the sum efficiently. The time complexity for
construction is O(n) where n is the number of elements, and the query operation has a time
complexity of O(log n) due to the binary search in the segment tree. The space complexity is O(n) to
store the segment tree nodes.

---
---
---
 --- 
# Range Sum Query VI
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [1, 3, 5]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 9
numArray.update(1, 2)
numArray.sumRange(0, 2) -> 8
- https://leetcode.com/problems/range-sum-query-vi/
- Difficulty: 50
- Frequent: 55
- Tags: ['Segment Tree']

### Range Sum Query VI -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        left += self.n
        right += self.n + 1
        sum = 0
        while left < right:
            if left % 2 == 1:
                sum += self.tree[left]
                left += 1
            if right % 2 == 1:
                right -= 1
                sum += self.tree[right]
            left //= 2
            right //= 2
        return sum
```
#### TC: O(log n) for update and sumRange operations **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor, where the second half of the tree array holds the input numbers, and the
first half holds the sums of the segments. The update function modifies the value at a specific
index and updates the tree accordingly. The sumRange function calculates the sum of a given range by
traversing the tree and summing the relevant segments.

---
### Range Sum Query VI -- Best TC Solution:
```python
from collections import deque

def maxResult(nums, k):
    dp = [0] * len(nums)
    dp[0] = nums[0]
    q = deque([0])
    for i in range(1, len(nums)):
        while q and q[0] < i - k:
            q.popleft()
        dp[i] = nums[i] + dp[q[0]]
        while q and dp[i] >= dp[q[-1]]:
            q.pop()
        q.append(i)
    return dp[-1]
```
#### TC: O(n) **SC:** O(n)

The code uses dynamic programming with a deque to efficiently calculate the maximum score. It
iterates through the nums array, maintaining a deque of indices within the window size k. The dp
array stores the maximum score at each index. The code optimizes the calculation by updating the
deque based on the current index and the maximum score. This approach ensures a time complexity of
O(n) and a space complexity of O(n).

---
### Range Sum Query VI -- Best SC Solution:
```python
# Segment Tree Approach

class SegmentTreeNode:
    def __init__(self, start, end):
        self.start = start
        self.end = end
        self.total = 0
        self.left = None
        self.right = None


class SegmentTree:
    def __init__(self, nums):
        def build_tree(nums, l, r):
            if l > r:
                return None
            if l == r:
                node = SegmentTreeNode(l, r)
                node.total = nums[l]
                return node
            mid = (l + r) // 2
            left = build_tree(nums, l, mid)
            right = build_tree(nums, mid + 1, r)
            node = SegmentTreeNode(l, r)
            node.total = left.total + right.total
            node.left = left
            node.right = right
            return node
        self.root = build_tree(nums, 0, len(nums) - 1)

    def update(self, i, val):
        def update_tree(node, i, val):
            if node.start == node.end:
                node.total = val
                return val
            mid = (node.start + node.end) // 2
            if i <= mid:
                update_tree(node.left, i, val)
            else:
                update_tree(node.right, i, val)
            node.total = node.left.total + node.right.total
            return node.total
        update_tree(self.root, i, val)

    def sum_range(self, i, j):
        def query_tree(node, i, j):
            if node.start == i and node.end == j:
                return node.total
            mid = (node.start + node.end) // 2
            if j <= mid:
                return query_tree(node.left, i, j)
            elif i >= mid + 1:
                return query_tree(node.right, i, j)
            else:
                return query_tree(node.left, i, mid) + query_tree(node.right, mid + 1, j)
        return query_tree(self.root, i, j)


# Usage
nums = [1, 3, 5]
obj = SegmentTree(nums)
print(obj.sum_range(0, 2))  # Output: 9
obj.update(1, 2)
print(obj.sum_range(0, 2))  # Output: 8
```
#### TC: Time Complexity:
- Build Segment Tree: O(n)
- Update: O(log n)
- Range Sum Query: O(log n) **SC:** Space Complexity: O(n) for the segment tree

The code implements a Segment Tree data structure to efficiently handle range sum queries and
updates. The SegmentTreeNode class represents each node in the segment tree, while the SegmentTree
class builds the tree from the input array.  - Building the segment tree takes O(n) time complexity
as each element is visited once. - Updating a value in the tree has a time complexity of O(log n) as
we traverse the tree height. - Querying the sum in a range also has a time complexity of O(log n) as
we navigate the tree based on the range.  The space complexity is O(n) to store the segment tree
nodes.  Overall, the Segment Tree approach provides an efficient solution for handling range sum
queries with logarithmic time complexity for updates and queries.

---
---
---
 --- 
# Range Sum Query IX
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [1, 3, 5]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 9
numArray.update(1, 2)
numArray.sumRange(0, 2) -> 8
- https://leetcode.com/problems/range-sum-query-ix/
- Difficulty: 50
- Frequent: 55
- Tags: ['Segment Tree']

### Range Sum Query IX -- Shortest Solution:
```python
class NumMatrix:
    def __init__(self, matrix):
        if not matrix or not matrix[0]:
            return
        self.m, self.n = len(matrix), len(matrix[0])
        self.tree = [[0] * (self.n + 1) for _ in range(self.m + 1)]
        self.nums = [[0] * self.n for _ in range(self.m)]
        for i in range(self.m):
            for j in range(self.n):
                self.update(i, j, matrix[i][j])

    def update(self, row, col, val):
        if not self.m or not self.n:
            return
        delta = val - self.nums[row][col]
        self.nums[row][col] = val
        i = row + 1
        while i <= self.m:
            j = col + 1
            while j <= self.n:
                self.tree[i][j] += delta
                j += j & -j
            i += i & -i

    def sumRegion(self, row1, col1, row2, col2):
        return self._sum(row2 + 1, col2 + 1) - self._sum(row1, col2 + 1) - self._sum(row2 + 1, col1) + self._sum(row1, col1)

    def _sum(self, row, col):
        s = 0
        i = row
        while i > 0:
            j = col
            while j > 0:
                s += self.tree[i][j]
                j -= j & -j
            i -= i & -i
        return s
```
#### TC: O(log(m) * log(n)) **SC:** O(m * n)

The solution uses a 2D Binary Indexed Tree (BIT) to efficiently handle both updates and sum queries.
The `update` function adjusts the BIT with the difference between the new and old values, while the
`sumRegion` function calculates the sum of a submatrix by combining results from the BIT. The `_sum`
helper function computes the prefix sum up to a given row and column in the BIT.

---
### Range Sum Query IX -- Best TC Solution:
```python
# Segment Tree Approach

class SegmentTreeNode:
    def __init__(self, start, end):
        self.start = start
        self.end = end
        self.total = 0
        self.left = None
        self.right = None


class NumArray:
    def __init__(self, nums: List[int]):
        self.root = self.build_tree(nums, 0, len(nums) - 1)

    def build_tree(self, nums, start, end):
        if start > end:
            return None
        node = SegmentTreeNode(start, end)
        if start == end:
            node.total = nums[start]
        else:
            mid = (start + end) // 2
            node.left = self.build_tree(nums, start, mid)
            node.right = self.build_tree(nums, mid + 1, end)
            node.total = node.left.total + node.right.total
        return node

    def update(self, i: int, val: int) -> None:
        self.update_tree(self.root, i, val)

    def update_tree(self, node, i, val):
        if node.start == node.end:
            node.total = val
            return val
        mid = (node.start + node.end) // 2
        if i <= mid:
            self.update_tree(node.left, i, val)
        else:
            self.update_tree(node.right, i, val)
        node.total = node.left.total + node.right.total
        return node.total

    def sumRange(self, i: int, j: int) -> int:
        return self.query_tree(self.root, i, j)

    def query_tree(self, node, i, j):
        if node.start == i and node.end == j:
            return node.total
        mid = (node.start + node.end) // 2
        if j <= mid:
            return self.query_tree(node.left, i, j)
        elif i >= mid + 1:
            return self.query_tree(node.right, i, j)
        else:
            return self.query_tree(node.left, i, mid) + self.query_tree(node.right, mid + 1, j)
```
#### TC: Time Complexity:
- Build Tree: O(n)
- Update: O(log n)
- Query: O(log n)

Overall Time Complexity: O(n) for building the tree and O(log n) for update and query operations.

 **SC:** Space Complexity: O(n) for the segment tree structure.

The code implements the Range Sum Query IX problem using the Segment Tree approach. The
SegmentTreeNode class represents each node in the segment tree, and the NumArray class handles
building the tree, updating values, and querying the sum range efficiently.   Insights: - Building
the segment tree has a time complexity of O(n) as each element is visited once. - Updating and
querying have a time complexity of O(log n) due to the binary tree structure. - The space complexity
is O(n) for storing the segment tree structure. - The code provides a clean and efficient solution
for handling range sum queries.

---
### Range Sum Query IX -- Best SC Solution:
```python
# Segment Tree Approach

class SegmentTree:
    def __init__(self, nums):
        n = len(nums)
        self.tree = [0] * (2 * n)
        self.n = n
        for i in range(n, 2 * n):
            self.tree[i] = nums[i - n]
        for i in range(n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, i, val):
        i += self.n
        self.tree[i] = val
        while i > 1:
            self.tree[i // 2] = self.tree[i] + self.tree[i ^ 1]
            i //= 2

    def query(self, left, right):
        left += self.n
        right += self.n
        res = 0
        while left < right:
            if left % 2 == 1:
                res += self.tree[left]
                left += 1
            if right % 2 == 1:
                right -= 1
                res += self.tree[right]
            left //= 2
            right //= 2
        return res


class NumArray:
    def __init__(self, nums: List[int]):
        self.tree = SegmentTree(nums)

    def update(self, i: int, val: int) -> None:
        self.tree.update(i, val)

    def sumRange(self, left: int, right: int) -> int:
        return self.tree.query(left, right + 1)
```
#### TC: Time Complexity:
- Initialization: O(n)
- Update: O(log n)
- Query: O(log n) **SC:** Space Complexity: O(n)

- The Segment Tree approach is used to efficiently handle range sum queries. - The SegmentTree class
is implemented to build the segment tree, update values, and query the sum within a range. - The
NumArray class is a wrapper that utilizes the SegmentTree class to perform operations on the input
array. - Initialization builds the segment tree with a time complexity of O(n). - Update and query
operations have a time complexity of O(log n) due to the binary tree structure of the segment tree.
- The space complexity is O(n) to store the segment tree array. - The update function modifies the
segment tree efficiently by updating the values and recalculating the parent nodes. - The query
function traverses the segment tree to find the sum within the specified range. - Overall, the
Segment Tree approach provides a clean and efficient solution for handling range sum queries.

---
---
---
 --- 
# Range Sum Query XII
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [1, 3, 5]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 9
numArray.update(1, 2)
numArray.sumRange(0, 2) -> 8
- https://leetcode.com/problems/range-sum-query-xii/
- Difficulty: 50
- Frequent: 55
- Tags: ['Segment Tree']

### Range Sum Query XII -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)
    
    def build_tree(self, nums):
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]
    
    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]
    
    def sumRange(self, left, right):
        l, r = left + self.n, right + self.n
        sum = 0
        while l <= r:
            if l % 2 == 1:
                sum += self.tree[l]
                l += 1
            if r % 2 == 0:
                sum += self.tree[r]
                r -= 1
            l //= 2
            r //= 2
        return sum
```
#### TC: O(log n) for update and sumRange operations **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor, where the second half of the tree array holds the input numbers, and the
first half holds the sums of the segments. The update function modifies the value at a specific
index and updates the tree accordingly. The sumRange function calculates the sum of a given range by
traversing the tree and summing the relevant segments.

---
### Range Sum Query XII -- Best TC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)

    def build_tree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index: int, val: int) -> None:
        index += self.n
        self.tree[index] = val
        while index > 0:
            left = right = index
            if index % 2 == 0:
                right = index + 1
            else:
                left = index - 1
            self.tree[index // 2] = self.tree[left] + self.tree[right]
            index //= 2

    def sumRange(self, left: int, right: int) -> int:
        left += self.n
        right += self.n
        total = 0
        while left <= right:
            if left % 2 == 1:
                total += self.tree[left]
                left += 1
            if right % 2 == 0:
                total += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(index, val)
# param_2 = obj.sumRange(left, right)
```
#### TC: Time Complexity:
- Initialization: O(n)
- Update: O(log n)
- Range Sum Query: O(log n)

Overall Time Complexity: O(n) for initialization, O(log n) for update and range sum query **SC:** Space Complexity: O(n) for the segment tree

- The segment tree approach is used to efficiently handle range sum queries and updates. - The
build_tree method constructs the segment tree from the given array. - The update method updates the
value at a specific index in the array and propagates the changes up the tree. - The sumRange method
calculates the sum of elements within a given range efficiently using the segment tree. - The
segment tree allows for faster range sum queries compared to brute force methods. - Understanding
the tree structure and the update/sumRange logic is key to grasping the implementation.

---
### Range Sum Query XII -- Best SC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)

    def build_tree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index: int, val: int) -> None:
        index += self.n
        self.tree[index] = val
        while index > 0:
            left = right = index
            if index % 2 == 0:
                right = index + 1
            else:
                left = index - 1
            self.tree[index // 2] = self.tree[left] + self.tree[right]
            index //= 2

    def sumRange(self, left: int, right: int) -> int:
        left += self.n
        right += self.n
        total = 0
        while left <= right:
            if left % 2 == 1:
                total += self.tree[left]
                left += 1
            if right % 2 == 0:
                total += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(index, val)
# param_2 = obj.sumRange(left, right)
```
#### TC: Time Complexity:
- Initialization: O(n)
- Update: O(log n)
- Range Sum Query: O(log n)

Overall Time Complexity: O(n) for initialization and O(log n) for update and range sum query. **SC:** Space Complexity: O(n) for the segment tree structure.

The code implements the Range Sum Query problem using the Segment Tree approach. The NumArray class
initializes the segment tree with the given array of numbers. The build_tree method constructs the
segment tree. The update method updates a value at a specific index in the array and adjusts the
segment tree accordingly. The sumRange method calculates the sum of elements within a given range
efficiently using the segment tree structure.  The Segment Tree approach provides an efficient
solution for range sum queries with a time complexity of O(log n) for both update and range sum
queries. The space complexity is O(n) for storing the segment tree structure, where n is the number
of elements in the input array.

---
---
---
 --- 
# Range Sum Query - Immutable
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [-2, 0, 3, -5, 2, -1]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 1
numArray.sumRange(2, 5) -> -1
numArray.sumRange(0, 5) -> -3
- https://leetcode.com/problems/range-sum-query-immutable/
- Difficulty: 30
- Frequent: 50
- Tags: ['Segment Tree']

### Range Sum Query - Immutable -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.sums = [0] * (len(nums) + 1)
        for i in range(len(nums)):
            self.sums[i + 1] = self.sums[i] + nums[i]

    def sumRange(self, i, j):
        return self.sums[j + 1] - self.sums[i]
```
#### TC: O(1) for sumRange, O(n) for initialization **SC:** O(n)

The code uses a prefix sum array to store cumulative sums of the input array. During initialization,
it computes the cumulative sum for each index and stores it in the `sums` array. The `sumRange`
function then simply calculates the sum of elements between indices `i` and `j` by subtracting the
cumulative sum at `i` from the cumulative sum at `j+1`. This makes the range sum query operation
very efficient with O(1) time complexity.

---
### Range Sum Query - Immutable -- Best TC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        n = len(nums)
        self.tree = [0] * (2 * n)
        self.n = n
        self.buildTree(nums)

    def buildTree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, i: int, val: int) -> None:
        i += self.n
        self.tree[i] = val
        while i > 0:
            left = right = i
            if i % 2 == 0:
                right = i + 1
            else:
                left = i - 1
            self.tree[i // 2] = self.tree[left] + self.tree[right]
            i //= 2

    def sumRange(self, i: int, j: int) -> int:
        i += self.n
        j += self.n
        total = 0
        while i <= j:
            if i % 2 == 1:
                total += self.tree[i]
                i += 1
            if j % 2 == 0:
                total += self.tree[j]
                j -= 1
            i //= 2
            j //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```
#### TC: Time Complexity:
- Initialization: O(n)
- Update: O(log n)
- Range Sum Query: O(log n)

where n is the number of elements in the input array. **SC:** Space Complexity: O(n)

where n is the number of elements in the input array.

- The Segment Tree approach is used to efficiently handle range sum queries and updates. - The
buildTree method constructs the segment tree from the input array. - The update method updates the
value at a specific index in the array and propagates the changes up the tree. - The sumRange method
calculates the sum of elements within a given range efficiently using the segment tree structure. -
The time complexity for initialization, update, and range sum query operations is logarithmic due to
the segment tree structure.

---
### Range Sum Query - Immutable -- Best SC Solution:
```python
# Approach: Prefix Sum

class NumArray:
    def __init__(self, nums: List[int]):
        self.prefix_sum = [0]
        for num in nums:
            self.prefix_sum.append(self.prefix_sum[-1] + num)

    def sumRange(self, left: int, right: int) -> int:
        return self.prefix_sum[right + 1] - self.prefix_sum[left]

# Time Complexity: O(1) for sumRange, O(n) for __init__
# Space Complexity: O(n) for prefix_sum
```
#### TC: O(1) for sumRange, O(n) for __init__ **SC:** O(n) for prefix_sum

The code implements the Prefix Sum approach to solve the Range Sum Query - Immutable problem. The
constructor initializes a prefix sum array where each element stores the sum of all elements up to
that index. The sumRange method then calculates the sum of elements between the given indices using
the prefix sum array. This approach provides constant time complexity for sumRange queries and
linear time complexity for initialization. The space complexity is linear due to the prefix sum
array storing the cumulative sums.

---
---
---
 --- 
# Range Sum Query IV
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [-2, 0, 3, -5, 2, -1]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 1
numArray.sumRange(2, 5) -> -1
numArray.sumRange(0, 5) -> -3
- https://leetcode.com/problems/range-sum-query-iv/
- Difficulty: 35
- Frequent: 45
- Tags: ['Segment Tree']

### Range Sum Query IV -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.nums = nums
        self.tree = [0] * (2 * len(nums))
        self.build_tree()

    def build_tree(self):
        n = len(self.nums)
        for i in range(n):
            self.tree[n + i] = self.nums[i]
        for i in range(n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, val):
        n = len(self.nums)
        pos = n + index
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        n = len(self.nums)
        l, r = n + left, n + right
        summ = 0
        while l <= r:
            if l % 2 == 1:
                summ += self.tree[l]
                l += 1
            if r % 2 == 0:
                summ += self.tree[r]
                r -= 1
            l //= 2
            r //= 2
        return summ
```
#### TC: O(log n) **SC:** O(n)

The solution uses a Segment Tree to efficiently handle range sum queries and updates. The tree is
built in the constructor, and both update and sumRange operations are performed in O(log n) time.
The space complexity is O(n) due to the storage of the segment tree.

---
### Range Sum Query IV -- Best TC Solution:
```python
# Segment Tree Approach

class SegmentTreeNode:
    def __init__(self, start, end, val, left=None, right=None):
        self.start = start
        self.end = end
        self.val = val
        self.left = left
        self.right = right

class NumArray:
    def __init__(self, nums: List[int]):
        self.root = self.build_tree(nums, 0, len(nums) - 1)
        
    def build_tree(self, nums, start, end):
        if start > end:
            return None
        if start == end:
            return SegmentTreeNode(start, end, nums[start])
        mid = start + (end - start) // 2
        left = self.build_tree(nums, start, mid)
        right = self.build_tree(nums, mid + 1, end)
        return SegmentTreeNode(start, end, left.val + right.val, left, right)

    def sumRange(self, left: int, right: int) -> int:
        return self.query(self.root, left, right)

    def query(self, node, start, end):
        if node.start == start and node.end == end:
            return node.val
        mid = node.start + (node.end - node.start) // 2
        if end <= mid:
            return self.query(node.left, start, end)
        elif start > mid:
            return self.query(node.right, start, end)
        else:
            return self.query(node.left, start, mid) + self.query(node.right, mid + 1, end)
```
#### TC: Time Complexity: 
- Build Tree: O(n)
- Query: O(log n) **SC:** Space Complexity: O(n)

The code implements the Range Sum Query IV problem using the Segment Tree approach. The
SegmentTreeNode class represents each node in the segment tree, and the NumArray class builds the
segment tree from the input array. The build_tree function constructs the segment tree recursively,
and the sumRange function calculates the sum of a given range by querying the segment tree. The
query function recursively traverses the segment tree to find the sum of the specified range. The
time complexity for building the tree is O(n), and for querying, it is O(log n) due to the binary
search in the segment tree. The space complexity is O(n) to store the segment tree nodes.

---
---
---
 --- 
# Range Sum Query VII
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [-2, 0, 3, -5, 2, -1]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 1
numArray.sumRange(2, 5) -> -1
numArray.sumRange(0, 5) -> -3
- https://leetcode.com/problems/range-sum-query-vii/
- Difficulty: 35
- Frequent: 45
- Tags: ['Segment Tree']

### Range Sum Query VII -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.nums = nums
        self.prefix_sums = [0] * (len(nums) + 1)
        for i in range(len(nums)):
            self.prefix_sums[i + 1] = self.prefix_sums[i] + nums[i]

    def sumRange(self, left, right):
        return self.prefix_sums[right + 1] - self.prefix_sums[left]
```
#### TC: O(1) for sumRange query, O(n) for preprocessing **SC:** O(n)

The code uses a prefix sum array to store cumulative sums of the input array. The constructor
initializes the prefix sum array in O(n) time. The sumRange function then computes the sum of
elements between indices left and right in constant O(1) time by subtracting the prefix sum at left
from the prefix sum at right + 1.

---
### Range Sum Query VII -- Best TC Solution:
```python
from typing import List

class RangeSumQuery:
    def __init__(self, nums: List[int]):
        self.prefix_sum = [0] * (len(nums) + 1)
        for i in range(len(nums)):
            self.prefix_sum[i + 1] = self.prefix_sum[i] + nums[i]

    def sumRange(self, left: int, right: int) -> int:
        return self.prefix_sum[right + 1] - self.prefix_sum[left]

# Example usage
nums = [-2, 0, 3, -5, 2, -1]
rsq = RangeSumQuery(nums)
print(rsq.sumRange(0, 2))  # Output: 1
print(rsq.sumRange(2, 5))  # Output: -1
```
#### TC: Time Complexity: O(N) for initialization, O(1) for each query **SC:** Space Complexity: O(N) for storing the prefix sum array

The code defines a class RangeSumQuery that initializes the prefix sum array to store the cumulative
sum of the input array. The sumRange method then calculates the sum of a range by subtracting the
prefix sum at the left index from the prefix sum at the right index. This approach allows for
constant time complexity for each query after the initialization. The space complexity is O(N) to
store the prefix sum array, where N is the length of the input array.

---
### Range Sum Query VII -- Best SC Solution:
```python
# Segment Tree Approach

class SegmentTree:
    def __init__(self, nums):
        n = len(nums)
        self.tree = [0] * (2 * n)
        self.n = n
        self.build_tree(nums)

    def build_tree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, val):
        index += self.n
        self.tree[index] = val
        while index > 1:
            self.tree[index // 2] = self.tree[index] + self.tree[index ^ 1]
            index //= 2

    def query(self, left, right):
        left += self.n
        right += self.n
        res = 0
        while left <= right:
            if left % 2 == 1:
                res += self.tree[left]
                left += 1
            if right % 2 == 0:
                res += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return res


class RangeSumQuery:
    def __init__(self, nums: List[int]):
        self.segment_tree = SegmentTree(nums)

    def update(self, index: int, val: int) -> None:
        self.segment_tree.update(index, val)

    def sumRange(self, left: int, right: int) -> int:
        return self.segment_tree.query(left, right)
```
#### TC: Time Complexity:
- Build Tree: O(n)
- Update: O(log n)
- Query: O(log n)

Overall Time Complexity: O(n) for building the tree and O(log n) for update and query operations. **SC:** Space Complexity: O(n) for the segment tree data structure.

- The code implements a Segment Tree data structure to efficiently handle range sum queries. - The
SegmentTree class is used to build the tree, update values, and query the sum within a given range.
- The RangeSumQuery class utilizes the SegmentTree class to perform update and sumRange operations.
- Building the tree has a time complexity of O(n) as it iterates through the input array. - Update
and query operations have a time complexity of O(log n) due to the binary tree structure. - The
space complexity is O(n) to store the segment tree data structure.

---
---
---
 --- 
# Range Sum Query X
>Given an integer array nums, handle multiple sum range queries efficiently.

>nums = [-2, 0, 3, -5, 2, -1]
numArray = NumArray(nums)
numArray.sumRange(0, 2) -> 1
numArray.sumRange(2, 5) -> -1
numArray.sumRange(0, 5) -> -3
- https://leetcode.com/problems/range-sum-query-x/
- Difficulty: 35
- Frequent: 45
- Tags: ['Segment Tree']

### Range Sum Query X -- Shortest Solution:
```python
class NumArray:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        for i in range(self.n):
            self.tree[self.n + i] = nums[i]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[i * 2] + self.tree[i * 2 + 1]

    def update(self, index, val):
        pos = index + self.n
        self.tree[pos] = val
        while pos > 1:
            pos //= 2
            self.tree[pos] = self.tree[2 * pos] + self.tree[2 * pos + 1]

    def sumRange(self, left, right):
        left += self.n
        right += self.n + 1
        sum = 0
        while left < right:
            if left % 2 == 1:
                sum += self.tree[left]
                left += 1
            if right % 2 == 1:
                right -= 1
                sum += self.tree[right]
            left //= 2
            right //= 2
        return sum
```
#### TC: O(log n) **SC:** O(n)

The code uses a Segment Tree to efficiently handle range sum queries and updates. The tree is built
in the constructor by initializing the leaves with the input array and then computing the internal
nodes. The update function modifies a value and updates the tree accordingly. The sumRange function
calculates the sum of a range by traversing the tree and summing the relevant segments.

---
### Range Sum Query X -- Best TC Solution:
```python
# Segment Tree Approach

class NumArray:
    def __init__(self, nums: List[int]):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.build_tree(nums)

    def build_tree(self, nums):
        for i in range(self.n, 2 * self.n):
            self.tree[i] = nums[i - self.n]
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index: int, val: int) -> None:
        index += self.n
        self.tree[index] = val
        while index > 0:
            left = right = index
            if index % 2 == 0:
                right = index + 1
            else:
                left = index - 1
            self.tree[index // 2] = self.tree[left] + self.tree[right]
            index //= 2

    def sumRange(self, left: int, right: int) -> int:
        left += self.n
        right += self.n
        total = 0
        while left <= right:
            if left % 2 == 1:
                total += self.tree[left]
                left += 1
            if right % 2 == 0:
                total += self.tree[right]
                right -= 1
            left //= 2
            right //= 2
        return total

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(index, val)
# param_2 = obj.sumRange(left, right)
```
#### TC: Time Complexity:
- Build Tree: O(n)
- Update: O(log n)
- Sum Range: O(log n) **SC:** Space Complexity: O(n)

The code implements the Range Sum Query problem using the Segment Tree approach. The NumArray class
is designed to efficiently handle updates and range sum queries on a given array of numbers. The
build_tree method constructs the segment tree from the input array. The update method updates the
value at a specific index and propagates the changes up the tree. The sumRange method calculates the
sum of values within a given range efficiently by traversing the tree. Overall, the Segment Tree
approach provides a balanced trade-off between time and space complexity for handling range sum
queries.

---
---
---