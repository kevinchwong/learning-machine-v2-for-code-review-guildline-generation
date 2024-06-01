# Binary Search
## Frequent score for this algorithmic framework: 90 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Median of Two Sorted Arrays](#median-of-two-sorted-arrays) | 100 | 50 |
| 2 | [Find the Duplicate Number](#find-the-duplicate-number) | 95 | 41 |
| 3 | [Search in Rotated Sorted Array](#search-in-rotated-sorted-array) | 92 | 46 |
| 4 | [Find First and Last Position of Element in Sorted Array](#find-first-and-last-position-of-element-in-sorted-array) | 90 | 42 |
| 5 | [Search Insert Position](#search-insert-position) | 85 | 35 |
| 6 | [Split Array Largest Sum](#split-array-largest-sum) | 85 | 48 |
| 7 | [Count of Smaller Numbers After Self](#count-of-smaller-numbers-after-self) | 85 | 49 |
| 8 | [Find Peak Element](#find-peak-element) | 80 | 40 |
| 9 | [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array) | 78 | 33 |
| 10 | [Search a 2D Matrix II](#search-a-2d-matrix-ii) | 75 | 44 |
| 11 | [Kth Smallest Element in a Sorted Matrix](#kth-smallest-element-in-a-sorted-matrix) | 75 | 45 |
| 12 | [Find K Closest Elements](#find-k-closest-elements) | 70 | 38 |
| 13 | [Capacity To Ship Packages Within D Days](#capacity-to-ship-packages-within-d-days) | 65 | 47 |
| 14 | [Aggressive Cows](#aggressive-cows) | 60 | 50 |
| 15 | [Find Right Interval](#find-right-interval) | 55 | 37 |
| 16 | [H-Index II](#h-index-ii) | 50 | 36 |
| 17 | [Find Smallest Letter Greater Than Target](#find-smallest-letter-greater-than-target) | 45 | 34 |
| 18 | [Maximum Value at a Given Index in a Bounded Array](#maximum-value-at-a-given-index-in-a-bounded-array) | 40 | 43 |
---
Binary Search is an efficient algorithm for finding an item from a sorted list of items. It works by repeatedly dividing the search interval in half. If the value of the search key is less than the item in the middle of the interval, narrow the interval to the lower half. Otherwise, narrow it to the upper half. Repeat until the value is found or the interval is empty.
```python
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

# Example usage
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
target = 7
print(binary_search(arr, target))  # Output: 6
```
### Insight:
1. Binary Search requires the input array to be sorted.
2. The algorithm repeatedly divides the search interval in half, making it much more efficient than linear search for large datasets.
3. The time complexity is O(log n) because the search interval is halved each time.
4. The space complexity is O(1) because it uses a constant amount of space.
5. If the target element is not found, the function returns -1.
---
 --- 
# Median of Two Sorted Arrays
>Find the median of two sorted arrays.

>Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
- https://leetcode.com/problems/median-of-two-sorted-arrays/
- Difficulty: 50
- Frequent: 100
- Tags: ['Array', 'Binary Search', 'Divide and Conquer']

### Median of Two Sorted Arrays -- Shortest Solution:
```python
def findMedianSortedArrays(nums1, nums2):
    A, B = nums1, nums2
    if len(A) > len(B):
        A, B = B, A
    m, n = len(A), len(B)
    imin, imax, half_len = 0, m, (m + n + 1) // 2
    while imin <= imax:
        i = (imin + imax) // 2
        j = half_len - i
        if i < m and A[i] < B[j - 1]:
            imin = i + 1
        elif i > 0 and A[i - 1] > B[j]:
            imax = i - 1
        else:
            if i == 0: max_of_left = B[j - 1]
            elif j == 0: max_of_left = A[i - 1]
            else: max_of_left = max(A[i - 1], B[j - 1])
            if (m + n) % 2 == 1:
                return max_of_left
            if i == m: min_of_right = B[j]
            elif j == n: min_of_right = A[i]
            else: min_of_right = min(A[i], B[j])
            return (max_of_left + min_of_right) / 2.0
```
#### TC: O(log(min(m, n))) **SC:** O(1)

1. The code uses binary search to partition the two arrays such that the left half contains the
smaller elements and the right half contains the larger elements. 2. The partition is adjusted by
comparing elements around the partition to ensure the correct order. 3. The median is then
calculated based on the maximum of the left half and the minimum of the right half. 4. This approach
ensures that the solution is efficient and works in logarithmic time relative to the smaller array.

---
---
---
 --- 
# Find the Duplicate Number
>Find the duplicate number in an array of n+1 integers.

>Input: nums = [1,3,4,2,2]
Output: 2
- https://leetcode.com/problems/find-the-duplicate-number/
- Difficulty: 41
- Frequent: 95
- Tags: ['Array', 'Binary Search', 'Two Pointers']

### Find the Duplicate Number -- Shortest Solution:
```python
def findDuplicate(nums):
    slow, fast = nums[0], nums[0]
    while True:
        slow, fast = nums[slow], nums[nums[fast]]
        if slow == fast:
            break
    slow = nums[0]
    while slow != fast:
        slow, fast = nums[slow], fast[fast]
    return slow
```
#### TC: O(n) **SC:** O(1)

The code uses Floyd's Tortoise and Hare algorithm to detect a cycle in the array, which corresponds
to the duplicate number. The first loop finds the intersection point of the two runners. The second
loop finds the entrance to the cycle, which is the duplicate number.

---
---
---
 --- 
# Search in Rotated Sorted Array
>Search a target value in a rotated sorted array.

>Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
- https://leetcode.com/problems/search-in-rotated-sorted-array/
- Difficulty: 46
- Frequent: 92
- Tags: ['Array', 'Binary Search']

### Search in Rotated Sorted Array -- Shortest Solution:
```python
def search(nums, target):
    l, r = 0, len(nums) - 1
    while l <= r:
        mid = (l + r) // 2
        if nums[mid] == target:
            return mid
        if nums[l] <= nums[mid]:
            if nums[l] <= target < nums[mid]:
                r = mid - 1
            else:
                l = mid + 1
        else:
            if nums[mid] < target <= nums[r]:
                l = mid + 1
            else:
                r = mid - 1
    return -1
```
#### TC: O(log n) **SC:** O(1)

The code uses a modified binary search to handle the rotated sorted array. By checking the sorted
half of the array and adjusting the search range accordingly, it efficiently narrows down the
target's location. The time complexity is O(log n) due to the binary search, and the space
complexity is O(1) as it uses a constant amount of extra space.

---
---
---
 --- 
# Find First and Last Position of Element in Sorted Array
>Find the starting and ending position of a given target value in a sorted array.

>Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
- https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
- Difficulty: 42
- Frequent: 90
- Tags: ['Array', 'Binary Search']

### Find First and Last Position of Element in Sorted Array -- Shortest Solution:
```python
def searchRange(nums, target):
    def findBound(isFirst):
        l, r = 0, len(nums) - 1
        while l <= r:
            mid = (l + r) // 2
            if nums[mid] == target:
                if isFirst:
                    if mid == l or nums[mid - 1] < target:
                        return mid
                    r = mid - 1
                else:
                    if mid == r or nums[mid + 1] > target:
                        return mid
                    l = mid + 1
            elif nums[mid] < target:
                l = mid + 1
            else:
                r = mid - 1
        return -1
    return [findBound(True), findBound(False)]
```
#### TC: O(log n) **SC:** O(1)

The solution uses binary search to find the first and last positions of the target element in the
sorted array. The `findBound` function is used to locate the boundary indices. By toggling the
`isFirst` flag, the function can either search for the first occurrence or the last occurrence of
the target. The binary search ensures that the time complexity is logarithmic, making the solution
efficient.

---
---
---
 --- 
# Search Insert Position
>Find the index where a target should be inserted in a sorted array.

>Input: nums = [1,3,5,6], target = 5
Output: 2
- https://leetcode.com/problems/search-insert-position/
- Difficulty: 35
- Frequent: 85
- Tags: ['Array', 'Binary Search']

### Search Insert Position -- Shortest Solution:
```python
def searchInsert(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return left
```
#### TC: O(log n) **SC:** O(1)

The code uses a binary search algorithm to find the target or the position where it should be
inserted. The `left` and `right` pointers are used to maintain the search range. The `mid` index is
calculated to check the middle element. If the middle element is equal to the target, the index is
returned. If the middle element is less than the target, the search continues in the right half by
updating `left`. If the middle element is greater, the search continues in the left half by updating
`right`. If the target is not found, `left` will be the insertion position.

---
---
---
 --- 
# Split Array Largest Sum
>Split an array into m subarrays such that the largest sum among them is minimized.

>Input: nums = [7,2,5,10,8], m = 2
Output: 18
- https://leetcode.com/problems/split-array-largest-sum/
- Difficulty: 48
- Frequent: 85
- Tags: ['Array', 'Binary Search', 'Dynamic Programming']

### Split Array Largest Sum -- Shortest Solution:
```python
def splitArray(nums, m):
    def can_split(max_sum):
        current_sum, splits = 0, 1
        for num in nums:
            if current_sum + num > max_sum:
                splits += 1
                current_sum = num
                if splits > m:
                    return False
            else:
                current_sum += num
        return True
    left, right = max(nums), sum(nums)
    while left < right:
        mid = (left + right) // 2
        if can_split(mid):
            right = mid
        else:
            left = mid + 1
    return left
```
#### TC: O(n log(sum(nums) - max(nums))) **SC:** O(1)

The solution uses a binary search approach to find the minimum possible largest sum for splitting
the array into `m` subarrays. The `can_split` function checks if it's possible to split the array
into `m` or fewer subarrays without any subarray having a sum greater than `max_sum`. The binary
search narrows down the range between the maximum single element and the sum of all elements to find
the optimal solution.

---
### Split Array Largest Sum -- Best TC Solution:
```python
from typing import List

def splitArray(nums: List[int], m: int) -> int:
    def valid(mid):
        count = 1
        total = 0
        for num in nums:
            total += num
            if total > mid:
                total = num
                count += 1
                if count > m:
                    return False
        return True

    left, right = max(nums), sum(nums)
    while left < right:
        mid = left + (right - left) // 2
        if valid(mid):
            right = mid
        else:
            left = mid + 1
    return left
```
#### TC: O(n * log(sum(nums) - max(nums)) **SC:** O(1

The code uses binary search to find the minimum largest sum of the subarrays. The valid function
checks if it's possible to split the array into m subarrays with a maximum sum of mid. The binary
search iterates between the maximum element in the array and the sum of all elements. The algorithm
efficiently finds the optimal solution with a time complexity of O(n * log(sum(nums) - max(nums))
and a space complexity of O(1).

---
---
---
 --- 
# Count of Smaller Numbers After Self
>Count the number of smaller elements to the right of each element in an array.

>Input: nums = [5,2,6,1]
Output: [2,1,1,0]
- https://leetcode.com/problems/count-of-smaller-numbers-after-self/
- Difficulty: 49
- Frequent: 85
- Tags: ['Array', 'Binary Search', 'Divide and Conquer', 'Binary Indexed Tree', 'Segment Tree']

### Count of Smaller Numbers After Self -- Shortest Solution:
```python
from sortedcontainers import SortedList

def countSmaller(nums):
    result = []
    sorted_list = SortedList()
    for num in reversed(nums):
        result.append(sorted_list.bisect_left(num))
        sorted_list.add(num)
    return result[::-1]
```
#### TC: O(n log n) **SC:** O(n)

1. **SortedList**: We use `SortedList` from the `sortedcontainers` module to maintain a sorted list
of numbers as we iterate through the input list in reverse. 2. **Reverse Iteration**: By iterating
the list in reverse, we can efficiently count the number of smaller elements to the right of each
element. 3. **Binary Search**: The `bisect_left` method of `SortedList` performs a binary search to
find the position where the current number would fit in the sorted list, which directly gives the
count of smaller numbers to its right. 4. **Appending and Reversing**: We append the counts to the
result list and finally reverse it to match the original order of the input list.

---
### Count of Smaller Numbers After Self -- Best TC Solution:
```python
from sortedcontainers import SortedList

class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        sorted_nums = SortedList()
        result = []
        for num in reversed(nums):
            result.append(sorted_nums.bisect_left(num))
            sorted_nums.add(num)
        return result[::-1]
```
#### TC: O(n*log(n)) **SC:** O(n)

The code uses a SortedList data structure from the sortedcontainers library to efficiently maintain
a sorted list of numbers. It iterates through the input list of numbers in reverse order, using
bisect_left to find the index where each number should be inserted in the sorted list. By adding
each number to the sorted list and keeping track of the index, it calculates the count of smaller
numbers after each element. Finally, it reverses the result list to match the original order of the
input list. The time complexity is O(n*log(n)) due to the insertion operation in the SortedList, and
the space complexity is O(n) to store the result list.

---
---
---
 --- 
# Find Peak Element
>Find a peak element in an array.

>Input: nums = [1,2,3,1]
Output: 2
- https://leetcode.com/problems/find-peak-element/
- Difficulty: 40
- Frequent: 80
- Tags: ['Array', 'Binary Search']

### Find Peak Element -- Shortest Solution:
```python
def findPeakElement(nums):
    l, r = 0, len(nums) - 1
    while l < r:
        mid = (l + r) // 2
        if nums[mid] > nums[mid + 1]:
            r = mid
        else:
            l = mid + 1
    return l
```
#### TC: O(log n) **SC:** O(1)

The code uses a binary search approach to efficiently find a peak element in the list. By comparing
the middle element with its next neighbor, it decides which half of the list to discard, ensuring
that a peak element is always retained in the remaining half. This process continues until the
search space is reduced to a single element, which is the peak.

---
---
---
 --- 
# Find Minimum in Rotated Sorted Array
>Find the minimum element in a rotated sorted array.

>Input: nums = [3,4,5,1,2]
Output: 1
- https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
- Difficulty: 33
- Frequent: 78
- Tags: ['Array', 'Binary Search']

### Find Minimum in Rotated Sorted Array -- Shortest Solution:
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid
        return nums[left]
```
#### TC: O(log n) **SC:** O(1)

The code uses a binary search approach to find the minimum element in a rotated sorted array. By
comparing the middle element with the rightmost element, it determines which half of the array to
discard, effectively reducing the search space by half each iteration. This ensures an optimal time
complexity of O(log n). The space complexity is O(1) as it only uses a few extra variables.

---
---
---
 --- 
# Search a 2D Matrix II
>Search a target value in a 2D matrix.

>Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
Output: true
- https://leetcode.com/problems/search-a-2d-matrix-ii/
- Difficulty: 44
- Frequent: 75
- Tags: ['Array', 'Binary Search', 'Divide and Conquer']

### Search a 2D Matrix II -- Shortest Solution:
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or not matrix[0]:
            return False
        row, col = 0, len(matrix[0]) - 1
        while row < len(matrix) and col >= 0:
            if matrix[row][col] == target:
                return True
            elif matrix[row][col] > target:
                col -= 1
            else:
                row += 1
        return False
```
#### TC: O(m + n) **SC:** O(1)

The algorithm starts from the top-right corner of the matrix and iteratively eliminates either a row
or a column. If the current element is greater than the target, it moves left (decreases the column
index). If the current element is less than the target, it moves down (increases the row index).
This way, it efficiently narrows down the search space.

---
---
---
 --- 
# Kth Smallest Element in a Sorted Matrix
>Find the kth smallest element in a sorted matrix.

>Input: matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
Output: 13
- https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
- Difficulty: 45
- Frequent: 75
- Tags: ['Array', 'Binary Search', 'Heap']

### Kth Smallest Element in a Sorted Matrix -- Shortest Solution:
```python
import heapq

def kthSmallest(matrix, k):
    return list(heapq.merge(*matrix))[k-1]
```
#### TC: O(n^2 log n) **SC:** O(n^2)

The code uses Python's heapq.merge to merge all rows of the matrix into a single sorted iterator.
Then, it converts this iterator to a list and retrieves the k-th smallest element. This approach
leverages the fact that the matrix rows are already sorted, making the merge operation efficient.

---
### Kth Smallest Element in a Sorted Matrix -- Best TC Solution:
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

The code uses a min heap to keep track of the smallest elements from each row of the matrix. It
initializes the heap with the first element from each row and then iteratively pops the smallest
element from the heap and pushes the next element from the same row if available. This process
continues until the kth smallest element is found. The time complexity is O(klogn) because we
perform k heap operations, each taking logn time. The space complexity is O(n) for the heap that
stores at most n elements.

---
---
---
 --- 
# Find K Closest Elements
>Find the k closest elements to a given value in a sorted array.

>Input: arr = [1,2,3,4,5], k = 4, x = 3
Output: [1,2,3,4]
- https://leetcode.com/problems/find-k-closest-elements/
- Difficulty: 38
- Frequent: 70
- Tags: ['Array', 'Binary Search', 'Two Pointers']

### Find K Closest Elements -- Shortest Solution:
```python
from bisect import bisect_left

def findClosestElements(arr, k, x):
    pos = bisect_left(arr, x)
    left, right = pos - 1, pos
    while k > 0:
        if left < 0 or (right < len(arr) and abs(arr[right] - x) < abs(arr[left] - x)):
            right += 1
        else:
            left -= 1
        k -= 1
    return arr[left + 1:right]
```
#### TC: O(log n + k) **SC:** O(1)

1. The `bisect_left` function is used to find the insertion position of `x` in `arr` to maintain
sorted order. 2. Two pointers, `left` and `right`, are initialized around the insertion position. 3.
The while loop runs `k` times to find the `k` closest elements by comparing the distances from `x`.
4. The final slice `arr[left + 1:right]` gives the `k` closest elements.

---
### Find K Closest Elements -- Best TC Solution:
```python
from collections import deque
class Solution:
    def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
        left, right = 0, len(arr) - k
        while left < right:
            mid = left + (right - left) // 2
            if x - arr[mid] > arr[mid + k] - x:
                left = mid + 1
            else:
                right = mid
        return arr[left:left + k]
```
#### TC: O(log(N-K) + K) **SC:** O(1)

The code uses binary search to find the starting index of the K closest elements. It iteratively
adjusts the left and right pointers based on the comparison of the distances from the target 'x'.
The time complexity is O(log(N-K) + K) where N is the length of the input array 'arr' and K is the
number of closest elements to find. The space complexity is O(1) as the algorithm uses constant
extra space.

---
---
---
 --- 
# Capacity To Ship Packages Within D Days
>Find the least weight capacity of a ship to ship all packages within D days.

>Input: weights = [1,2,3,4,5,6,7,8,9,10], D = 5
Output: 15
- https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/
- Difficulty: 47
- Frequent: 65
- Tags: ['Array', 'Binary Search']

### Capacity To Ship Packages Within D Days -- Shortest Solution:
```python
def shipWithinDays(weights, D):
    def canShip(capacity):
        days = 1
        total = 0
        for weight in weights:
            total += weight
            if total > capacity:
                days += 1
                total = weight
                if days > D:
                    return False
        return True

    left, right = max(weights), sum(weights)
    while left < right:
        mid = (left + right) // 2
        if canShip(mid):
            right = mid
        else:
            left = mid + 1
    return left
```
#### TC: O(N log S) **SC:** O(1)

1. The problem is solved using a binary search approach to find the minimum capacity needed to ship
all packages within D days. 2. The `canShip` function checks if it's possible to ship all packages
with a given capacity within D days. 3. The binary search is performed between the maximum weight
(left) and the sum of all weights (right). 4. The time complexity is O(N log S), where N is the
number of weights and S is the sum of weights. 5. The space complexity is O(1) as we are using a
constant amount of extra space.

---
### Capacity To Ship Packages Within D Days -- Best TC Solution:
```python
from typing import List
def shipWithinDays(weights: List[int], D: int) -> int:
    left, right = max(weights), sum(weights)
    while left < right:
        mid = left + (right - left) // 2
        days = 1
        current_weight = 0
        for weight in weights:
            if current_weight + weight > mid:
                days += 1
                current_weight = 0
            current_weight += weight
        if days > D:
            left = mid + 1
        else:
            right = mid
    return left
```
#### TC: O(N * log(sum(weights) - max(weights)) **SC:** O(1

The code uses binary search to find the minimum weight capacity that can ship all packages within D
days. It initializes the search range between the maximum weight and the total weight. It then
iteratively calculates the mid weight and simulates the shipping process to determine the number of
days required. Based on the comparison with D, it updates the search range accordingly until the
optimal weight capacity is found. The time complexity is O(N * log(sum(weights) - max(weights))
where N is the number of weights, and the space complexity is O(1).

---
---
---
 --- 
# Aggressive Cows
>Place cows in stalls such that the minimum distance between any two of them is maximized.

>Input: stalls = [1,2,4,8,9], k = 3
Output: 3
- https://www.spoj.com/problems/AGGRCOW/
- Difficulty: 50
- Frequent: 60
- Tags: ['Binary Search', 'Greedy']

### Aggressive Cows -- Shortest Solution:
```python
def can_place_cows(stalls, distance, cows):
    count, last_position = 1, stalls[0]
    for stall in stalls[1:]:
        if stall - last_position >= distance:
            count += 1
            last_position = stall
            if count == cows:
                return True
    return False

def aggressive_cows(stalls, cows):
    stalls.sort()
    low, high = 1, stalls[-1] - stalls[0]
    while low <= high:
        mid = (low + high) // 2
        if can_place_cows(stalls, mid, cows):
            low = mid + 1
        else:
            high = mid - 1
    return high
```
#### TC: O(n log d) **SC:** O(1)

1. The problem is solved using a binary search approach to find the maximum minimum distance. 2. The
`can_place_cows` function checks if it is possible to place the cows with at least the given minimum
distance. 3. The main function `aggressive_cows` sorts the stalls and uses binary search to find the
largest minimum distance. 4. The time complexity is O(n log d), where n is the number of stalls and
d is the difference between the maximum and minimum stall positions. 5. The space complexity is O(1)
as we are using a constant amount of extra space.

---
### Aggressive Cows -- Best TC Solution:
```python
# Function to check if it's possible to place 'c' cows with a minimum distance 'mid'
def is_possible(c, mid, stalls):
    cows = 1
    prev_stall = stalls[0]
    for i in range(1, len(stalls)):
        if stalls[i] - prev_stall >= mid:
            cows += 1
            prev_stall = stalls[i]
            if cows == c:
                return True
    return False

# Main function to find the maximum minimum distance
# to place 'c' cows in 'n' stalls

def max_min_distance(c, n, stalls):
    stalls.sort()
    low = 1
    high = stalls[-1] - stalls[0]
    result = 0
    while low <= high:
        mid = low + (high - low) // 2
        if is_possible(c, mid, stalls):
            result = mid
            low = mid + 1
        else:
            high = mid - 1
    return result

# Example Usage
n = 5
c = 3
stalls = [1, 2, 8, 4, 9]
print(max_min_distance(c, n, stalls))
```
#### TC: O(N log N) **SC:** O(1)

The code uses binary search to find the maximum minimum distance to place 'c' cows in 'n' stalls.
The is_possible function checks if it's possible to place 'c' cows with a minimum distance 'mid'.
The main function max_min_distance sorts the stalls and iterates using binary search to find the
maximum minimum distance. This approach optimally solves the Aggressive Cows problem with a time
complexity of O(N log N) and a space complexity of O(1).

---
---
---
 --- 
# Find Right Interval
>Find the smallest start interval that is greater than or equal to the end of each interval.

>Input: intervals = [[1,2],[2,3],[0,1]]
Output: [-1,0,1]
- https://leetcode.com/problems/find-right-interval/
- Difficulty: 37
- Frequent: 55
- Tags: ['Array', 'Binary Search']

### Find Right Interval -- Shortest Solution:
```python
from bisect import bisect_left

def findRightInterval(intervals):
    indexed_intervals = sorted((e[0], i) for i, e in enumerate(intervals))
    result = []
    for start, end in intervals:
        idx = bisect_left(indexed_intervals, (end,))
        result.append(indexed_intervals[idx][1] if idx < len(indexed_intervals) else -1)
    return result
```
#### TC: O(n log n) **SC:** O(n)

1. The intervals are first indexed and sorted based on their start times. 2. For each interval,
binary search is used to find the smallest start time that is greater than or equal to the end time
of the current interval. 3. The result list is populated with the corresponding indices or -1 if no
such interval exists.

---
---
---
 --- 
# H-Index II
>Find the h-index given a sorted list of citations.

>Input: citations = [0,1,3,5,6]
Output: 3
- https://leetcode.com/problems/h-index-ii/
- Difficulty: 36
- Frequent: 50
- Tags: ['Array', 'Binary Search']

### H-Index II -- Shortest Solution:
```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        n = len(citations)
        left, right = 0, n - 1
        while left <= right:
            mid = (left + right) // 2
            if citations[mid] == n - mid:
                return n - mid
            elif citations[mid] < n - mid:
                left = mid + 1
            else:
                right = mid - 1
        return n - left
```
#### TC: O(log n) **SC:** O(1)

The code uses a binary search approach to find the H-Index in a sorted list of citations. The
H-Index is defined as the maximum value h such that the given author has published h papers that
have each been cited at least h times. By leveraging the sorted property of the list, the binary
search efficiently narrows down the potential H-Index values, resulting in a logarithmic time
complexity. The space complexity is constant as no additional space proportional to the input size
is used.

---
---
---
 --- 
# Find Smallest Letter Greater Than Target
>Find the smallest letter in a sorted list that is greater than a given target.

>Input: letters = ['c','f','j'], target = 'a'
Output: 'c'
- https://leetcode.com/problems/find-smallest-letter-greater-than-target/
- Difficulty: 34
- Frequent: 45
- Tags: ['Array', 'Binary Search']

### Find Smallest Letter Greater Than Target -- Shortest Solution:
```python
from bisect import bisect_right

def nextGreatestLetter(letters, target):
    index = bisect_right(letters, target)
    return letters[index % len(letters)]
```
#### TC: O(log n) **SC:** O(1)

The function uses the `bisect_right` method from the `bisect` module to find the insertion point for
the target in the sorted list `letters`. The insertion point is the index where the target would go
to maintain order. If the insertion point is equal to the length of the list, it wraps around to the
beginning using modulo operation. This ensures that the smallest letter greater than the target is
returned, even if it is at the start of the list.

---
---
---
 --- 
# Maximum Value at a Given Index in a Bounded Array
>Find the maximum value at a given index in a bounded array.

>Input: n = 4, index = 2, maxSum = 6
Output: 2
- https://leetcode.com/problems/maximum-value-at-a-given-index-in-a-bounded-array/
- Difficulty: 43
- Frequent: 40
- Tags: ['Array', 'Binary Search']

### Maximum Value at a Given Index in a Bounded Array -- Shortest Solution:
```python
def maxValue(n, index, maxSum):
    def check(mid):
        left = max(0, mid - index)
        right = max(0, mid - ((n - 1) - index))
        return (mid + left) * (mid - left + 1) // 2 + (mid + right) * (mid - right + 1) // 2 - mid <= maxSum
    low, high = 1, maxSum
    while low < high:
        mid = (low + high + 1) // 2
        if check(mid):
            low = mid
        else:
            high = mid - 1
    return low
```
#### TC: O(log(maxSum) * log(n)) **SC:** O(1)

The solution uses a binary search approach to find the maximum value at a given index in a bounded
array. The `check` function calculates if a given middle value can be the maximum value at the
specified index without exceeding the `maxSum`. The binary search iterates over possible values,
narrowing down the range until the maximum valid value is found.

---
### Maximum Value at a Given Index in a Bounded Array -- Best TC Solution:
```python
from math import ceil

def max_value(n: int, index: int, maxSum: int) -> int:
    def check(x: int) -> bool:
        left_sum = x if x <= index else x + index - 1 - ceil(maxSum - x, n - index)
        right_sum = x if x <= n - index - 1 else x + n - index - 1 - ceil(maxSum - x, index)
        return left_sum + right_sum + x <= maxSum

    left, right = 0, maxSum
    while left < right:
        mid = (left + right + 1) // 2
        if check(mid):
            left = mid
        else:
            right = mid - 1
    return left
```
#### TC: O(log(maxSum)) **SC:** O(1)

The code uses binary search to find the maximum value that can be placed at the given index in a
bounded array. The check function determines if a value x is valid based on the sum constraints. The
binary search iterates until it finds the maximum valid value. The time complexity is O(log(maxSum))
due to the binary search, and the space complexity is O(1) as no extra space is used.

---
---
---