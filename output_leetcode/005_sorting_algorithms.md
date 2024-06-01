# Sorting Algorithms
## Frequent score for this algorithmic framework: 5 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Meeting Rooms II](#meeting-rooms-ii) | 75 | 55 |
| 2 | [Merge Sorted Array](#merge-sorted-array) | 70 | 40 |
| 3 | [Meeting Rooms](#meeting-rooms) | 65 | 45 |
| 4 | [Car Fleet](#car-fleet) | 65 | 55 |
| 5 | [Largest Number](#largest-number) | 65 | 60 |
| 6 | [Maximum Number of Events That Can Be Attended](#maximum-number-of-events-that-can-be-attended) | 65 | 60 |
| 7 | [Reorder Data in Log Files](#reorder-data-in-log-files) | 60 | 50 |
| 8 | [Sort List](#sort-list) | 60 | 55 |
| 9 | [H-Index](#h-index) | 60 | 55 |
| 10 | [Hand of Straights](#hand-of-straights) | 60 | 60 |
| 11 | [Relative Sort Array](#relative-sort-array) | 55 | 50 |
| 12 | [Minimum Absolute Difference](#minimum-absolute-difference) | 55 | 50 |
| 13 | [Rearrange Words in a Sentence](#rearrange-words-in-a-sentence) | 55 | 50 |
| 14 | [Sort Integers by The Number of 1 Bits](#sort-integers-by-the-number-of-1-bits) | 55 | 50 |
| 15 | [Pancake Sorting](#pancake-sorting) | 55 | 55 |
| 16 | [Sort Transformed Array](#sort-transformed-array) | 55 | 55 |
| 17 | [Sort the Matrix Diagonally](#sort-the-matrix-diagonally) | 55 | 55 |
| 18 | [Maximum Gap](#maximum-gap) | 55 | 60 |
| 19 | [Wiggle Sort II](#wiggle-sort-ii) | 55 | 65 |
---
Sorting algorithms are methods for arranging elements in lists into a specific order, typically numerical or lexicographical. Common algorithms include Quick Sort, Merge Sort, and Bubble Sort, each with different performance characteristics. Sorting is fundamental in computer science for optimizing data retrieval and processing.
```python
# Bubble Sort
 def bubble_sort(arr):
     n = len(arr)
     for i in range(n):
         for j in range(0, n-i-1):
             if arr[j] > arr[j+1]:
                 arr[j], arr[j+1] = arr[j+1], arr[j]
     return arr

# Quick Sort
 def quick_sort(arr):
     if len(arr) <= 1:
         return arr
     pivot = arr[len(arr) // 2]
     left = [x for x in arr if x < pivot]
     middle = [x for x in arr if x == pivot]
     right = [x for x in arr if x > pivot]
     return quick_sort(left) + middle + quick_sort(right)

# Merge Sort
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
```
### Insight:
1. Bubble Sort is simple but inefficient for large datasets.
2. Quick Sort is generally fast but can degrade to O(n^2) in the worst case.
3. Merge Sort guarantees O(n log n) time complexity but requires additional space for merging.
4. Understanding the trade-offs between time and space complexity is crucial for choosing the right sorting algorithm for a given problem.
5. Sorting is often a preliminary step for more complex algorithms, making its efficiency critical.
---
 --- 
# Meeting Rooms II
>Find the minimum number of conference rooms required.

>Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
- https://leetcode.com/problems/meeting-rooms-ii/
- Difficulty: 55
- Frequent: 75
- Tags: ['Sorting', 'Heap']

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
complexity is O(N) for the heap.

---
---
---
 --- 
# Merge Sorted Array
>Merge two sorted arrays into one sorted array.

>Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
- https://leetcode.com/problems/merge-sorted-array/
- Difficulty: 40
- Frequent: 70
- Tags: ['Sorting', 'Array']

### Merge Sorted Array -- Shortest Solution:
```python
from typing import List

class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        nums1[m:] = nums2
        nums1.sort()
```
#### TC: O((m+n)log(m+n)) **SC:** O(1)

The code defines a class Solution with a merge method that takes in two sorted arrays nums1 and
nums2 along with their respective lengths m and n. It merges the two arrays by concatenating nums2
to nums1, then sorts the merged array in-place. The time complexity is O((m+n)log(m+n)) due to the
sorting operation, and the space complexity is O(1) as the sorting is done in-place without using
any extra space.

---
---
---
 --- 
# Meeting Rooms
>Determine if a person could attend all meetings.

>Input: [[0, 30],[5, 10],[15, 20]]
Output: false
- https://leetcode.com/problems/meeting-rooms/
- Difficulty: 45
- Frequent: 65
- Tags: ['Sorting', 'Array']

### Meeting Rooms -- Shortest Solution:
```python
# Sort the intervals based on the start time
intervals.sort(key=lambda x: x[0])

# Check if there is any overlap between intervals
for i in range(1, len(intervals)):
    if intervals[i][0] < intervals[i-1][1]:
        return False

return True
```
#### TC: O(n log n) **SC:** O(1)

1. Sorting the intervals based on the start time allows us to easily identify any overlaps. 2. By
iterating through the sorted intervals, we can compare the start time of the current interval with
the end time of the previous interval to check for overlaps. 3. If the start time of the current
interval is less than the end time of the previous interval, there is an overlap, and we return
False. 4. If no overlaps are found, we return True indicating that all meetings can be held
concurrently.

---
### Meeting Rooms -- Best TC Solution:
```python
# Function to check if a person can attend all meetings
# Time Complexity: O(nlogn) - Sorting
# Space Complexity: O(1)
def canAttendMeetings(intervals):
    intervals.sort(key=lambda x: x[0])
    for i in range(1, len(intervals)):
        if intervals[i][0] < intervals[i-1][1]:
            return False
    return True
```
#### TC: O(nlogn) **SC:** O(1)

The code sorts the intervals based on the start time. It then iterates through the sorted intervals
and checks if the start time of the current interval is less than the end time of the previous
interval. If it is, it means there is an overlap, and the function returns False. Otherwise, it
returns True, indicating that all meetings can be attended without any overlaps.

---
---
---
 --- 
# Car Fleet
>Calculate the number of car fleets that will arrive at the destination.

>Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
Output: 3
- https://leetcode.com/problems/car-fleet/
- Difficulty: 55
- Frequent: 65
- Tags: ['Sorting', 'Array']

### Car Fleet -- Shortest Solution:
```python
from typing import List
def carFleet(target: int, position: List[int], speed: List[int]) -> int:
    cars = sorted(zip(position, speed), reverse=True)
    times = [(target - pos) / spd for pos, spd in cars]
    fleets = 0
    while len(times) > 1:
        lead_time = times.pop(0)
        if lead_time < times[0]:
            fleets += 1
        else:
            times[0] = lead_time
    return fleets + bool(times)
```
#### TC: O(N log N) **SC:** O(N)

The code sorts the cars based on their positions in descending order. It calculates the time each
car needs to reach the target. Then, it iterates through the cars to determine if a car can form a
fleet with the car in front of it. If the lead car takes less time to reach the target than the car
in front, they can form a fleet. The code efficiently handles the fleet formation logic using a
while loop and pop operations on the times list. The time complexity is O(N log N) due to sorting,
and the space complexity is O(N) to store the times array.

---
---
---
 --- 
# Largest Number
>Arrange numbers to form the largest number.

>Input: [3,30,34,5,9]
Output: "9534330"
- https://leetcode.com/problems/largest-number/
- Difficulty: 60
- Frequent: 65
- Tags: ['Sorting', 'String']

### Largest Number -- Shortest Solution:
```python
from typing import List

def largestNumber(nums: List[int]) -> str:
    nums = list(map(str, nums))
    nums.sort(key=lambda x: x * 10, reverse=True)
    return str(int(''.join(nums)))
```
#### TC: O(nlogn) **SC:** O(n)

The code converts the numbers in the input list to strings, sorts them in a custom way to form the
largest number, and then joins and returns the result. The key function used in sorting concatenates
the strings in a way that ensures the correct order for forming the largest number. Finally, the
code handles edge cases by converting the result back to an integer and then to a string before
returning it.

---
---
---
 --- 
# Maximum Number of Events That Can Be Attended
>Find the maximum number of events that can be attended.

>Input: [[1,2],[2,3],[3,4],[1,2]]
Output: 4
- https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/
- Difficulty: 60
- Frequent: 65
- Tags: ['Sorting', 'Greedy']

### Maximum Number of Events That Can Be Attended -- Shortest Solution:
```python
from heapq import heappush, heappop

def max_events(events):
    events.sort(reverse=True)
    h = []
    res = d = 0
    while events or h:
        if not h:
            d = events[-1][0]
        while events and events[-1][0] <= d:
            s, e = events.pop()
            heappush(h, e)
        heappop(h)
        res += 1
        d += 1
        while h and h[0] < d:
            heappop(h)
    return res
```
#### TC: O(N log N) **SC:** O(N)

The code uses a greedy approach to maximize the number of events that can be attended. It sorts the
events in descending order based on their end time. It then uses a heap to keep track of the events
that can be attended at each day. The algorithm iterates through the days, attending the event with
the earliest end time on each day. The time complexity is O(N log N) due to sorting, and the space
complexity is O(N) for the heap.

---
---
---
 --- 
# Reorder Data in Log Files
>Reorder log files so that all letter-logs come before digit-logs.

>Input: ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
- https://leetcode.com/problems/reorder-data-in-log-files/
- Difficulty: 50
- Frequent: 60
- Tags: ['Sorting', 'String']

### Reorder Data in Log Files -- Shortest Solution:
```python
def reorderLogFiles(logs):
    def custom_sort(log):
        id_, rest = log.split(' ', 1)
        return (0, rest, id_) if rest[0].isalpha() else (1,)
    return sorted(logs, key=custom_sort)

# Example
logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
reordered_logs = reorderLogFiles(logs)
print(reordered_logs)
```
#### TC: O(N * M * log(N)) **SC:** O(N)

1. The function `reorderLogFiles` takes a list of log strings as input and returns the reordered log
strings. 2. The `custom_sort` function is used as the key function for sorting the logs. It splits
each log into an identifier and the rest of the log. 3. Logs starting with alphabetic characters are
sorted first based on the rest of the log and then by the identifier. 4. Logs starting with numeric
characters are sorted after the alphabetic logs. 5. The logs are sorted using the `sorted` function
with the custom sort key. 6. The time complexity is O(N * M * log(N)), where N is the number of logs
and M is the average length of each log. 7. The space complexity is O(N) to store the sorted logs.

---
---
---
 --- 
# Sort List
>Sort a linked list in O(n log n) time and constant space.

>Input: [4,2,1,3]
Output: [1,2,3,4]
- https://leetcode.com/problems/sort-list/
- Difficulty: 55
- Frequent: 60
- Tags: ['Sorting', 'Linked List']

### Sort List -- Shortest Solution:
```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        mid = slow.next
        slow.next = None
        left = self.sortList(head)
        right = self.sortList(mid)
        return self.merge(left, right)
    
    def merge(self, l1, l2):
        dummy = ListNode()
        cur = dummy
        while l1 and l2:
            if l1.val < l2.val:
                cur.next = l1
                l1 = l1.next
            else:
                cur.next = l2
                l2 = l2.next
            cur = cur.next
        cur.next = l1 or l2
        return dummy.next
```
#### TC: O(n log n) **SC:** O(log n)

The code implements the merge sort algorithm to sort a linked list. It recursively divides the list
into two halves, sorts them, and then merges the sorted halves. The time complexity is O(n log n)
due to the merge sort algorithm, and the space complexity is O(log n) due to the recursive calls on
the stack. The code is clean, concise, and easy to understand, making good use of the ListNode class
and helper functions for sorting and merging.

---
---
---
 --- 
# H-Index
>Calculate the H-Index given an array of citations.

>Input: [3,0,6,1,5]
Output: 3
- https://leetcode.com/problems/h-index/
- Difficulty: 55
- Frequent: 60
- Tags: ['Sorting', 'Array']

### H-Index -- Shortest Solution:
```python
from typing import List

def hIndex(citations: List[int]) -> int:
    citations.sort(reverse=True)
    h_index = 0
    for i, citation in enumerate(citations, start=1):
        if citation >= i:
            h_index = i
        else:
            break
    return h_index
```
#### TC: O(nlogn) **SC:** O(1)

The code first sorts the citations array in descending order. Then, it iterates through the sorted
array and checks if the current citation value is greater than or equal to its index. If it is, it
updates the h_index. If not, it breaks out of the loop. This approach leverages sorting to
efficiently find the h-index in O(nlogn) time complexity and O(1) space complexity.

---
---
---
 --- 
# Hand of Straights
>Check if an array can be divided into groups of W consecutive numbers.

>Input: [1,2,3,6,2,3,4,7,8], W = 3
Output: true
- https://leetcode.com/problems/hand-of-straights/
- Difficulty: 60
- Frequent: 60
- Tags: ['Sorting', 'Greedy']

### Hand of Straights -- Shortest Solution:
```python
from collections import Counter

def isNStraightHand(hand, W):
    if len(hand) % W != 0:
        return False
    count = Counter(hand)
    hand.sort()
    for card in hand:
        if count[card] > 0:
            for i in range(W):
                if count[card + i] == 0:
                    return False
                count[card + i] -= 1
    return True
```
#### TC: O(NlogN) **SC:** O(N)

The code uses a Counter to count the occurrences of each card in the hand. It then sorts the hand
and iterates through each card. For each card, it checks if there are enough consecutive cards to
form a straight hand of size W. If not, it returns False. The time complexity is O(NlogN) due to
sorting, and the space complexity is O(N) for the Counter.

---
---
---
 --- 
# Relative Sort Array
>Sort an array based on the order defined by another array.

>Input: arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
Output: [2,2,2,1,4,3,3,9,6,7,19]
- https://leetcode.com/problems/relative-sort-array/
- Difficulty: 50
- Frequent: 55
- Tags: ['Sorting', 'Array']

### Relative Sort Array -- Shortest Solution:
```python
from collections import Counter

def relativeSortArray(arr1, arr2):
    count = Counter(arr1)
    res = []
    for num in arr2:
        res.extend([num] * count[num])
    remaining = sorted([num for num in arr1 if num not in arr2])
    return res + remaining
```
#### TC: O(NlogN) **SC:** O(N)

The code uses Counter to count the occurrences of elements in arr1. It then iterates through arr2 to
construct the result array based on the counts. The remaining elements not in arr2 are sorted and
appended to the result. The time complexity is O(NlogN) due to the sorting of remaining elements,
and the space complexity is O(N) for the Counter and result array.

---
### Relative Sort Array -- Best TC Solution:
```python
from collections import Counter

def relativeSortArray(arr1, arr2):
    count = Counter(arr1)
    result = []
    for num in arr2:
        result.extend([num] * count[num])
        del count[num]
    remaining = sorted(count.elements())
    return result + remaining

# Example
arr1 = [2,3,1,3,2,4,6,7,9,2,19]
arr2 = [2,1,4,3,9,6]
print(relativeSortArray(arr1, arr2)
```
#### TC: O(nlogn) **SC:** O(n)

The code uses the Counter class from the collections module to count the occurrences of each element
in arr1. It then iterates through arr2 to construct the result array by repeating each element based
on its count. The remaining elements in arr1 that are not in arr2 are sorted and appended to the
result. This approach ensures that the elements are sorted according to the order in arr2 while
maintaining the relative order of other elements.

---
---
---
 --- 
# Minimum Absolute Difference
>Find all pairs with the minimum absolute difference in an array.

>Input: [4,2,1,3]
Output: [[1,2],[2,3],[3,4]]
- https://leetcode.com/problems/minimum-absolute-difference/
- Difficulty: 50
- Frequent: 55
- Tags: ['Sorting', 'Array']

### Minimum Absolute Difference -- Shortest Solution:
```python
from typing import List
def minimumAbsDifference(arr: List[int]) -> List[List[int]:
    arr.sort()
    min_diff = float('inf')
    result = []
    for i in range(1, len(arr)):
        diff = arr[i] - arr[i-1]
        if diff < min_diff:
            min_diff = diff
            result = [[arr[i-1], arr[i]]]
        elif diff == min_diff:
            result.append([arr[i-1], arr[i]])
    return result
```
#### TC: O(n log n) **SC:** O(n)

The code first sorts the input array in ascending order. It then iterates through the sorted array
to find the minimum absolute difference between adjacent elements. The time complexity is O(n log n)
due to the sorting operation, and the space complexity is O(n) to store the result. The code
efficiently finds the minimum absolute difference and returns the pairs of elements that have that
difference.

---
---
---
 --- 
# Rearrange Words in a Sentence
>Rearrange words in a sentence by the length of the words.

>Input: "Leetcode is cool"
Output: "Is cool leetcode"
- https://leetcode.com/problems/rearrange-words-in-a-sentence/
- Difficulty: 50
- Frequent: 55
- Tags: ['Sorting', 'String']

### Rearrange Words in a Sentence -- Shortest Solution:
```python
from collections import defaultdict
def arrange_words(text: str) -> str:
    words = text.lower().split()
    words_dict = defaultdict(list)
    for word in words:
        words_dict[len(word)].append(word)
    sorted_words = [word for length in sorted(words_dict.keys()) for word in words_dict[length]]
    sorted_text = ' '.join(sorted_words)
    return sorted_text.capitalize()

# Example
print(arrange_words("Leetcode is cool"))  # Output: "Is cool leetcode"
```
#### TC: O(NlogN) **SC:** O(N)

The code first splits the input text into words, converts them to lowercase, and then groups them
based on their lengths using a defaultdict. It then sorts the words based on their lengths and
reconstructs the text. The time complexity is O(NlogN) due to sorting, and the space complexity is
O(N) to store the words in the dictionary.

---
### Rearrange Words in a Sentence -- Best TC Solution:
```python
# Rearrange Words in a Sentence

def arrange_words(text: str) -> str:
    words = text.lower().split()
    words.sort(key=len)
    return ' '.join(words).capitalize()

# Example
input_text = "Leetcode is the best platform for coding"
output_text = arrange_words(input_text)
print(output_text)
```
#### TC: O(N log N) **SC:** O(N)

The code first converts the input text to lowercase and splits it into individual words. It then
sorts the words based on their lengths. Finally, it joins the sorted words back together with the
first word capitalized. The time complexity is O(N log N) due to the sorting operation, and the
space complexity is O(N) to store the words.

---
---
---
 --- 
# Sort Integers by The Number of 1 Bits
>Sort integers by the number of 1 bits in their binary representation.

>Input: [0,1,2,3,4,5,6,7,8]
Output: [0,1,2,4,8,3,5,6,7]
- https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits/
- Difficulty: 50
- Frequent: 55
- Tags: ['Sorting', 'Bit Manipulation']

### Sort Integers by The Number of 1 Bits -- Shortest Solution:
```python
from typing import List

class Solution:
    def sortByBits(self, arr: List[int]) -> List[int]:
        return sorted(arr, key=lambda x: (bin(x).count('1'), x))
```
#### TC: O(n log n) **SC:** O(n)

The code defines a class Solution with a method sortByBits that takes a list of integers arr as
input. It sorts the input list based on two keys: the number of 1 bits in the binary representation
of each integer and the integer itself. The sorted() function is used with a lambda function as the
key to achieve this sorting. This approach leverages the built-in sorting algorithm in Python,
resulting in a concise and efficient solution. The time complexity is O(n log n) due to the sorting
operation, and the space complexity is O(n) to store the sorted list.

---
### Sort Integers by The Number of 1 Bits -- Best TC Solution:
```python
from typing import List

class Solution:
    def countBits(self, num: int) -> List[int]:
        def count_ones(n):
            count = 0
            while n:
                n &= n - 1
                count += 1
            return count
        res = [0] * (num + 1)
        for i in range(1, num + 1):
            res[i] = res[i >> 1] + (i & 1)
        return res

# Example usage
solution = Solution()
print(solution.countBits(5))  # Output: [0, 1, 1, 2, 1, 2]
```
#### TC: O(n) **SC:** O(n)

The code defines a class Solution with a method countBits that calculates the number of 1 bits for
each number from 0 to num. It uses a helper function count_ones to count the number of 1 bits in a
number efficiently. The main logic is to fill the result array res by iterating from 1 to num and
calculating the number of 1 bits based on the previous values. The time complexity is O(n) as it
iterates through all numbers from 0 to num, and the space complexity is O(n) to store the result
array.

---
---
---
 --- 
# Pancake Sorting
>Sort an array using pancake flips.

>Input: [3,2,4,1]
Output: [4,2,4,3]
- https://leetcode.com/problems/pancake-sorting/
- Difficulty: 55
- Frequent: 55
- Tags: ['Sorting', 'Array']

### Pancake Sorting -- Shortest Solution:
```python
# Pancake Sorting

def pancakeSort(A):
    result = []
    n = len(A)
    for x in range(n, 1, -1):
        i = A.index(x)
        if i != x - 1:
            result.extend([i + 1, x])
            A = A[:i:-1] + A[:i]
    return result

# Example
A = [3, 2, 4, 1]
print(pancakeSort(A))  # Output: [3, 4, 2, 3, 1, 2, 1, 1]
```
#### TC: O(N^2) **SC:** O(1)

The pancakeSort function implements the pancake sorting algorithm to sort an array A. It iterates
through the array in reverse order, finding the index of the current maximum element and flipping
the array to move the element to its correct position. The result list keeps track of the flips
performed. The time complexity of the algorithm is O(N^2) due to the nested loops, where N is the
length of the input array. The space complexity is O(1) as the algorithm sorts the array in place
without using any additional data structures.

---
---
---
 --- 
# Sort Transformed Array
>Apply a quadratic function to each element and return the sorted array.

>Input: nums = [-4,-2,2,4], a = 1, b = 3, c = 5
Output: [3,9,15,33]
- https://leetcode.com/problems/sort-transformed-array/
- Difficulty: 55
- Frequent: 55
- Tags: ['Sorting', 'Array']

### Sort Transformed Array -- Shortest Solution:
```python
from typing import List

class Solution:
    def sortTransformedArray(self, nums: List[int], a: int, b: int, c: int) -> List[int]:
        def f(x):
            return a * x * x + b * x + c
        res = [0] * len(nums)
        i, j = 0, len(nums) - 1
        idx = len(nums) - 1 if a >= 0 else 0
        while i <= j:
            if a >= 0:
                if f(nums[i]) >= f(nums[j]):
                    res[idx] = f(nums[i])
                    i += 1
                else:
                    res[idx] = f(nums[j])
                    j -= 1
                idx -= 1
            else:
                if f(nums[i]) <= f(nums[j]):
                    res[idx] = f(nums[i])
                    i += 1
                else:
                    res[idx] = f(nums[j])
                    j -= 1
                idx += 1
        return res
```
#### TC: O(n) **SC:** O(1)

The code defines a function f(x) to calculate the transformed value based on the coefficients a, b,
and c. It then initializes an array 'res' to store the sorted transformed values. Using two pointers
i and j, the code iterates through the input array 'nums' and fills the 'res' array with the
transformed values in sorted order. The direction of filling the 'res' array depends on the
coefficient 'a'. The time complexity of the code is O(n) as it iterates through the input array
once. The space complexity is O(1) as it only uses a constant amount of extra space for the 'res'
array.

---
---
---
 --- 
# Sort the Matrix Diagonally
>Sort each diagonal of a matrix in ascending order.

>Input: [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
Output: [[1,1,1,1],[1,2,2,2],[1,2,3,3]]
- https://leetcode.com/problems/sort-the-matrix-diagonally/
- Difficulty: 55
- Frequent: 55
- Tags: ['Sorting', 'Matrix']

### Sort the Matrix Diagonally -- Shortest Solution:
```python
from collections import defaultdict

def diagonalSort(mat):
    m, n = len(mat), len(mat[0])
    diagonals = defaultdict(list)
    for i in range(m):
        for j in range(n):
            diagonals[i - j].append(mat[i][j])
    
    for k in diagonals:
        diagonals[k].sort(reverse=True)
    
    for i in range(m):
        for j in range(n):
            mat[i][j] = diagonals[i - j].pop()
    
    return mat
```
#### TC: O((m*n)log(min(m,n))) **SC:** O(m*n)

The code first creates a defaultdict to store elements of each diagonal based on the difference of
row and column indices. It then sorts each diagonal in reverse order. Finally, it updates the matrix
with the sorted elements from the diagonals. The time complexity is O((m*n)log(min(m,n))) due to
sorting each diagonal, and the space complexity is O(m*n) to store the diagonals.

---
### Sort the Matrix Diagonally -- Best TC Solution:
```python
from collections import defaultdict

def diagonalSort(mat):
    m, n = len(mat), len(mat[0])
    diagonals = defaultdict(list)
    for i in range(m):
        for j in range(n):
            diagonals[i - j].append(mat[i][j])
    
    for k in diagonals:
        diagonals[k].sort(reverse=True)
    
    for i in range(m):
        for j in range(n):
            mat[i][j] = diagonals[i - j].pop()
    
    return mat
```
#### TC: O(M*N*log(min(M,N))) **SC:** O(M*N)

The code first creates a defaultdict to store elements of each diagonal based on their difference in
row and column indices. It then sorts each diagonal in reverse order. Finally, it updates the matrix
with the sorted elements from the diagonals. The time complexity is O(M*N*log(min(M,N))) due to
sorting each diagonal, and the space complexity is O(M*N) to store the diagonals.

---
---
---
 --- 
# Maximum Gap
>Find the maximum gap between the successive elements in a sorted form of an array.

>Input: [3,6,9,1]
Output: 3
- https://leetcode.com/problems/maximum-gap/
- Difficulty: 60
- Frequent: 55
- Tags: ['Sorting', 'Array']

### Maximum Gap -- Shortest Solution:
```python
from typing import List

class Solution:
    def maximumGap(self, nums: List[int]) -> int:
        if len(nums) < 2:
            return 0
        nums.sort()
        max_gap = 0
        for i in range(1, len(nums)):
            max_gap = max(max_gap, nums[i] - nums[i-1])
        return max_gap
```
#### TC: O(n log n) **SC:** O(1)

The code defines a class Solution with a method maximumGap that takes a list of integers as input.
It first checks if the length of the input list is less than 2 and returns 0 in that case. It then
sorts the input list in ascending order using the sort() method. It initializes a variable max_gap
to 0 and iterates through the sorted list to find the maximum difference between adjacent elements,
which represents the maximum gap. The code returns the maximum gap at the end. The time complexity
of the code is O(n log n) due to the sorting operation, and the space complexity is O(1) as it only
uses a constant amount of extra space.

---
### Maximum Gap -- Best TC Solution:
```python
# Function to find the maximum gap between elements in a sorted array

def maximumGap(nums):
    if len(nums) < 2:
        return 0
    nums.sort()
    max_gap = 0
    for i in range(1, len(nums)):
        max_gap = max(max_gap, nums[i] - nums[i-1])
    return max_gap

# Example
nums = [3, 6, 9, 1]
print(maximumGap(nums) # Output: 3
```
#### TC: O(nlogn) **SC:** O(1)

The code defines a function that calculates the maximum gap between elements in a sorted array. It
first checks if the array has less than 2 elements and returns 0 in that case. It then sorts the
array and iterates through the sorted array to find the maximum difference between adjacent
elements. The time complexity is O(nlogn) due to the sorting operation, and the space complexity is
O(1) as the algorithm uses constant extra space.

---
---
---
 --- 
# Wiggle Sort II
>Rearrange an array to follow the wiggle property.

>Input: [1,5,1,1,6,4]
Output: [1,6,1,5,1,4]
- https://leetcode.com/problems/wiggle-sort-ii/
- Difficulty: 65
- Frequent: 55
- Tags: ['Sorting', 'Array']

### Wiggle Sort II -- Shortest Solution:
```python
# Wiggle Sort II
# Time Complexity: O(nlogn)
# Space Complexity: O(n)

from typing import List

def wiggleSort(nums: List[int]) -> None:
    nums.sort()
    mid = (len(nums) - 1) // 2
    nums[::2], nums[1::2] = nums[mid::-1], nums[:mid:-1]

# Example
nums = [1, 5, 1, 1, 6, 4]
wiggleSort(nums)
print(nums)
```
#### TC: O(nlogn) **SC:** O(n)

The code first sorts the input array in ascending order. Then, it rearranges the elements in a
'wiggle' pattern by interleaving the smaller half of the sorted array in reverse order with the
larger half of the sorted array in reverse order. This approach ensures that adjacent elements have
the desired relationship (greater or smaller) in the final output. The time complexity is O(nlogn)
due to the sorting step, and the space complexity is O(n) to store the sorted array.

---
### Wiggle Sort II -- Best SC Solution:
```python
from typing import List

def wiggleSort(nums: List[int]) -> None:
    nums.sort()
    mid = len(nums) // 2
    nums[::2], nums[1::2] = nums[:mid][::-1], nums[mid:][::-1]
```
#### TC: O(n log n) **SC:** O(1)

The code first sorts the input array. Then, it rearranges the sorted array in a 'wiggle' pattern by
reversing the first half and the second half of the array and interleaving them. This approach
ensures that the elements alternate in a way that satisfies the wiggle sort condition. The time
complexity is O(n log n) due to the sorting operation, and the space complexity is O(1) as the
sorting is done in-place without using any additional space.

---
---
---