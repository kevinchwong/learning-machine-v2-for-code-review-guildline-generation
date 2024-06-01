# Greedy Algorithm
## Frequent score for this algorithmic framework: 65 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Jump Game](#jump-game) | 80 | 55 |
| 2 | [Jump Game II](#jump-game-ii) | 75 | 60 |
| 3 | [Best Time to Buy and Sell Stock II](#best-time-to-buy-and-sell-stock-ii) | 70 | 50 |
| 4 | [Candy](#candy) | 70 | 65 |
| 5 | [Split Array into Consecutive Subsequences](#split-array-into-consecutive-subsequences) | 65 | 60 |
| 6 | [Queue Reconstruction by Height](#queue-reconstruction-by-height) | 65 | 70 |
| 7 | [Gas Station](#gas-station) | 60 | 50 |
| 8 | [Minimum Number of Arrows to Burst Balloons](#minimum-number-of-arrows-to-burst-balloons) | 60 | 50 |
| 9 | [Reorganize String](#reorganize-string) | 60 | 55 |
| 10 | [Wiggle Subsequence](#wiggle-subsequence) | 55 | 50 |
| 11 | [Non-overlapping Intervals](#non-overlapping-intervals) | 55 | 55 |
| 12 | [Erase Overlap Intervals](#erase-overlap-intervals) | 55 | 55 |
| 13 | [Assign Cookies](#assign-cookies) | 50 | 40 |
| 14 | [Is Subsequence](#is-subsequence) | 50 | 40 |
| 15 | [Monotone Increasing Digits](#monotone-increasing-digits) | 50 | 45 |
| 16 | [Lemonade Change](#lemonade-change) | 45 | 30 |
---
Greedy algorithms make a series of choices, each of which looks best at the moment, with the hope of finding a global optimum. They do not reconsider their choices, which can lead to suboptimal solutions for some problems but work well for others, like finding the minimum spanning tree or the shortest path in a graph.
```python
# Example 1: Coin Change Problem
# Find the minimum number of coins that make a given value

def coin_change(coins, amount):
    coins.sort(reverse=True)
    count = 0
    for coin in coins:
        while amount >= coin:
            amount -= coin
            count += 1
    return count

# Example 2: Fractional Knapsack Problem
# Get the maximum total value in the knapsack

class Item:
    def __init__(self, value, weight):
        self.value = value
        self.weight = weight

    def __lt__(self, other):
        return self.value / self.weight > other.value / other.weight


def fractional_knapsack(items, capacity):
    items.sort()
    total_value = 0.0
    for item in items:
        if capacity - item.weight >= 0:
            capacity -= item.weight
            total_value += item.value
        else:
            total_value += item.value * (capacity / item.weight)
            break
    return total_value
```
### Insight:
1. Greedy algorithms are easy to implement and understand.
2. They are not always optimal but can be very efficient for specific problems.
3. Sorting the input is often a preliminary step, which dominates the time complexity.
4. They work well when a problem has the greedy-choice property and optimal substructure.
---
 --- 
# Jump Game
>Determine if you can reach the last index.

>Input: nums = [2,3,1,1,4]
Output: true
- https://leetcode.com/problems/jump-game/
- Difficulty: 55
- Frequent: 80
- Tags: ['Array', 'Greedy']

### Jump Game -- Shortest Solution:
```python
def canJump(nums):
    max_reach = 0
    for i, num in enumerate(nums):
        if i > max_reach:
            return False
        max_reach = max(max_reach, i + num)
    return True
```
#### TC: O(n) **SC:** O(1)

The code uses a greedy algorithm to determine if you can reach the last index of the array. It
iterates through the array while keeping track of the maximum index that can be reached
(`max_reach`). If at any point the current index `i` is greater than `max_reach`, it means you
cannot move forward and the function returns `False`. If the loop completes, it means you can reach
the last index and the function returns `True`. The time complexity is O(n) because it involves a
single pass through the array, and the space complexity is O(1) because it uses a constant amount of
extra space.

---
---
---
 --- 
# Jump Game II
>Find the minimum number of jumps to reach the last index.

>Input: nums = [2,3,1,1,4]
Output: 2
- https://leetcode.com/problems/jump-game-ii/
- Difficulty: 60
- Frequent: 75
- Tags: ['Array', 'Greedy']

### Jump Game II -- Shortest Solution:
```python
nums = [2,3,1,1,4]

class Solution:
    def jump(self, nums):
        jumps = 0
        current_end = 0
        farthest = 0
        for i in range(len(nums) - 1):
            farthest = max(farthest, i + nums[i])
            if i == current_end:
                jumps += 1
                current_end = farthest
        return jumps

solution = Solution()
print(solution.jump(nums))
```
#### TC: O(n) **SC:** O(1)

1. Initialize `jumps`, `current_end`, and `farthest` to keep track of the number of jumps, the end
of the current jump range, and the farthest point that can be reached, respectively. 2. Iterate
through the list `nums` up to the second-to-last element. 3. For each element, update `farthest` to
be the maximum of its current value and the sum of the current index and the value at that index. 4.
If the current index reaches `current_end`, increment the `jumps` counter and update `current_end`
to `farthest`. 5. Return the total number of jumps needed to reach the end of the list.

---
### Jump Game II -- Best TC Solution:
```python
# Greedy Algorithm Approach

def jump(nums):
    if len(nums) == 1:
        return 0
    jumps = 0
    current_end = 0
    farthest = 0
    for i in range(len(nums) - 1):
        farthest = max(farthest, i + nums[i])
        if i == current_end:
            jumps += 1
            current_end = farthest
    return jumps

# Example
nums = [2, 3, 1, 1, 4]
print(jump(nums))  # Output: 2
```
#### TC: Time Complexity: O(n) **SC:** Space Complexity: O(1)

The code uses a Greedy Algorithm approach to solve the Jump Game II problem efficiently. It iterates
through the array of numbers, updating the farthest reachable index at each step. When the current
index reaches the current end, it increments the jumps count and updates the current end to the
farthest reachable index. This approach ensures that the minimum number of jumps required to reach
the end of the array is calculated. The Time Complexity of the code is O(n) as it iterates through
the array once, and the Space Complexity is O(1) as it uses a constant amount of extra space.

---
---
---
 --- 
# Best Time to Buy and Sell Stock II
>Maximize profit by buying and selling stocks multiple times.

>Input: prices = [7,1,5,3,6,4]
Output: 7
- https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
- Difficulty: 50
- Frequent: 70
- Tags: ['Array', 'Greedy']

### Best Time to Buy and Sell Stock II -- Shortest Solution:
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        return sum(max(prices[i+1] - prices[i], 0) for i in range(len(prices) - 1))
```
#### TC: O(n) **SC:** O(1)

The code iterates through the list of prices and calculates the profit for every consecutive pair of
days where the price increases. It sums up all these profits to get the maximum profit. The use of a
generator expression inside the sum function makes the code concise and efficient.

---
---
---
 --- 
# Candy
>Distribute candies to children with certain conditions.

>Input: ratings = [1,0,2]
Output: 5
- https://leetcode.com/problems/candy/
- Difficulty: 65
- Frequent: 70
- Tags: ['Array', 'Greedy']

### Candy -- Shortest Solution:
```python
def candy(ratings):
    n = len(ratings)
    candies = [1] * n
    for i in range(1, n):
        if ratings[i] > ratings[i - 1]:
            candies[i] = candies[i - 1] + 1
    for i in range(n - 2, -1, -1):
        if ratings[i] > ratings[i + 1]:
            candies[i] = max(candies[i], candies[i + 1] + 1)
    return sum(candies)
```
#### TC: O(n) **SC:** O(n)

The solution uses a two-pass greedy algorithm. The first pass ensures that each child has more
candies than the previous one if their rating is higher. The second pass ensures that each child has
more candies than the next one if their rating is higher, while keeping the maximum candies from
both passes. This guarantees the minimum candies distribution while satisfying the problem's
conditions.

---
---
---
 --- 
# Split Array into Consecutive Subsequences
>Split the array into consecutive subsequences.

>Input: nums = [1,2,3,3,4,5]
Output: true
- https://leetcode.com/problems/split-array-into-consecutive-subsequences/
- Difficulty: 60
- Frequent: 65
- Tags: ['Array', 'Greedy', 'Heap']

### Split Array into Consecutive Subsequences -- Shortest Solution:
```python
from collections import Counter, defaultdict

def isPossible(nums):
    freq = Counter(nums)
    appendfreq = defaultdict(int)
    for num in nums:
        if not freq[num]:
            continue
        freq[num] -= 1
        if appendfreq[num - 1] > 0:
            appendfreq[num - 1] -= 1
            appendfreq[num] += 1
        elif freq[num + 1] > 0 and freq[num + 2] > 0:
            freq[num + 1] -= 1
            freq[num + 2] -= 1
            appendfreq[num + 2] += 1
        else:
            return False
    return True
```
#### TC: O(n) **SC:** O(n)

1. **Counter and defaultdict**: The code uses `Counter` to count the frequency of each number in the
input list and `defaultdict` to keep track of the potential subsequences that can be extended. 2.
**Main Loop**: The main loop iterates through each number in the input list. If the frequency of the
current number is zero, it continues to the next number. 3. **Appending to Subsequences**: If there
is a subsequence ending with the previous number (`num - 1`), the current number is appended to that
subsequence. 4. **Creating New Subsequences**: If the current number cannot be appended to an
existing subsequence, the code checks if it can start a new subsequence with the next two numbers
(`num + 1` and `num + 2`). If not, it returns `False`. 5. **Return True**: If the loop completes
without returning `False`, it means the array can be split into consecutive subsequences, and the
function returns `True`.

---
---
---
 --- 
# Queue Reconstruction by Height
>Reconstruct the queue based on height and position.

>Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
- https://leetcode.com/problems/queue-reconstruction-by-height/
- Difficulty: 70
- Frequent: 65
- Tags: ['Array', 'Greedy']

### Queue Reconstruction by Height -- Shortest Solution:
```python
people.sort(key=lambda x: (-x[0], x[1]))
queue = []
for p in people:
    queue.insert(p[1], p)
return queue
```
#### TC: O(n^2) **SC:** O(n)

1. **Sorting**: The people are sorted in descending order of height. If two people have the same
height, they are sorted by the number of people in front of them in ascending order. 2.
**Insertion**: The sorted people are then inserted into the queue one by one at the index equal to
the number of people in front of them. 3. **Greedy Approach**: This approach ensures that each
person is placed in the correct position relative to the people already placed in the queue.

---
---
---
 --- 
# Gas Station
>Find the starting gas station index to complete the circuit.

>Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
- https://leetcode.com/problems/gas-station/
- Difficulty: 50
- Frequent: 60
- Tags: ['Array', 'Greedy']

### Gas Station -- Shortest Solution:
```python
def canCompleteCircuit(gas, cost):
    if sum(gas) < sum(cost):
        return -1
    total, start = 0, 0
    for i in range(len(gas)):
        total += gas[i] - cost[i]
        if total < 0:
            total, start = 0, i + 1
    return start
```
#### TC: O(n) **SC:** O(1)

1. The algorithm first checks if the total gas is less than the total cost. If it is, it's
impossible to complete the circuit, so it returns -1. 2. It then iterates through each gas station,
maintaining a running total of gas left after subtracting the cost to get to the next station. 3. If
at any point the total gas left is negative, it resets the total and sets the starting station to
the next station. 4. The starting station at the end of the iteration is the answer.

---
---
---
 --- 
# Minimum Number of Arrows to Burst Balloons
>Find the minimum number of arrows to burst all balloons.

>Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
- https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/
- Difficulty: 50
- Frequent: 60
- Tags: ['Array', 'Greedy']

### Minimum Number of Arrows to Burst Balloons -- Shortest Solution:
```python
points.sort(key=lambda x: x[1])
arrows = 1
end = points[0][1]
for x, y in points[1:]:
    if x > end:
        arrows += 1
        end = y
return arrows
```
#### TC: O(n log n) **SC:** O(1)

1. **Sorting by End Points**: The balloons are sorted by their end points. This helps in minimizing
the number of arrows needed. 2. **Initialization**: We start with one arrow and set the end point of
the first balloon as the initial end. 3. **Iterating Through Balloons**: For each balloon, if the
start point is greater than the current end, it means a new arrow is needed. We then update the end
to the current balloon's end. 4. **Counting Arrows**: The number of arrows is incremented each time
a new arrow is needed. 5. **Efficiency**: The algorithm efficiently determines the minimum number of
arrows by leveraging the sorted end points to minimize overlaps.

---
---
---
 --- 
# Reorganize String
>Reorganize the string so that no two adjacent characters are the same.

>Input: s = "aab"
Output: "aba"
- https://leetcode.com/problems/reorganize-string/
- Difficulty: 55
- Frequent: 60
- Tags: ['Greedy', 'Heap']

### Reorganize String -- Shortest Solution:
```python
from collections import Counter
import heapq

def reorganizeString(S):
    count = Counter(S)
    max_heap = [(-value, key) for key, value in count.items()]
    heapq.heapify(max_heap)
    prev_val, prev_char = 0, ''
    result = []
    while max_heap:
        val, char = heapq.heappop(max_heap)
        result.append(char)
        if prev_val < 0:
            heapq.heappush(max_heap, (prev_val, prev_char))
        val += 1
        prev_val, prev_char = val, char
    result = ''.join(result)
    return result if len(result) == len(S) else ''
```
#### TC: O(n log n) **SC:** O(n)

1. **Counter and Max-Heap**: The code uses a `Counter` to count the frequency of each character and
a max-heap to always get the character with the highest remaining count. 2. **Heap Operations**: The
heap operations ensure that the character with the highest frequency is placed first, and then the
next highest, ensuring no two adjacent characters are the same. 3. **Rebuilding the String**: The
result string is built by appending characters from the heap and pushing back the previous character
if it still has a remaining count. 4. **Validation**: Finally, the code checks if the length of the
result string matches the input string to ensure a valid reorganization.

---
### Reorganize String -- Best TC Solution:
```python
import heapq
class Solution:
    def reorganizeString(self, S: str) -> str:
        char_count = {}
        for char in S:
            char_count[char] = char_count.get(char, 0) + 1
        max_heap = [(-value, key) for key, value in char_count.items()]
        heapq.heapify(max_heap)
        result = []
        while len(max_heap) > 1:
            count1, char1 = heapq.heappop(max_heap)
            count2, char2 = heapq.heappop(max_heap)
            result.extend([char1, char2])
            if count1 + 1:
                heapq.heappush(max_heap, (count1 + 1, char1))
            if count2 + 1:
                heapq.heappush(max_heap, (count2 + 1, char2))
        return ''.join(result) + (max_heap[0][1] if max_heap else '')
```
#### TC: O(N log N) **SC:** O(N)

The code uses a greedy approach to reorganize the input string. It first counts the frequency of
each character in the string and creates a max heap based on the character frequencies. It then
iteratively extracts the two most frequent characters from the heap, appends them to the result
string, and updates their frequencies in the heap. This process continues until there is only one
character left in the heap. The final result is the reorganized string. The time complexity is O(N
log N) due to the heap operations, and the space complexity is O(N) to store the character
frequencies and the heap.

---
---
---
 --- 
# Wiggle Subsequence
>Find the length of the longest wiggle subsequence.

>Input: nums = [1,7,4,9,2,5]
Output: 6
- https://leetcode.com/problems/wiggle-subsequence/
- Difficulty: 50
- Frequent: 55
- Tags: ['Array', 'Greedy', 'Dynamic Programming']

### Wiggle Subsequence -- Shortest Solution:
```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        if len(nums) < 2:
            return len(nums)
        up = down = 1
        for i in range(1, len(nums)):
            if nums[i] > nums[i - 1]:
                up = down + 1
            elif nums[i] < nums[i - 1]:
                down = up + 1
        return max(up, down)
```
#### TC: O(n) **SC:** O(1)

The solution uses a greedy algorithm to track the length of the longest wiggle subsequence. It
maintains two variables, `up` and `down`, which represent the lengths of the longest wiggle
subsequences that end in a rising or falling wiggle, respectively. As it iterates through the list,
it updates these variables based on the current and previous elements. The final result is the
maximum of these two variables.

---
---
---
 --- 
# Non-overlapping Intervals
>Find the minimum number of intervals to remove to make the rest non-overlapping.

>Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
- https://leetcode.com/problems/non-overlapping-intervals/
- Difficulty: 55
- Frequent: 55
- Tags: ['Array', 'Greedy']

### Non-overlapping Intervals -- Shortest Solution:
```python
def eraseOverlapIntervals(intervals):
    intervals.sort(key=lambda x: x[1])
    end, count = float('-inf'), 0
    for interval in intervals:
        if interval[0] >= end:
            end = interval[1]
        else:
            count += 1
    return count
```
#### TC: O(n log n) **SC:** O(1)

The code sorts the intervals based on their end times. It then iterates through the sorted
intervals, keeping track of the end time of the last added interval. If the start time of the
current interval is greater than or equal to the end time of the last added interval, it updates the
end time. Otherwise, it increments the count of intervals to be removed. This ensures that the
maximum number of non-overlapping intervals are included.

---
---
---
 --- 
# Erase Overlap Intervals
>Find the minimum number of intervals to remove to make the rest non-overlapping.

>Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
- https://leetcode.com/problems/erase-overlap-intervals/
- Difficulty: 55
- Frequent: 55
- Tags: ['Array', 'Greedy']

### Erase Overlap Intervals -- Shortest Solution:
```python
intervals.sort(key=lambda x: x[1])
end = float('-inf')
erase_count = 0
for interval in intervals:
    if interval[0] >= end:
        end = interval[1]
    else:
        erase_count += 1
erase_count
```
#### TC: O(n log n) **SC:** O(1)

The code sorts the intervals based on their end times. It then iterates through the sorted
intervals, keeping track of the end time of the last added interval. If the start time of the
current interval is greater than or equal to the end time of the last added interval, it updates the
end time. Otherwise, it increments the erase count. This ensures that the minimum number of
intervals are removed to eliminate all overlaps.

---
---
---
 --- 
# Assign Cookies
>Assign cookies to children to maximize the number of content children.

>Input: g = [1,2,3], s = [1,1]
Output: 1
- https://leetcode.com/problems/assign-cookies/
- Difficulty: 40
- Frequent: 50
- Tags: ['Array', 'Greedy']

### Assign Cookies -- Shortest Solution:
```python
def findContentChildren(g, s):
    g.sort()
    s.sort()
    i = j = 0
    while i < len(g) and j < len(s):
        if s[j] >= g[i]:
            i += 1
        j += 1
    return i
```
#### TC: O(n log n + m log m) **SC:** O(1)

The code sorts the greed factors and the cookie sizes first. Then, it uses two pointers to iterate
through both lists. If a cookie can satisfy a child's greed factor, it moves to the next child.
Regardless, it always moves to the next cookie. The number of content children is returned at the
end.

---
### Assign Cookies -- Best TC Solution:
```python
# Greedy Algorithm Approach

def find_content_children(g: List[int], s: List[int]) -> int:
    g.sort()
    s.sort()
    i = 0
    j = 0
    while i < len(g) and j < len(s):
        if g[i] <= s[j]:
            i += 1
        j += 1
    return i
```
#### TC: O(n log n) **SC:** O(1

The code uses a Greedy Algorithm approach to solve the problem efficiently. It sorts the two input
lists, then iterates through them to find the maximum number of content children that can be
satisfied. The time complexity is O(n log n) due to the sorting operation, and the space complexity
is O(1) as no extra space is used apart from a few variables.

---
---
---
 --- 
# Is Subsequence
>Check if s is a subsequence of t.

>Input: s = "abc", t = "ahbgdc"
Output: true
- https://leetcode.com/problems/is-subsequence/
- Difficulty: 40
- Frequent: 50
- Tags: ['Greedy', 'Two Pointers']

### Is Subsequence -- Shortest Solution:
```python
def isSubsequence(s, t):
    it = iter(t)
    return all(c in it for c in s)
```
#### TC: O(n + m) **SC:** O(1)

The code uses an iterator to traverse the string `t`. For each character in `s`, it checks if the
character can be found in the remaining part of `t` using the iterator. This ensures that the order
of characters in `s` is maintained in `t`. The `all` function returns `True` if all characters in
`s` are found in sequence in `t`, otherwise it returns `False`. This approach is efficient and
concise.

---
### Is Subsequence -- Best TC Solution:
```python
# Greedy Algorithm Approach

def isSubsequence(s: str, t: str) -> bool:
    if not s:
        return True
    if not t:
        return False
    s_ptr = 0
    t_ptr = 0
    while t_ptr < len(t):
        if s[s_ptr] == t[t_ptr]:
            s_ptr += 1
            if s_ptr == len(s):
                return True
        t_ptr += 1
    return False

# Example
s = "abc"
t = "ahbgdc"
print(isSubsequence(s, t))  # Output: True
```
#### TC: O(n) **SC:** O(1)

The code uses a Greedy Algorithm approach to check if string s is a subsequence of string t. It
iterates through both strings using two pointers, incrementing the pointer for s only when a match
is found. The time complexity is O(n) where n is the length of string t, as we iterate through t
once. The space complexity is O(1) as we use a constant amount of extra space regardless of the
input size.

---
---
---
 --- 
# Monotone Increasing Digits
>Find the largest number that is less than or equal to n with monotone increasing digits.

>Input: n = 332
Output: 299
- https://leetcode.com/problems/monotone-increasing-digits/
- Difficulty: 45
- Frequent: 50
- Tags: ['Greedy']

### Monotone Increasing Digits -- Shortest Solution:
```python
class Solution:
    def monotoneIncreasingDigits(self, N: int) -> int:
        s = list(str(N))
        i = 1
        while i < len(s) and s[i] >= s[i - 1]:
            i += 1
        while 0 < i < len(s) and s[i] < s[i - 1]:
            s[i - 1] = str(int(s[i - 1]) - 1)
            i -= 1
        s[i + 1:] = '9' * (len(s) - i - 1)
        return int(''.join(s))
```
#### TC: O(n) **SC:** O(n)

1. Convert the number to a list of characters for easy manipulation. 2. Find the first position
where the digits stop being monotone increasing. 3. Move backwards to ensure the digits before this
position are also monotone increasing by decrementing the necessary digit. 4. Change all digits
after this position to '9' to maximize the number. 5. Convert the list back to an integer and return
it.

---
### Monotone Increasing Digits -- Best TC Solution:
```python
# Greedy Algorithm Approach

def monotone_increasing_digits(N):
    nums = list(str(N))
    n = len(nums)
    j = n
    for i in range(n - 1, 0, -1):
        if nums[i - 1] > nums[i]:
            j = i
            nums[i - 1] = str(int(nums[i - 1]) - 1)
    for k in range(j, n):
        nums[k] = '9'
    return int(''.join(nums))

# Example
N = 332
print(monotone_increasing_digits(N))  # Output: 299
```
#### TC: O(log N) **SC:** O(log N)

The code uses a greedy approach to find the largest monotone increasing number less than or equal to
the given number N. It iterates through the digits of N from right to left, identifying the point
where a decrease occurs. It then decreases the digit at that point and sets all subsequent digits to
9 to maintain monotonicity. The time complexity is O(log N) as it involves iterating through the
digits of N, and the space complexity is also O(log N) due to the list used to store the digits.

---
---
---
 --- 
# Lemonade Change
>Determine if you can provide every customer with correct change.

>Input: bills = [5,5,5,10,20]
Output: true
- https://leetcode.com/problems/lemonade-change/
- Difficulty: 30
- Frequent: 45
- Tags: ['Greedy']

### Lemonade Change -- Shortest Solution:
```python
def lemonadeChange(bills):
    fives = tens = 0
    for bill in bills:
        if bill == 5:
            fives += 1
        elif bill == 10:
            if not fives:
                return False
            fives -= 1
            tens += 1
        else:
            if tens and fives:
                tens -= 1
                fives -= 1
            elif fives >= 3:
                fives -= 3
            else:
                return False
    return True
```
#### TC: O(n) **SC:** O(1)

The code uses a greedy algorithm to ensure that each customer is given the correct change. It
maintains counters for $5 and $10 bills. For each bill received, it updates the counters and checks
if the correct change can be given. If at any point the correct change cannot be given, it returns
False. Otherwise, it returns True after processing all bills.

---
### Lemonade Change -- Best TC Solution:
```python
from typing import List

class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        five = ten = 0
        for bill in bills:
            if bill == 5:
                five += 1
            elif bill == 10:
                five -= 1
                ten += 1
            elif ten > 0:
                ten -= 1
                five -= 1
            else:
                five -= 3
            if five < 0:
                return False
        return True
```
#### TC: O(N) **SC:** O(1)

The code uses a greedy algorithm approach to solve the Lemonade Change problem efficiently. It
maintains counts of available $5 and $10 bills and processes each customer's payment accordingly.
The algorithm iterates through the bills array once, updating the bill counts and checking if change
can be given for each customer. The time complexity is O(N) where N is the number of customers, and
the space complexity is O(1) as only a constant amount of extra space is used to store the bill
counts.

---
---
---