# Sliding Window
## Frequent score for this algorithmic framework: 55 

|     | Leetcode Problem | Frequency Score | Difficulty |
| --- | --- | --- | --- |
| 1 | [Sliding Window Maximum](#sliding-window-maximum) | 75 | 60 |
| 2 | [Permutation in String](#permutation-in-string) | 65 | 50 |
| 3 | [Find All Anagrams in a String](#find-all-anagrams-in-a-string) | 65 | 50 |
| 4 | [Max Consecutive Ones III](#max-consecutive-ones-iii) | 65 | 55 |
| 5 | [Fruit Into Baskets](#fruit-into-baskets) | 60 | 50 |
| 6 | [Longest Repeating Character Replacement](#longest-repeating-character-replacement) | 60 | 55 |
| 7 | [Longest Subarray of 1's After Deleting One Element](#longest-subarray-of-1-s-after-deleting-one-element) | 60 | 55 |
| 8 | [Binary Subarrays With Sum](#binary-subarrays-with-sum) | 60 | 55 |
| 9 | [Grumpy Bookstore Owner](#grumpy-bookstore-owner) | 55 | 50 |
| 10 | [Maximum Number of Vowels in a Substring of Given Length](#maximum-number-of-vowels-in-a-substring-of-given-length) | 55 | 50 |
| 11 | [Replace the Substring for Balanced String](#replace-the-substring-for-balanced-string) | 55 | 55 |
| 12 | [Count Number of Nice Subarrays](#count-number-of-nice-subarrays) | 55 | 55 |
| 13 | [Maximum Erasure Value](#maximum-erasure-value) | 55 | 55 |
| 14 | [Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](#longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit) | 55 | 60 |
| 15 | [Subarrays with K Different Integers](#subarrays-with-k-different-integers) | 55 | 65 |
---
The Sliding Window technique involves maintaining a subset of elements within a window that moves over the data structure. This is useful for problems involving subarrays or substrings, where you need to find an optimal subset that meets certain criteria. The window can either be fixed or dynamic in size.
```python
# Example 1: Fixed-size sliding window to find the maximum sum of k consecutive elements

def max_sum_subarray(arr, k):
    n = len(arr)
    if n < k:
        return -1
    max_sum = sum(arr[:k])
    window_sum = max_sum
    for i in range(n - k):
        window_sum = window_sum - arr[i] + arr[i + k]
        max_sum = max(max_sum, window_sum)
    return max_sum

# Example 2: Dynamic-size sliding window to find the smallest subarray with a sum >= target

def min_length_subarray(arr, target):
    n = len(arr)
    min_length = float('inf')
    current_sum = 0
    start = 0
    for end in range(n):
        current_sum += arr[end]
        while current_sum >= target:
            min_length = min(min_length, end - start + 1)
            current_sum -= arr[start]
            start += 1
    return min_length if min_length != float('inf') else 0
```
### Insight:
1. Sliding Window is efficient for problems involving contiguous subarrays or substrings.
2. It reduces the time complexity from O(n^2) to O(n) by avoiding redundant calculations.
3. The fixed-size window is simpler but less flexible than the dynamic-size window.
4. Edge cases include handling arrays smaller than the window size or when no valid subarray exists.
5. Always initialize your window sum and adjust it as the window slides over the array.
---
 --- 
# Sliding Window Maximum
>Find the maximum value in each sliding window of size k.

>Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
- https://leetcode.com/problems/sliding-window-maximum/
- Difficulty: 60
- Frequent: 75
- Tags: ['Sliding Window']

### Sliding Window Maximum -- Shortest Solution:
```python
from collections import deque

def maxSlidingWindow(nums, k):
    d = deque()
    res = []
    for i, n in enumerate(nums):
        while d and nums[d[-1]] < n:
            d.pop()
        d.append(i)
        if d[0] == i - k:
            d.popleft()
        if i >= k - 1:
            res.append(nums[d[0]])
    return res
```
#### TC: O(n) **SC:** O(k)

1. The deque `d` stores indices of array elements, and it maintains the decreasing order of values
from front to back. 2. For each element in the array, we remove elements from the back of the deque
while they are smaller than the current element, ensuring the largest element is always at the
front. 3. We remove the front element if it is out of the current window's range. 4. Once the first
window is processed, we start adding the maximum for each window to the result list.

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
        for i, num in enumerate(nums):
            while q and nums[q[-1]] < num:
                q.pop()
            q.append(i)
            if q[0] == i - k:
                q.popleft()
            if i >= k - 1:
                res.append(nums[q[0]])
        return res
```
#### TC: O(N) **SC:** O(N)

The code implements the sliding window maximum using a deque to efficiently track the maximum
element within the window. It iterates through the input array, maintaining a deque that stores
indices of elements in non-increasing order. The code removes indices from the deque that are out of
the current window and appends the current index while maintaining the maximum element at the front
of the deque. The result is updated with the maximum element in each window. The time complexity is
O(N) as each element is processed once, and the space complexity is O(N) due to the deque storing at
most k elements.

---
---
---
 --- 
# Permutation in String
>Check if s2 contains a permutation of s1.

>Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
- https://leetcode.com/problems/permutation-in-string/
- Difficulty: 50
- Frequent: 65
- Tags: ['Sliding Window']

### Permutation in String -- Shortest Solution:
```python
from collections import Counter

def checkInclusion(s1: str, s2: str) -> bool:
    s1_count = Counter(s1)
    window = len(s1)
    for i in range(len(s2) - window + 1):
        if Counter(s2[i:i+window]) == s1_count:
            return True
    return False
```
#### TC: O(n * m) **SC:** O(1)

The code uses a sliding window approach to check if any substring of `s2` is a permutation of `s1`.
It first counts the frequency of characters in `s1` using `Counter`. Then, it iterates over `s2`
with a window size equal to the length of `s1`, checking if the character count of the current
window matches `s1`'s character count. If a match is found, it returns `True`. If no match is found
after checking all possible windows, it returns `False`. The time complexity is O(n * m) where `n`
is the length of `s2` and `m` is the length of `s1`. The space complexity is O(1) because the
`Counter` objects use a fixed amount of space.

---
### Permutation in String -- Best TC Solution:
```python
from collections import Counter

def checkInclusion(s1: str, s2: str) -> bool:
    if len(s1) > len(s2):
        return False
    s1_count = Counter(s1)
    s2_count = Counter(s2[:len(s1)])
    for i in range(len(s1), len(s2)):
        if s1_count == s2_count:
            return True
        s2_count[s2[i - len(s1)]] -= 1
        if s2_count[s2[i - len(s1)]] == 0:
            del s2_count[s2[i - len(s1)]]
        s2_count[s2[i]] += 1
    return s1_count == s2_count
```
#### TC: O(n) **SC:** O(1)

The code uses a sliding window approach to check if s1 is a permutation of any substring in s2. It
compares the counts of characters in s1 and the current window of s2. By sliding the window and
updating the character counts, it efficiently checks for permutations. The time complexity is O(n)
where n is the length of s2, and the space complexity is O(1) since the character counts are stored
in fixed-size dictionaries.

---
---
---
 --- 
# Find All Anagrams in a String
>Find all start indices of p's anagrams in s.

>Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation: The substring starting at index 0 and 6 are "cba" and "bac" respectively, which are anagrams of "abc".
- https://leetcode.com/problems/find-all-anagrams-in-a-string/
- Difficulty: 50
- Frequent: 65
- Tags: ['Sliding Window']

### Find All Anagrams in a String -- Shortest Solution:
```python
from collections import Counter

def findAnagrams(s: str, p: str) -> List[int]:
    ns, np = len(s), len(p)
    if ns < np:
        return []
    p_count = Counter(p)
    s_count = Counter(s[:np-1])
    result = []
    for i in range(np - 1, ns):
        s_count[s[i]] += 1
        if s_count == p_count:
            result.append(i - np + 1)
        s_count[s[i - np + 1]] -= 1
        if s_count[s[i - np + 1]] == 0:
            del s_count[s[i - np + 1]]
    return result
```
#### TC: O(n) **SC:** O(1)

1. The function uses the sliding window technique to find all anagrams of `p` in `s`. 2. `Counter`
from the `collections` module is used to count the frequency of characters in `p` and the initial
window of `s`. 3. The window slides over `s`, updating the counts and checking for matches with
`p`'s count. 4. If the counts match, the start index of the anagram is added to the result list. 5.
The space complexity is O(1) because the size of the `Counter` is bounded by the size of the
character set, which is constant.

---
---
---
 --- 
# Max Consecutive Ones III
>Find the maximum number of consecutive 1's in the array if you can flip at most k 0's.

>Input: nums = [1,1,0,0,1,1,1,0,1,1,1], k = 2
Output: 6
Explanation: Replace the two 0s with 1s to form the longest subarray of 1s.
- https://leetcode.com/problems/max-consecutive-ones-iii/
- Difficulty: 55
- Frequent: 65
- Tags: ['Sliding Window']

### Max Consecutive Ones III -- Shortest Solution:
```python
def longestOnes(nums, k):
    left = 0
    for right in range(len(nums)):
        k -= 1 - nums[right]
        if k < 0:
            k += 1 - nums[left]
            left += 1
    return right - left + 1
```
#### TC: O(n) **SC:** O(1)

The solution uses a sliding window approach to maintain a window with at most 'k' zeros. The 'left'
pointer moves right to shrink the window when the count of zeros exceeds 'k'. The 'right' pointer
expands the window by moving right. The difference between 'right' and 'left' pointers gives the
length of the longest subarray with at most 'k' zeros.

---
---
---
 --- 
# Fruit Into Baskets
>Find the total amount of fruit you can collect in each of two baskets.

>Input: fruits = [1,2,1]
Output: 3
Explanation: We can collect [1,2,1].
- https://leetcode.com/problems/fruit-into-baskets/
- Difficulty: 50
- Frequent: 60
- Tags: ['Sliding Window']

### Fruit Into Baskets -- Shortest Solution:
```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        basket = {}
        left = 0
        max_fruits = 0
        for right in range(len(fruits)):
            basket[fruits[right]] = basket.get(fruits[right], 0) + 1
            while len(basket) > 2:
                basket[fruits[left]] -= 1
                if basket[fruits[left]] == 0:
                    del basket[fruits[left]]
                left += 1
            max_fruits = max(max_fruits, right - left + 1)
        return max_fruits
```
#### TC: O(n) **SC:** O(1)

The solution uses a sliding window approach to maintain a window that contains at most two types of
fruits. The `basket` dictionary keeps track of the count of each type of fruit in the current
window. The `left` pointer adjusts to ensure the window remains valid (contains at most two types of
fruits). The `max_fruits` variable keeps track of the maximum number of fruits collected in any
valid window. The algorithm iterates through the list of fruits once, making it efficient with a
linear time complexity.

---
---
---
 --- 
# Longest Repeating Character Replacement
>Find the length of the longest substring containing the same letter after replacing at most k characters.

>Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'B' with an 'A' so that the longest substring containing the same letter is "AAAA".
- https://leetcode.com/problems/longest-repeating-character-replacement/
- Difficulty: 55
- Frequent: 60
- Tags: ['Sliding Window']

### Longest Repeating Character Replacement -- Shortest Solution:
```python
from collections import defaultdict

def characterReplacement(s: str, k: int) -> int:
    count = defaultdict(int)
    max_count = 0
    left = 0
    for right in range(len(s)):
        count[s[right]] += 1
        max_count = max(max_count, count[s[right]])
        if right - left + 1 - max_count > k:
            count[s[left]] -= 1
            left += 1
    return len(s) - left
```
#### TC: O(n) **SC:** O(1)

The solution uses a sliding window approach to maintain a window with the most frequent character
count. The window size is adjusted by moving the left pointer when the condition (window size - max
frequency character count > k) is violated. This ensures that the window always contains the most
frequent character with at most k replacements.

---
---
---
 --- 
# Longest Subarray of 1's After Deleting One Element
>Find the longest subarray of 1's after deleting one element.

>Input: nums = [1,1,0,1]
Output: 3
Explanation: After deleting the 0, the longest subarray of 1's is [1,1,1].
- https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/
- Difficulty: 55
- Frequent: 60
- Tags: ['Sliding Window']

### Longest Subarray of 1's After Deleting One Element -- Shortest Solution:
```python
def longest_subarray(nums):
    left = 0
    max_len = 0
    zero_count = 0
    for right in range(len(nums)):
        if nums[right] == 0:
            zero_count += 1
        while zero_count > 1:
            if nums[left] == 0:
                zero_count -= 1
            left += 1
        max_len = max(max_len, right - left)
    return max_len
```
#### TC: O(n) **SC:** O(1)

The code uses a sliding window approach to maintain a window with at most one zero. It iterates
through the array with a right pointer, and adjusts the left pointer to ensure the window contains
at most one zero. The maximum length of such a window is tracked and returned.

---
---
---
 --- 
# Binary Subarrays With Sum
>Find the number of non-empty subarrays with a sum equal to goal.

>Input: nums = [1,0,1,0,1], goal = 2
Output: 4
Explanation: The 4 subarrays are [1,0,1], [0,1,0,1], [1,0,1,0], and [0,1,0,1].
- https://leetcode.com/problems/binary-subarrays-with-sum/
- Difficulty: 55
- Frequent: 60
- Tags: ['Sliding Window']

### Binary Subarrays With Sum -- Shortest Solution:
```python
from collections import defaultdict

def numSubarraysWithSum(nums, goal):
    count = defaultdict(int)
    count[0] = 1
    prefix_sum = result = 0
    for num in nums:
        prefix_sum += num
        result += count[prefix_sum - goal]
        count[prefix_sum] += 1
    return result
```
#### TC: O(n) **SC:** O(n)

The code uses a prefix sum and a hashmap to efficiently count the number of subarrays that sum to
the given goal. The prefix sum keeps track of the cumulative sum of elements, and the hashmap stores
the frequency of each prefix sum encountered. By checking the difference between the current prefix
sum and the goal, the code can determine how many subarrays ending at the current index have the
desired sum.

---
---
---
 --- 
# Grumpy Bookstore Owner
>Find the maximum number of customers that can be satisfied in a bookstore.

>Input: customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], X = 3
Output: 16
Explanation: The bookstore owner keeps themselves not grumpy for the last 3 minutes. The maximum number of customers is 16.
- https://leetcode.com/problems/grumpy-bookstore-owner/
- Difficulty: 50
- Frequent: 55
- Tags: ['Sliding Window']

### Grumpy Bookstore Owner -- Shortest Solution:
```python
class Solution:
    def maxSatisfied(self, customers, grumpy, X):
        total_satisfied = sum(c for c, g in zip(customers, grumpy) if not g)
        max_additional_satisfied = additional_satisfied = sum(c * g for c, g in zip(customers[:X], grumpy[:X]))
        for i in range(X, len(customers)):
            additional_satisfied += customers[i] * grumpy[i] - customers[i - X] * grumpy[i - X]
            max_additional_satisfied = max(max_additional_satisfied, additional_satisfied)
        return total_satisfied + max_additional_satisfied
```
#### TC: O(n) **SC:** O(1)

1. Calculate the total number of satisfied customers without using the secret technique. 2. Use a
sliding window to find the maximum number of additional satisfied customers by applying the secret
technique for any X-minute window. 3. The sum of the initially satisfied customers and the maximum
additional satisfied customers gives the result.

---
---
---
 --- 
# Maximum Number of Vowels in a Substring of Given Length
>Find the maximum number of vowels in a substring of given length.

>Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowels.
- https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/
- Difficulty: 50
- Frequent: 55
- Tags: ['Sliding Window']

### Maximum Number of Vowels in a Substring of Given Length -- Shortest Solution:
```python
def maxVowels(s: str, k: int) -> int:
    vowels = set('aeiou')
    max_vowels = current_vowels = sum(1 for i in range(k) if s[i] in vowels)
    for i in range(k, len(s)):
        current_vowels += (s[i] in vowels) - (s[i - k] in vowels)
        max_vowels = max(max_vowels, current_vowels)
    return max_vowels
```
#### TC: O(n) **SC:** O(1)

The code uses a sliding window approach to maintain a count of vowels in the current window of
length `k`. It initializes the count for the first window and then slides the window one character
at a time, updating the count by adding the new character and removing the character that is no
longer in the window. This ensures that the solution is efficient with a linear time complexity.

---
---
---
 --- 
# Replace the Substring for Balanced String
>Find the minimum length of the substring that can be replaced to make the string balanced.

>Input: s = "QWER"
Output: 0
Explanation: s is already balanced.
- https://leetcode.com/problems/replace-the-substring-for-balanced-string/
- Difficulty: 55
- Frequent: 55
- Tags: ['Sliding Window']

### Replace the Substring for Balanced String -- Shortest Solution:
```python
from collections import Counter

def balancedString(s: str) -> int:
    count = Counter(s)
    n = len(s)
    required = n // 4
    left = 0
    min_len = n
    for right in range(n):
        count[s[right]] -= 1
        while left < n and all(count[char] <= required for char in 'QWER'):
            min_len = min(min_len, right - left + 1)
            count[s[left]] += 1
            left += 1
    return min_len
```
#### TC: O(n) **SC:** O(1)

1. The problem is approached using the sliding window technique to find the smallest substring that
can be replaced to balance the string. 2. A Counter is used to keep track of the frequency of each
character in the string. 3. The `required` variable represents the ideal frequency of each character
in a balanced string. 4. The `left` pointer is used to shrink the window from the left side, while
the `right` pointer expands the window from the right side. 5. The `while` loop ensures that the
window is valid (i.e., all characters are within the required frequency) and updates the minimum
length of the substring that needs to be replaced.

---
---
---
 --- 
# Count Number of Nice Subarrays
>Find the number of subarrays with exactly k odd numbers.

>Input: nums = [1,1,2,1,1], k = 3
Output: 2
Explanation: The subarrays are [1,1,2,1] and [1,2,1,1].
- https://leetcode.com/problems/count-number-of-nice-subarrays/
- Difficulty: 55
- Frequent: 55
- Tags: ['Sliding Window']

### Count Number of Nice Subarrays -- Shortest Solution:
```python
from collections import defaultdict

def numberOfSubarrays(nums, k):
    count = defaultdict(int)
    count[0] = 1
    odd = result = 0
    for num in nums:
        odd += num % 2
        result += count[odd - k]
        count[odd] += 1
    return result
```
#### TC: O(n) **SC:** O(n)

1. We use a hash map to keep track of the count of prefix sums where the prefix sum is the number of
odd numbers encountered so far. 2. We initialize the hash map with count[0] = 1 to handle the case
where a subarray itself has exactly k odd numbers. 3. As we iterate through the array, we update the
count of odd numbers and check if there is a prefix sum that, when subtracted from the current
prefix sum, equals k. If so, we add the count of such prefix sums to the result. 4. This approach
ensures that we only traverse the array once, making it efficient.

---
---
---
 --- 
# Maximum Erasure Value
>Find the maximum sum of a subarray with unique elements.

>Input: nums = [4,2,4,5,6]
Output: 17
Explanation: The subarray [2,4,5,6] has the maximum sum of 17.
- https://leetcode.com/problems/maximum-erasure-value/
- Difficulty: 55
- Frequent: 55
- Tags: ['Sliding Window']

### Maximum Erasure Value -- Shortest Solution:
```python
def maximumUniqueSubarray(nums):
    seen = set()
    left = 0
    max_sum = curr_sum = 0
    for right in range(len(nums)):
        while nums[right] in seen:
            seen.remove(nums[left])
            curr_sum -= nums[left]
            left += 1
        seen.add(nums[right])
        curr_sum += nums[right]
        max_sum = max(max_sum, curr_sum)
    return max_sum
```
#### TC: O(n) **SC:** O(n)

The solution uses a sliding window approach to maintain a window of unique elements. The `seen` set
keeps track of the unique elements in the current window. The `left` pointer adjusts to ensure all
elements in the window are unique. The `curr_sum` keeps the sum of the current window, and `max_sum`
records the maximum sum found. The algorithm efficiently processes each element in linear time.

---
---
---
 --- 
# Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit
>Find the longest continuous subarray with absolute difference less than or equal to limit.

>Input: nums = [8,2,4,7], limit = 4
Output: 2
Explanation: The longest subarray is [2,4] with absolute difference 2.
- https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/
- Difficulty: 60
- Frequent: 55
- Tags: ['Sliding Window']

### Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit -- Shortest Solution:
```python
from collections import deque

def longest_subarray(nums, limit):
    max_d, min_d = deque(), deque()
    left = 0
    for right, num in enumerate(nums):
        while max_d and num > max_d[-1]:
            max_d.pop()
        while min_d and num < min_d[-1]:
            min_d.pop()
        max_d.append(num)
        min_d.append(num)
        if max_d[0] - min_d[0] > limit:
            if max_d[0] == nums[left]:
                max_d.popleft()
            if min_d[0] == nums[left]:
                min_d.popleft()
            left += 1
    return len(nums) - left
```
#### TC: O(n) **SC:** O(n)

The solution uses a sliding window approach with two deques to keep track of the maximum and minimum
values in the current window. The left pointer is adjusted whenever the absolute difference between
the maximum and minimum values exceeds the limit. This ensures that the window always satisfies the
condition, and the length of the longest valid subarray is calculated.

---
---
---
 --- 
# Subarrays with K Different Integers
>Find the number of subarrays with exactly K different integers.

>Input: nums = [1,2,1,2,3], k = 2
Output: 7
Explanation: Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,3].
- https://leetcode.com/problems/subarrays-with-k-different-integers/
- Difficulty: 65
- Frequent: 55
- Tags: ['Sliding Window']

### Subarrays with K Different Integers -- Shortest Solution:
```python
from collections import Counter

def subarraysWithKDistinct(A, K):
    def atMostK(A, K):
        count = Counter()
        left = 0
        res = 0
        for right in range(len(A)):
            if count[A[right]] == 0:
                K -= 1
            count[A[right]] += 1
            while K < 0:
                count[A[left]] -= 1
                if count[A[left]] == 0:
                    K += 1
                left += 1
            res += right - left + 1
        return res
    return atMostK(A, K) - atMostK(A, K - 1)
```
#### TC: O(n) **SC:** O(n)

The solution uses a sliding window approach to count the number of subarrays with at most K distinct
integers. By calculating the difference between the number of subarrays with at most K distinct
integers and the number of subarrays with at most K-1 distinct integers, we get the number of
subarrays with exactly K distinct integers. The Counter from the collections module helps in keeping
track of the count of each integer in the current window.

---
### Subarrays with K Different Integers -- Best TC Solution:
```python
from collections import defaultdict

def at_most_k(nums, k):
    count = 0
    left = 0
    freq = defaultdict(int)
    result = 0
    for right in range(len(nums)):
        if freq[nums[right]] == 0:
            k -= 1
        freq[nums[right]] += 1
        while k < 0:
            freq[nums[left]] -= 1
            if freq[nums[left]] == 0:
                k += 1
            left += 1
        count += right - left + 1
        result += count
    return result

def subarrays_with_k_distinct(nums, k):
    return at_most_k(nums, k) - at_most_k(nums, k - 1)
```
#### TC: O(N) **SC:** O(N)

The code uses a sliding window approach to solve the problem efficiently. The 'at_most_k' function
calculates the number of subarrays with at most k distinct integers using a sliding window. The
'subarrays_with_k_distinct' function then calculates the total number of subarrays with exactly k
distinct integers by subtracting the count of subarrays with at most k-1 distinct integers from the
count of subarrays with at most k distinct integers. This approach ensures a time complexity of O(N)
and a space complexity of O(N), where N is the length of the input array.

---
### Subarrays with K Different Integers -- Best SC Solution:
```python
from collections import defaultdict

def at_most_k(nums, k):
    count = 0
    left = 0
    freq = defaultdict(int)
    result = 0
    for right in range(len(nums)):
        if freq[nums[right]] == 0:
            k -= 1
        freq[nums[right]] += 1
        while k < 0:
            freq[nums[left]] -= 1
            if freq[nums[left]] == 0:
                k += 1
            left += 1
        count += right - left + 1
        result += count
    return result

def subarrays_with_k_distinct(nums, k):
    return at_most_k(nums, k) - at_most_k(nums, k - 1)
```
#### TC: Time Complexity: O(N) **SC:** Space Complexity: O(N)

The code uses a sliding window approach to solve the problem efficiently. The 'at_most_k' function
calculates the number of subarrays with at most k distinct integers. The 'subarrays_with_k_distinct'
function then calculates the total number of subarrays with exactly k distinct integers by
subtracting the result of 'at_most_k(nums, k - 1)' from 'at_most_k(nums, k)'. This approach ensures
a time complexity of O(N) and a space complexity of O(N), making it an optimal solution for the
problem.

---
---
---