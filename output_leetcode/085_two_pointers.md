# Two Pointers
## Frequent score for this algorithmic framework: 85 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [3Sum](#3sum) | 90 | 50 |
| 2 | [Container With Most Water](#container-with-most-water) | 85 | 45 |
| 3 | [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters) | 85 | 45 |
| 4 | [Two Sum II - Input Array Is Sorted](#two-sum-ii-input-array-is-sorted) | 80 | 30 |
| 5 | [Trapping Rain Water](#trapping-rain-water) | 80 | 60 |
| 6 | [Remove Nth Node From End of List](#remove-nth-node-from-end-of-list) | 75 | 35 |
| 7 | [Linked List Cycle](#linked-list-cycle) | 75 | 40 |
| 8 | [Remove Duplicates from Sorted Array](#remove-duplicates-from-sorted-array) | 70 | 20 |
| 9 | [Move Zeroes](#move-zeroes) | 70 | 20 |
| 10 | [Sort Colors](#sort-colors) | 70 | 30 |
| 11 | [Palindrome Linked List](#palindrome-linked-list) | 70 | 35 |
| 12 | [Reverse String](#reverse-string) | 65 | 15 |
| 13 | [Partition Labels](#partition-labels) | 65 | 35 |
| 14 | [Minimum Size Subarray Sum](#minimum-size-subarray-sum) | 65 | 40 |
| 15 | [Reverse Vowels of a String](#reverse-vowels-of-a-string) | 60 | 25 |
| 16 | [Valid Palindrome II](#valid-palindrome-ii) | 60 | 30 |
| 17 | [Subarray Product Less Than K](#subarray-product-less-than-k) | 60 | 40 |
| 18 | [Squares of a Sorted Array](#squares-of-a-sorted-array) | 55 | 20 |
---
The Two Pointers technique involves using two pointers to iterate through a data structure, typically an array or a linked list. The pointers can move towards each other, away from each other, or in the same direction, depending on the problem. This technique is often used to solve problems involving pairs, subarrays, or partitions efficiently.
```python
# Example 1: Removing Duplicates from Sorted Array
# Given a sorted array nums, remove the duplicates in-place such that each element appears only once and returns the new length.
def remove_duplicates(nums):
    if not nums:
        return 0
    write_pointer = 1
    for read_pointer in range(1, len(nums)):
        if nums[read_pointer] != nums[read_pointer - 1]:
            nums[write_pointer] = nums[read_pointer]
            write_pointer += 1
    return write_pointer

# Example 2: Two Sum II - Input array is sorted
# Given an array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number.
def two_sum(numbers, target):
    left, right = 0, len(numbers) - 1
    while left < right:
        current_sum = numbers[left] + numbers[right]
        if current_sum == target:
            return [left + 1, right + 1]
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return []
```
### Insight:
1. The Two Pointers technique is particularly useful for problems involving sorted arrays or linked lists.
2. It can significantly reduce the time complexity compared to a brute-force approach.
3. The technique is versatile and can be adapted to various problems, such as finding pairs, subarrays, or partitions.
4. Understanding the movement of pointers is crucial for correctly implementing this technique.
5. Edge cases, such as empty arrays or arrays with all identical elements, should be carefully handled.
---
 --- 
# 3Sum
>Find all unique triplets in the array which gives the sum of zero.

>Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
- https://leetcode.com/problems/3sum/
- Difficulty: 50
- Frequent: 90
- Tags: ['Array', 'Two Pointers', 'Sorting']

### 3Sum -- Shortest Solution:
```python
def threeSum(nums):
    nums.sort()
    res = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        l, r = i + 1, len(nums) - 1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s < 0:
                l += 1
            elif s > 0:
                r -= 1
            else:
                res.append([nums[i], nums[l], nums[r]])
                while l < r and nums[l] == nums[l + 1]:
                    l += 1
                while l < r and nums[r] == nums[r - 1]:
                    r -= 1
                l += 1
                r -= 1
    return res
```
#### TC: O(n^2) **SC:** O(n)

1. **Sorting**: The first step is to sort the array. This helps in easily skipping duplicates and
using the two-pointer technique. 2. **Iterating**: We iterate through the array, fixing one element
and using two pointers to find the other two elements that sum up to zero. 3. **Two Pointers**: For
each fixed element, we use two pointers (one starting just after the fixed element and the other
starting from the end of the array) to find pairs that sum up to the negative of the fixed element.
4. **Skipping Duplicates**: To avoid duplicate triplets, we skip over duplicate elements both when
fixing an element and when moving the pointers. 5. **Appending Results**: When a triplet is found,
it is added to the result list, and the pointers are adjusted to continue searching for other
possible pairs. 6. **Efficiency**: The sorting step takes O(n log n) time, and the two-pointer
search takes O(n) for each element, leading to an overall time complexity of O(n^2). The space
complexity is O(n) due to the space required for the output list and the sorting operation.

---
---
---
 --- 
# Container With Most Water
>Find two lines that together with the x-axis form a container, such that the container contains the most water.

>Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
- https://leetcode.com/problems/container-with-most-water/
- Difficulty: 45
- Frequent: 85
- Tags: ['Array', 'Two Pointers', 'Greedy']

### Container With Most Water -- Shortest Solution:
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l, r = 0, len(height) - 1
        max_area = 0
        while l < r:
            max_area = max(max_area, (r - l) * min(height[l], height[r]))
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        return max_area
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer approach to find the maximum area. The pointers start at the beginning
and end of the list and move towards each other. At each step, the area is calculated and the
maximum area is updated. The pointer pointing to the shorter line is moved inward to potentially
find a taller line that could form a larger area.

---
### Container With Most Water -- Best TC Solution:
```python
# Two Pointers Approach

def max_area(height):
    max_area = 0
    left = 0
    right = len(height) - 1
    while left < right:
        width = right - left
        h = min(height[left], height[right])
        max_area = max(max_area, width * h)
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    return max_area

# Example
height = [1,8,6,2,5,4,8,3,7]
print(max_area(height))
```
#### TC: O(N) **SC:** O(1)

The code uses the Two Pointers approach to efficiently find the maximum area between two vertical
lines. By moving the pointers towards each other based on the height of the lines, the algorithm
calculates the maximum area. This approach reduces the time complexity to O(N) and the space
complexity to O(1), making it an optimal solution for the problem.

---
---
---
 --- 
# Longest Substring Without Repeating Characters
>Find the length of the longest substring without repeating characters.

>Input: s = "abcabcbb"
Output: 3
- https://leetcode.com/problems/longest-substring-without-repeating-characters/
- Difficulty: 45
- Frequent: 85
- Tags: ['Hash Table', 'Two Pointers', 'String']

### Longest Substring Without Repeating Characters -- Shortest Solution:
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_map = {}
        left = 0
        max_length = 0
        for right in range(len(s)):
            if s[right] in char_map:
                left = max(left, char_map[s[right]] + 1)
            char_map[s[right]] = right
            max_length = max(max_length, right - left + 1)
        return max_length
```
#### TC: O(n) **SC:** O(min(n, m))

The code uses a sliding window approach with two pointers (`left` and `right`). The `char_map`
dictionary keeps track of the last seen index of each character. As we iterate through the string
with the `right` pointer, if we encounter a repeating character, we move the `left` pointer to the
right of the last seen index of that character. This ensures that the substring between `left` and
`right` has all unique characters. The `max_length` variable keeps track of the maximum length of
such substrings encountered during the iteration.

---
### Longest Substring Without Repeating Characters -- Best SC Solution:
```python
# Function to find the length of the longest substring without repeating characters

def lengthOfLongestSubstring(s: str) -> int:
    max_length = 0
    start = 0
    char_index = {}

    for end in range(len(s)):
        if s[end] in char_index:
            start = max(start, char_index[s[end]] + 1)
        char_index[s[end]] = end
        max_length = max(max_length, end - start + 1)

    return max_length

# Example usage
s = "abcabcbb"
result = lengthOfLongestSubstring(s)
print(result)
```
#### TC: O(n) **SC:** O(min(m, n))

The code uses a sliding window approach with two pointers to track the longest substring without
repeating characters. The 'char_index' dictionary is used to store the index of each character in
the string. The 'start' pointer is updated to the maximum of its current position and the next index
of the repeating character. The maximum length of the substring is updated at each iteration. The
time complexity is O(n) where n is the length of the input string, and the space complexity is
O(min(m, n)) where m is the size of the character set.

---
---
---
 --- 
# Two Sum II - Input Array Is Sorted
>Find two numbers such that they add up to a specific target number.

>Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
- https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/
- Difficulty: 30
- Frequent: 80
- Tags: ['Array', 'Two Pointers', 'Binary Search']

### Two Sum II - Input Array Is Sorted -- Shortest Solution:
```python
def twoSum(numbers, target):
 left, right = 0, len(numbers) - 1
 while left < right:
 s = numbers[left] + numbers[right]
 if s == target:
 return [left + 1, right + 1]
 elif s < target:
 left += 1
 else:
 right -= 1
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer approach to find two numbers in a sorted array that add up to a target
value. The left pointer starts at the beginning and the right pointer starts at the end. Depending
on the sum of the two pointers, the pointers are moved inward until the target sum is found.

---
---
---
 --- 
# Trapping Rain Water
>Calculate how much water it is able to trap after raining.

>Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
- https://leetcode.com/problems/trapping-rain-water/
- Difficulty: 60
- Frequent: 80
- Tags: ['Array', 'Two Pointers', 'Dynamic Programming']

### Trapping Rain Water -- Shortest Solution:
```python
heights = [0,1,0,2,1,0,1,3,2,1,2,1]
left, right = 0, len(heights) - 1
left_max, right_max = 0, 0
water = 0
while left < right:
    if heights[left] < heights[right]:
        if heights[left] >= left_max:
            left_max = heights[left]
        else:
            water += left_max - heights[left]
        left += 1
    else:
        if heights[right] >= right_max:
            right_max = heights[right]
        else:
            water += right_max - heights[right]
        right -= 1
print(water)
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer approach to traverse the height array from both ends towards the center.
It keeps track of the maximum heights encountered from both the left and right sides. By comparing
the current height with the maximum heights, it calculates the trapped water at each step. The time
complexity is O(n) because it processes each element once, and the space complexity is O(1) as it
uses a constant amount of extra space.

---
---
---
 --- 
# Remove Nth Node From End of List
>Remove the nth node from the end of list and return its head.

>Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
- https://leetcode.com/problems/remove-nth-node-from-end-of-list/
- Difficulty: 35
- Frequent: 75
- Tags: ['Linked List', 'Two Pointers']

### Remove Nth Node From End of List -- Shortest Solution:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeNthFromEnd(head: ListNode, n: int) -> ListNode:
    dummy = ListNode(0, head)
    first = second = dummy
    for _ in range(n + 1):
        first = first.next
    while first:
        first = first.next
        second = second.next
    second.next = second.next.next
    return dummy.next
```
#### TC: O(L) **SC:** O(1)

The code uses a two-pointer approach to efficiently remove the nth node from the end of the list.
The `dummy` node simplifies edge cases, such as removing the head. The `first` pointer advances
`n+1` steps ahead, ensuring that when `first` reaches the end, `second` is just before the target
node. This allows for easy removal of the nth node from the end.

---
### Remove Nth Node From End of List -- Best TC Solution:
```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        first = dummy
        second = dummy
        for _ in range(n + 1):
            first = first.next
        while first is not None:
            first = first.next
            second = second.next
        second.next = second.next.next
        return dummy.next
```
#### TC: O(N) **SC:** O(1)

The code uses the Two Pointers approach to find the Nth node from the end of the linked list. It
initializes two pointers, 'first' and 'second', with a gap of 'n' nodes between them. By moving both
pointers simultaneously, when the 'first' pointer reaches the end of the list, the 'second' pointer
will be at the Nth node from the end. Then, it adjusts the pointers to remove the Nth node
efficiently. The time complexity is O(N) as it traverses the list once, and the space complexity is
O(1) as it uses constant extra space.

---
---
---
 --- 
# Linked List Cycle
>Detect if a cycle exists in a linked list.

>Input: head = [3,2,0,-4], pos = 1
Output: true
- https://leetcode.com/problems/linked-list-cycle/
- Difficulty: 40
- Frequent: 75
- Tags: ['Linked List', 'Two Pointers']

### Linked List Cycle -- Shortest Solution:
```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def hasCycle(head: ListNode) -> bool:
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```
#### TC: O(n) **SC:** O(1)

The code uses the two pointers approach, also known as Floyd's Tortoise and Hare algorithm. The
`slow` pointer moves one step at a time while the `fast` pointer moves two steps at a time. If there
is a cycle, the `fast` pointer will eventually meet the `slow` pointer, indicating a cycle. If the
`fast` pointer reaches the end of the list, it means there is no cycle.

---
---
---
 --- 
# Remove Duplicates from Sorted Array
>Remove duplicates in-place such that each element appears only once and returns the new length.

>Input: nums = [1,1,2]
Output: 2, nums = [1,2]
- https://leetcode.com/problems/remove-duplicates-from-sorted-array/
- Difficulty: 20
- Frequent: 70
- Tags: ['Array', 'Two Pointers']

### Remove Duplicates from Sorted Array -- Shortest Solution:
```python
def remove_duplicates(nums):
    if not nums:
        return 0
    i = 0
    for j in range(1, len(nums)):
        if nums[j] != nums[i]:
            i += 1
            nums[i] = nums[j]
    return i + 1
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer approach to remove duplicates in-place from a sorted array. The first
pointer `i` keeps track of the position of the last unique element, while the second pointer `j`
iterates through the array. When a new unique element is found, it is placed next to the last unique
element, and the pointer `i` is incremented. The function returns the length of the array with
unique elements.

---
---
---
 --- 
# Move Zeroes
>Move all 0's to the end of the array while maintaining the relative order of the non-zero elements.

>Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
- https://leetcode.com/problems/move-zeroes/
- Difficulty: 20
- Frequent: 70
- Tags: ['Array', 'Two Pointers']

### Move Zeroes -- Shortest Solution:
```python
def moveZeroes(nums):
    i = 0
    for j in range(len(nums)):
        if nums[j] != 0:
            nums[i], nums[j] = nums[j], nums[i]
            i += 1
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer approach to move all non-zero elements to the front of the list while
maintaining their relative order. The first pointer `i` keeps track of the position to place the
next non-zero element, and the second pointer `j` iterates through the list. When a non-zero element
is found, it is swapped with the element at position `i`, and `i` is incremented. This ensures that
all non-zero elements are moved to the front, and all zeros are moved to the end.

---
---
---
 --- 
# Sort Colors
>Sort the array with colors represented by 0, 1, and 2.

>Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
- https://leetcode.com/problems/sort-colors/
- Difficulty: 30
- Frequent: 70
- Tags: ['Array', 'Two Pointers', 'Sorting']

### Sort Colors -- Shortest Solution:
```python
def sortColors(nums):
    low, mid, high = 0, 0, len(nums) - 1
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:
            nums[high], nums[mid] = nums[mid], nums[high]
            high -= 1
```
#### TC: O(n) **SC:** O(1)

The code uses the Dutch National Flag algorithm to sort the array in a single pass. It maintains
three pointers: `low`, `mid`, and `high`. The `low` pointer tracks the position for the next 0, the
`mid` pointer scans through the array, and the `high` pointer tracks the position for the next 2. By
swapping elements accordingly, the array is sorted in linear time with constant space.

---
---
---
 --- 
# Palindrome Linked List
>Check if a linked list is a palindrome.

>Input: head = [1,2,2,1]
Output: true
- https://leetcode.com/problems/palindrome-linked-list/
- Difficulty: 35
- Frequent: 70
- Tags: ['Linked List', 'Two Pointers']

### Palindrome Linked List -- Shortest Solution:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        prev = None
        while slow:
            next_node = slow.next
            slow.next = prev
            prev = slow
            slow = next_node
        left, right = head, prev
        while right:
            if left.val != right.val:
                return False
            left = left.next
            right = right.next
        return True
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer technique to find the middle of the linked list. It then reverses the
second half of the list in-place. Finally, it compares the first half and the reversed second half
node by node to check for palindrome properties. This approach ensures that the solution is both
time and space efficient.

---
---
---
 --- 
# Reverse String
>Reverse the input string in-place with O(1) extra memory.

>Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
- https://leetcode.com/problems/reverse-string/
- Difficulty: 15
- Frequent: 65
- Tags: ['Two Pointers', 'String']

### Reverse String -- Shortest Solution:
```python
s.reverse()
```
#### TC: O(n) **SC:** O(1)

The solution leverages Python's built-in list method `reverse()`, which reverses the elements of the
list in place. This method is optimal for this problem as it operates in linear time, O(n), where n
is the length of the list, and it uses constant space, O(1), since it modifies the list in place
without requiring additional storage.

---
### Reverse String -- Best TC Solution:
```python
# Two Pointers Approach

def reverseString(s):
    left, right = 0, len(s) - 1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
    return s

# Example
s = ['h', 'e', 'l', 'l', 'o']
result = reverseString(s)
print(result)
```
#### TC: O(N) **SC:** O(1)

The code uses the Two Pointers approach to reverse the input string. It initializes two pointers,
one at the beginning and one at the end of the string, and swaps the characters at these pointers
while moving towards the center. This approach allows us to reverse the string in-place without
using any extra space. The time complexity of the code is O(N) where N is the length of the input
string, as we iterate through half of the string to perform the swapping. The space complexity is
O(1) as we only use a constant amount of extra space regardless of the input size.

---
---
---
 --- 
# Partition Labels
>Partition the string into as many parts as possible so that each letter appears in at most one part.

>Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
- https://leetcode.com/problems/partition-labels/
- Difficulty: 35
- Frequent: 65
- Tags: ['Hash Table', 'Two Pointers', 'String']

### Partition Labels -- Shortest Solution:
```python
def partitionLabels(S):
    last = {c: i for i, c in enumerate(S)}
    j = anchor = 0
    ans = []
    for i, c in enumerate(S):
        j = max(j, last[c])
        if i == j:
            ans.append(i - anchor + 1)
            anchor = i + 1
    return ans
```
#### TC: O(N) **SC:** O(1)

1. Create a dictionary to store the last occurrence of each character. 2. Use two pointers: `j` to
track the end of the current partition and `anchor` to mark the start. 3. Iterate through the
string, updating `j` to the farthest last occurrence of the current character. 4. When the current
index `i` matches `j`, it means we have found a partition. Append the size of the partition to the
result list and update `anchor` to the next index. 5. The algorithm ensures that each character in a
partition does not appear in any other partition.

---
---
---
 --- 
# Minimum Size Subarray Sum
>Find the minimal length of a contiguous subarray of which the sum is greater than or equal to target.

>Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
- https://leetcode.com/problems/minimum-size-subarray-sum/
- Difficulty: 40
- Frequent: 65
- Tags: ['Array', 'Two Pointers', 'Binary Search']

### Minimum Size Subarray Sum -- Shortest Solution:
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        left = 0
        total = 0
        min_length = float('inf')
        for right in range(len(nums)):
            total += nums[right]
            while total >= target:
                min_length = min(min_length, right - left + 1)
                total -= nums[left]
                left += 1
        return min_length if min_length != float('inf') else 0
```
#### TC: O(n) **SC:** O(1)

The code uses a two-pointer approach to maintain a sliding window. The `right` pointer expands the
window by moving to the right, and the `left` pointer contracts the window from the left when the
sum of the window is greater than or equal to the target. This ensures that we find the minimum
length subarray that meets the condition. The time complexity is O(n) because each element is
processed at most twice, and the space complexity is O(1) because no additional space proportional
to the input size is used.

---
### Minimum Size Subarray Sum -- Best TC Solution:
```python
# Two Pointers Approach

def minSubArrayLen(target, nums):
    if not nums:
        return 0
    left = 0
    min_len = float('inf')
    current_sum = 0
    for right in range(len(nums)):
        current_sum += nums[right]
        while current_sum >= target:
            min_len = min(min_len, right - left + 1)
            current_sum -= nums[left]
            left += 1
    return min_len if min_len != float('inf') else 0

# Example Usage
# target = 7
# nums = [2,3,1,2,4,3]
# print(minSubArrayLen(target, nums))
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(1)

The code uses a two pointers approach to find the minimum size subarray sum that is greater than or
equal to the target. It iterates through the array with two pointers, adjusting the window size
based on the current sum. The while loop helps in shrinking the window size to find the minimum
length subarray. The code is clean, efficient, and easy to understand, with a time complexity of
O(n) and constant space complexity of O(1).

---
---
---
 --- 
# Reverse Vowels of a String
>Reverse only the vowels of a string.

>Input: s = "hello"
Output: "holle"
- https://leetcode.com/problems/reverse-vowels-of-a-string/
- Difficulty: 25
- Frequent: 60
- Tags: ['Two Pointers', 'String']

### Reverse Vowels of a String -- Shortest Solution:
```python
def reverseVowels(s: str) -> str:
    vowels = set('aeiouAEIOU')
    s = list(s)
    i, j = 0, len(s) - 1
    while i < j:
        if s[i] not in vowels:
            i += 1
        elif s[j] not in vowels:
            j -= 1
        else:
            s[i], s[j] = s[j], s[i]
            i += 1
            j -= 1
    return ''.join(s)
```
#### TC: O(n) **SC:** O(n)

The code uses a two-pointer approach to reverse the vowels in a string. It initializes two pointers,
one at the beginning and one at the end of the string. It then moves the pointers towards each
other, swapping vowels when both pointers point to vowels. The use of a set for vowels allows for
O(1) average-time complexity checks. The string is converted to a list to allow in-place
modifications, and then joined back into a string before returning.

---
---
---
 --- 
# Valid Palindrome II
>Return true if the string can be palindrome after deleting at most one character.

>Input: s = "abca"
Output: true
- https://leetcode.com/problems/valid-palindrome-ii/
- Difficulty: 30
- Frequent: 60
- Tags: ['Two Pointers', 'String', 'Greedy']

### Valid Palindrome II -- Shortest Solution:
```python
def validPalindrome(s):
    def is_palindrome_range(i, j):
        return all(s[k] == s[j-k+i] for k in range(i, j))
    i, j = 0, len(s) - 1
    while i < j:
        if s[i] != s[j]:
            return is_palindrome_range(i+1, j+1) or is_palindrome_range(i, j)
        i, j = i + 1, j - 1
    return True
```
#### TC: O(n) **SC:** O(1)

The function `validPalindrome` uses a helper function `is_palindrome_range` to check if a substring
is a palindrome. The main function iterates with two pointers from the start and end of the string
towards the center. If a mismatch is found, it checks if either the substring excluding the left
character or the substring excluding the right character is a palindrome. This allows for at most
one character to be removed to form a palindrome.

---
### Valid Palindrome II -- Best TC Solution:
```python
# Function to check if a string is a palindrome

def is_palindrome(s, left, right):
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True


def valid_palindrome(s: str) -> bool:
    left = 0
    right = len(s) - 1
    while left < right:
        if s[left] != s[right]:
            return is_palindrome(s, left + 1, right) or is_palindrome(s, left, right - 1)
        left += 1
        right -= 1
    return True
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(1)

The code defines a function to check if a given string is a palindrome. It then uses a two-pointer
approach to iterate through the string and determine if it can be made a palindrome by removing at
most one character. The function is_palindrome checks if a substring is a palindrome. The
valid_palindrome function iterates through the string using two pointers, checking if the characters
at the pointers are equal. If they are not, it calls the is_palindrome function for two cases:
removing the character at the left pointer or the character at the right pointer. This approach
efficiently handles the removal of at most one character to make the string a palindrome.

---
---
---
 --- 
# Subarray Product Less Than K
>Find the number of (contiguous) subarrays where the product of all the elements in the subarray is less than k.

>Input: nums = [10,5,2,6], k = 100
Output: 8
- https://leetcode.com/problems/subarray-product-less-than-k/
- Difficulty: 40
- Frequent: 60
- Tags: ['Array', 'Two Pointers']

### Subarray Product Less Than K -- Shortest Solution:
```python
class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        if k <= 1: return 0
        prod = 1
        left = 0
        count = 0
        for right, val in enumerate(nums):
            prod *= val
            while prod >= k:
                prod //= nums[left]
                left += 1
            count += right - left + 1
        return count
```
#### TC: O(n) **SC:** O(1)

The solution uses a sliding window (two pointers) approach to maintain a subarray with a product
less than k. The `left` pointer adjusts to ensure the product condition is met, while the `right`
pointer expands the window. The count of valid subarrays is incremented by the number of new
subarrays ending at the current `right` index.

---
### Subarray Product Less Than K -- Best TC Solution:
```python
from typing import List

class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        if k <= 1:
            return 0
        count = 0
        prod = 1
        left = 0
        for right in range(len(nums)):
            prod *= nums[right]
            while prod >= k:
                prod /= nums[left]
                left += 1
            count += right - left + 1
        return count
```
#### TC: O(N) **SC:** O(1)

The code defines a class Solution with a method numSubarrayProductLessThanK that takes a list of
integers nums and an integer k as input. It uses a two-pointer approach to find the number of
subarrays where the product is less than k. The algorithm iterates through the array while
maintaining a sliding window defined by the left and right pointers. It calculates the product of
elements within the window and adjusts the window based on the product value compared to k. The
count variable keeps track of the number of valid subarrays. The time complexity of the solution is
O(N) where N is the length of the input array, and the space complexity is O(1) as it only uses a
constant amount of extra space.

---
---
---
 --- 
# Squares of a Sorted Array
>Return an array of the squares of each number sorted in non-decreasing order.

>Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
- https://leetcode.com/problems/squares-of-a-sorted-array/
- Difficulty: 20
- Frequent: 55
- Tags: ['Array', 'Two Pointers']

### Squares of a Sorted Array -- Shortest Solution:
```python
from typing import List

def sortedSquares(nums: List[int]) -> List[int]:
    return sorted(x * x for x in nums)
```
#### TC: O(n log n) **SC:** O(n)

The code leverages Python's built-in `sorted` function and a generator expression to create a list
of squared values. The `sorted` function sorts these squared values in non-decreasing order. This
approach is concise and leverages Python's efficient sorting algorithm, Timsort, which has a time
complexity of O(n log n). The space complexity is O(n) due to the space required to store the
squared values.

---
### Squares of a Sorted Array -- Best TC Solution:
```python
from typing import List

class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        n = len(nums)
        result = [0] * n
        left, right = 0, n - 1
        for i in range(n - 1, -1, -1):
            if abs(nums[left]) &gt;= abs(nums[right]):
                result[i] = nums[left] ** 2
                left += 1
            else:
                result[i] = nums[right] ** 2
                right -= 1
        return result
```
#### TC: O(n) **SC:** O(n)

The code defines a class Solution with a method sortedSquares that takes a list of integers as
input. It initializes a result list with the same length as the input list. Using two pointers, it
iterates from the end of the list to the beginning, comparing the absolute values of the elements
pointed by the left and right pointers. The squared value of the greater absolute element is added
to the result list. Finally, the squared and sorted result list is returned. The time complexity of
this solution is O(n) as it iterates through the input list once, and the space complexity is O(n)
to store the result list.

---
---
---