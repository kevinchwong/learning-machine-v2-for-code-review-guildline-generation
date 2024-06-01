# Binary Indexed Tree
## Frequent score for this algorithmic framework: 30 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Count of Range Sum](#count-of-range-sum) | 55 | 70 |
| 2 | [K-th Smallest Prime Fraction](#k-th-smallest-prime-fraction) | 50 | 45 |
---
A Binary Indexed Tree (BIT), also known as a Fenwick Tree, is a data structure that provides efficient methods for cumulative frequency tables. It allows querying prefix sums and updating elements in logarithmic time, making it useful for scenarios requiring dynamic cumulative frequency tables.
```python
class BIT:
    def __init__(self, size):
        self.size = size
        self.tree = [0] * (size + 1)

    def update(self, index, value):
        while index <= self.size:
            self.tree[index] += value
            index += index & -index

    def query(self, index):
        sum = 0
        while index > 0:
            sum += self.tree[index]
            index -= index & -index
        return sum

# Example usage
bit = BIT(10)
bit.update(3, 5)
bit.update(5, 2)
print(bit.query(5))  # Output: 7
print(bit.query(3))  # Output: 5
```
### Insight:
1. The BIT is initialized with a size, and an internal tree array is created with size+1 elements to handle 1-based indexing.
2. The update function increments the value at a given index and propagates the change to all relevant nodes.
3. The query function computes the prefix sum up to a given index by traversing the tree and summing relevant nodes.
4. The index manipulation using index & -index helps in efficiently navigating the tree structure.
---
 --- 
# Count of Range Sum
>Count the number of range sums that lie in a given range.

>Input: nums = [-2,5,-1], lower = -2, upper = 2
Output: 3
- https://leetcode.com/problems/count-of-range-sum/
- Difficulty: 70
- Frequent: 55
- Tags: ['Binary Indexed Tree', 'Segment Tree', 'Merge Sort']

### Count of Range Sum -- Shortest Solution:
```python
class FenwickTree:
    def __init__(self, size):
        self.size = size
        self.tree = [0] * (size + 1)

    def update(self, index, delta):
        while index <= self.size:
            self.tree[index] += delta
            index += index & -index

    def query(self, index):
        sum = 0
        while index > 0:
            sum += self.tree[index]
            index -= index & -index
        return sum

class Solution:
    def countRangeSum(self, nums, lower, upper):
        prefix_sums = [0]
        for num in nums:
            prefix_sums.append(prefix_sums[-1] + num)

        all_sums = set(prefix_sums)
        for sum in prefix_sums:
            all_sums.add(sum - lower)
            all_sums.add(sum - upper)

        sorted_sums = sorted(all_sums)
        ranks = {v: i + 1 for i, v in enumerate(sorted_sums)}

        fenwick_tree = FenwickTree(len(ranks))
        count = 0
        for sum in prefix_sums:
            count += fenwick_tree.query(ranks[sum] - ranks[upper] - 1) - fenwick_tree.query(ranks[sum] - ranks[lower] - 1)
            fenwick_tree.update(ranks[sum], 1)

        return count
```
#### TC: O(n log n) **SC:** O(n)

1. **Fenwick Tree (Binary Indexed Tree)**: This data structure is used to efficiently update
elements and calculate prefix sums in a list of numbers. 2. **Prefix Sums**: The prefix sums of the
input list are calculated to transform the range sum problem into a prefix sum problem. 3.
**Coordinate Compression**: The prefix sums and their adjusted values (by adding/subtracting the
lower and upper bounds) are sorted and ranked to fit into the Fenwick Tree. 4. **Query and Update**:
For each prefix sum, the number of valid range sums ending at the current position is calculated
using the Fenwick Tree, and then the current prefix sum is added to the tree for future queries.

---
### Count of Range Sum -- Best TC Solution:
```python
# Binary Indexed Tree Approach

class BIT:
    def __init__(self, n):
        self.bit = [0] * (n + 1)

    def update(self, i, val):
        while i < len(self.bit):
            self.bit[i] += val
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            res += self.bit[i]
            i -= i & -i
        return res


def countRangeSum(nums, lower, upper):
    prefix_sum = [0]
    for num in nums:
        prefix_sum.append(prefix_sum[-1] + num)
    sorted_set = sorted(set(prefix_sum))
    bit = BIT(len(sorted_set))
    count = 0
    for num in prefix_sum:
        count += bit.query(bisect_right(sorted_set, num - lower)) - bit.query(bisect_left(sorted_set, num - upper))
        bit.update(bisect_left(sorted_set, num) + 1, 1)
    return count
```
#### TC: O(nlogn) **SC:** O(n)

The code implements the Count of Range Sum problem using the Binary Indexed Tree (BIT) approach. It
first calculates the prefix sum of the input array and stores it in a list. Then, it creates a
sorted set of the prefix sums to handle the range queries efficiently. The BIT class is used to
update and query the prefix sums within the specified range. By iterating through the prefix sums
and updating the BIT, the code calculates the count of range sums that fall within the given lower
and upper bounds. The time complexity of the solution is O(nlogn) due to sorting, and the space
complexity is O(n) to store the prefix sums and the BIT array.

---
---
---
 --- 
# K-th Smallest Prime Fraction
>Find the k-th smallest fraction formed by the elements of the array.

>Input: arr = [1, 2, 3, 5], k = 3
Output: [2, 5]
- https://leetcode.com/problems/k-th-smallest-prime-fraction/
- Difficulty: 45
- Frequent: 50
- Tags: ['Binary Indexed Tree', 'Heap', 'Binary Search']

### K-th Smallest Prime Fraction -- Shortest Solution:
```python
import heapq

def kthSmallestPrimeFraction(arr, k):
    heap = [(arr[i] / arr[-1], i, len(arr) - 1) for i in range(len(arr) - 1)]
    heapq.heapify(heap)
    for _ in range(k - 1):
        _, i, j = heapq.heappop(heap)
        if j - 1 > i:
            heapq.heappush(heap, (arr[i] / arr[j - 1], i, j - 1))
    return [arr[heap[0][1]], arr[heap[0][2]]]
```
#### TC: O(k log n) **SC:** O(n)

The code uses a min-heap to efficiently find the k-th smallest prime fraction. Initially, it pushes
fractions of the form arr[i]/arr[-1] into the heap. Then, it repeatedly pops the smallest fraction
from the heap and pushes the next possible fraction from the same row until it finds the k-th
smallest fraction. The heap operations ensure that the solution is both time and space efficient.

---
### K-th Smallest Prime Fraction -- Best TC Solution:
```python
from heapq import heappush, heappop

class Solution:
    def kthSmallestPrimeFraction(self, A, K):
        n = len(A)
        pq = [(A[0] / A[i], 0, i) for i in range(1, n)]
        for _ in range(K - 1):
            frac, i, j = heappop(pq)
            if i + 1 < j:
                heappush(pq, (A[i + 1] / A[j], i + 1, j))
        return pq[0][0]
```
#### TC: O(K*logN) **SC:** O(N)

The code uses a min heap to keep track of the fractions formed by dividing the elements of the array
A. It iterates K times, each time popping the smallest fraction from the heap and pushing the next
fraction formed by incrementing the numerator index. This approach ensures that the K-th smallest
prime fraction is found efficiently. The time complexity is O(K*logN) due to the K iterations and
the heap operations, while the space complexity is O(N) for the heap storing fractions.

---
---
---